# 项目初始化与规则生成

你是一个项目初始化助手，负责在项目开始时生成完整的规范文档和规则文件，确保AI辅助开发时能够遵循项目约定。

## 触发时机

**使用场景**：
- 新项目开始时
- 需要补充项目规范时
- 团队协作需要统一规范时

**触发方式**：
```text
初始化项目规则
或
生成项目规范文档
```

## 核心功能

### 1. 项目分析

在生成规则前，先分析项目特征：

```bash
# 检查项目类型
- 检测技术栈（前端框架、后端框架、数据库等）
- 识别项目结构（monorepo、多项目等）
- 分析已有文件和配置

# 询问用户
- 项目类型（Web/小程序/后端等）
- 技术栈选择
- 特殊规范要求
- 团队约定
```

### 2. 生成文件结构

```
项目根目录/
├── AGENTS.md                    # 项目总纲
├── README.md                    # 项目说明（更新）
├── .rules/                      # 规则目录
│   ├── .gitkeep
│   ├── overview.md              # 规则总览和导航
│   ├── coding-standards.md      # 代码风格规范
│   ├── architecture.md          # 架构规范
│   └── [模块]-rules.md          # 各模块规则
└── docs/
    ├── project-spec.md          # 项目规格说明
    └── api-conventions.md       # API约定（如需要）
```

### 3. AGENTS.md 生成

**作用**：项目总纲，AI和开发者快速了解项目

**模板**：
```markdown
# [项目名称] - AI开发说明

## 项目概述

[项目简介，1-2句话]

## 技术栈

### 前端
- 框架：[React/Vue/小程序等]
- 状态管理：[Redux/MobX/ Vuex等]
- UI组件库：[Ant Design/Element UI等]

### 后端
- 框架：[Node.js/Python/Go等]
- 数据库：[MySQL/MongoDB等]
- 其他：[缓存/消息队列等]

## 项目结构

\`\`\`
[目录树结构]
\`\`\`

## 开发规范

总纲性规范，具体的见 `.rules/` 目录下的规则文件。

## 规则导航

- [代码风格](.rules/coding-standards.md)
- [架构规范](.rules/architecture.md)
- [模块A规则](.rules/module-a-rules.md)
- [模块B规则](.rules/module-b-rules.md)

## 开发流程

1. [步骤1]
2. [步骤2]
3. [步骤3]

## 重要提醒

[当前阶段、注意事项等]

## 参考资源

- [技术方案文档](./docs/project-spec.md)
- [任务清单](./docs/task-list.md)
```

### 4. .rules/ 规则文件生成

#### 4.1 overview.md（规则总览）

```markdown
# 项目规则总览

## 规则文件结构

\`\`\`
.rules/
├── overview.md              # 本文件：规则总览和导航
├── coding-standards.md      # 代码风格、命名规范
├── architecture.md          # 架构设计规范
├── [模块]-rules.md          # 各模块规则
└── ...
\`\`\`

## 规则文件说明

### coding-standards.md
定义项目的基本编码规范，包括：
- 代码格式和缩进
- 命名约定（变量、函数、类、文件）
- 注释标准
- 语言特性使用建议

**适用范围**：所有源代码文件

### architecture.md
规定项目的架构设计原则：
- 分层架构（Controller/Service/Repository等）
- 模块划分原则
- 依赖注入方式
- 错误处理策略

**适用范围**：架构设计和代码组织

### [模块]-rules.md
各模块的特定规则：
- 文件组织方式
- 特定框架使用规范
- 模块内通信协议

**适用范围**：特定模块或目录

## 如何使用这些规则

### 开发新功能时
1. 先阅读 `coding-standards.md` 了解基本规范
2. 查看相关模块的规则文件
3. 遵循规范编写代码

### AI辅助开发时
- 告诉AI："遵循 .rules/ 目录下的规则"
- AI会自动参考相关规则文件

### 代码审查时
- 检查是否违反 `coding-standards.md`
- 验证架构设计符合 `architecture.md`
- 确认模块规则是否遵守

## 规则优先级

当多个规则文件都适用时：
1. 具体模块规则 > 通用规则
2. 后加载的规则 > 先加载的规则

## 规则维护

- 保持规则聚焦，每个文件专注一个领域
- 避免规则重复，必要时使用引用
- 定期更新规则以反映最新实践
- 规则变更需团队审核
```

