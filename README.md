# Listen Landing

这是声音明信片的零后端公网落地页。

## MVP 部署方式

不需要先申请域名。任选一个静态托管服务，把本目录作为静态站点发布即可：

- Cloudflare Pages
- Vercel
- GitHub Pages

发布后会得到类似 `https://cp17-radio.pages.dev/` 的 HTTPS 地址。Flutter 打包时传入：

```powershell
flutter build apk --debug --dart-define=CP17_LISTEN_BASE_URL=https://cp17-radio.pages.dev/
```

如果未来将页面部署到 `/listen/` 路径，则传入：

```powershell
flutter build apk --debug --dart-define=CP17_LISTEN_BASE_URL=https://example.com/listen/
```

## 正式域名

正式域名不是 MVP 前置条件。建议等分享玩法、文案和可播放率验证稳定后再购买，例如：

- `cp17.radio`
- `listen.cp17.app`
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
- `now`：当前曲目或直播状态
- `mode`：`live` / `antipode`
- `story`：分享故事文案
- `lat` / `lng`：地图坐标

注意：微信、WhatsApp 等社交 App 通常不会在聊天卡片里直接播放音频；用户点击链接进入网页后播放，这是平台限制。
