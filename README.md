# HPM 听书 → APK · 方法 A（PWABuilder · 浏览器里点几下出 APK）

## ⚠️ 修复历史

如果你已经按之前的 README 上传过文件，PWABuilder 报"Fix the links to your icons / Fix the icon types" 错误，**请用这一版重新上传**。

之前的 `index.html` 把 manifest 用 data: URL 内嵌进 HTML，导致 PWABuilder 读到内嵌的 manifest 而不是外部文件，所有图标 URL 也都是 data:。这一版的 `index.html` 用外部 `./manifest.webmanifest` 链接，图标都是独立的 PNG 文件。

---

## 文件清单（19 个文件）

把这 19 个文件全部传到 GitHub Pages 根目录：

```
hpm-pwa/
├── index.html                       ← 17.4 MB · 主程序
├── manifest.webmanifest             ← PWA 元信息（独立文件 · 关键）
├── sw.js                            ← Service Worker
├── icon-72.png                      ← 启动器图标 · 各尺寸
├── icon-96.png
├── icon-128.png
├── icon-144.png
├── icon-152.png
├── icon-192.png
├── icon-384.png
├── icon-512.png
├── icon-maskable-192.png            ← 自适应图标（Android 8+）
├── icon-maskable-512.png
├── screenshot-1.png                 ← 商店截图 · 书架
├── screenshot-2.png                 ← 商店截图 · 朗读界面
└── README.md
```

---

## 上传到 GitHub Pages

### 如果是第一次

1. https://github.com/new → 新建 public 仓库（建议名字 `hpm-tingshu`），不勾选任何初始文件
2. 进入仓库 → "uploading an existing file"
3. **把 `hpm-pwa/` 里的全部 16 个文件（不是文件夹）拖进上传框**
4. 滚到底 → Commit changes
5. Settings → Pages → Source: `main` / `/ (root)` → Save
6. 等 1-2 分钟，记下 URL：`https://<用户名>.github.io/hpm-tingshu/`

### 如果你已经上传过

1. 仓库主页 → 点已存在的 `index.html`、`manifest.webmanifest` 等文件 → 点笔图标编辑 → 删除内容 → 上传新文件
   或更直接：
2. 仓库主页 → **Add file → Upload files** → 把新的 16 个文件**全部覆盖**进去
3. 顺便把旧的 `icon-maskable-512.png`、`screenshot-*` 一起上传

确保新版的 `index.html` 顶部出现这一行：

```html
<link rel="manifest" href="./manifest.webmanifest">
```

而**不是** `href="data:application/manifest+json;base64,..."`

---

## PWABuilder 出 APK

1. https://www.pwabuilder.com/
2. 输入 `https://<用户名>.github.io/hpm-tingshu/` → Start
3. 应该看到几乎全绿评分（约 40+/45 分）
4. 右上 **Package For Stores** → Android → **Generate Package**
5. 弹窗选 **Signing key: Generate** → Download
6. 解压 zip → `app-release-signed.apk` 拷到手机点击安装

---

## 常见错误

| 错误 | 原因 | 修复 |
|---|---|---|
| Fix the links to your icons | manifest 用了 data: URL | 用本版 README 的新 `index.html` |
| Fix the icon types | 同上 | 同上 |
| Add a service worker | sw.js 没传 / 路径错 | 确认 `sw.js` 在仓库根目录 |
| Add screenshots | 没截图 | 本版已附 `screenshot-1.png`、`screenshot-2.png` |
| Fix the icon sizes | 图标尺寸不够全 | 本版已生成 72/96/128/144/152/192/384/512 全套 |
