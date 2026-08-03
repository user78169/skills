---
name: "html-prototype-creator"
description: "Generates pixel-accurate HTML prototypes from screenshots, PRDs, or descriptions. Invoke when user asks to create/draw/build a prototype, wireframe, or mockup as HTML."
---

# HTML Prototype Creator

将截图、PRD 或自然语言描述转化为高保真 HTML 原型。核心原则：**先确认再动手，一击即中**。

## 何时使用

- 用户提供页面截图，要求生成 HTML 原型
- 用户描述页面需求，要求画出原型
- 用户要求在已有原型基础上修改/新增功能
- 用户要求生成移动端/Web 端/后台管理端原型

## 工作流程

### 第1步：识别输入类型

| 输入类型 | 处理方式 |
|---------|---------|
| 截图 | 先用框架图提示词识别模块结构，再生成原型 |
| PRD/需求文档 | 提取页面清单和功能点，拟定布局草案 |
| 自然语言描述 | 提取页面名称、核心功能、平台类型 |
| 已有原型修改 | 读取现有 HTML，定位修改区域 |

### 第2步：确认关键决策点（强制步骤，不可跳过）

**在写任何 HTML 之前，必须用 AskUserQuestion 工具向用户确认以下维度。** 每个维度提供推荐选项，让用户快速选择而非自由思考。

#### 确认清单（5个维度）

**维度1：平台与尺寸**
- 移动端（375px 手机框）← 推荐
- Web 端（1440px 后台布局）
- 双端（同时生成）

**维度2：模块清单与粒度**
- 展示当前识别到的模块清单，询问：
  - A. 模块齐全，按此生成 ← 推荐
  - B. 需要增减模块
  - C. 需要调整某个模块的内部结构
- 粒度确认：模块内部展开到什么程度？
  - 只画模块标题+占位 ← 适合概览
  - 画出关键子元素（按钮、卡片、列表项）← 推荐
  - 完整还原所有细节 ← 适合交付

**维度3：视觉风格**
- 清爽白底+品牌色点缀 ← 推荐
- 深色主题
- 按截图风格还原
- 自定义（用户描述）

**维度4：交互需求**
- 纯静态展示 ← 最快
- 模块悬停标注（hover 显示模块编号）← 推荐
- 可切换视图/弹窗/Tab切换
- 可滚动+完整交互

**维度5：特殊功能**
- 无 ← 推荐（先出基础版）
- 预览功能（后台原型适用）
- 多状态切换（空/加载/有数据）
- 模块标注+可编辑

#### 确认规则

- 用 `AskUserQuestion` 工具一次性问 2-4 个维度（工具上限4题）
- 每题提供 2-4 个选项，第一个标"推荐"
- 用户选"推荐"项则快速通过，选其他项则追问细节
- 如果用户在原始需求中已明确某个维度（如"移动端"），跳过该维度
- **绝不跳过确认直接画** —— 这是一击即中的核心保障

### 第3步：生成原型

确认完成后，立即生成 HTML 文件。生成时遵循以下规范：

#### 文件结构规范

```
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[页面名] · HTML原型</title>
  <style>
    /* 1. 全局重置 */
    /* 2. 手机框/页面容器 */
    /* 3. 各模块样式（按模块编号注释分隔） */
    /* 4. 交互样式（hover/active/动画） */
    /* 5. 模块标注样式（如启用） */
  </style>
</head>
<body>
  <!-- 手机框 -->
  <div class="phone">
    <!-- 每个模块用注释标注序号和名称 -->
    <!-- 1. 状态栏 -->
    <!-- 2. 导航栏 -->
    ...
  </div>
</body>
</html>
```

#### CSS 规范

| 规范项 | 要求 |
|--------|------|
| 颜色变量 | 在 `:root` 中定义品牌色，模块中引用 |
| 字体 | `-apple-system, "PingFang SC", "Helvetica Neue", sans-serif` |
| 间距 | 使用 4 的倍数（4/8/12/16/20/24px） |
| 圆角 | 卡片 8-12px，按钮 16-22px，手机框 32-40px |
| 阴影 | `0 8px 40px rgba(0,0,0,0.15)` 用于浮层 |
| 模块分隔 | CSS 中用 `/* ========== N. 模块名 ========== */` 注释 |
| 占位图 | 用渐变色 `linear-gradient(135deg, ...)` 代替图片 |

#### HTML 规范

| 规范项 | 要求 |
|--------|------|
| 模块注释 | 每个模块前加 `<!-- N. 模块名 -->` |
| 语义化 | 用 `header/main/section/footer` 而非全 div |
| 重复元素 | 列表项用相同的 class，数据用占位文字 |
| 交互元素 | 按钮用 `<button>`，链接用 `<a>` |
| 表单元素 | 用真实 `<input>/<select>/<textarea>` |

