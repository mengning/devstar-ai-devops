# 基于 Gitea 和 Ollama (open-webui) 的代码合并自动检查

如果您需要对合并的代码进行审核，但又希望避免将代码发送给第三方，或者您的网络环境处于离线状态（无法连接到第三方平台），那么本项目将是一个理想的选择。

该项目结合了 Gitea 和 Ollama (open-webui)，能够自动审核合并代码，并将结果通过评论的形式推送到相应的合并请求中，供开发人员或审核人员参考使用。

- [如何使用 How to use](https://github.com/kekxv/AiReviewPR#%E5%A6%82%E4%BD%95%E4%BD%BF%E7%94%A8-How-to-use)
- [输入参数](https://github.com/kekxv/AiReviewPR#%E8%BE%93%E5%85%A5%E5%8F%82%E6%95%B0)
- [Input Parameters](https://github.com/kekxv/AiReviewPR#input-parameters)

# 如何使用 How to use

 [ollma部署LLM](docs/deploy-llm.md)

使用方式和普通的 github actions 没什么区别(gitea actions 基本兼容 github actions)。

需要设置一个 ollama host，如果使用的是 open-webui ，建议加上授权token。

```
name: ai-reviews

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - name: Review code
        uses: kekxv/AiReviewPR@v0.0.6
        with:
          model: 'deepseek-r1:1.5b'
          host: 'http://172.20.184.132:11434' //设置为自己的ollama host
          REVIEW_PULL_REQUEST: false
```



效果如下：

result：

![](docs/img/aireview-1.png)

![](docs/img/aireview-2.png)

## 输入参数



1. **repository**

- **描述**: 存储库名称，格式为 `owner/repo`（例如 `actions/checkout`）。
- **默认值**: `${{ github.repository }}`（当前 GitHub 仓库的名称）。

1. **REVIEW_PULL_REQUEST**

- **描述**: 指定是否要比较从提交开始到最新的记录；设置为 `false` 表示只审核最新一次提交。
- **默认值**: `false`。

1. **BASE_REF**

- **描述**: 当前 GitHub 事件中的 pull_request 的基准分支。
- **默认值**: `${{ github.event.pull_request.base.ref }}`。

1. **PULL_REQUEST_NUMBER**

- **描述**: 当前 GitHub 事件中的 pull_request 编号。
- **默认值**: `${{ github.event.pull_request.number }}`。

1. **CHINESE**

- **描述**: 使用中文（作废）。
- **默认值**: `""`。

1. **LANGUAGE**

- **描述**: 使用语言（中文）。
- **默认值**: `"Chinese"`。

1. **token**

- **描述**: 用于访问存储库的个人访问令牌（PAT）。该令牌配置在本地 git 配置中，允许脚本运行经过身份验证的 git 命令。作业结束时会移除 PAT。
- **建议**: 使用权限最少的服务账号生成新的 PAT 并仅选择必要的作用域。
- **文档链接**: [了解如何创建和使用加密密钥](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets)。
- **默认值**: `${{ github.token }}`。

1. **model**

- **描述**: 用于代码审核的 AI 模型。
- **必需**: 是。
- **默认值**: `'gemma2:2b'`。

1. **host**

- **描述**: Ollama 主机地址。
- **必需**: 是。
- **默认值**: `'http://127.0.0.1:11434'`。

1. **PROMPT_GENRE**

- **描述**: 提示生成的类型。
- **默认值**: `' '`（空格）。

1. **reviewers_prompt**

- **描述**: Ollama 系统提示信息。
- **必需**: 否。
- **默认值**: `""`（空字符串）。

1. **ai_token**

- **描述**: AI 令牌。
- **必需**: 否。
- **默认值**: `" "`（空格）。

1. **include_files**

- **描述**: 要包括审查的文件的以逗号分隔的列表。
- **必需**: 否。
- **默认值**: `" "`（默认为空，表示不限制）。

1. **exclude_files**

- **描述**: 要排除审查的文件的以逗号分隔的列表。
- **必需**: 否。
- **默认值**: `" "`（默认为空，表示不传递文件）。
