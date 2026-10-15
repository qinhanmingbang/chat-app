# Chat（Supabase + GitHub Pages）

## 文件清单（都放在仓库根目录）
| 文件 | 作用 |
|---|---|
| index.html | 网页主体 |
| config.js | **只改这一个文件填密钥**，以后更新 index.html 不会再覆盖它 |
| manifest.json、sw.js、icon-192.png、icon-512.png | 添加到手机桌面（PWA） |
| upgrade.sql | 数据库升级脚本（在 Supabase 的 SQL Editor 运行） |

## 首次部署
1. Supabase 新建项目，Authentication → Providers → Email，需要的话关闭 Confirm email
2. 依次运行建表脚本：基础表 → 群聊 → 步数 → upgrade.sql
3. Storage 新建公开桶 `chat-files`
4. GitHub 新建仓库，上传全部文件，Settings → Pages → Deploy from a branch → main
5. 编辑 config.js，填 Supabase 的 Project URL 和 anon public key

## 升级步骤
1. 先在 Supabase 运行 upgrade.sql（必须先做）
2. GitHub → Add file → Upload files，上传新文件（同名替换）
3. 第一次升级需要新建 config.js 并填入密钥，以后不用再改
4. 等 1–2 分钟，链接后加 ?v=数字 刷新

## 已知限制
- 视频通话：免费 STUN 在少数网络下连不上。要更稳定需要 TURN 服务器，填在 config.js 的 TURN 里
- 浏览器不能后台常驻：关掉页面后收不到消息和来电（Web Push 需要额外的推送服务，本项目未实现）
- 图片/语音/文件在公开桶里，知道完整链接的人能打开；需要严格私密要改成私有桶加临时链接
- 零钱只在本应用内使用，不能提现；“修改零钱”是演示功能，正式使用请删除 set_my_balance 函数
- 单文件最大 50MB（Supabase 免费版限制）
- 资讯内容写在 index.html 的 NEWS 列表里，是静态内容
