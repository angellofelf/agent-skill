---
name: code-reviewer
description: "代码审查技能，按照团队规范自动审查代码，检查潜在问题和最佳实践，特别针对MPX文件进行详细规范检查，支持生成HTML格式审查报告并保存到本地"
version: "1.1.0"
author: "Lingma"
tags: ["code-review", "mpx", "typescript", "best-practices", "static-analysis", "html-report", "local-storage"]
---

## 技能描述

这是一个专业的代码审查技能，严格按照团队编码规范和 `.lingma/rules/mpx_rule.md` 中的详细要求，对代码进行全面的质量检查和最佳实践评估。

当用户请求代码审查时，我会：
1. 分析提供的代码文件内容
2. 严格按照 MPX 规范进行逐项检查
3. **只在控制台输出简洁的统计摘要信息**
4. **自动生成美观的 HTML 格式报告并保存到项目根目录的 `cr-records` 文件夹**
5. **HTML 文件命名格式：`code-review-report-{timestamp}.html`**
6. **确保每次审查都生成完整的静态 HTML 文件，包含所有审查内容**
7. **强制文件验证机制**：生成后自动检查文件是否真实存在于指定路径
8. **提供可点击的文件链接**：生成标准 `file://` 协议的绝对路径链接，可直接点击访问
9. **错误处理机制**：文件生成失败时提供明确的错误信息和重试建议

## 核心审查维度

### 📁 目录结构与文件命名
- 检查文件名是否使用小写字母加中划线格式（如：my-component.mpx）
- 验证文件行数是否超过500行需要拆分
- 确认组件存放位置是否符合规范
- 检查页面级组件目录结构是否正确
- 验证是否缺少必要的分离文件（const.ts, controller.ts等）

### 🎨 Template 模板规范
- 验证使用 wx: 开头的小程序语法
- 禁止三目表达式的使用
- 检查可选链操作符和空字符串兜底
- 验证事件绑定使用 catchtap 防止冒泡
- 确认 wx:for 循环包含 wx:key 属性
- 检查 wx:style 和 wx:class 的合理使用

### 💻 Script 脚本规范
- 强制要求使用 lang="ts"
- 验证 ES6 语法和 lodash 的正确使用
- 检查命名规范（camelCase/PascalCase/UPPER_CASE）
- 确认 JSDoc 注释的完整性
- 禁止使用 setData，要求直接赋值
- 验证参数校验和错误处理逻辑
- 检查业务逻辑是否合理分离

### 🎨 Style 样式规范
- 检查使用 less 语法和 scoped 属性
- 验证样式命名规范（简单名词/BEM原则）
- 禁止使用 unoCSS 语法
- 检查样式嵌套是否合理

### ⚙️ JSON 配置规范
- 验证组件引用命名规范
- 检查 usingComponents 字段的正确使用

### HTML 报告生成功能（必选 - 强制执行）

**每次代码审查都必须自动生成专业的 HTML 审查报告**，并且包含完整的自我验证和反馈机制：

#### 报告生成步骤（必须严格执行）
1. **创建 HTML 报告文件**，包含以下内容：
   - 页面标题和审查时间
   - 审查摘要和总体评分（大号显示，带进度条）
   - 四个维度的评分卡片：
     * 代码质量（Code Quality）
     * 安全性（Security）
     * 性能（Performance）
     * 可维护性（Maintainability）
   - 问题列表（按严重程度分类）：
     * 🔴 严重问题（Critical）- 红色标识
     * 🟡 中等问题（Medium）- 黄色标识
     * 🟢 轻微问题（Minor）- 绿色标识
   - 改进建议和代码示例（带语法高亮）
   - 优点列表

2. **样式要求**：
   - 使用现代化的 CSS 设计（渐变背景、卡片阴影、圆角）
   - 响应式布局，适配不同屏幕尺寸
   - 使用专业的配色方案
   - 代码块使用等宽字体和语法高亮
   - 添加图标和视觉元素提升可读性

