# Void Radio Listen Landing

这是 Void Radio 声音明信片的零后端公网落地页。

## MVP 部署方式

不需要先申请域名。任选一个静态托管服务，把本目录作为静态站点发布即可：

- Cloudflare Pages
- Vercel
- GitHub Pages

发布后会得到类似 `https://listen.feedar.cc/` 的 HTTPS 地址。Flutter 打包时传入：

```powershell
flutter build apk --debug --dart-define=CP17_LISTEN_BASE_URL=https://listen.feedar.cc/
```

如果未来将页面部署到 `/listen/` 路径，则传入：

```powershell
flutter build apk --debug --dart-define=CP17_LISTEN_BASE_URL=https://example.com/listen/
```

## 正式域名

正式域名不是 MVP 前置条件。建议等分享玩法、文案和可播放率验证稳定后再购买，例如：

- `void.radio`
- `listen.void.radio`
- `radio.yourdomain.com`

如果复用现有 `feedar.cc`，建议优先使用子域名，不直接切换主域名根路径，避免影响当前交易系统：

- `listen.feedar.cc`：声音明信片和可播放落地页
- `radio.feedar.cc`：后续官网、下载页或 App Links 入口

DNS 层只需要给子域名加 CNAME 到 Cloudflare Pages / Vercel / GitHub Pages 提供的目标地址。交易系统继续保留在当前主域名或原有子域名上。

绑定正式域名后，再补 Android App Links / iOS Universal Links，让已安装用户点击分享链接时优先回到 App，未安装用户留在网页播放。

## 链接参数

落地页通过 URL 参数渲染内容：

- `station`：电台名
- `place`：地区
- `freq`：频率或 NET
- `stream`：可播放直播流地址
- `fallback`：备用直播流地址，多个 URL 用 `|` 分隔
- `now`：当前曲目或直播状态
- `mode`：`live` / `antipode`
- `story`：分享故事文案
- `lat` / `lng`：地图坐标
- `app`：可选 App 下载 / TestFlight / APK 分发链接；未传时不显示“获取 Void Radio”按钮

注意：微信、WhatsApp 等社交 App 通常不会在聊天卡片里直接播放音频；用户点击链接进入网页后播放，这是平台限制。移动端浏览器也通常禁止自动播放音频，所以页面必须通过“立即收听”按钮显式调用 `audio.play()`。如果主流播放失败，页面会在同一次用户播放意图下依次尝试 `fallback` 备用流。

## 发布审核辅助页面

- `privacy.html`：隐私政策，说明零账号、零后端、定位用途、第三方电台目录和分享链接参数。
- `support.html`：支持与反馈，供用户报告坏源、缺少电台、定位或播放问题。
- `legal.html`：数据来源与版权说明，供 Google Play 审核、权利人更正/移除请求和商店描述引用。

App 内设置页的“隐私政策”和“支持与反馈”入口指向这两个公网页面。当前联系邮箱写作 `support@feedar.cc`，正式上架前需要确认该邮箱可正常收信。
