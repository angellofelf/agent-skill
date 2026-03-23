---
name: design2code
description: 将 MasterGo 自动生成的 demo.jsx 参考组件转化为可交互的真实 React 组件。使用场景：用户提供了一个从设计稿生成的 demo.jsx 文件和设计稿链接，需要将其转化为使用 Ant Design、ECharts 或项目内部组件的真实可交互组件。优先从 demo.jsx 获取像素级布局和样式，从 DSL 获取设计系统 Token。
---

# Demo 参考组件转真实组件

## 概述

将 MasterGo 自动生成的 demo.jsx（纯静态、内联样式）转化为可交互的真实 React 组件，使用 Ant Design、项目组件库等。

## 输入要求

用户需要提供：
1. **demo.jsx 文件路径** - MasterGo 自动生成的参考组件
2. **设计稿链接**（可选）- MasterGo 设计稿 URL，用于获取 DSL 数据

## 核心原则

### 样式来源优先级
1. **demo.jsx**（主要）- 像素级布局、绝对定位、视觉细节
2. **DSL (MCP)**（辅助）- 颜色 Token、字体规范、设计系统

### 技术栈替换规则

| demo.jsx 元素 | 替换为 |
|--------------|--------|
| 纯文本 div | Ant Design 组件（Select、DatePicker、Input 等） |
| 静态按钮 | Ant Design Button |
| 静态表格 | Ant Design Table 或项目表格组件 |
| 图表占位 | ECharts 或项目图表组件 |
| 树形选择 | DepartmentTreeSelect 等项目组件 |

## 执行步骤

### Step 1: 读取并分析 demo.jsx

```
1. 读取 demo.jsx 文件完整内容
2. 提取关键信息：
   - 整体布局结构（header、sidebar、content）
   - 颜色值（background、color、borderColor）
   - 尺寸值（width、height、padding、margin、gap）
   - 字体值（fontSize、fontFamily、lineHeight）
   - 定位值（position、left、top、right、bottom）
   - 交互元素（按钮、输入框、下拉框等）
```

### Step 2: 获取设计稿 DSL（如提供链接）

```
1. 从 MasterGo 链接提取 fileId 和 layerId
2. 调用 mcp__getDsl 获取 DSL 数据
3. 提取 Token：
   - paint_xxx -> 颜色变量
   - font_xxx -> 字体变量
   - effect_xxx -> 阴影效果
```

### Step 3: 创建组件结构

```
文件结构：
src/componentLibrary/[name]/
├── [Name].jsx       # 主组件文件
├── [Name].less      # 样式文件
└── index.js         # 导出文件
```

### Step 4: 生成 JSX 组件

**必须遵循的规则：**

1. **保留像素级布局**
   - 保持 demo.jsx 中的 width、height、left、top 等值
   - 使用 CSS 变量或 Less 变量管理颜色

2. **替换为可交互组件**
   ```jsx
   // demo.jsx 中的静态 div
   <div style={{ padding: '5px 8px', border: '1px solid rgba(0,0,0,0.13)' }}>
     全部
   </div>
   
   // 替换为
   <Select value={value} onChange={handleChange}>
     <Option value="all">全部</Option>
   </Select>
   ```

3. **添加状态管理**
   ```jsx
   const [value, setValue] = useState(defaultValue);
   const handleChange = useCallback((val) => {
     setValue(val);
   }, []);
   ```

4. **预留事件接口**
   ```jsx
   // 必须预留的事件
   onSearch    // 查询
   onReset     // 重置
   onExport    // 导出
   onSave      // 保存
   ```

### Step 5: 生成 LESS 样式

**颜色变量命名（基于 DSL Token）：**
```less
// 中性色
@gray-0: #FFFFFF;
@gray-10: #F5F5F5;
@gray-30: #E5E5E5;
@gray-60: rgba(0, 0, 0, 0.36);
@gray-70: rgba(0, 0, 0, 0.6);
@gray-90: rgba(0, 0, 0, 0.84);

// 品牌色
@blue-30: #89B9FF;
@blue-50: #1473FF;

// 边框色
@border-color: rgba(0, 0, 0, 0.13);
```

**布局保持：**
```less
.component-name {
  // 保持 demo.jsx 中的精确值
  width: 1224px;
  position: absolute;
  left: 204px;
  top: 76px;
}
```

### Step 6: 验证和调整

检查清单：
- [ ] 布局尺寸与 demo.jsx 一致
- [ ] 颜色值与 DSL Token 匹配
- [ ] 所有静态元素已替换为可交互组件
- [ ] 状态管理已添加
- [ ] 事件处理函数已预留
- [ ] 无 TypeScript/ESLint 错误

## 输出示例

### 输入：demo.jsx 片段
```jsx
<div style={{
  width: '284px',
  display: 'flex',
  flexDirection: 'column',
  gap: '2px'
}}>
  <span style={{
    color: 'rgba(0,0,0,0.6)',
    fontSize: '14px',
    lineHeight: '22px'
  }}>是否超标</span>
  <div style={{
    padding: '5px 8px',
    background: '#FFFFFF',
    border: '1px solid rgba(0,0,0,0.13)',
    borderRadius: '4px'
  }}>全部</div>
</div>
```

### 输出：真实组件
```jsx
// JSX
<div className="form-item">
  <label>是否超标</label>
  <Select value={isExceed} onChange={setIsExceed}>
    {exceedOpts.map(opt => (
      <Option key={opt.key} value={opt.key}>{opt.value}</Option>
    ))}
  </Select>
</div>

// LESS
.form-item {
  width: 284px;
  display: flex;
  flex-direction: column;
  gap: 2px;
  
  label {
    color: @gray-70;
    font-size: 14px;
    line-height: 22px;
  }
  
  .ant-select {
    border-radius: 4px;
    border-color: @border-color;
  }
}
```

## 注意事项

1. **不要修改 demo.jsx** - 它仅作为参考
2. **保持像素级还原** - 特别是定位、尺寸、间距
3. **使用项目组件库** - 优先使用项目内部组件而非新建
4. **添加 AI 注释** - 在文件头部添加 `@ai-generated` 标记
5. **预留扩展点** - 为后续功能预留接口和状态

## 相关工具

- `mcp__getDsl` - 获取 MasterGo 设计稿 DSL 数据
- `mcp__getMeta` - 获取设计稿元数据
- `search_codebase` - 查找项目中的现有组件
