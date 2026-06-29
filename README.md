# Learnova-docs

Learnova 站点的文档库，基于 Hugo 与 `hugo-book` 主题构建。


### 1. 环境准备
本地撰写文档需要安装 **Hugo Extended（扩展版）**。
- **Windows**: `winget install Hugo.Hugo.Extended`
- **macOS**: `brew install hugo`
- **Linux**: 安装包管理器提供的 `hugo`，或从 [Hugo Releases](https://github.com/gohugoio/hugo/releases) 下载带 `extended` 后缀的二进制。

*验证命令：* 运行 `hugo version`，并确保输出中包含 `+extended`。

### 2. 新增文档与层级结构
所有的文档均存放在 `content/docs/` 目录下。
- **新建页面**：运行 `hugo new docs/文件名.md`，或直接在目录下新建 Markdown 文件。
- **排序与配置**：在页面头部的 Front Matter 中指定属性：
  ```markdown
  ---
  title: "文章标题"
  weight: 10          # 越小越靠前，用于控制左侧树状菜单排序
  draft: false        # 草稿状态改为 false 后才能正常发布
  ---
  ```
- **附件管理**：
  - **页面专属附件**（推荐）：将页面命名为 `index.md`（无下划线），把图片等附件放在同级目录下，在文档中直接相对路径引用：`![描述](img.png)`。
  - **全局静态文件**：存放在 `static/assets/` 路径下，通过 `/assets/文件名` 绝对路径引用。

### 3. 本地测试与预览
在项目根目录下运行以下命令启动本地服务：
```bash
hugo server -D
```
访问 `http://localhost:1313/` 即可实时预览修改，保存文件时浏览器会自动刷新。
不知道为啥，反正最好先build一下文件才会显示最新


### 4. 构建与服务器部署
当文档撰写完成，需要将网站编译并部署至服务器。

#### 第一步：本地构建（生产环境打包）
在项目根目录下运行以下命令进行构建：
```bash
hugo --minify --gc --cleanDestinationDir
```
* **说明**：此命令会将整站静态化，进行极致压缩并清理无用缓存。编译完成后，会在项目根目录下生成 **`public/`** 目录（此目录为编译好的全部静态网页文件）。

#### 第二步：上传部署至服务器
使用 `scp` 或 `rsync` 命令行工具将生成的 `public/` 目录下的所有文件上传至您的 Web 服务器指定根目录下。

* **使用 scp 传输**（适合简单上传）：
  ```bash
  scp -r public/* 用户名@服务器IP:/var/www/html/
  ```
* **使用 rsync 传输**（推荐，增量同步且自动清理服务器冗余文件）：
  ```bash
  rsync -avz --delete public/ 用户名@服务器IP:/var/www/html/
  ```
  *(请将 `/var/www/html/` 替换为您服务器上 Nginx/Apache 配置的实际网页根目录)*
