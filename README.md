# jiakuo的 Hugo 网站结构说明

这个仓库是使用 [Hugo](https://gohugo.io/) 搭建的静态博客网站，采用了 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题
本说明文件的目的并非介绍博客内容，而是帮助**未来的我**快速了解仓库结构

---

## 📁 仓库目录结构与说明

```plaintext
.
├── .github/workflows/     # ✅ GitHub Actions 自动部署脚本（一般不需改动）
├── archetypes/            # ✅ 新建文章时的默认模板，可根据需求调整
├── assets/
│   └── css/               # ✅ 可写自定义样式，例如 override.css
├── content/
│   ├── posts/             # ✅ 博文存放目录，每篇文章一个 Markdown 文件
│   └── drafts/            # ⚠️ 可选：写到一半未完成的草稿
├── static/
│   └── images/            # ✅ 推荐添加：用来放置插图、封面图等资源（可通过 /images/... 引用）
├── themes/                # ✅ Hugo 主题目录，使用 Git submodule 引入PaperMod（不要直接修改）
├── .gitignore
├── .gitmodules            # ✅ 子模块定义文件（由 git 管理）
└── hugo.yaml              # ✅ Hugo 配置主文件（网站标题、菜单、布局等）
```

### ✅ 常编辑区

| 目录/文件          | 用途与说明 |
|-------------------|------------|
| `content/`        | **所有的文章都放这里**，通常是 Markdown 格式。可以按分类建子文件夹（如 posts/、finance/、tech/）|
| `assets/css/`     | 放自定义的 CSS 样式文件，建议只编辑 `override.css`，这样主题更新不会影响自己的样式 |
| `static/`         | 放一些静态资源，如 favicon.ico、社交卡片图、robots.txt、自定义图标等。这里的文件会原样拷贝到最终站点 |
| `hugo.yaml`       | 网站配置文件（语言/标题/主题/菜单/SEO），每次改主题结构、添加菜单、设置部署路径时都可能需要编辑 |
| `archetypes/`     | 新文章时自动插入的模板 Front Matter（即文章头部信息）配置，可以自定义字段 |

---

### 🚫 基本不要动的区域

| 目录/文件             | 用途与说明 |
|----------------------|------------|
| `themes/PaperMod/`   | Hugo 的主题目录。通过 Git Submodule 引入的主题，因此*不要手动修改其内容**，否则更新时容易冲突 |
| `.github/workflows/` | GitHub Actions 的配置目录，主要包含 `deploy.yml`，用于将网站自动部署到 GitHub Pages |
| `.gitignore`         | 忽略某些不应上传的文件夹（如 public、资源缓存等）。除非有特殊目的，不建议频繁修改 |
| `public/`            | Hugo 构建出来的最终静态网页文件夹，已经被 `.gitignore` 忽略。**不要提交它的内容** |
| `.hugo_build.lock`   | Hugo 内部使用的锁文件，也在 `.gitignore` 中忽略 |

---

## 🧪 常用开发 & 写作流程

### ✏️ 写文章

可以用命令新建文章：

```bash
hugo new posts/xxx.md
```

也可以手动创建 `.md` 文件，放在 `content/posts/` 或其他自建子文件夹中，注意填写 front matter（标题、日期、草稿状态等）

### 🔍 本地预览

```bash
hugo server -D
```

然后浏览器访问： [http://localhost:1313/](http://localhost:1313/)

### 🚀 发布流程（自动化）

1. 确保 `.github/workflows/deploy.yml` 存在（已经配置好）
2. 每次写完文章执行：

   ```bash
   git add .
   git commit -m "新增文章/更新内容"
   git push
   ```

3. GitHub Actions 会自动运行，约 1 分钟后自动部署到网站

---

