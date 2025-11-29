# Hugo 完整操作手册

## 目录
1. [基础概念](#基础概念)
2. [常用命令](#常用命令)
3. [文章管理](#文章管理)
4. [配置说明](#配置说明)
5. [主题管理](#主题管理)
6. [部署到 GitHub Pages](#部署到-github-pages)
7. [常见问题](#常见问题)

---

## 基础概念

### Hugo 是什么？
Hugo 是一个用 Go 语言编写的静态网站生成器，特点是速度极快、使用简单。

### 目录结构
```
Blog/
├── archetypes/       # 内容模板（新建文章时的默认格式）
├── content/          # 网站内容（Markdown 文件）
│   ├── _index.md    # 首页内容
│   └── posts/       # 博客文章
├── layouts/          # HTML 模板（页面布局）
├── static/           # 静态资源（图片、CSS、JS）
├── themes/           # 主题目录
├── public/           # 生成的静态网站（部署这个目录）
└── hugo.toml         # 配置文件
```

### Front Matter（文章头部信息）
每篇文章开头的元数据：
```toml
+++
title = '文章标题'
date = 2025-11-02T01:36:23+08:00
draft = false          # true=草稿，false=发布
tags = ['标签1', '标签2']
categories = ['分类']
+++

文章正文内容...
```

---

## 常用命令

### 1. 启动开发服务器
```bash
# 基础启动（不显示草稿）
hugo server

# 显示草稿内容
hugo server -D

# 允许外部访问
hugo server --bind 0.0.0.0

# 指定端口
hugo server -p 8080
```

访问地址：`http://localhost:1313`

### 2. 创建内容

```bash
# 创建首页
hugo new _index.md

# 创建新文章
hugo new posts/my-first-post.md

# 创建关于页面
hugo new about.md

# 创建指定目录的文章
hugo new posts/tech/golang-tutorial.md
```

### 3. 构建网站

```bash
# 基础构建（输出到 public/）
hugo

# 包含草稿
hugo -D

# 清理并重新构建
hugo --cleanDestinationDir

# 构建到指定目录
hugo -d docs
```

### 4. 其他命令

```bash
# 查看版本
hugo version

# 检查配置
hugo config

# 列出所有内容
hugo list all

# 查看帮助
hugo help
```

---

## 文章管理

### 完整的文章创建流程

#### 步骤 1：创建文章
```bash
hugo new posts/my-article.md
```

#### 步骤 2：编辑文章
```bash
# 使用你喜欢的编辑器
nano content/posts/my-article.md
# 或
vim content/posts/my-article.md
# 或
code content/posts/my-article.md
```

#### 步骤 3：编写内容
```markdown
+++
title = '我的新文章'
date = 2025-11-02T10:00:00+08:00
draft = true                          # 先设为草稿
tags = ['Hugo', '教程']
categories = ['技术']
description = '这是文章描述'
+++

## 这是标题

这是正文内容...

### 小标题

- 列表项 1
- 列表项 2

```代码块```
```

#### 步骤 4：预览草稿
```bash
hugo server -D
```
在浏览器访问 `http://localhost:1313`

#### 步骤 5：发布文章
确认无误后，将 `draft = true` 改为 `draft = false`：
```toml
+++
title = '我的新文章'
date = 2025-11-02T10:00:00+08:00
draft = false                         # 改为 false
+++
```

#### 步骤 6：重新构建
```bash
hugo
```

### 草稿 vs 发布

| 状态 | draft 值 | hugo server | hugo server -D | hugo（构建） |
|------|----------|-------------|----------------|--------------|
| 草稿 | true     | ❌ 不显示   | ✅ 显示        | ❌ 不生成    |
| 发布 | false    | ✅ 显示     | ✅ 显示        | ✅ 生成      |

---

## 配置说明

### hugo.toml 基础配置

```toml
baseURL = 'https://yourusername.github.io/'
languageCode = 'zh-CN'
title = '我的博客'
theme = 'ananke'                    # 如果使用主题

# 分页
paginate = 10

# 作者信息
[author]
  name = '你的名字'
  email = 'your@email.com'

# 菜单
[[menu.main]]
  name = '首页'
  url = '/'
  weight = 1

[[menu.main]]
  name = '文章'
  url = '/posts/'
  weight = 2

[[menu.main]]
  name = '关于'
  url = '/about/'
  weight = 3

# 参数
[params]
  description = '我的个人博客'
  dateFormat = '2006-01-02'
```

### 常用配置项

```toml
# 是否显示未来日期的文章
buildFuture = false

# 是否显示过期内容
buildExpired = false

# 是否启用表情符号
enableEmoji = true

# 是否启用 Git 信息
enableGitInfo = true

# 默认内容语言
defaultContentLanguage = 'zh-cn'

# 自动检测中文/日文/韩文
hasCJKLanguage = true
```

---

## 主题管理

### 方法 1：使用 Git Submodule（推荐）

```bash
# 添加主题
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke

# 更新主题
git submodule update --remote --merge

# 克隆项目后初始化主题
git submodule update --init --recursive
```

然后在 `hugo.toml` 中添加：
```toml
theme = 'ananke'
```

### 方法 2：直接下载

```bash
cd themes
git clone https://github.com/theNewDynamic/gohugo-theme-ananke.git ananke
```

### 热门主题推荐

1. **PaperMod** - 简洁快速的博客主题
   ```bash
   git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
   ```

2. **Stack** - 卡片式设计
   ```bash
   git submodule add https://github.com/CaiJimmy/hugo-theme-stack.git themes/stack
   ```

3. **LoveIt** - 功能丰富的中文主题
   ```bash
   git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
   ```

### 自定义主题

如果不想用第三方主题，可以自己创建布局文件：
```bash
layouts/
├── _default/
│   ├── baseof.html      # 基础模板
│   ├── list.html        # 列表页
│   └── single.html      # 单页
└── index.html           # 首页
```

---

## 部署到 GitHub Pages

### 方法 1：GitHub Actions 自动部署（推荐）

#### 步骤 1：创建 GitHub 仓库
```bash
# 在 GitHub 创建仓库：yourusername.github.io
# 或创建普通仓库，使用 /docs 或 gh-pages 分支
```

#### 步骤 2：创建 GitHub Actions 配置文件

创建文件 `.github/workflows/hugo.yml`：

```yaml
name: Deploy Hugo site to Pages

on:
  # 推送到 master 分支时触发
  push:
    branches:
      - master
  # 允许手动触发
  workflow_dispatch:

# 设置权限
permissions:
  contents: read
  pages: write
  id-token: write

# 只允许一个并发部署
concurrency:
  group: "pages"
  cancel-in-progress: false

# 默认使用 bash
defaults:
  run:
    shell: bash

jobs:
  # 构建任务
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.149.0
    steps:
      - name: 安装 Hugo
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb

      - name: 检出代码
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0

      - name: 配置 Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: 安装 Node.js 依赖（如果需要）
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"

      - name: 使用 Hugo 构建
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: 上传构建结果
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # 部署任务
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: 部署到 GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

#### 步骤 3：推送代码

```bash
# 初始化 git（如果还没有）
git init

# 添加所有文件（注意：不要添加 public/ 目录）
echo "public/" >> .gitignore
echo ".hugo_build.lock" >> .gitignore

git add .
git commit -m "Initial commit"

# 添加远程仓库
git remote add origin https://github.com/yourusername/yourusername.github.io.git

# 推送
git branch -M master
git push -u origin master
```

#### 步骤 4：启用 GitHub Pages

1. 进入 GitHub 仓库设置 → Pages
2. Source 选择 "GitHub Actions"
3. 等待部署完成

#### 步骤 5：访问网站
```
https://yourusername.github.io/
```

### 方法 2：手动部署

#### 适用于 username.github.io 仓库

```bash
# 构建网站
hugo

# 进入 public 目录
cd public

# 初始化 git（如果是第一次）
git init
git add .
git commit -m "Deploy website"
git branch -M main
git remote add origin https://github.com/yourusername/yourusername.github.io.git
git push -u origin main
```

#### 使用部署脚本

创建 `deploy.sh`：
```bash
#!/bin/bash

echo "开始构建网站..."
hugo

echo "进入 public 目录..."
cd public

echo "添加更改..."
git add .

echo "提交更改..."
msg="Rebuilding site $(date)"
if [ -n "$*" ]; then
    msg="$*"
fi
git commit -m "$msg"

echo "推送到 GitHub..."
git push origin main

echo "部署完成！"
cd ..
```

使用：
```bash
chmod +x deploy.sh
./deploy.sh "更新文章"
```

### 方法 3：使用 gh-pages 分支

```bash
# 安装 gh-pages（需要 Node.js）
npm install -g gh-pages

# 构建
hugo

# 部署
gh-pages -d public -b gh-pages
```

然后在 GitHub 仓库设置中，将 Pages 的 Source 设为 `gh-pages` 分支。

---

## 常见问题

### 1. 页面显示 404 / Page Not Found

**原因：**
- `content/` 目录为空
- 文章是草稿状态 (`draft = true`)
- 没有安装主题或没有布局模板

**解决：**
```bash
# 检查是否有内容
ls content/

# 使用 -D 参数查看草稿
hugo server -D

# 检查是否有主题或布局
ls themes/
ls layouts/
```

### 2. 主题不生效

**原因：**
- `hugo.toml` 中没有配置主题
- 主题目录名称不匹配

**解决：**
```toml
# 在 hugo.toml 中添加
theme = '主题目录名'
```

### 3. 中文内容显示乱码

**解决：**
```toml
languageCode = 'zh-CN'
hasCJKLanguage = true
```

### 4. 图片不显示

**位置：**
- 将图片放在 `static/images/` 目录
- 在 Markdown 中引用：`![描述](/images/pic.jpg)`

或使用页面资源：
```
content/
└── posts/
    └── my-post/
        ├── index.md
        └── image.jpg
```
引用：`![描述](image.jpg)`

### 5. 修改后页面没有更新

**解决：**
```bash
# 停止 hugo server
# 清理并重启
hugo server --cleanDestinationDir
```

### 6. Git Submodule 相关问题

**克隆项目后主题目录为空：**
```bash
git submodule update --init --recursive
```

**更新主题：**
```bash
git submodule update --remote --merge
```

### 7. 部署后 CSS/JS 404

**原因：** baseURL 配置不正确

**解决：**
```toml
# 确保 baseURL 正确
baseURL = 'https://yourusername.github.io/'

# 如果是项目页面（不是 username.github.io）
baseURL = 'https://yourusername.github.io/projectname/'
```

### 8. 构建速度慢

**优化：**
```toml
# 禁用不需要的功能
disableKinds = ['taxonomy', 'term']

# 减少图片处理
[imaging]
  resampleFilter = 'box'
  quality = 75
```

---

## 日常使用工作流

### 写新文章的标准流程

```bash
# 1. 创建文章
hugo new posts/my-new-post.md

# 2. 编辑文章
vim content/posts/my-new-post.md

# 3. 预览（包含草稿）
hugo server -D

# 4. 在浏览器中查看效果
# 访问 http://localhost:1313

# 5. 确认无误后，将 draft 改为 false

# 6. 停止服务器（Ctrl+C）

# 7. 如果使用 GitHub Actions
git add .
git commit -m "Add new post: my-new-post"
git push

# 8. 如果手动部署
hugo
cd public
git add .
git commit -m "Update site"
git push
```

### 管理草稿

```bash
# 查看所有草稿
hugo list drafts

# 查看所有内容
hugo list all

# 预览草稿
hugo server -D
```

---

## 高级技巧

### 1. 使用 Shortcodes

Hugo 内置了很多 shortcodes，可以在 Markdown 中使用：

```markdown
{{< youtube id="dQw4w9WgXcQ" >}}

{{< figure src="/images/pic.jpg" title="标题" >}}

{{< highlight go >}}
package main
import "fmt"
func main() {
    fmt.Println("Hello World")
}
{{< /highlight >}}
```

### 2. 自定义 Archetypes

编辑 `archetypes/default.md`：
```markdown
+++
title = '{{ replace .File.ContentBaseName "-" " " | title }}'
date = {{ .Date }}
draft = true
tags = []
categories = []
description = ''
+++

## 简介

正文内容...
```

### 3. 设置文章摘要

方法 1：自动摘要（前 70 个词）
```toml
[params]
  summaryLength = 70
```

方法 2：手动分隔
```markdown
这是摘要内容...

<!--more-->

这是正文内容...
```

### 4. 添加评论系统

使用 Disqus、Utterances 或 Giscus：

```toml
[params]
  # Disqus
  disqusShortname = 'your-disqus-shortname'

  # 或在模板中集成 utterances
```

### 5. 添加搜索功能

使用 Algolia、Lunr.js 或 Fuse.js（具体看主题支持）。

---

## 有用的资源

### 官方文档
- 官网：https://gohugo.io/
- 文档：https://gohugo.io/documentation/
- 主题库：https://themes.gohugo.io/

### 社区
- Hugo Discourse：https://discourse.gohugo.io/
- GitHub：https://github.com/gohugoio/hugo

### 工具
- Hugo 主题浏览：https://themes.gohugo.io/
- Markdown 编辑器：Typora, VS Code, Obsidian
- 图床：GitHub, 阿里云 OSS, 七牛云

---

## 快速参考

### 最常用的 5 个命令

```bash
hugo server -D              # 本地预览（含草稿）
hugo new posts/xxx.md       # 新建文章
hugo                        # 构建网站
git add . && git commit && git push  # 推送（如果用 Actions）
hugo version                # 查看版本
```

### Front Matter 模板

```toml
+++
title = ''
date = 2025-11-02T00:00:00+08:00
draft = false
tags = []
categories = []
description = ''
featured_image = ''
+++
```

---

## 总结

Hugo 的核心工作流：
1. `hugo new` - 创建内容
2. `hugo server -D` - 本地预览
3. 编辑 Markdown 文件，将 `draft = false`
4. `hugo` - 构建网站
5. 部署 `public/` 目录

**记住：**
- 草稿需要 `draft = false` 才会被构建
- 主题或布局模板是必需的
- `public/` 目录是最终的静态网站
- GitHub Actions 可以自动化整个部署流程

祝你使用愉快！🚀
