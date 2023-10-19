---
language:
- zh
- en
tags:
- codeshell
- wisdomshell
- pku-kcl
- openbankai
---

# CodeShell

CodeShell是[北京大学知识计算实验室](http://se.pku.edu.cn/kcl/)联合四川天府银行AI团队研发的多语言代码大模型基座。CodeShell具有70亿参数，在五千亿Tokens进行了训练，上下文窗口长度为8194。在权威的代码评估Benchmark（HumanEval与MBPP）上，CodeShell取得同等规模最好的性能。与此同时，我们提供了与CodeShell配套的部署方案与IDE插件，请参考代码库[CodeShell](https://github.com/WisdomShell/codeshell)。本仓库为CodeShell-7B-Chat的Int4量化模型的仓库。

CodeShell is a multi-language code LLM developed by the [Knowledge Computing Lab](http://se.pku.edu.cn/kcl/) of Peking University. CodeShell has 7 billion parameters and was trained on 500 billion tokens with a context window length of 8194. On authoritative code evaluation benchmarks (HumanEval and MBPP), CodeShell achieves the best performance of its scale. Meanwhile, we provide deployment solutions and IDE plugins that complement CodeShell. Please refer to the [CodeShell code repository](https://github.com/WisdomShell/codeshell) for more details. This repository is for the Int4 quantized model of CodeShell-7B-Chat.


## Main Characteristics of CodeShell

* **强大的性能**：CodelShell在HumanEval和MBPP上达到了7B代码基座大模型的最优性能
* **完整的体系**：除了代码大模型，同时开源IDE（VS Code与JetBrains）插件，形成开源的全栈技术体系
* **轻量化部署**：支持本地C++部署，提供轻量快速的本地化软件开发助手解决方案
* **全面的评测**：提供支持完整项目上下文、覆盖代码生成、代码缺陷检测与修复、测试用例生成等常见软件开发活动的多任务评测体系（即将开源）
* **高效的训练**：基于高效的数据治理体系，CodeShell在完全冷启动情况下，只训练了五千亿Token即获得了优异的性能

* **Powerful Performance**: CodeShell achieves optimal performance for a 7B code base model on HumanEval and MBPP.
* **Complete Ecosystem**: In addition to the mega code model, open-source IDE plugins (for VS Code and JetBrains) are also available, forming a comprehensive open-source full-stack technology system.
* **Lightweight Deployment**: Supports local C++ deployment, offering a lightweight and fast localized software development assistant solution.
* **Comprehensive Evaluation**: Provides a multi-task evaluation system that supports full project context, covering code generation, code defect detection and repair, test case generation, and other common software development activities (to be open-sourced soon).
* **Efficient Training**: Based on an efficient data governance system, CodeShell, even when starting from scratch, achieved outstanding performance with training on just 500 trillion tokens.

## Quickstart

CodeShell-7B-Chat量化版本 提供了Hugging Face格式的模型，开发者可以通过下列代码加载并使用。

CodeShell-7B-Chat-int4 offers a model in the Hugging Face format. Developers can load and use it with the following code.

```python
import time
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

device = torch.device('cuda:0')
model = AutoModelForCausalLM.from_pretrained('WisdomShell/CodeShell-7B-Chat-int4', trust_remote_code=True).to(device)
tokenizer = AutoTokenizer.from_pretrained('WisdomShell/CodeShell-7B-Chat-int4')

history = []
query = '你是谁?'
response = model.chat(query, history, tokenizer)
print(response)
history.append((query, response))

query = '用Python写一个HTTP server'
response = model.chat(query, history, tokenizer)
print(response)
history.append((query, response))
```

开发者也可以通过VS Code与JetBrains插件与CodeShell-7B-Chat量化版本交互，详情请参[VSCode插件仓库](https://github.com/WisdomShell/codeshell-vscode)与[IntelliJ插件仓库](https://github.com/WisdomShell/codeshell-intellij)。

Developers can also interact with CodeShell-7B-Chat-int4 through VS Code and JetBrains plugins. For details, please refer to the [VSCode Plugin Repository](https://github.com/WisdomShell/codeshell-vscode) and [IntelliJ Plugin Repository](https://github.com/WisdomShell/codeshell-intellij).

## Model Details


Code Shell使用GPT-2作为基础架构，采用Grouped-Query Attention、RoPE相对位置编码等技术。

Code Shell uses GPT-2 as its foundational architecture and incorporates technologies such as Grouped-Query Attention and RoPE relative position encoding.


| Hyper-parameter | Value  |
|---|---|
| n_layer | 42 |
| n_embd | 4096 |
| n_inner | 16384 |
| n_head | 32 |
| num_query_groups | 8 |
| seq-length | 8192 |
| vocab_size | 70144 |


## Evaluation

我们选取了目前最流行的两个代码评测数据集（HumanEval与MBPP）对模型进行评估，与目前最先进的两个7b代码大模型CodeLllama与Starcoder相比，Codeshell 取得了最优的成绩。具体评测结果如下。


We selected the two most popular code evaluation datasets currently available (HumanEval and MBPP) to assess the model. Compared to the two most advanced 7b LLM for code, CodeLllama and Starcoder, Codeshell achieved the best results. The specific evaluation results are as follows.

### Pass@1
|   任务   |  CodeShell-7b | CodeLlama-7b | Starcoder-7b |
| ------- | --------- | --------- | --------- |
| humaneval	 | **34.32** | 29.44 | 27.80 |
| mbpp		 | **38.65** | 37.60 | 34.16 |
| multiple-js	 | **33.17** | 31.30 | 27.02 |
| multiple-java	 | **30.43** | 29.24 | 24.30 |
| multiple-cpp	 | **28.21** | 27.33 | 23.04 |
| multiple-swift | 24.30 | **25.32** | 15.70 |
| multiple-php	 | **30.87** | 25.96 | 22.11 |
| multiple-d	 | 8.85 | **11.60** | 8.08 |
| multiple-jl	 | 22.08 | **25.28** | 22.96 |
| multiple-lua	 | 22.39 | **30.50** | 22.92 |
| multiple-r	 | **20.52** | 18.57 | 14.29 |
| multiple-rkt	 | **17.20** | 12.55 | 10.43 |
| multiple-rs	 | 24.55 | **25.90** | 22.82 |

# Statement

我们郑重声明，我们开发团队基于CodeShell模型开发了基于vscode和intellij的智能编码助手插件并均已开源。除此以外，无论是针对iOS、Android、HarmonyOS、Web，还是其他任何平台，我们的开发团队均未开发任何基于CodeShell模型的应用程序。我们强烈敦促所有用户不要利用CodeShell模型从事危害国家和社会安全或违法活动。同时，我们要求用户不要在未经适当的安全审查和备案的互联网服务中使用CodeShell模型。我们希望所有用户都能遵守这一原则，以确保在合规和合法的环境下发展科技。

尽管我们在确保模型训练过程中使用数据合规性方面已付出巨大努力，但由于模型和数据的复杂性，可能会出现难以预料的问题。因此，对于使用CodeShell开源模型导致的任何问题，包括但不限于数据安全问题、公共舆论风险，或模型被误用、滥用、传播或不当利用等风险和问题，我们概不负责。

We hereby declare that our development team has developed intelligent coding assistant plugins for vscode and intellij based on the CodeShell model, both of which have been open-sourced. Beyond this, whether for iOS, Android, HarmonyOS, Web, or any other platform, our development team has not developed any applications based on the CodeShell model. We strongly urge all users not to use the CodeShell model for activities that endanger national and social security or are illegal. At the same time, we request users not to use the CodeShell model in internet services that have not undergone proper security reviews and registration. We hope all users will adhere to this principle to ensure the development of technology in a compliant and legal environment.

Despite our significant efforts to ensure compliance in the data used during the model training process, unforeseen issues may arise due to the complexity of the models and data. Therefore, we are not responsible for any issues arising from the use of the open-sourced CodeShell model, including but not limited to data security issues, public opinion risks, or risks and problems related to the model being misused, abused, disseminated, or exploited improperly.

# License

社区使用CodeShell模型需要遵循[CodeShell模型许可协议](https://huggingface.co/WisdomShell/CodeShell-7B/resolve/main/CodeShell%E6%A8%A1%E5%9E%8B%E8%AE%B8%E5%8F%AF%E5%8D%8F%E8%AE%AE.pdf)及[Apache 2.0 许可证](https://www.apache.org/licenses/LICENSE-2.0)。CodeShell模型允许用于商业用途，但如果您计划将CodeShell模型或其派生产品用于商业用途，需要您确认主体符合以下条件：

1. 关联方的服务或产品的每日平均活跃用户数（DAU）原则上不能超过100万。
2. 关联方不得是面向个人用户的软件服务提供商或云服务提供商。
3. 关联方不存在将获得授予的商业许可，在未经许可的前提下将其再授权给其他第三方的可能性。

在满足上述条件的前提下，您需要通过向codeshell.opensource@gmail.com发送电子邮件提交申请。经审核通过后，将授予您一个全球的、非排他的、不可转让的、不可再授权的商业版权许可。



Community use of the CodeShell model requires adherence to the CodeShell Model License Agreement and the Apache 2.0 License. The CodeShell model is allowed for commercial use, but if you plan to use the CodeShell model or its derivatives for commercial purposes, you need to ensure that the entity meets the following conditions:

1. The Daily Active Users (DAU) of your or your affiliate's service or product is less than 1 million.
2. You and your affiliates must not be a software service provider or cloud service provider targeting individual users.
3. You and your affiliates should not have the possibility of sub-licensing to other third parties without obtaining the commercial license granted.

Upon meeting the above conditions, you need to submit an application by sending an email to codeshell.opensource@gmail.com. After approval, you will be granted a global, non-exclusive, non-transferable, non-sublicensable commercial copyright license.

