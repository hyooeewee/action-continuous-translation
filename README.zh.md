# 🌍 持续翻译 (Continuous Translation)

> **使用 AI 自动翻译你的 Markdown 文档** - 由 [GitHub Actions](https://github.com/actions) 和 [GitHub Models](https://github.com/models) 驱动，并内置支持 [Astro Starlight](https://starlight.astro.build/)！

[![GitHub Action](https://img.shields.io/badge/GitHub-Action-blue?logo=github)](https://github.com/marketplace/actions/continuous-translation)
[![Documentation](https://img.shields.io/badge/📖-Documentation-green)](https://pelikhan.github.io/action-continuous-translation/)

## ✨ 特性

- 🚀 **增量翻译** - 仅翻译变更的内容，节省时间和 API 成本
- 🎯 **智能 AST 解析** - 保留 Markdown 的结构和格式
- 🔄 **缓存管理** - 智能缓存，避免重复翻译
- 📚 **兼容 Astro Starlight** - 为文档站点提供内置支持
- 🌐 **多语言支持** - 同时翻译成多种语言
- 🔍 **质量验证** - 自动验证翻译质量
- ⚡ **原生 GitHub Actions** - 与你的 CI/CD 流水线无缝集成
- 🤖 **AI 驱动** - 利用 GitHub Models 实现流畅的高质量翻译

## 📚 资源

- 📖 [**文档**](https://pelikhan.github.io/action-continuous-translation/) - 完整的配置指南和 API 参考（同样由本 Action 翻译 - 参见[翻译仪表盘](https://pelikhan.github.io/action-continuous-translation/dashboard/)）
- ✍️ [**博客文章**](https://microsoft.github.io/genaiscript/blog/continuous-translations/) - 深入探讨背后的技术
- 🌐 **翻译版本**: [English](./README.md) | [Français](./README.fr.md) | [Español](./README.es.md) | [Português (Brasil)](./README.pt-br.md) | [العربية](./README.ar.md) | [简体中文](./README.zh-cn.md)

## 🔧 工作原理

本 Action 利用 [GenAIScript](https://microsoft.github.io/genaiscript/) 智能地分析和翻译你的 Markdown 文档。以下是幕后的实现原理：

1. **📄 解析** - 将 Markdown 转换为 AST（抽象语法树）
2. **🔍 分析** - 识别需要翻译的内容与已有的翻译
3. **🤖 翻译** - 使用 AI 生成高质量的翻译
4. **✅ 验证** - 确保翻译质量并将其注入文档
5. **💾 缓存** - 保存翻译以供后续增量更新
6. **📝 提交** - 自动将更改提交到你的仓库

## ⚙️ 配置

### 📝 基本设置

| 参数                | 说明                                   | 默认值                                         |
| ------------------- | -------------------------------------- | ---------------------------------------------- |
| `lang`              | 目标翻译语言（ISO 代码，逗号分隔）     | `fr`                                           |
| `source`            | 源语言（ISO 代码）                     | `en`                                           |
| `files`             | 要翻译的文件（分号分隔）               | `README.md`                                    |
| `instructions`      | 自定义翻译指令                         | -                                              |
| `instructions_file` | 包含翻译指令的文件路径                 | -                                              |
| `glossary_file`     | 包含术语表术语的文件路径               | -                                              |
| `translations_dir`  | 存放翻译的文件夹                       | `translations`                                 |
| `filename_template` | 用于生成翻译文件路径的 Jinja 模板      | `{{dirname}}/{{basename}}.{{lang}}{{extname}}` |

### 限制

| 参数                     | 说明                                                    | 默认值 |
| ------------------------ | ------------------------------------------------------- | ------ |
| `max_translation_tokens` | 翻译 LLM 调用可用的最大 token 数（避免速率限制）        | `8000` |
| `max_validation_tokens`  | 验证 LLM 调用可用的最大 token 数（避免速率限制）        | `2000` |

### 🌟 Astro Starlight 集成

| 参数             | 说明                            | 必填                |
| ---------------- | ------------------------------- | ------------------- |
| `starlight_dir`  | Astro Starlight 文档的根目录    | 仅 Starlight 需要   |
| `starlight_base` | Starlight 文档的基础别名        | 可选                |

### 🔧 诊断与调试

| 参数    | 说明                                                                   | 默认值  |
| ------- | ---------------------------------------------------------------------- | ------- |
| `debug` | 启用调试日志（[了解更多](https://microsoft.github.io/genaiscript/reference/scripts/logging/)） | `false` |

### 🤖 AI 提供商配置

#### GitHub Models（推荐）

| 参数            | 说明                                                                                                                | 默认值                            |
| --------------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| `github_token`  | 具有 `models: read` 权限的 GitHub token（[配置指南](https://microsoft.github.io/genaiscript/reference/github-actions/#github-models-permissions)） | `${{ secrets.GITHUB_TOKEN }}`    |

#### OpenAI

| 参数               | 说明                   | 默认值                            |
| ------------------ | ---------------------- | --------------------------------- |
| `openai_api_key`   | OpenAI API 密钥        | `${{ secrets.OPENAI_API_KEY }}`   |
| `openai_api_base`  | OpenAI API 基础 URL    | `${{ env.OPENAI_API_BASE }}`      |

#### Azure OpenAI

| 参数                            | 说明                                            | 默认值                                     |
| ------------------------------- | ----------------------------------------------- | ------------------------------------------ |
| `azure_openai_api_endpoint`     | Azure OpenAI 端点                               | `${{ env.AZURE_OPENAI_API_ENDPOINT }}`     |
| `azure_openai_api_key`          | Azure OpenAI API 密钥（Microsoft Entra ID 不需要） | `${{ secrets.AZURE_OPENAI_API_KEY }}`     |
| `azure_openai_subscription_id`  | 用于部署列表的订阅 ID（仅 Entra ID）            | `${{ env.AZURE_OPENAI_SUBSCRIPTION_ID }}` |
| `azure_openai_api_version`      | Azure OpenAI API 版本                           | `${{ env.AZURE_OPENAI_API_VERSION }}`     |
| `azure_openai_api_credentials`  | API 凭据类型                                    | `${{ env.AZURE_OPENAI_API_CREDENTIALS }}` |

#### 模型别名

| 参数            | 说明                          | 默认值 |
| --------------- | ----------------------------- | ------ |
| `model_alias`   | 由 `alias: modelid` 对组成的类似 YAML 的字符串 |        |

更多详情请参阅[模型](/action-continuous-translation/models/)文档。

## 📤 输出

| 输出   | 说明                 |
| ------ | -------------------- |
| `text` | 生成的翻译文本输出   |

## 🚀 快速开始

### 简单配置

将此步骤添加到你的 GitHub Actions 工作流中，即可将你的 README 翻译成法语和西班牙语：

```yaml
uses: pelikhan/action-continuous-translation@v0
with:
  github_token: ${{ secrets.GITHUB_TOKEN }}
  lang: fr,es
```

### 完整工作流示例

将此文件保存到你的 `.github/workflows/` 目录下，命名为 `continuous-translation.yml`：

```yaml
name: Continuous Translation
on:
  workflow_dispatch:
  push:
    branches:
      - main
    paths:
      - "README.md"
      - "docs/src/content/docs/**"
permissions:
  contents: write
  models: read
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
jobs:
  continuous_translation:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/cache@v4
        with:
          path: .genaiscript/cache/**
          key: continuous-translation-${{ github.run_id }}
          restore-keys: |
            continuous-translation-
      - uses: pelikhan/action-continuous-translation@v0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          lang: fr,es
      - uses: stefanzweifel/git-auto-commit-action@v5
        with:
          file_pattern: "**.md* translations/**"
          commit_message: "[cai] translated docs"
          commit_user_name: "genaiscript"
```

<div align="center">

**使用 [GenAIScript](https://microsoft.github.io/genaiscript/) 制作，充满 ❤️**

</div>