3. **保存和验证**：
   - 文件名格式：`code-review-report-{YYYYMMDD-HHmmss}.html`
   - 保存到工作区根目录的 `cr-records` 文件夹
   - **必须执行自我检查**：验证文件是否成功创建
   - **必须报告生成结果**：告知用户文件位置和大小
   - **强制路径验证**：使用 fs.existsSync() 确认文件真实存在
   - **文件完整性检查**：验证文件大小和基本内容结构
   - **错误回退机制**：生成失败时提供替代方案和调试信息

### 报告特色（必须实现）
- 🎨 **现代化设计**：渐变背景、卡片阴影、圆角设计
- 📊 **可视化评分**：圆形进度条显示总体评分
- 🎯 **四维度评分**：独立的评分卡片展示
- 💻 **代码高亮**：等宽字体和语法高亮显示
- 🔴🟡🟢 **彩色分级**：不同严重程度使用不同颜色标识
- 📱 **响应式布局**：适配不同屏幕尺寸
- 📈 **统计展示**：问题分布的可视化统计

### 文件管理（必须执行）
- **自动创建 `cr-records` 目录**（如不存在）
- **文件命名格式**：`code-review-report-{YYYYMMDD-HHmmss}.html`
- **保存位置**：项目根目录下的 `cr-records` 文件夹
- **文件完整性**：确保 HTML 文件包含所有审查内容，可直接在浏览器中查看

## 核心审查维度

### 📁 目录结构与文件命名
- 检查文件名是否使用小写字母加中划线格式（如：my-component.mpx）
- 验证文件行数是否超过500行需要拆分
- 确认组件存放位置是否符合规范
- 检查页面级组件目录结构是否正确

### 🎨 Template 模板规范
- 验证使用 wx: 开头的小程序语法
- 禁止三目表达式的使用
- 检查可选链操作符和空字符串兜底
- 验证事件绑定使用 catchtap 防止冒泡
- 确认 wx:for 循环包含 wx:key 属性
- 检查 wx:style 和 wx:class 的合理使用

### 💻 Script 脚本规范
- 强制要求使用 lang="ts"
- 验证 ES6 语法和 lodash 的正确使用
- 检查命名规范（camelCase/PascalCase/UPPER_CASE）
- 确认 JSDoc 注释的完整性
- 禁止使用 setData，要求直接赋值
- 验证参数校验和错误处理逻辑

### 🎨 Style 样式规范
- 检查使用 less 语法和 scoped 属性
- 验证样式命名规范（简单名词/BEM原则）
- 禁止使用 unoCSS 语法

### ⚙️ JSON 配置规范
- 验证组件引用命名规范
- 检查 usingComponents 字段的正确使用

## 特殊审查重点

### 🔧 代码架构
- 业务逻辑是否合理分离到 controller.ts
- 常量是否提取到 const.ts
- 工具函数是否复用 utils 目录现有功能
- 核心逻辑与技术栈是否解耦

### 📝 文档规范
- properties/data 属性是否有完整注释
- computed/methods 方法是否有 JSDoc 文档
- 复杂业务逻辑是否有足够说明

### ⚡ 性能优化
- 数据更新是否采用显式赋值
- 循环渲染是否优化
- 事件处理是否高效

## 使用方法

### 触发方式
当您提到代码审查相关内容时自动激活：
- "审查这段代码"
- "检查代码质量问题"
- "按照规范审查这个文件"
- "进行代码评审"
- "对 [文件路径] 进行代码审查"

### 审查执行流程（必须遵循 - 包含自我验证）
1. **代码分析**：深入分析提供的代码文件内容
2. **规范检查**：严格按照 MPX 规范逐项检查
3. **问题识别**：识别严重问题、中等问题、轻微问题
4. **优点发现**：找出代码中的优秀实践
5. **评分计算**：给出总体评分和四个维度的详细评分
6. **报告生成**：**必须自动生成完整的 HTML 静态报告**
7. **文件保存**：保存到 `./cr-records/code-review-report-{timestamp}.html`
8. **自我验证**：**检查文件是否成功生成，验证文件大小和内容完整性**
9. **简洁输出**：**只输出统计摘要和可点击的文件链接，不输出详细问题列表**
10. **结果告知**：提供文件访问链接和基本信息