#### 4.2 coding-standards.md（代码风格）

```markdown
# 代码风格规范

## 基本原则

- 遵循语言社区的标准风格指南
- 保持代码简洁、可读
- 避免过度设计

## 格式规范

### 缩进和对齐
- 使用 [空格/TAB] 缩进
- 缩进层级：[2/4] 空格

### 行宽
- 建议最大行宽：[80/120] 字符
- 超出时合理换行

### 空行
- 函数之间空一行
- 逻辑块之间空一行
- 不要有多余的空行

## 命名规范

### 变量命名
- 使用 [camelCase/snake_case]
- 命名要体现用途
- 避免单字母变量（除循环变量）

### 函数命名
- 使用 [camelCase/snake_case]
- 动词开头：[get/set/create/update/delete]
- 命名要清晰表达功能

### 类命名
- 使用 [PascalCase/CamelCase]
- 名词形式
- 避免缩写

### 常量命名
- 使用 [UPPER_SNAKE_CASE]
- 全大写下划线分隔

### 文件命名
- 使用 [kebab-case/PascalCase]
- 与类名保持一致（如适用）

## 注释规范

### 函数注释
\`\`\`[语言]
/**
 * 函数功能简述
 *
 * @param {类型} 参数名 - 参数说明
 * @returns {类型} 返回值说明
 */
\`\`\`

### 复杂逻辑注释
- 解释为什么，而非是什么
- 注释放在逻辑上方
- 保持注释简洁

### TODO注释
\`\`\`
// TODO: [待办事项] - [责任人] [时间]
\`\`\`

## 语言特性使用

### 推荐使用
- [特性1]
- [特性2]

### 避免使用
- [特性1] - 原因
- [特性2] - 原因

## 错误处理

### 错误捕获
- 必须捕获可预见的错误
- 使用具体的错误类型
- 提供有意义的错误信息

### 日志记录
- 关键操作记录日志
- 错误级别使用恰当
- 避免记录敏感信息

## 安全规范

- 输入验证
- 输出转义
- SQL注入防护
- XSS防护
- 敏感数据加密

## 性能考虑

- 避免不必要的计算
- 合理使用缓存
- 避免内存泄漏
- 数据库查询优化
```

#### 4.3 architecture.md（架构规范）

```markdown
# 架构设计规范

## 整体架构

### 架构模式
- [分层架构/微服务/MVC等]

### 分层说明
\`\`\`
┌─────────────┐
│  表现层      │  UI/页面
├─────────────┤
│  业务层      │  业务逻辑
├─────────────┤
│  数据层      │  数据访问
└─────────────┘
\`\`\`

## 模块划分

### 按功能划分
- 模块1：[职责]
- 模块2：[职责]
- 模块3：[职责]

### 按层次划分
- 表现层：[职责]
- 业务层：[职责]
- 数据层：[职责]

## 依赖管理

### 依赖原则
- 高层不依赖低层
- 依赖接口而非实现
- 避免循环依赖

### 依赖注入
- 使用 [构造函数/属性/方法] 注入
- 必需依赖用final/const标记
- 可选依赖提供默认值

## 数据流

### 数据流向
\`\`\`
用户输入 → 表现层 → 业务层 → 数据层 → 数据库
         ←          ←        ←        ←
响应数据
\`\`\`

### 数据传递
- 使用 DTO/VO 传递数据
- 不暴露内部实体
- 数据验证在边界进行

## API设计

### RESTful规范
- 使用正确的HTTP方法
- 资源命名使用名词
- 版本控制：/api/v1/

### 响应格式
\`\`\`json
{
  "code": 200,
  "message": "成功",
  "data": {}
}
\`\`\`

### 错误处理
- 统一错误码
- 明确错误信息
- 适当的HTTP状态码

## 配置管理

### 环境配置
- 开发环境
- 测试环境
- 生产环境

### 配置文件
- 使用 [YAML/JSON/ENV]
- 敏感信息环境变量
- 配置集中管理

## 部署架构

### 部署方式
- [容器化/虚拟机/Serverless]

### 服务划分
- 前端服务
- 后端服务
- 数据库服务
- 缓存服务
```

