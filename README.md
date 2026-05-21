# HPM 听书 → APK · 方法 A（PWABuilder · 浏览器里点几下出 APK）

不用 Android Studio、不用 Node、不用 JDK，全程在浏览器里完成。

---

## 步骤总览（约 10 分钟）

1. 把这个 `hpm-pwa/` 文件夹推到 GitHub
2. 开启 GitHub Pages，拿到公开 HTTPS 网址
3. 打开 PWABuilder，输入网址 → 下载 APK
4. 装到手机

---

## ① 推到 GitHub

如果不熟 git，最简单的方法：

**方式 1 · 网页拖拽（推荐新手）**

1. 去 https://github.com/new 新建仓库，名字随便（比如 `hpm-tingshu`），选 **Public**，**不要** 勾选 "Add a README"
2. 创建后页面会出现 "uploading an existing file" 链接，点进去
3. 把这个 `hpm-pwa/` 文件夹里**所有文件**（`index.html`, `manifest.webmanifest`, `sw.js`, 三张 png, `README.md`）拖到上传框
4. 滚到底，点绿色按钮 **Commit changes**

**方式 2 · 命令行（熟 git 的话）**

```bash
cd hpm-pwa
git init -b main
git add .
git commit -m "HPM 听书 PWA v0.7"
git remote add origin https://github.com/<你的用户名>/hpm-tingshu.git
git push -u origin main
```

---

## ② 开启 GitHub Pages

1. 仓库页面 → **Settings**（顶部菜单）
2. 左侧菜单 → **Pages**
3. **Source** 选 **Deploy from a branch**
4. **Branch** 选 **main** · folder 选 **/ (root)** · 点 **Save**
5. 等 1-2 分钟，刷新页面，顶部会出现绿色横幅：

   > Your site is live at **https://<用户名>.github.io/hpm-tingshu/**

   复制这个网址。

**验证一下**：浏览器打开这个网址，HPM 应该正常显示并可以朗读。

---

## ③ 用 PWABuilder 出 APK

1. 打开 https://www.pwabuilder.com/
2. 输入你刚才的 GitHub Pages 网址 → 点 **Start**
3. 它会给 PWA 评分（**HPM 听书已经按 PWA 规范配好了 manifest + service worker + icons，分数应该很高**）
4. 如果有红色 issues，按提示点 "Fix" 即可；这个项目通常一切都是绿的
5. 点页面右上角 **Package For Stores**
6. 选 **Android** 那个卡片，点 **Generate Package**
7. **Package options** 弹窗：
   - **Package ID**: `com.hpm.tingshu`（已自动填好）
   - **App name**: `HPM 听书`
   - **Launcher name**: `HPM`
   - **App version**: `1.0.0`
   - **Signing key**: 选 **Use mine** 或 **Generate** 都行；**Generate** 最省事
   - 其他保持默认
8. 点 **Download**，会得到一个 zip：`hpm-tingshu.zip`
9. 解压，里面的 **`app-release-signed.apk`** 就是可以装手机的 APK

> 同包里的 **`app-release-bundle.aab`** 是给 Google Play 上架用的；个人用 APK 即可。

---

## ④ 装到手机

**Android 手机：**

- 把 APK 通过微信/邮件/USB 传到手机
- 点击 APK 文件 → 系统会提示"允许安装未知来源" → 同意 → 安装
- 完成后桌面会出现 HPM 金色字标图标

**或者用 adb（电脑接 USB 数据线）：**

```bash
adb install -r app-release-signed.apk
```

---

## 重要说明 · PWABuilder 出的 APK 的本质

PWABuilder 出的是 **TWA (Trusted Web Activity)** APK：

- ✅ 桌面图标、独立窗口、看起来跟原生 App 一样
- ✅ 离线可用（Service Worker 缓存）
- ✅ 全部数据存本机 IndexedDB / localStorage，APK 重装也在
- ⚠️ 后台朗读：屏幕亮时 OK，息屏后 Android 会冻结 WebView，朗读会停（这是所有 PWA-to-APK 工具的共通限制）

如果要真正后台朗读，需要原生 TTS 桥接 —— 那就只能走 Android Studio + 上一个包的 `BACKGROUND.md` 方案。

---

## 后续更新 App

改完 `ui_kits/hpm-android/` 里的代码后，告诉我，我重新打包 `hpm-pwa/index.html`，你只要：

1. 把新文件覆盖到 GitHub 仓库
2. 等 1 分钟 GitHub Pages 自动更新
3. 已安装的 PWA APK **下次启动时会自动拉到新版本**（不用重装 APK）

这就是 PWA 比传统 APK 强的地方 —— 改了不用重新发版。

---

## 文件清单

| 文件 | 作用 |
|---|---|
| `index.html` | 完整 App 单文件（17.4 MB，含全部代码、图标、音色配置） |
| `manifest.webmanifest` | PWA 元信息（PWABuilder 读这个） |
| `sw.js` | Service Worker（离线缓存，PWABuilder 必需） |
| `icon-192.png` / `icon-512.png` | App 启动器图标 |
| `icon-maskable-512.png` | Android 8+ 自适应图标 |
| `README.md` | 你正在看的这个 |
