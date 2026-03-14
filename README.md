# 🎵 Daily Music Releases Digest

**全球新唱片每日速报智能体** — 基于 [Perplexity Computer](https://www.perplexity.ai/) 的自动化 Agent Skill

自动搜集全球每日新发行的专辑和单曲，按风格分类，翻译成你的语言，附带 Apple Music 链接，定时发送邮件摘要到你的邮箱。

---

## ✨ 功能特性

- 🌍 **全球覆盖** — 从 Billboard、Pitchfork、NME、Spotify、Apple Music 等多个权威来源搜集新发行信息
- 🎸 **风格过滤** — 支持 20 种音乐风格自由组合，也可以全选
- 🌐 **多语言翻译** — 摘要自动翻译成你指定的语言（中文、英文、日文等）
- 🍎 **Apple Music 链接** — 每张专辑/单曲自动附带 Apple Music 收听链接，点击即可收听
- 📧 **定时邮件推送** — 每天/每周自动发送到你的邮箱
- ⚙️ **灵活配置** — 随时修改风格、语言、频率、邮箱

## 📋 支持的音乐风格

| 风格 | Emoji |
|------|-------|
| Alternative（另类） | 🎸 |
| Rock（摇滚） | 🎸 |
| Pop（流行） | 🎵 |
| Gospel（福音） | ✝️ |
| Metal（金属） | 🤘 |
| Indie Folk（独立民谣） | 🪕 |
| Ambient（氛围） | 🌊 |
| Hip-Hop / Rap（嘻哈） | 🎤 |
| R&B / Soul（节奏蓝调） | 🎶 |
| Electronic / Dance（电子/舞曲） | 🎧 |
| Country（乡村） | 🤠 |
| Jazz（爵士） | 🎷 |
| Classical（古典） | 🎻 |
| K-Pop（韩流） | 💜 |
| J-Pop（日流） | 🌸 |
| Latin（拉丁） | 💃 |
| Reggae / Dancehall（雷鬼） | 🎼 |
| Blues（蓝调） | 🎹 |
| Punk（朋克） | ⚡ |
| World Music（世界音乐） | 🌍 |

也可以自定义不在列表中的风格关键词。

## 🚀 快速开始

### 前置条件

- 一个 [Perplexity](https://www.perplexity.ai/) 账户
- 已连接 Gmail（用于发送邮件）

### 安装步骤

1. 下载本仓库中的 `SKILL.md` 文件
2. 打开 [Perplexity Computer 技能管理页面](https://www.perplexity.ai/computer/skills)
3. 导入 `SKILL.md` 文件
4. 在对话中告诉 Perplexity Computer 你的需求，例如：

```
帮我做一个每天自动搜集全球新唱片上市信息的智能体，
只要 Alternative、Rock、Pop、Metal 这几个风格，
翻译成中文，每天中午发送到我的邮箱。
```

智能体会自动加载该技能并完成配置。

## 📖 使用示例

### 示例 1：中文用户，指定风格

> 帮我做一个每天自动搜集全球新唱片上市信息的智能体，只要 Alternative、Rock、Pop、Metal 这几个风格，翻译成中文，每天中午发送到我的邮箱。

### 示例 2：英文用户，全部风格，每周一次

> Set up a weekly new album digest every Monday morning at 9am, covering all genres.

### 示例 3：简单请求

> I want to get notified about new indie folk and ambient releases.

## 📬 邮件样例

收到的邮件格式如下：

```
==============================
🎵 每日全球新唱片速报
   2026年03月14日
==============================

🎸 Alternative（另类）
------------------------------
1. Radiohead（电台司令）
   专辑: "The New Album"（新专辑）
   发行日期: 2026-03-14
   简介: 英国传奇乐队时隔五年发布的全新录音室专辑，
         延续了实验电子与另类摇滚的融合风格。
   Apple Music: https://music.apple.com/album/the-new-album/123456789
   推荐曲目: Track One, Track Two

🎵 Pop（流行）
------------------------------
1. Dua Lipa（杜阿·利帕）
   单曲: "Sunshine"（阳光）
   发行日期: 2026-03-14
   简介: 英国流行天后的最新夏日单曲。
   Apple Music: https://music.apple.com/album/sunshine/987654321

==============================
今日共 5 张新发行
==============================
```

*（以上为格式示例，非真实发行数据）*

## ⚙️ 可配置参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| 音乐风格 | 要追踪的风格列表 | 全部风格 |
| 语言 | 摘要翻译的目标语言 | 用户主语言 |
| 邮箱 | 接收邮箱地址 | 用户上下文邮箱 |
| 频率 | 每天/每周 + 具体时间 | 每天中午 12:00 |
| 数据来源 | 音乐信息来源网站 | Billboard, Pitchfork, NME, Spotify, Apple Music |
| Apple Music 链接 | 是否为每张专辑附带收听链接 | 是 |

## 🔧 修改配置

设置完成后，你可以随时通过对话修改：

- **增减风格**："帮我加上 Jazz 和 Blues"
- **修改时间**："改成每天早上 8 点发送"
- **修改语言**："改成英文摘要"
- **暂停/停止**："暂停每日新唱片速报"
- **修改邮箱**："发送到另一个邮箱 xxx@xxx.com"

## 🏗️ 技术架构

```
用户配置偏好
    ↓
Perplexity Computer 加载 SKILL.md
    ↓
创建定时任务（Cron）
    ↓
每日定时触发 ──→ 多源网络搜索（Billboard, Pitchfork, Spotify...）
                    ↓
              风格过滤 & 去重
                    ↓
              查找 Apple Music 链接
                    ↓
              翻译 & 格式化
                    ↓
              通过 Gmail 发送邮件
```

## 📝 注意事项

- 该技能依赖网络搜索获取发行信息，覆盖范围取决于搜索结果质量
- 小众风格（如 Ambient、Gospel）某些日期可能搜索结果较少
- 仅在有符合条件的新发行时才会发送邮件
- 需要用户已在 Perplexity Computer 中连接 Gmail
- 每张专辑/单曲均附带 Apple Music 链接，未上架的会标注“暂未上架 Apple Music”

## 📄 许可证

本项目采用 [MIT License](LICENSE) 开源。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request。

如果你有好的改进想法，比如：
- 支持更多数据来源
- 增加新的音乐风格分类
- 优化邮件格式
- 支持更多推送渠道（Slack、Telegram 等）
- 支持更多流媒体链接（Spotify、YouTube Music 等）

请随时提交 PR。

## ⭐ Star History

如果这个项目对你有帮助，请给一个 Star ⭐ 支持一下。
