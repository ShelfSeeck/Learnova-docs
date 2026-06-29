[<img src="static/LEARNOVA.svg" width="400" alt="LEARNOVA Logo" />](https://ta.njunova.com/docs/)


社区地址: [Learnova](https://ta.njunova.com/) 
线上文档地址: [Learnova 文档站](https://ta.njunova.com/docs/)

这是 Learnova 站点的文档库，基于 Hugo 及其 hugo-book 主题构建。

> ——Learnova是一个面向呢喃的Skill社区和简单的Agent学习平台。

---

## 快速上手与协作流

本地编写前需安装 **Hugo Extended (扩展版)**。在 Windows 上使用 `winget install Hugo.Hugo.Extended`，在 macOS 上使用 `brew install hugo` 即可。若使用 Linux，请确保通过二进制或包管理器获取带 `extended` 后缀的版本（可通过 `hugo version` 确认输出中含 `+extended`）。

对按下方文档管理细节进行文档编辑之后，
在项目根目录下运行 `hugo server -D`，即可通过 `http://localhost:1313/docs/` 本地实时预览修改，保存文件时浏览器会自动刷新。

为维护主分支稳定，建议 fork 本仓库后进行修改。本仓库已配置 GitHub Action 自动化部署流，当修改被合并或 push 到主分支 (`main`) 时，云端会自动编译并完成服务器同步，无需在本地手动执行构建和 `scp`/`rsync` 上传。

---

## 核心设计与文档管理细节



左侧的树状菜单是根据 `content/docs/` 目录下的文件夹和文件结构**自动生成**的。当组员想要调整或新增内容时，根据不同的开发目的进行如下操作：

#### 目的一：我想在侧边栏新建一个“分类”（例如新建一个包含多篇文章的“开发指南”章节）
1. **新建文件夹**：在 `content/docs/` 下新建一个文件夹，如 `dev-guide`。
2. **创建分类首页**：在 `dev-guide` 文件夹下新建一个文件，**必须命名为 `_index.md`**（注意带下划线）。
3. **效果**：在 `_index.md` 头部的 `title` 里写什么，左侧菜单栏就会显示什么分类名称，它将作为该分类的入口和引导页，其下可以以相同的方式继续容纳子菜单或子页面。
（最终的子文档请命名为index.md，且不要带下划线，否则会被当作分类入口而非文章。）
#### 目的二：我想在某个分类下，新增一篇“具体的文档/文章”

强烈建议统一运行 `hugo new content docs/分类目录/文件名.md` (或 `/文件夹名/index.md`) 命令来初始化新建文档，然后再手动打开文件进行编辑修改。

- **如果文章只有文字，没有任何本地图片等附件**：直接新建或使用命令生成一个普通的 Markdown 文件（例如 `quickstart.md`）。
- **如果文章包含图片或 PDF 等专属附件**：建议新建以文章名命名的文件夹并在其下创建 `index.md`（无下划线），把图片等附件拖入同级目录下，并在 Markdown 中直接以相对路径引用（例如 `![描述](image.png)`）。这样移动文章时附件不会失效，且本地编辑器预览更友好。若有全局公用资源才放入 `static/assets/` 并以 `/docs/assets/文件名` 引用。

#### 目的三：我想在一篇文章中插入图片、PDF 等专属附件

1. **新建文章文件夹**：如果文章之前只是一个单独的 `.md` 文件（例如 `my-article.md`），请先建立一个与文章同名的文件夹（名为 `my-article`），接着把文章重命名为 **`index.md`**（注意没有下划线），并移入该文件夹中。
2. **放入附件**：把需要插入的图片或 PDF 文件拖进这个新建的文件夹里，让它们与 `index.md` 处于同一个文件夹下。
3. **在正文中直接引用**：在 `index.md` 中，直接写文件名即可插入图片。例如：
   ```markdown
   ![图片描述](screenshot.png)
   ```
   *这种做法的好处是，文章和图片打包在一个文件夹内。即使以后重命名或移动整个文件夹，图片链接也不会失效，且在 Obsidian 中可以直接预览。*

*注：如果该文件夹是一个包含 `_index.md`（带下划线）的分类目录，它除了可以继续嵌套子文件夹或子文章外，同样也支持这种附件存放方式。你可以直接把图片放进该目录下，并在 `_index.md` 中用相对路径直接引用。*

#### 菜单属性配置 (Front Matter)

无论是 `_index.md` 还是独立的 `.md` 文件，在文件最顶部都需要包含一段元数据（Front Matter）来配置它在菜单里的表现：
```markdown
---
title: "在左侧菜单中显示的名字"
weight: 10                  # 控制排序，数值越小越靠前
bookCollapseSection: true   # (仅对分支 _index.md 生效) 设为 true 时该分类在左侧默认折叠
draft: false                # 必须为 false 才能正常渲染发布
---
```