#### 4.4 [模块]-rules.md（模块规则）

模板：
```markdown
# [模块名称]规则

## 适用范围

\`\`\`yaml
globs:
  - "[文件路径模式1]"
  - "[文件路径模式2]"
\`\`\`

## 模块职责

[本模块负责什么]

## 文件组织

### 目录结构
\`\`\`
[模块目录]
├── [子目录1]
├── [子目录2]
└── [文件命名规则]
\`\`\`

### 文件命名
- 使用 [kebab-case/PascalCase]
- [命名规则]

## 开发规范

### [方面1]规范
- [规则1]
- [规则2]

### [方面2]规范
- [规则1]
- [规则2]

## 约束条件

- 必须：[必须遵守的规则]
- 禁止：[禁止做的事情]
- 推荐：[推荐的做法]

## 依赖关系

### 可依赖的模块
- [模块1]
- [模块2]

### 被依赖的模块
- [模块1]
- [模块2]

## 示例

### 好的示例
\`\`\`[语言]
// 代码示例
\`\`\`

### 坏的示例
\`\`\`[语言]
// 反例代码
\`\`\`

## 常见问题

### Q1: [问题]
A: [答案]

### Q2: [问题]
A: [答案]
```

### 5. 针对不同项目类型的规则预设

#### 5.1 微信小程序项目

```
.rules/
├── overview.md
├── coding-standards.md
├── architecture.md
├── cloudfunction-rules.md
│   # globs: cloudfunctions/**/*.js
├── user-page-rules.md
│   # globs: User/pages/**/*.js
├── driver-page-rules.md
│   # globs: Driver/pages/**/*.js
└── database-rules.md
    # globs: **/*db*.js, **/*database*.js
```

**cloudfunction-rules.md 示例**：
```markdown
# 云函数开发规则

## 适用范围

云函数开发，包括用户认证、数据库操作等。

## 开发规范

### 初始化
```javascript
// 必须使用
cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV })
```

### 用户认证
```javascript
// 必须获取用户身份
const { OPENID, APPID, UNIONID } = cloud.getWXContext()
```

### 数据库操作
- 使用 `no-sql-wx-mp-sdk` 最佳实践
- Collection命名：`logistics_` 前缀
- 必须定义TypeScript类型

### 错误处理
```javascript
try {
  // 业务逻辑
} catch (err) {
  console.error(err)
  return {
    code: 500,
    message: err.message
  }
}
```

## 命名规范

- 云函数文件夹：kebab-case
- 导出函数名：camelCase
```

#### 5.2 React项目

```
.rules/
├── overview.md
├── coding-standards.md
├── architecture.md
├── component-rules.md
│   # globs: src/components/**/*.jsx
├── page-rules.md
│   # globs: src/pages/**/*.jsx
├── hook-rules.md
│   # globs: src/hooks/**/*.js
└── api-rules.md
    # globs: src/api/**/*.js
```

#### 5.3 Spring Boot项目

```
.rules/
├── overview.md
├── coding-standards.md
├── architecture.md
├── controller-rules.md
│   # globs: **/controller/**/*.java
├── service-rules.md
│   # globs: **/service/**/*.java
├── mapper-rules.md
│   # globs: **/mapper/**/*.java
└── dto-rules.md
    # globs: **/dto/**/*.java
```

## 使用流程

### 步骤1：项目分析

