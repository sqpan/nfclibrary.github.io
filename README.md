# NFC Library — 用于托管 PDF 并通过 NFC 直达

说明（中文）：

## 项目目的
将 PDF 文件放在 `pdfs/` 目录，通过静态站点公开访问。同时提供一个简单的 NFC 跳转页面（`/nfc/?id=slug`）。写入 NFC 标签时写入该 URL（含 id 参数），手机触碰后会自动跳转到对应的 PDF。

## 部署（GitHub Pages）
- 如果你希望站点在根域名（例如 `https://<your-username>.github.io/`）下访问，请把仓库重命名为 `<your-username>.github.io`。
- 当前仓库名若为 `nfclibrary.github.io`，则默认的访问地址为：
  `https://<your-username>.github.io/nfclibrary.github.io/`
- 你也可以在仓库设置中启用 GitHub Pages，并使用自定义域（CNAME）。

## 目录结构（示例）
- index.html — 主页，列出文档
- css/styles.css — 样式
- pdfs/ — 上传 PDF 的目录（把你的 .pdf 放到这里）
- data/files.json — 文档元数据，用来生成列表和 NFC 映射
- nfc/index.html — NFC 跳转页，接受 `?id=slug` 并重定向到对应 PDF

## 添加新 PDF 的步骤
1. 把 PDF 文件上传到 `pdfs/`，例如 `pdfs/my-doc.pdf`。
2. 在 `data/files.json` 中新增一条记录，例如：
   ```json
   {
     "slug": "my-doc",
     "title": "我的文档",
     "filename": "my-doc.pdf",
     "description": "简短说明",
     "created_at": "2026-01-26"
   }
   ```
3. 提交并推送到 GitHub。刷新主页即可看到新文档，NFC URL 会是：
   ```
   https://<your-site-base>/nfc/?id=my-doc
   ```

## 如何写 NFC 标签
- 推荐使用手机上的 NFC 写入工具，例如「NFC Tools」（Android / iOS）或 NXP TagWriter。
- 写入一个 URL 记录，内容填：
  `https://<your-site-base>/nfc/?id=<slug>`
  例如 `https://sqpan.github.io/nfclibrary.github.io/nfc/?id=example-doc`
- 当有人用手机触碰此 NFC 标签（并支持 URL 打开），手机会打开该 URL，然后自动重定向到对应 PDF。

注意：
- 若希望 NFC 链接在站点根域名（如 `https://sqpan.github.io/`）下工作，请将仓库改名为 `sqpan.github.io` 或在写入 NFC 标签时使用你实际的站点根 URL。
- 如果你想避免在每次添加文档后手动维护多页面，当前实现只需维护 `data/files.json`；NFC 跳转页会根据 `id` 在此 JSON 中查找文件并跳转。

## 安全与访问控制
- 该站点是公开的：只要知道 URL 的人都可以访问 PDF。若需要访问控制，需要使用有认证保护的托管服务（例如将文件放在私人云、或者使用带权限的存储/后端）。
- 若 PDF 包含敏感信息，请不要公开托管在公共 GitHub Pages 上。

## 进一步改进建议
- 自动化脚本：提供一个小脚本（例如 GitHub Action）在你把 PDF 推送到某目录时，自动更新 data/files.json（或通过 PR 自动添加条目）。
- 短链服务：若想用更短的 URL，可在站外搭配短链服务或自定义域名。
- 统计与访问日志：如果需要统计谁通过 NFC 打开了哪些 PDF，可以在跳转页面前接入统计（例如 Google Analytics / Matomo），或使用中转服务记录后再重定向。

如果你需要，我可以帮助创建 Pull Request 将这个分支合并到 main（我无法直接打开 PR via this tool, 但可以为你生成 PR 标题和描述或给出创建 PR 的链接）。
