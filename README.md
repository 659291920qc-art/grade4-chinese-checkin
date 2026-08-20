# 任务：四年级上册语文打卡表 — 增加手动添加错字功能

**日期：** 2026-08-20 21:27 GMT+8
**文件：** `C:\Users\ASUS\.openclaw\workspace\四年级上册语文打卡表.html`

## 目标
为"错字本"页面增加手动添加错字的能力，覆盖原数据结构中没有覆盖的字/词/多音字，方便用户随时补录错题。

## 关键变更

### 1. UI 改动（HTML）
在"错词 Tab"统计行下方加了：
- **`manual-add-bar`**：常驻条，左侧是橙色「➕ 手动添加错字」按钮，右侧提示文案
- **`manual-form`**（默认隐藏）：点击展开为橙色边框表单，包含 5 个字段：
  - 类型（下拉：会写字 / 词语 / 多音字）
  - 所属课文（下拉：自动从 `lessons` 数组生成）
  - 错字内容（必填，单字限制根据类型校验）
  - 拼音/读音（可选）
  - 备注（可选，最多 100 字）

### 2. CSS 新增
- `.manual-add-bar` / `.manual-toggle-btn` — 橙色强调色
- `.manual-form` — 圆角卡片 + 4px 橙色左边框 + slideDown 入场动画
- `.manual-form-grid` — 2 列网格，窄屏自动塌缩为单列
- `.error-tag-manual` — 「手动」橙底小标签
- `.error-note` — 备注 pill（带 tooltip，溢出省略）
- `.error-actions .manual-del` — 灰色描边删除按钮

### 3. JS 新增
| 函数 | 作用 |
|------|------|
| `buildLessonOptions()` | 从 `lessons` 动态填充课文下拉 |
| `updateManualFormHint()` | 切换类型时动态更新 placeholder |
| `toggleManualForm()` / `closeManualForm()` | 展开/关闭表单，重置字段 |
| `submitManualError()` | 校验 → 写入 `state.errors` → 重绘 |
| `deleteError(key)` | 删除手动条目（带 confirm）|
| `escapeHtml(s)` | 通用 HTML 转义（备注渲染用） |

### 4. 数据约定
手动添加的错字使用如下 `state.errors[key]` 结构：
```js
{
  count: 1,              // 若重复添加 +1
  char: "臣",            // 兼容已有渲染
  lessonNo: 3,
  resolved: false,
  manual: true,          // 新增：标识手动添加
  createdAt: "2026-08-20",
  pinyin: "chén",        // 新增：自定义拼音（可选）
  note: "右边是'土'"     // 新增：备注（可选）
}
```
Key 规则沿用原约定：`c_第几课_字`、`w_第几课_词`、`d_第几课_字`。

### 5. 兼容性
- 已学习的字如果用户重新"手动添加"，仅累加 `count` 并标记 `manual: true`，不覆盖原 `records` 状态
- 渲染逻辑：`getErrorInfo` 优先使用 `err.pinyin`，自动给手动条目显示「手动」标签、备注和「删除」按钮
- 数据键与原系统一致，错词 Tab 的「未克服 / 已克服」分类、计数、克服装置均无需改动
- `localStorage` key (`grade4_chinese_state_v2`) 不变，旧数据完全兼容

### 6. 校验结果
- ✅ HTML 标签平衡（124 div open/124 close，1 script open/1 close，1 style open/1 close）
- ✅ JS `new Function(js)` 无语法错误（37100 字符）
- ✅ 18 项静态检查全部通过（函数定义、DOM 绑定、CSS 类、初始化调用、去重逻辑等）

## 待用户决策（未做）
- 是否需要在「总表」Tab 也加手动添加？目前只在错字本 Tab 添加。
- 是否需要导出手动错字到 CSV/TXT？目前没有导出功能。

## 结论
已实现手动添加错字功能；原有打卡 / 复习 / 多音字 / 错字本流程不受影响。直接刷新页面即可使用，数据保存在原有 localStorage。