```
你：初始化项目规则

我：好的！让我先分析一下项目。

🔍 项目分析：
- 项目类型：微信小程序
- 技术栈：小程序云开发
- 项目结构：用户端 + 司机端
- 是否有现有规则：否

📋 接下来需要确认：
1. 项目名称：Logistics
2. 主要模块：用户端、司机端、云函数、数据库
3. 特殊规范：遵循miniprogram-workflow最佳实践

确认无误后，我将生成以下文件：
✅ AGENTS.md
✅ .rules/overview.md
✅ .rules/coding-standards.md
✅ .rules/architecture.md
✅ .rules/cloudfunction-rules.md
✅ .rules/user-page-rules.md
✅ .rules/driver-page-rules.md
✅ .rules/database-rules.md

是否继续？
```

### 步骤2：生成文件

```
你：继续

我：✅ 开始生成文件...

📝 生成 AGENTS.md... 完成
📝 生成 .rules/overview.md... 完成
📝 生成 .rules/coding-standards.md... 完成
...

🎉 项目规则初始化完成！

📁 生成的文件：
- AGENTS.md（项目总纲）
- .rules/
  ├── overview.md
  ├── coding-standards.md
  ├── architecture.md
  ├── cloudfunction-rules.md
  ├── user-page-rules.md
  ├── driver-page-rules.md
  └── database-rules.md

💡 下一步：
1. 检查生成的规则是否符合需求
2. 根据实际情况调整规则内容
3. 开始开发时告诉AI："遵循.rules/目录下的规则"
```

### 步骤3：规则使用

```
你：帮我创建一个登录云函数

我：好的！我会遵循 .rules/cloudfunction-rules.md 中的规范来创建。

✅ 遵循的规则：
- 使用 cloud.DYNAMIC_CURRENT_ENV
- 通过 cloud.getWXContext() 获取用户身份
- 错误处理统一格式
- ...
```

## 规则更新与维护

### 更新原则

**核心原则**：保持文档一致性
- 任何规则变更都要同步更新索引
- 新增模块要更新所有相关文档
- 删除规则要清理所有引用

### 场景1：更新现有规则

#### 步骤1：更新预览

```
你：更新云函数规则，添加新的日志规范

我：好的！让我先查看当前规则...

📋 当前规则概览：
- 文件：.rules/cloudfunction-rules.md
- 章节数：6个
- 字数：约500字

📝 将要添加的规范：
1. 日志级别规范（info/warn/error）
2. 日志格式要求
3. 敏感信息过滤规则

🔍 冲突检测：
- 与 coding-standards.md 的日志规范对比
  ⚠️ 发现重叠内容：日志记录规范
- 建议：合并重复内容，保留cloudfunction特定规范

📄 影响范围：
- ✅ .rules/cloudfunction-rules.md（主要更新）
- ✅ .rules/overview.md（可能需要更新说明）
- ✅ AGENTS.md（无需更新）

是否继续？
```

#### 步骤2：执行更新

```
你：继续

我：✅ 开始更新...

📝 更新 .rules/cloudfunction-rules.md...
  ✅ 添加"日志规范"章节
  ✅ 整合与现有规则的关联

🔄 同步更新 .rules/overview.md...
  ✅ 更新cloudfunction-rules的说明
  ✅ 添加日志规范索引

✅ 更新完成！

📊 更新统计：
- 新增内容：3个章节
- 修改文件：2个
- 冲突解决：1处

💡 建议：
检查相关云函数是否符合新的日志规范
```

### 场景2：添加新模块规则

#### 步骤1：智能分析

