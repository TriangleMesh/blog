+++
title = '如何使用 Hugo 和 GitHub 搭建博客'
date = 2024-07-26T19:47:00+08:00
# draft = true
+++

## 为什么选择 Hugo

- **容易上手**：Hugo 提供大量现成模板，几乎不需要前端知识，就能快速搭建网站。
- **页面生成速度快**：相比我之前使用的 Hexo，Hugo 生成页面的速度明显更快。这是因为 Hugo 使用预编译模板，减少了渲染开销，并采用编译速度和运行性能都很出色的 Go 语言编写。
- **模板美观**：Hugo 的模板整体上比 Hexo 更简洁。我看过不少博客后，觉得目前使用的这个 Hugo 模板在设计上最合心意。
- **对 Go 的亲切感**：我的第一个 Web 后端项目就是用 Go 写的，因此对这门语言有一些感情。虽然使用 Hugo 搭建博客并不需要直接编写多少 Go 代码，这种联系还是影响了我的选择。

## 搭建过程中参考的资料

- **[Hugo 官方文档](https://gohugo.io/documentation/)**
- **使用 GitHub 和 Hugo 搭建网站的教程**
- **GitHub 上使用同一主题的其他用户的博客和仓库**
- **[Hugo 主题官网](https://themes.gohugo.io/)**

> 在这些资料中，我尤其推荐 [Hugo 官方文档](https://gohugo.io/documentation/)。它更新及时，内容详细，也比较容易阅读；其他网上教程可能已经过时。跟着 [Quick Start](https://gohugo.io/getting-started/quick-start/) 就能搭起博客框架。之后可以根据需要修改主题文件，初期重点关注 `config.toml` 和 `content` 文件夹，并使用 `hugo server` 在本地预览改动。

## 文件结构

> 也可以参考 [Hugo 文档：目录结构](https://gohugo.io/getting-started/directory-structure/)。

### `content` 和 `public`

- `content` 文件夹保存博客的原始 Markdown 文件。
- `public` 文件夹保存 Hugo 根据 Markdown 自动生成的 HTML 文件，之后会将它们推送到名为 `<replace_with_your_github_username>.github.io` 的 GitHub 仓库。
- `public` 中的文件是自动生成的，通常不需要手动修改。运行 `hugo` 就会自动转换新添加的文件。不过，仅仅删除 `content` 中的文件，并不会自动删除 `public` 中对应的旧 HTML 文件。虽然保留旧文件未必影响显示，但会让项目结构不够整洁。我的做法是删除 `public` 目录下除 `.git` 文件之外的所有文件，再让 Hugo 按当前内容重新生成 HTML。

### `themes`

- `themes` 文件夹保存 Hugo 主题文件，其中包括：
    - `exampleSite` 文件夹：提供示例网站信息。建议将其中的 `.toml` 文件，以及 `content` 和 `static` 文件夹复制到博客项目的对应位置进行测试。
    - `layouts` 和 `static` 文件夹：包含构建网站结构及渲染效果的 HTML、CSS 文件。之后可以修改这些文件，以及博客项目中的其他配置文件，调整网站布局和样式。

## GitHub 仓库

### 创建仓库

如果使用 GitHub 托管项目文件，一般需要三个仓库：

1. 保存整个站点信息的仓库，名称可以自定。
2. 保存 `public` 文件夹中 HTML 文件的仓库，名称为 `<replace_with_your_github_username>.github.io`。这个仓库必须使用该名称，它也将成为博客的网址。
3. 主题仓库（可选）。虽然很多教程建议直接使用原主题仓库，我还是将主题 fork 到了自己的账号下，方便管理和维护。

> 在这三个仓库中，第一个仓库是第二、第三个仓库的主仓库。推荐使用 submodule 管理它们，具体可以参考我的另一篇[博客文章](https://trianglemesh.github.io/blog/2024/07/26/git-submodule-simplified-tutorial/)。也可以直接将第二、第三个仓库克隆到第一个仓库中进行管理。

### 使用 GitHub Actions 自动部署

- 手动推送文件比较繁琐，因此也可以使用 GitHub Actions 自动部署。这样每次推送更新时，GitHub 都会自动重新生成并部署网站文件。
