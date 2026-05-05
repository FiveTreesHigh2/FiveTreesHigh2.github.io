# 个人主页

基于 Hugo 的个人主页 + Blog，部署在 GitHub Pages。

## 🚀 第一次部署（一次性配置）

### 1. 在 GitHub 上创建仓库

仓库名建议命名为 **`YOUR_USERNAME.github.io`**（把 `YOUR_USERNAME` 换成你自己的 GitHub 用户名），这样网址就会是 `https://YOUR_USERNAME.github.io`。

> 你也可以用任意仓库名，比如 `blog`，那网址就是 `https://YOUR_USERNAME.github.io/blog/`。

### 2. 修改 `hugo.toml` 里的个人信息

打开根目录的 `hugo.toml`，把这几个地方改成你自己的：

```toml
baseURL = "https://YOUR_USERNAME.github.io/"   # 改成你的 GitHub Pages 网址
title   = "Your Name"                          # 浏览器标签上显示的站点名

[params]
  author = "Your Name"
  bio    = "在这里写一段简短的自我介绍..."
  email  = "you@example.com"
  github = "https://github.com/YOUR_USERNAME"
```

### 3. 推到 GitHub

```bash
git init
git add .
git commit -m "initial site"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_USERNAME.github.io.git
git push -u origin main
```

### 4. 启用 GitHub Pages（关键一步）

在 GitHub 仓库页面：

1. 点 **Settings** → 左侧菜单 **Pages**
2. **Source** 选择 **GitHub Actions**（不是默认的 "Deploy from a branch"）
3. 保存

然后切回仓库的 **Actions** 标签页，等几秒会看到部署任务在跑。跑完之后访问你的网址就能看到了。

---

## ✍️ 日常写新文章

这正是你想要的工作流：**写 Markdown，push，自动上线。**

### 方法一：命令行

```bash
hugo new posts/my-new-post.md
```

会在 `content/posts/` 下生成一个带模板的 `.md` 文件。

### 方法二：直接新建文件

在 `content/posts/` 目录下新建一个 `.md` 文件即可，开头加上这段 frontmatter：

```markdown
+++
title = '文章标题'
date = 2026-05-05T10:00:00+08:00
draft = false
tags = ["标签1", "标签2"]
summary = "一句话摘要，会显示在文章详情页顶部"
+++

正文从这里开始，正常写 Markdown 就行。
```

> ⚠️ **`draft = false` 才会发布**。如果你写了一半还没写完，留 `true`，它就不会出现在线上。

### 发布

```bash
git add .
git commit -m "new post: 我的新文章"
git push
```

push 之后 GitHub Actions 自动构建 + 部署，约 30 秒后线上就更新了。

---

## 🖥️ 本地预览

写文章的时候肯定想边写边看效果。先装 Hugo：

- **macOS**: `brew install hugo`
- **Windows**: `choco install hugo-extended` 或从 [github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases) 下载
- **Linux**: 见上面 release 页面

然后在项目根目录跑：

```bash
hugo server -D
```

打开 `http://localhost:1313` 就能看到。`-D` 是 "包含草稿" 的意思，方便你预览还没发布的文章。改 `.md` 文件保存后浏览器会自动刷新。

---

## 📁 项目结构

```
.
├── hugo.toml                 # 站点配置（个人信息在这里改）
├── content/
│   ├── about.md              # "关于" 页面
│   └── posts/                # 👈 所有博客文章放这里
│       ├── welcome.md
│       └── slow-down.md
├── layouts/                  # 页面模板（不太需要动）
│   ├── _default/
│   │   ├── baseof.html       # 所有页面的外壳
│   │   ├── list.html         # 列表页（/posts/ 长这样）
│   │   └── single.html       # 单篇文章页
│   ├── partials/
│   │   ├── header.html       # 顶部导航
│   │   └── footer.html       # 页脚
│   └── index.html            # 首页
├── assets/css/main.css       # 👈 想改样式来这里
├── archetypes/default.md     # `hugo new` 时的模板
└── .github/workflows/
    └── deploy.yml            # 自动部署配置
```

---

## 🎨 想自定义外观？

- **改颜色 / 字体** → 编辑 `assets/css/main.css` 顶部的 CSS 变量（`:root` 那块）
- **改导航菜单** → 编辑 `hugo.toml` 底部的 `[[menu.main]]` 区块
- **首页文案** → 编辑 `hugo.toml` 里的 `bio`，或改 `layouts/index.html`

---

## 💡 常见问题

**Q: 我 push 了但网站没更新？**
去仓库 Actions 页面看一下任务是不是失败了。最常见的原因是 `hugo.toml` 语法错误。

**Q: 文章写好了但没显示？**
检查 frontmatter 里 `draft` 是不是 `false`，以及 `date` 是不是早于当前时间。

**Q: 想换主题怎么办？**
当前用的是自己写的 layouts，如果想换 Hugo 社区主题，可以去 [themes.gohugo.io](https://themes.gohugo.io/) 挑一个，然后按它的 README 配置。