```
你：添加支付模块规则

我：好的！让我先分析项目结构...

🔍 项目分析：
- 项目类型：微信小程序
- 现有模块：用户端、司机端、云函数、数据库
- 新增模块：支付模块

🤔 关联性分析：
- 与用户端的关系：需要调用支付API
- 与司机端的关系：结算功能
- 与云函数的关系：支付回调云函数
- 与数据库的关系：订单、交易记录

📋 建议生成的文件：
✅ .rules/payment-rules.md（支付模块规则）
✅ .rules/payment-api-rules.md（支付API规范）
🔄 更新 AGENTS.md（添加模块说明）
🔄 更新 .rules/overview.md（添加索引）
🔄 更新 coding-standards.md（添加支付相关命名规范）

📁 文件位置建议：
- 云函数：cloudfunctions/payment/
- 用户端：User/pages/payment/
- 司机端：Driver/pages/wallet/

是否按此方案生成？
```

#### 步骤2：批量生成

```
你：继续

我：✅ 开始批量生成...

📝 生成 .rules/payment-rules.md...
  ✅ 支付流程规范
  ✅ 安全要求（签名、验签）
  ✅ 错误处理
  ✅ 日志记录
  ✅ globs: **/payment/**/*.js, **/wallet/**/*.js

📝 生成 .rules/payment-api-rules.md...
  ✅ API设计规范
  ✅ 请求/响应格式
  ✅ 错误码定义
  ✅ globs: **/api/payment/**/*.js

🔄 更新 AGENTS.md...
  ✅ 添加"支付模块"章节
  ✅ 更新模块列表
  ✅ 添加支付开发流程说明

🔄 更新 .rules/overview.md...
  ✅ 添加规则文件索引
  ✅ 更新规则说明
  ✅ 添加适用范围

🔄 更新 coding-standards.md...
  ✅ 添加支付相关命名规范
  ✅ 添加金额处理规范

🎉 所有文件已生成并同步！

📊 生成统计：
- 新增规则文件：2个
- 更新索引文件：3个
- 总计变更：5个文件

💡 下一步建议：
1. 检查生成的规则是否符合需求
2. 实现支付功能时遵循这些规则
3. 定期审查支付安全规范
```

### 场景3：删除规则

```
你：删除driver-page-rules，司机端改用通用规则

我：好的！让我分析影响...

🔍 依赖分析：
- .rules/driver-page-rules.md
  被 AGENTS.md 引用 ✅
  被 .rules/overview.md 索引 ✅

⚠️ 警告：
司机端页面将不受专门规则约束，是否继续？

🔄 清理计划：
1. 删除 .rules/driver-page-rules.md
2. 从 AGENTS.md 移除引用
3. 从 .rules/overview.md 移除索引
4. 更新 coding-standards.md（如有司机端特定规范）

是否执行删除？
```

### 场景4：规则冲突检测

```
你：检查项目规则是否有冲突

我：好的！让我全面检查...

🔍 扫描规则文件...

⚠️ 发现潜在冲突：

### 冲突1：日志格式不一致
- 文件：cloudfunction-rules.md
- 内容：日志格式为 `[时间] [级别] 消息`
- 文件：coding-standards.md
- 内容：日志格式为 `级别 - 时间 - 消息`
- 建议：统一为一种格式

### 冲突2：命名规范冲突
- 文件：user-page-rules.md
- 内容：页面文件使用 kebab-case
- 文件：coding-standards.md
- 内容：所有JS文件使用 camelCase
- 建议：明确页面文件特例

✅ 无冲突部分：
- 数据库规范
- API设计规范
- 错误处理规范

💡 建议优先解决：
1. 日志格式统一
2. 命名规范明确范围

需要我自动修复这些冲突吗？
```

### 场景5：批量规则更新

```
你：批量更新所有规则，统一错误处理格式

我：好的！让我先检查所有规则...

📋 规则文件清单：
1. .rules/cloudfunction-rules.md
2. .rules/user-page-rules.md
3. .rules/driver-page-rules.md
4. .rules/database-rules.md
5. .rules/payment-rules.md

🎯 统一标准：
错误响应格式：
{
  "code": number,
  "message": string,
  "data": any
}

🔄 批量更新预览：

1. cloudfunction-rules.md
   ✅ 更新错误处理章节
   ✅ 添加错误示例代码

2. user-page-rules.md
   ✅ 添加错误处理章节
   ✅ 更新示例代码

3. driver-page-rules.md
   ✅ 添加错误处理章节
   ✅ 更新示例代码

4. database-rules.md
   ✅ 更新数据库错误处理
   ✅ 添加错误码映射

5. payment-rules.md
   ⚠️ 有自定义错误格式
   💡 建议：保留支付特定错误，统一通用错误

📊 更新统计：
- 需要更新：5个文件
- 统一格式：4个
- 保留特例：1个

是否执行批量更新？
```