#### 移动端原型规范

```
- 手机框：375×812px，圆角40px，阴影
- 状态栏：44px 高，含时间+信号+电量
- 内容区：flex:1，overflow-y:auto（可滚动）
- 底部导航：固定高度，flex 布局
- 模块标注：hover 时右侧浮出彩色标签
```

#### 后台原型规范

```
- 顶栏：56px 高，深色背景，含面包屑
- 侧边栏：200px 宽，菜单分组
- 内容区：flex:1，padding 24px
- 表格：斑马纹+hover 高亮
- 表单：卡片分组，label 在上
- 按钮：主操作蓝色，次操作白底灰边
```

### 第4步：交付与跟进

生成后：
1. 用 `computer://` 链接分享文件
2. 简要说明原型包含哪些模块和交互
3. 主动询问是否需要调整（给出常见调整方向）

## 返工防范清单

以下情况最易导致返工，生成前必须确认：

| 返工风险点 | 确认方式 | 不确认的后果 |
|-----------|---------|-------------|
| 模块数量不对 | 列出模块清单让用户确认 | 漏画/多画模块 |
| 模块内部结构不对 | 描述关键模块的内部布局 | 层级画错需重做 |
| 按钮数量/位置不对 | 确认按钮是每卡片一个还是统一一个 | 交互逻辑错误 |
| 预览范围不对 | 确认是单模块预览还是全模块预览 | 预览功能需重做 |
| 展示粒度不对 | 确认是概览级还是交付级 | 粒度不符需返工 |
| 平台不对 | 确认移动端/Web端/双端 | 尺寸布局全错 |

## 常见调整速查

用户反馈某部分不对时，按以下方向快速调整：

| 用户反馈 | 调整方向 |
|---------|---------|
| "模块画少了" | 补充遗漏模块，检查截图底部是否被截断 |
| "层级画得太平" | 用嵌套 div + 内部 flex/grid 表示父子关系 |
| "按钮太多/太少" | 确认按钮归属（每卡片一个 vs 统一一个） |
| "想加预览功能" | 添加 overlay 弹窗 + 手机框渲染 + 高亮当前模块 |
| "想改某个模块" | 定位对应 CSS+HTML 区块，局部替换 |
| "风格不对" | 调整 `:root` 中的品牌色变量 |
| "想要交互" | 添加 JS：视图切换/弹窗/Tab/开关 |

## 设计模式库

### 移动端常见模块

| 模块 | CSS 关键属性 | HTML 结构 |
|------|-------------|----------|
| 状态栏 | `height:44px; display:flex; justify-content:space-between` | 左时间+右信号电量 |
| 搜索栏 | `height:36px; background:#f5f5f5; border-radius:18px` | 输入框+按钮 |
| Tab 导航 | `display:flex; justify-content:space-around` | N个tab项，active高亮 |
| Banner | `border-radius:12px; background:linear-gradient` | 图+文案+CTA |
| 卡片网格 | `display:flex; gap:8px` 或 `grid` | N个相同结构卡片 |
| 列表项 | `display:flex; align-items:center; gap:12px` | 左图+中文+右操作 |
| 底部导航 | `height:60px; display:flex; justify-content:space-around` | N个图标+文字 |

### 后台常见模块

| 模块 | CSS 关键属性 | HTML 结构 |
|------|-------------|----------|
| 顶栏 | `height:56px; background:#1a1a2e; color:#fff` | Logo+面包屑+用户 |
| 侧边栏 | `width:200px; border-right:1px solid #e8e8e8` | 菜单分组+active高亮 |
| 筛选栏 | `display:flex; gap:16px; padding:16px 20px` | 下拉+输入+按钮 |
| 数据表格 | `width:100%; border-collapse:collapse` | th灰底+tr hover |
| 表单卡片 | `padding:24px; border-radius:6px` | 标题+form-grid |
| 分页 | `display:flex; gap:6px; justify-content:flex-end` | 页码按钮 |
| 弹窗 | `position:fixed; z-index:1000; background:rgba(0,0,0,0.5)` | overlay+content |

## 输出要求

1. **语言**：HTML 中的文本内容使用中文
2. **文件格式**：单个 `.html` 文件，自包含（CSS+JS 内联，无外部依赖）
3. **保存位置**：用户工作目录
4. **文件命名**：`[页面名]原型.html`（如 `东方甄选首页原型.html`）
5. **分享方式**：用 `computer://` 链接分享
