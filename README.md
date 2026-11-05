# sec-headers

本地粘贴 HTTP 响应头，立刻看到安全相关头是否齐全、缺什么、风险提示。**纯前端，不上传。**

在线试用：<https://temptation2017.github.io/sec-headers/>

## 做什么

- 粘贴完整 HTTP 响应头（或 `Header: value` 行）
- 对照常见安全头清单：有/无、原始值摘要、中文说明、缺失风险提示
- 标出可能泄露技术栈的头（`Server`、`X-Powered-By` 等）为「信息暴露」提示
- 一键复制缺失项为 Markdown 待办

**不是漏洞扫描器**，不会发请求、不评分、不声称「已加固」。

## 怎么打开

- 打开 GitHub Pages：<https://temptation2017.github.io/sec-headers/>
- 或本地下载后双击 `index.html`，或：

```bash
python3 -m http.server 8080
# 浏览器访问 http://127.0.0.1:8080/
```

无需构建、无需 npm。

## 怎么用

1. 页面已预填一份**虚构**示例响应头（含部分安全头，故意缺几项，并带 `Server` / `X-Powered-By` 等）。
2. 点「检查」，或直接改文本框内容（会自动刷新）。
3. 查看「安全头清单」与「信息暴露提示」。
4. 点「复制缺失清单」，粘贴到 issue / PR / 备忘录。

从浏览器开发者工具复制响应头：Network → 选中请求 → Headers → Response Headers，复制后粘贴即可（可带或不带状态行）。

## 检查范围

| 头 | 说明要点 |
| --- | --- |
| Strict-Transport-Security | 强制 HTTPS |
| Content-Security-Policy | 限制资源来源，缓解 XSS |
| X-Content-Type-Options | 禁止 MIME 嗅探 |
| X-Frame-Options | 防点击劫持（CSP `frame-ancestors` 可等效） |
| Referrer-Policy | 控制 Referer 泄露 |
| Permissions-Policy | 限制设备/浏览器能力（兼容旧名 Feature-Policy） |
| Cross-Origin-Opener-Policy | 浏览上下文隔离 |
| Cross-Origin-Resource-Policy | 限制跨源加载 |
| Cross-Origin-Embedder-Policy | 跨源隔离前提之一 |

信息暴露提示（若出现）：`Server`、`X-Powered-By`、`X-AspNet-Version`、`X-Generator`、部分 Drupal/Shopify 指纹头等。

## 示例

虚构响应头片段：

```http
HTTP/1.1 200 OK
Server: nginx/1.24.0
X-Powered-By: Express
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self'
```

预期：HSTS / CSP / nosniff 为「已设置」；Permissions-Policy、COOP/CORP/COEP、Referrer-Policy、X-Frame-Options 等为「缺失」；`Server` 与 `X-Powered-By` 出现在信息暴露区。

## 限制与可改进点

- 只做**有无**与简短说明，不验证策略语法是否正确、不模拟浏览器生效。
- 不解析多策略组合冲突，不读取网络；无法代替专业扫描（如 Mozilla Observatory）。
- 可改进：CSP 指令摘要、HSTS 预加载条件提示、Cookie 安全属性（Secure/HttpOnly/SameSite）清单、导入 HAR 中的响应头。

## License

MIT