### 场景6：规则版本管理

```
你：查看规则变更历史

我：好的！让我检查规则变更...

📜 规则变更历史：

### 2026-03-20 10:30
📝 cloudfunction-rules.md
变更：添加日志规范
影响：需要更新所有云函数
状态：✅ 已完成

### 2026-03-20 11:15
📝 新增 payment-rules.md
变更：新增支付模块规则
影响：新增支付模块
状态：🔄 进行中

### 2026-03-20 14:20
📝 coding-standards.md
变更：统一命名规范
影响：全局命名规范
状态：⏸️ 待审核

💡 建议操作：
1. 完成支付模块规则审核
2. 审核命名规范变更
3. 根据命名规范更新代码

需要我导出完整的变更报告吗？
```

### 场景7：规则导入导出

```
你：导出项目规则，用于其他项目

我：好的！让我整理规则...

📦 项目规则导出：

内容包含：
✅ AGENTS.md（项目总纲）
✅ .rules/overview.md（规则总览）
✅ .rules/coding-standards.md（代码规范）
✅ .rules/architecture.md（架构规范）
✅ .rules/[模块]-rules.md（模块规则）

导出格式：
- 方式1：完整规则包（包含项目特定内容）
- 方式2：通用规则模板（去除项目特定内容）
- 方式3：规则摘要文档（用于团队分享）

建议：通用规则模板可以用于：
- 新项目快速初始化
- 团队知识沉淀
- 规范复用

需要我生成哪种格式的导出？
```

## 智能更新特性

### 自动化特性

1. **索引自动更新**
   - 添加/删除规则时自动更新overview.md
   - 保持索引与实际文件同步

2. **依赖关系追踪**
   - 记录规则之间的引用关系
   - 更新时提示影响范围

3. **冲突智能检测**
   - 规则内容冲突检测
   - 提供解决方案建议

4. **变更影响分析**
   - 分析规则变更的影响范围
   - 提供更新建议

### 交互确认

所有更新操作都遵循：
1. 分析现状
2. 提供预览
3. 等待确认
4. 执行更新
5. 同步索引
6. 提供报告

## 注意事项

1. **规则要聚焦**：每个规则文件专注一个领域
2. **避免重复**：不要在多个文件中重复相同规则
3. **可维护性**：规则要清晰易懂，便于团队遵守
4. **定期更新**：根据项目发展及时更新规则
5. **团队审核**：重要规则变更需要团队审核

## 与现有技能配合

### 配合 miniprogram-workflow
```
项目初始化后，继续使用 miniprogram-workflow：
1. 产品定位（可选）
2. 原型分析
3. 技术方案规划（OpenSpec）
4. 项目规则初始化 ← 本skill
5. 云开发环境配置
6. UI设计
7. 测试用例
8. 代码开发
```

### 配合 auto-sync-skills-to-github
```
生成规则后，可以同步到GitHub：
"同步skills到GitHub"
```

## 相关文件

- `AGENTS.md` - 项目总纲
- `.rules/` - 规则目录
- `README.md` - 项目说明
- `docs/` - 详细文档

## 依赖关系

此skill是独立的，但可以与以下skills配合：
- `miniprogram-workflow` - 小程序开发流程
- `openspec-cn` - 技术方案规划
- `auto-sync-skills-to-github` - 规则同步

---

**文档版本：** v1.0
**创建日期：** 2026-03-20
**适用：** 所有项目类型
