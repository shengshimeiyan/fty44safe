# fty44safe — Android 4.4 老盒子专用点播接口

专为 **Android 4.4 (Dalvik)** 老电视盒子整理适配的 FongMi/TVBox 系点播接口配置。
实测环境：中国移动 E900-S（Hi3798MV100 / Android 4.4）+ OK影视 X5 离线版 2.5.0。

## 📦 直接下载 APK（推荐）

X5 2.5 精修版（内置 44safe-v3 缓存配置，**开箱即用，无需联网订阅**）：

👉 **[下载 ok250x5.apk (Release v2.5)](https://github.com/shengshimeiyan/fty44safe/releases/latest)**

```text
sha256: 1475b26165475e6cf5e88106bf60349082f9b27090f5a756c709305b3dd310f8
```

- 单 dex，兼容 Android 4.4（okys289 是多 dex，4.4 装不上）
- 已缓存本仓库 `config.json`（44safe-v3）→ 装完即用，断网也能看

## 为什么老盒子需要专用接口

主流接口（如饭太硬）的 spider jar 内含 native `.so`，在 4.4 的 Dalvik 上加载即
SIGSEGV（闪退根因，logcat 实测确认）。本配置的做法：

- `"spider": ""` —— 不加载任何爬虫 jar
- 全部站点使用 `type: 1` 标准 XML 采集接口（纯 HTTP，无任何本地代码执行）
- 不依赖 `.so` / 多 dex / 新框架特性

## 内容

| 类型 | 数量 | 说明 |
| --- | --- | --- |
| 点播站 | 11 | 华华云 / 360 / 量子 / 红牛 / 暴风 / 光速 / 非凡 / 魔都 / 速博 / 金鹰 / 电影天堂 |
| 直播源 | 5 | Kimentanm / 综合直播 / 虎牙一起看 / 斗鱼一起看 / YY轮播 |
| 广告净化规则 | 11 组 | 针对各站 m3u8 切片广告：`#EXT-X-DISCONTINUITY` 投片检测 + 时长指纹 |

## 使用

### 方式 A：用 APK 内置缓存（4.4 老盒子首选）

安装 Release 里的 `ok250x5.apk` → 打开即已配置好，无需任何订阅操作。

### 方式 B：在线订阅

X5 → 设置 → 配置 → 输入订阅地址。

**4.4 老盒子用这个**（Cloudflare 边缘代理，默认允许 TLS1.0，
已在 E900-S 盒子内端到端实测成功）：

```text
https://gh.927223.xyz/https://raw.githubusercontent.com/shengshimeiyan/fty44safe/main/config.json
```

**新设备 / 新版本 X5**（支持 TLS1.2+，jsDelivr 国内一般可直连）：

```text
https://cdn.jsdelivr.net/gh/shengshimeiyan/fty44safe@main/config.json
```

> ⚠️ 4.4 盒子的 X5 OkHttp 仅支持 SSLv3/TLS1.0：jsDelivr / GitHub raw
> 直链均为 TLS1.2+，**从盒子内订阅必失败**（`ssl3 alert handshake failure`），
> 必须走上面的 gh.927223.xyz 代理。免费代理偶发抖动，抓取失败时 X5
> 会自动沿用缓存配置继续播放，稍后重试即可；断网也能看（方式 A）。

## ⚠️ 版本警告（4.4 用户必读）

- X5 **2.8.9 及以上是多 dex (multidex) 版本**：Android 4.4 上装得上但启动必崩
  （`NoClassDefFoundError: j$.util.Objects`）—— **4.4 只能用 2.5.0**
- 从高版本降级回 2.5.0 会先被 `INSTALL_FAILED_VERSION_DOWNGRADE` 挡住，必须先卸载；
  而卸载会把 `/data/data` 里的配置一起抹掉，等于全毁。别升。

## 版本历史

| 版本 | 文件 | 说明 |
| --- | --- | --- |
| v1 | `history/fty44v1.json` | 初版 |
| v2 | `history/fty44v2.json` | 增补广告净化规则 |
| v3（当前） | `history/fty44v3.json` = `config.json` | 11 站 + 11 组规则定稿 |

`config.json` 始终指向当前最新版，订阅地址永不变。

---

本配置仅为个人备份与学习用途；所含站点均为公开采集接口，如有侵权请联系删除。
