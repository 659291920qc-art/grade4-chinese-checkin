# 四年级上册语文·抗遗忘曲线打卡系统（RJ版）

> 基于艾宾浩斯遗忘曲线设计的小学语文学习打卡工具，单 HTML 文件零依赖，跨设备云端同步（GitHub Gist 私有存储）。

## 🌐 在线使用

**主访问地址**：https://659291920qc-art.github.io/grade4-chinese-checkin/

（首次打开会自动加载本地缓存；如需跨设备同步，按页面顶部「☁️ 云端同步」提示配置。）

## ✨ 核心功能

- 📊 **总表** — 当前学习进度与错字统计
- 📖 **每日学习** — 按艾宾浩斯曲线（1/2/4/7/15/30 天）安排的本课生字、词语、多音字
- 🔁 **每日复习** — 已学内容的间隔复习，按时段筛选
- ❌ **错字本** — 自动收集错字 + 手动添加（支持易错字、词语、多音字，可填拼音和备注）
- 🟠 **多音字专项** — 高亮对比 + 单字/词组多音练习

## ☁️ 云端同步（v2 重要更新）

之前版本（v1）数据只在浏览器 `localStorage` 里，换浏览器/换设备数据就没了。

**v2 起支持 GitHub Gist 私有云同步**，流程：

1. 首次访问 → 顶部出现 `☁️ 云同步未设置 · 点此配置` 提示条
2. 点击 → 输入 GitHub Personal Access Token（需要 `Gist: Read and write` 权限）
3. 自动创建一个私有 Gist → 之后每次打卡都会自动同步（8 秒 debounce）
4. 换浏览器/电脑打开同一 URL → 自动拉取最新数据 → 继续打卡

**💾 本地备份（不需网络）**：在「云同步管理」面板里也有 JSON 导入/导出按钮。点「📤 导出 JSON」会下载一个 `四年级语文打卡数据_YYYY-MM-DD.json` 文件，电脑里保存即可。点「📥 导入 JSON」可从之前的备份文件恢复数据——即使没配 token 也能用。

**Token 获取教程**（首次配置时页面内也有）：
1. 打开 https://github.com/settings/tokens?type=beta
2. Generate new token → 选 Fine-grained
3. Repository access: Public Repositories（或 All）
4. Permissions → **Gist: Read and write**（必勾）
5. 生成 → 复制 `github_pat_...` 开头的 token 粘贴进去

## 📦 技术栈

- 纯 HTML + CSS + 原生 JavaScript（零依赖，单文件）
- localStorage 主存 + GitHub Gist API 云端同步
- GitHub Pages 静态托管
- 字符编码：UTF-8

## 📅 变更记录

### v2.1 — JSON 导入/导出（2026-08-20）
- 新增 JSON 文件导出/导入（云同步的本地备份方案）
- 文件名：`四年级语文打卡数据_YYYY-MM-DD.json`
- 支持两种 JSON 格式：包装格式 `{ _format, exportedAt, data }` + 直接 state
- 导入前自动备份当前数据到 `_grade4_chinese_state_PRE_IMPORT` 键（可手动还原）
- 即使未配置云同步也能用
- 「云同步管理」面板整合所有数据管理入口

### v2 — 云端同步（2026-08-20）
- 新增 GitHub Gist 私有云同步功能
- 数据：`learnDay / reviewDateOffset / records / errors`
- 同步策略：8 秒 debounce 自动上传，启动时拉取最新
- 冲突解决：以最后修改时间戳为准
- UI：顶部新增 ☁️ 状态条（连接状态 / 最后同步时间 / 管理入口）
- **完全向后兼容**：未配置云同步的用户行为完全不变

### v1 — 手动添加错字功能（2026-08-20）
- 错字 Tab 增加「📝 手动添加错字」按钮
- 支持易错字、词语、多音字三种类型，可填拼音 + 备注
- 数据结构：`{ manual: true, pinyin, note, createdAt }`

## 🔒 隐私

- 数据**只存**两个地方：
  1. 你浏览器 localStorage（本地）
  2. 你的 GitHub 账号下的一个**私有** Gist（云端）
- 不经过任何第三方服务器
- Token 只存在你浏览器 localStorage，不上传
- 删除仓库 / 删除 Gist / 清浏览器缓存 → 数据消失，三者独立

## 🛠️ 本地开发

直接双击 `index.html` 即可在浏览器打开。所有功能可用，云同步功能需要能访问 `api.github.com`。