### 支持的文件类型
- `.mpx` 文件（重点审查）
- `.ts` 文件
- `.js` 文件
- 其他源代码文件


### HTML 报告访问（必须说明）
生成的报告将保存在项目根目录的 `cr-records` 文件夹中，用户可以：
- **直接在浏览器中打开查看完整的审查报告**
- **分享给团队成员审阅**
- **纳入版本控制系统进行历史追踪**
- **每次审查都会生成新的 HTML 文件，不会覆盖之前的报告**

## 输出格式

### HTML 报告生成
自动生成美观的 HTML 格式审查报告：

#### 📁 文件保存位置
- 保存目录: `./cr-records/` (项目根目录)
- 文件命名: `cr-report-{git-username}-{current-date}.html`
- 例如: `cr-report-zhangsan-2024-01-15.html`

#### 🎨 HTML 报告特色
- ✅ **优点**：列出做得好的地方
- ⚠️ **问题**：指出需要改进的地方（按严重程度分类）   
- 🔴 严重：需要立即修复的问题   
- 🟡 中等：建议修复的问题   
- 🟢 轻微：可选的改进建议
- 💡 **建议**：提供具体的改进方案和示例代码
- 📊 **总体评分**：1-10 分
### HTML 报告生成（必选）当用户要求审查代码时，**自动生成 HTML 报告**：
#### 报告生成步骤
1. **创建 HTML 报告文件**，包含以下内容：   
- 页面标题和审查时间   
- 审查摘要和总体评分（大号显示，带进度条）   
  - 四个维度的评分卡片：     
    * 代码质量（Code Quality）     
    * 安全性（Security）     
    * 性能（Performance）     
    * 可维护性（Maintainability）   
  - 问题列表（按严重程度分类）：     
    * 🔴 严重问题（Critical）- 红色标识     
    * 🟡 中等问题（Medium）- 黄色标识     
    * 🟢 轻微问题（Minor）- 绿色标识   
  - 改进建议和代码示例（带语法高亮）   
  - 优点列表
2. **样式要求**：   
  - 使用现代化的 CSS 设计（渐变背景、卡片阴影、圆角）   
  - 响应式布局，适配不同屏幕尺寸   
  - 使用专业的配色方案   
  - 代码块使用等宽字体和语法高亮   
  - 添加图标和视觉元素提升可读性
3. **保存和预览**：   
  - 文件名格式：`code-review-report-{timestamp}.html`   
  - 保存到工作区根目录
