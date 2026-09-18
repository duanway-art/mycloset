# 电子衣橱官网

多语言产品介绍页，部署到 `https://mycloset.nonoor.com/`。

## 结构

```
website/
  index.html          # 单页官网
  privacy/index.html  # 隐私政策
  terms/index.html    # 用户协议
  css/style.css
  css/legal.css
  js/i18n.js          # 5 语言文案
  js/main.js          # 语言切换 + 截图路径
  js/legal-content.js # 法律文档正文（由脚本生成）
  js/legal-page.js    # 法律页渲染与语言切换
  assets/
    app-icon.png
    favicon.png
    screenshots/      # 各语言营销截图（宽 780px）
      简体中文/
      繁体中文/
      英语/
      日语/
      韩语/
      法语/
      德语/
      泰语/
      意大利语/
      西班牙语/
```

## 本地预览

```bash
cd website
python3 -m http.server 8080
```

浏览器打开 http://localhost:8080 ，可用 `?lang=en` 切换语言。

## 更新截图

```bash
./scripts/prepare_website_assets.sh
```

会从 `AppStore/Screenshots/1242x2688/` 生成网页用截图。

## 更新法律文档

App 内法律文案变更后，重新生成网页内容：

```bash
python3 scripts/generate_legal_web_content.py
```

内容来自 `DianZiYiChu/Legal/LegalDocuments.swift` 与 `LegalDocumentLocalizedProvider.swift`，与 App 内展示保持一致。

## 部署到 nonoor.com

将 `website/` 目录全部上传到服务器根目录（与 `/privacy`、`/terms` 同级）。

示例（rsync）：

```bash
rsync -avz --delete website/ user@your-server:/var/www/mycloset.nonoor.com/
```

## 待填项

App 上架后，在 `website/js/main.js` 里把 `DEFAULT_APP_STORE_URL` 改成真实 App Store 链接。

## 支持语言

| 按钮 | 语言 | 截图目录 |
|------|------|----------|
| 简 | 简体中文 | 简体中文 |
| 繁 | 繁体中文 | 繁体中文 |
| EN | English | 英语 |
| JA | 日本語 | 日语 |
| KO | 한국어 | 韩语 |
| FR | Français | 法语 |
| DE | Deutsch | 德语 |
| TH | ไทย | 泰语 |
| IT | Italiano | 意大利语 |
| ES | Español | 西班牙语 |

语言优先级：URL `?lang=` → localStorage → 浏览器语言 → 简体中文。
