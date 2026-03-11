## 使用方法

### 1. 触发命令

安装后，可以通过以下命令触发：

```
/ai-stats [command] [options]
```

或简写形式：

```
/aistats [command] [options]
```

### 2. 子命令模式

```
/ai-stats                              # 默认：统计最新 commit
/ai-stats staged                       # 统计暂存区内容
/ai-stats commit <hash>                # 统计指定 commit
/ai-stats range <hash1>..<hash2>       # 统计 commit 范围
```

### 3. 参数说明

| 参数 | 位置 | 必填 | 说明 |
|------|------|------|------|
| `command` | 第1个 | 否 | 子命令：`staged`、`commit`、`range` 或省略 |
| `commit-hash` | 第2个 | 条件 | 当 command 为 `commit` 时的目标 hash |
| `range` | 第2个 | 条件 | 当 command 为 `range` 时的范围，格式 `hash1..hash2` |
| `task-name` | 倒数第2个 | 否 | 需求/任务名称，不传则使用当前 commit message |
| `git-username` | 最后1个 | 否 | Git 用户名，不传则使用 `git config user.name` |

### 4. 使用示例

#### 示例 1：使用默认参数（统计最新 commit）
```
/ai-stats
```
使用当前 commit message 和 Git 用户名。

#### 示例 2：统计暂存区
```
/ai-stats staged
```

#### 示例 3：统计指定 commit
```
/ai-stats commit abc1234
```

#### 示例 4：统计 commit 范围
```
/ai-stats range abc1234..def5678
```

#### 示例 5：指定任务名称和用户名
```
/ai-stats commit abc1234 "用户管理模块" aurorayang
```
同时指定需求名和用户名。

## AI 代码标记规范

为了让工具正确识别 AI 生成的代码，需要在代码中添加以下标记：

### 1. 完整 AI 生成文件

在文件头部添加：

```
/**
 * @ai-generated
 * @date 2026-03-06
 * @description 功能描述
 */
```

### 2. 部分 AI 生成代码

在 AI 生成代码段前后添加：

```
// @ai-generated-start
function aiGeneratedFunction() {
  // 这部分代码由 AI 生成
  console.log('AI generated');
}
// @ai-generated-end
```

## 输出示例

### 控制台输出

```
🔍 AI 代码统计
================
统计人: zhangsan (Git 用户名)
需求名称: 用户管理模块 (命令行指定)
变更文件: 5 个
新增行数: 320 行
删除行数: 45 行

🤖 AI 生成统计
================
AI 生成文件: 2 个
AI 生成行数: 180 行
AI 代码占比: 56.3%

📄 报告已生成
================
模式: 新建报告
路径: AI-Generate/zhangsan/ai-code-report-2026-03-06-用户管理模块.md
提示: 报告已保存，请在 pre-commit 中自动添加或手动 git add
```

## 报告存储结构

```
{project-root}/
└── AI-Generate/
    ├── zhangsan/
    │   ├── ai-code-report-2026-03-06-用户管理模块.md
    │   ├── ai-code-report-2026-03-06-订单模块.md
    │   └── summary.md (累计统计)
    ├── lisi/
    │   └── ai-code-report-2026-03-06-支付模块.md
    └── ...
```

### 作为 Git Hook 使用

可以在 `.git/hooks/post-commit` 中添加（统计最新 commit）：

```bash
#!/bin/bash
# 调用 AI 代码统计
echo "正在统计 AI 生成代码..."
node .lingma/skills/ai-code-stats/ai-code-stats.js
```

或在 `.git/hooks/pre-commit` 中添加（统计暂存区）：

```bash
#!/bin/bash
# 调用 AI 代码统计
echo "正在统计 AI 生成代码..."
node .lingma/skills/ai-code-stats/ai-code-stats.js staged
```

## 直接运行脚本

也可以直接运行脚本进行统计：

```bash
# 最新 commit
node .lingma/skills/ai-code-stats/ai-code-stats.js

# 暂存区
node .lingma/skills/ai-code-stats/ai-code-stats.js staged

# 指定 commit
node .lingma/skills/ai-code-stats/ai-code-stats.js commit abc1234

# commit 范围
node .lingma/skills/ai-code-stats/ai-code-stats.js range abc1234..def5678

# 完整参数示例
node .lingma/skills/ai-code-stats/ai-code-stats.js commit abc1234 "用户管理模块" aurorayang
```

## 支持的文件类型

- `.ts` - TypeScript
- `.tsx` - TypeScript React
- `.js` - JavaScript
- `.jsx` - JavaScript React
- `.mpx` - MPX 小程序框架
- `.vue` - Vue 单文件组件
- `.less` / `.scss` / `.css` - 样式文件

## 注意事项

1. **模式选择**：
   - `staged` 模式检查暂存区内容，适合 pre-commit 场景
   - `commit` 模式检查已提交的特定 commit
   - `range` 模式适合统计一个功能分支的所有变更

2. **确保 Git 配置正确**：需要正确设置 `user.name` 才能生成个人报告

3. **标记必须规范**：AI 生成代码需要按照规范添加标记才能被正确识别

4. **报告不会自动提交**：生成的报告文件需要手动添加到暂存区

5. **重复统计防护**：
   - 相同 commit hash 不会重复生成报告
   - 相同文件内容（blob hash）不会重复统计

## 版本历史

### v1.2.0
- 新增子命令支持：`staged`、`commit`、`range`
- 支持统计指定 commit 的代码
- 支持统计 commit 范围的代码
- 优化参数解析逻辑，兼容旧用法

### v1.1.0
- 新增用户名参数支持
- 支持 `/ai-stats [task-name] [username]` 格式
- 不传参数时默认使用 Git 用户名

### v1.0.0
- 初始版本
- 支持暂存区 AI 代码统计
- 生成个人专属报告
- 支持历史累计统计

## License

MIT