#### HTML 模板结构
```html<!DOCTYPE html><html lang="zh-CN"><head>    <meta charset="UTF-8">    <meta name="viewport" content="width=device-width, initial-scale=1.0">    <title>代码审查报告</title>    <style>        /* 现代化样式 */        * { margin: 0; padding: 0; box-sizing: border-box; }        body {            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);            padding: 20px;            line-height: 1.6;        }        .container {            max-width: 1200px;            margin: 0auto;            background: white;            border-radius: 20px;            padding: 40px;            box-shadow: 020px 60px rgba(0,0,0,0.3);        }        .header {            text-align: center;            margin-bottom: 40px;            padding-bottom: 20px;            border-bottom: 3px solid #667eea;        }        .score-circle {            width: 150px;            height: 150px;            border-radius: 50%;            background: conic-gradient(#667eea 0% var(--score), #e0e0e0 var(--score) 100%);            display: flex;            align-items: center;            justify-content: center;            margin: 20px auto;            position: relative;        }        .score-inner {            width: 120px;            height: 120px;            border-radius: 50%;            background: white;            display: flex;            align-items: center;            justify-content: center;            font-size: 48px;            font-weight: bold;            color: #667eea;        }        .metrics {            display: grid;            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));            gap: 20px;            margin: 30px 0;        }        .metric-card {            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);            padding: 20px;            border-radius: 15px;            box-shadow: 04px 6px rgba(0,0,0,0.1);        }        .issue {            margin: 15px 0;            padding: 15px;            border-left: 4px solid;            border-radius: 8px;            background: #f9f9f9;        }        .critical { border-color: #e74c3c; background: #fee; }        .medium { border-color: #f39c12; background: #ffeaa7; }        .minor { border-color: #27ae60; background: #d5f4e6; }        pre {            background: #2d2d2d;            color: #f8f8f2;            padding: 15px;            border-radius: 8px;            overflow-x: auto;            margin: 10px 0;        }        code {            font-family: 'Courier New', monospace;        }    </style></head><body>    <!-- 报告内容 --></body></html>```## 审查示例### 命名规范检查```python# ❌ 不好的命名def f(x, y):    return x + y# ✅ 好的命名def calculate_total_price(base_price: float, tax_rate: float) -> float:    return base_price * (1 + tax_rate)```### 安全性检查```python# ❌ SQL 注入风险query = f"SELECT * FROM users WHERE id = {user_id}"# ✅ 使用参数化查询query = "SELECT * FROM users WHERE id = ?"cursor.execute(query, (user_id,))```### 性能优化检查```javascript// ❌ 低效的循环for (let i = 0; i < arr.length; i++) {    for (let j = 0; j < arr.length; j++) {        // O(n²) 复杂度    }}// ✅ 使用 Map 优化constmap = new Map();arr.forEach(item => map.set(item.id, item)); // O(n)```
## 评分标准
### 总体评分（1-10分）
- **9-10分**：优秀，代码质量高，几乎没有问题
- **7-8分**：良好，有少量改进空间
- **5-6分**：中等，存在一些需要修复的问题
- **3-4分**：较差，有较多问题需要解决- **1-2分**：很差，存在严重问题
### 各维度评分每个维度（代码质量、安全性、性能、可维护性）独立评分：
- **优秀（8-10）**：符合最佳实践
- **良好（6-7）**：基本合格，有改进空间
- **需改进（4-5）**：存在明显问题
- **差（1-3）**：有严重缺陷
## 使用示例当用户说"帮我审查这段代码"或"review 这个文件"时：
1. 仔细分析代码
2. 识别问题和优点
3. 生成详细的 HTML 报告
4. 告知用户报告已生成并可以查看

## 详细审查报告示例

### 🖥️ 控制台输出示例（简洁版）
```
🎯 代码审查完成
📊 总体评分: 8.0/10
🔴 严重问题: 2项
🟡 中等问题: 6项
🟢 轻微问题: 8项
✅ 优秀实践: 5项

🔗 审查报告: [点击查看详细报告](file:///Users/didi/Desktop/workspace/git_local/es-mpx-call-car/cr-records/code-review-report-20260212-143025.html)
📁 文件大小: 28.5 KB
⏰ 完成时间: 2026-02-12 14:30:25
✅ 文件验证: 已确认真实生成
```

## 审查深度级别

### 基础审查
- 语法正确性检查
- 基本规范验证
- 常见错误识别

### 详细审查
- 完整规范检查
- 架构合理性评估
- 性能优化建议

### 深度审查
- 代码质量全方位分析
- 最佳实践深度挖掘
- 团队规范一致性检查

## 配置选项

### 审查严格度
- **严格模式**: 报告所有潜在问题
- **标准模式**: 平衡准确性和实用性
- **宽松模式**: 只报告明确违规问题

### 输出格式
- **控制台输出**: 在终端显示审查结果
- **HTML 报告**: 生成本地 HTML 文件（默认开启）
- **JSON 格式**: 输出结构化数据

### HTML 报告配置
```json
{
  "htmlReport": {
    "enabled": true,
    "outputDir": "./cr-records/",
    "filenamePattern": "cr-report-{username}-{date}.html",
    "includeCodeSnippets": true,
    "showCharts": true,
    "theme": "light"
  }
}
```

## 集成建议

### 开发流程集成
```bash
# 代码提交前自动审查
pre-commit hook: lingma code-reviewer --staged

# CI/CD 流程集成
pipeline step: lingma code-reviewer --files "src/**/*.mpx"

# 生成 HTML 报告并上传
lingma code-reviewer --html --output-dir ./reports/
```

### IDE 集成
- 保存文件时自动触发审查
- 实时显示代码质量问题
- 快速修复建议
- 一键打开 HTML 报告

### 命令行选项
```bash
# 基本使用
lingma code-reviewer <file-path>

# 只生成 HTML 报告
lingma code-reviewer --html-only <file-path>

# 指定输出目录
lingma code-reviewer --output-dir ./my-reports/ <file-path>

# 包含代码片段
lingma code-reviewer --with-snippets <file-path>
```

## 团队协作价值

### 统一标准
- 确保团队代码风格一致性
- 建立可衡量的代码质量标准
- 减少 Code Review 时间成本

### 知识传承
- 自动化的最佳实践推广
- 新成员快速了解团队规范
- 持续的技术债务管理

### 质量保障
- 早期发现问题降低修复成本
- 预防常见错误和反模式
- 持续提升代码可维护性

### 历史追踪
- 自动生成的审查记录便于追溯
- 可视化质量趋势分析
- 团队代码质量改进跟踪

## 版本历史

### v1.1.0: HTML 报告增强版
- 新增 HTML 格式审查报告生成功能
- 支持本地文件保存和历史记录追踪
- 添加响应式设计和交互功能
- 集成 Git 用户信息自动识别

### v1.0.0: 初始版本
- 基于 `.lingma/rules/mpx_rule.md` 完整规范实现
- 支持 MPX 文件专项审查
- 提供多级别审查深度
- 实现团队规范一致性检查

## 重要承诺

**我郑重承诺：每次代码审查都必定生成完整的 HTML 静态报告文件，并经过严格验证！**

**执行流程保证：**
1. **立即生成**：审查完成后立即调用文件创建工具生成HTML报告
2. **双重验证**：先创建文件，再验证文件存在
3. **路径确认**：使用绝对路径确保文件可访问
4. **内容完整**：报告包含所有审查维度和详细分析

- ✅ **强制生成**：HTML 报告生成是审查流程的必需步骤，不容省略
- ✅ **立即执行**：审查完成后立即调用create_file工具生成文件
- ✅ **内容完整**：报告必须包含所有审查发现、评分详情和改进建议
- ✅ **格式标准**：使用现代化设计，支持在浏览器中直接查看
- ✅ **文件独立**：每个报告都是完整的静态 HTML 文件，无需外部依赖
- ✅ **易于访问**：保存在项目根目录的 `cr-records` 文件夹中
- ✅ **永不缺失**：无论代码质量如何，都会生成 HTML 报告
- ✅ **命名规范**：文件名格式为 `code-review-report-{timestamp}.html`
- ✅ **双重验证**：先创建文件，再验证文件系统确认存在
- ✅ **简洁输出**：控制台只显示统计摘要和可点击链接
- ✅ **链接可用**：提供标准的 `file://` 协议链接，可直接点击访问
- ✅ **文件检查**：严格验证文件真实生成，禁止仅输出而不实际创建文件
- ✅ **真实存在验证**：使用文件系统检查确认文件确实存在
- ✅ **路径标准化**：使用绝对路径并转换为标准的 `file://` URL格式
- ✅ **错误处理**：生成失败时提供具体原因和解决方案
- ✅ **状态监控**：实时报告生成进度和结果
- ✅ **兼容性保证**：确保HTML文件在各浏览器中正常显示
- ✅ **执行保证**：使用create_file工具确保文件实际写入磁盘

**这是我作为 code-reviewer 技能的核心职责和必备功能！**



  