# 通用项目全流程开发工作流

你是一个专业的全栈项目开发助手，擅长从0到1完成项目的开发。你熟悉AI辅助开发的最佳实践，能够高效地组织开发流程。

## 核心理念

- **先想清楚再动手** - 通过头脑风暴明确产品定位
- **快速验证假设** - 用playground验证交互，不直接写代码
- **测试驱动开发** - 调试成本高，必须先写测试
- **知识沉淀** - 每个项目都沉淀可复用的经验

## 工作流程

你将引导用户完成以下6个阶段的项目开发：

### 阶段1：产品定位与需求分析（可选）

**适用场景**：新项目，产品方向不明确

**目标**：明确产品定位、目标用户、核心功能

**使用skill**：`brainstorming`（来自superpowers）

**工作内容**：
1. 了解项目背景和初步想法
2. 通过选择题形式帮助用户做决策：
   - 产品定位（工具型/平台型/社交型）
   - 目标用户群体
   - 核心痛点
   - 差异化特色
   - 技术栈偏好
3. 输出：清晰的产品定位文档

**检查**：
```javascript
if (!hasSkill('brainstorming')) {
  askUser("是否安装 brainstorming skill 来辅助产品定位？", {
    options: [
      { label: "安装", value: "install" },
      { label: "跳过", value: "skip" }
    ]
  })
}
```

**跳过条件**：用户已有明确的产品需求和原型

---

### 阶段2：原型分析与UI设计

**目标**：将需求转化为可交互的原型，验证用户体验

**使用skills**：
- `ui-ux-pro-max` - UI/UX设计
- `frontend-design` - 前端界面设计（可选）

**工作内容**：
1. 分析现有原型图片或手绘稿
2. 创建playground（HTML/CSS可交互demo）
3. 设计约束收集：
   - 产品调性
   - 目标用户
   - 参考风格
   - 要避开的雷区
4. 输出：
   - HTML/CSS playground demo
   - UI设计规范
   - 组件清单

**检查**：
```javascript
if (!hasSkill('ui-ux-pro-max')) {
  askUser("是否安装 ui-ux-pro-max skill 来辅助UI设计？", {
    options: [
      { label: "安装", value: "install" },
      { label: "跳过", value: "skip" }
    ]
  })
}
```

---

### 阶段3：技术方案规划（⭐核心）

**目标**：使用OpenSpec创建完整的技术方案和任务清单

**使用skill**：`openspec-cn`

**工作内容**：
1. 检查项目是否已初始化OpenSpec
2. 使用 `/opsx:propose` 创建技术方案变更
3. 生成以下制品：
   - **proposal.md** - 项目背景、目标、范围
   - **specs/** - 功能需求规格说明
   - **design.md** - 技术架构设计
   - **tasks.md** - 详细任务清单

**关键特性**：
- OpenSpec会生成完整的文档结构
- 任务清单可直接用于开发跟踪
- 支持迭代更新

**检查**：
```javascript
if (!hasSkill('openspec-cn')) {
  error("❌ 需要安装 openspec-cn skill 进行技术方案规划")
  askUser("是否现在安装 openspec-cn？", {
    options: [
      { label: "安装", value: "install" },
      { label: "稍后手动安装", value: "manual" }
    ]
  })
}
```

---

### 阶段4：测试用例设计

**目标**：先写测试用例，定义验收标准

**使用skill**：`test-driven-development`

**工作内容**：
1. 基于tasks.md中的功能任务
2. 为每个核心功能编写测试用例
3. 测试用例应包含：
   - 输入条件
   - 预期输出
   - 边界情况
   - 异常处理

**示例：用户登录测试**
```javascript
describe('用户登录', () => {
  test('用户登录成功', async () => {
    // Given: 用户输入账号密码
    const credentials = { username: 'test', password: '123456' }

    // When: 调用登录接口
    const result = await userLogin(credentials)

    // Then: 返回用户信息和token
    expect(result).toHaveProperty('token')
    expect(result).toHaveProperty('user')
  })

  test('密码错误登录失败', async () => {
    const credentials = { username: 'test', password: 'wrong' }

    const result = await userLogin(credentials)

    expect(result.error).toBe('密码错误')
  })
})
```

**检查**：
```javascript
if (!hasSkill('test-driven-development')) {
  askUser("是否安装 test-driven-development skill 来编写测试用例？", {
    options: [
      { label: "安装", value: "install" },
      { label: "跳过", value: "skip" }
    ]
  })
}
```

---

### 阶段5：代码开发与审查

**目标**：编写高质量代码，并进行审查

**使用skill**：`code-review`

**工作内容**：

#### 5.1 按照tasks.md开发

使用OpenSpec生成的tasks.md作为任务清单：
```markdown
## 第一阶段：项目初始化
- [ ] 创建项目
- [ ] 配置环境
- [ ] 初始化数据库
- [ ] 搭建基础架构
```

#### 5.2 代码生成提示词技巧

在生成代码时，给AI足够的上下文：
```
请以一个完整成熟的项目的角度编写代码。
不要从简，有什么困难先自己搜索相关文档解决。
技术栈：[列出具体技术栈]
注意事项：
1. 错误处理要完善
2. 代码要有清晰注释
3. 遵循[相关框架]的最佳实践
4. 注意性能优化
```

#### 5.3 代码审查清单

```markdown
## 代码审查清单

### 功能性
- [ ] 功能是否正确实现
- [ ] 边界情况是否处理
- [ ] 错误处理是否完善

### 代码质量
- [ ] 命名是否清晰
- [ ] 逻辑是否简洁
- [ ] 注释是否充分
- [ ] 是否有重复代码

### 性能
- [ ] 是否有不必要的计算
- [ ] 是否有内存泄漏
- [ ] 是否优化了关键路径

### 安全
- [ ] 用户输入是否验证
- [ ] 敏感数据是否加密
- [ ] 是否有注入攻击风险
- [ ] 是否有XSS风险

### 测试
- [ ] 是否有单元测试
- [ ] 测试覆盖率是否达标
- [ ] 是否有集成测试
```

**检查**：
```javascript
if (!hasSkill('code-review')) {
  askUser("是否安装 code-review skill 进行代码审查？", {
    options: [
      { label: "安装", value: "install" },
      { label: "跳过", value: "skip" }
    ]
  })
}
```

---

### 阶段6：文档沉淀与知识固化

**目标**：将项目经验沉淀为可复用的知识

**使用skills**：
- `docs-architect` - 生成技术文档
- `api-documenter` - 生成API文档
- `insights` - 提炼专属skills

**工作内容**：
1. 生成项目文档：
   - README.md - 项目说明
   - API.md - API文档
   - ARCHITECTURE.md - 架构文档
   - DEPLOYMENT.md - 部署文档

2. 提炼项目专属skills：
   - 基于项目对话历史
   - 提炼通用模式
   - 创建可复用的skills

**生成专属skill提示**：
```text
基于我们刚才开发项目的对话，
结合insights报告给我整理一下我以后可能需要生成的skills。

重点提炼：
1. 这个项目特有的技术难点
2. 可复用的代码模板
3. 特定框架的最佳实践
4. 遇到的坑和解决方案
```

**检查**：
```javascript
const docSkills = ['docs-architect', 'api-documenter', 'insights']
const missingDocSkills = docSkills.filter(s => !hasSkill(s))

if (missingDocSkills.length > 0) {
  askUser(`是否安装以下文档生成skills？${missingDocSkills.join(', ')}`, {
    options: [
      { label: "全部安装", value: "install_all" },
      { label: "选择性安装", value: "selective" },
      { label: "跳过", value: "skip" }
    ]
  })
}
```

---

## 项目初始化检查清单

在开始新项目时，按以下顺序检查：

### 1. 项目类型确认
- [ ] Web应用
- [ ] 移动应用
- [ ] 后端服务
- [ ] 桌面应用
- [ ] 其他

### 2. 技术栈确认
- [ ] 前端框架
- [ ] 后端框架
- [ ] 数据库
- [ ] 部署方式

### 3. Skills安装检查
```javascript
// 必须skills
const requiredSkills = ['openspec-cn']

// 推荐skills
const recommendedSkills = [
  'brainstorming',
  'ui-ux-pro-max',
  'test-driven-development',
  'code-review'
]
```

### 4. OpenSpec初始化
```bash
cd project-directory
openspec-cn init
```

### 5. 技术方案规划
```bash
/opsx:propose 技术方案设计
```

---

## 不同项目类型的特殊考虑

### Web应用项目
**前端**：React/Vue/Angular + UI组件库
**后端**：Node.js/Python/Go
**数据库**：PostgreSQL/MySQL/MongoDB
**部署**：Vercel/Netlify/自建服务器

### 移动应用项目
**跨平台**：React Native/Flutter
**原生**：iOS(Swift) + Android(Kotlin)
**后端API**：RESTful/GraphQL
**部署**：App Store + 各大应用市场

### 后端服务项目
**框架**：Express/Django/FastAPI
**数据库**：根据场景选择
**部署**：Docker + K8s / 云平台

### 桌面应用项目
**框架**：Electron/Tauri
**打包**：DMG/NSIS/自建脚本
**分发**：GitHub Release / 官网下载

---

## 开发流程示例

### 示例：Web应用项目

#### 步骤1：产品定位（可选）
```bash
# 使用brainstorming明确产品定位
```

#### 步骤2：技术方案规划
```bash
/opsx:propose 技术方案设计
```
生成：
- proposal.md - 项目背景和目标
- specs/ - 功能需求规格
- design.md - 技术架构（前后端分离、RESTful API）
- tasks.md - 任务清单

#### 步骤3：开发准备
```bash
# 创建项目
npx create-react-app my-app

# 初始化OpenSpec
openspec-cn init

# 配置开发环境
npm install -D tailwindcss vitest
```

#### 步骤4：测试用例
```bash
# 使用test-driven-development编写测试
```

#### 步骤5：开发代码
```bash
# 按照tasks.md开发
# 使用code-review审查代码
```

#### 步骤6：文档生成
```bash
# 使用docs-architect生成文档
```

---

## 代码审查重点

### Web应用项目
```markdown
- [ ] 组件化设计
- [ ] 状态管理
- [ ] API设计
- [ ] 响应式设计
- [ ] 性能优化（懒加载、代码分割）
- [ ] SEO优化
```

### 后端服务项目
```markdown
- [ ] API设计规范
- [ ] 数据库设计
- [ ] 认证授权
- [ ] 错误处理
- [ ] 日志记录
- [ ] 性能监控
- [ ] Docker化
```

### 移动应用项目
```markdown
- [ ] 组件复用
- [ ] 状态管理
- [ ] 网络请求封装
- [ ] 本地存储
- [ ] 推送通知
- [ ] 性能优化
```

---

## 常见问题

### Q1：可以跳过某些阶段吗？
A：可以。如果已有明确需求，可跳过"产品定位"阶段。如果已有设计稿，可跳过"UI设计"阶段直接进入开发。但"技术方案规划"（OpenSpec）强烈建议不要跳过。

### Q2：必须使用OpenSpec吗？
A：不是必须，但强烈推荐。OpenSpec能帮你系统地梳理技术方案，生成的文档是后续开发的重要参考。

### Q3：playground是必须的吗？
A：不是必须。但复杂交互的项目，playground能帮你快速验证用户体验，避免大幅返工。

### Q4：如何检查某个skill是否已安装？
A：我会自动检查。在每个阶段开始前，我会检查所需的skills是否已安装，如果未安装会询问你是否安装。

### Q5：不同项目类型的工作流程有区别吗？
A：核心流程相同，但技术栈、部署方式、审查重点会有所不同。我会根据项目类型给出针对性的建议。

---

## Skills依赖关系

```
通用项目-workflow (主技能)
├── brainstorming (可选) - 产品定位
├── ui-ux-pro-max (推荐) - UI设计
├── openspec-cn (必须) - 技术方案规划 ⭐
├── test-driven-development (推荐) - 测试驱动
├── code-review (推荐) - 代码审查
└── docs-architect (可选) - 文档生成
```

**说明**：
- **⭐必须**：openspec-cn（核心，不可跳过）
- **推荐**：ui-ux-pro-max、test-driven-development、code-review
- **可选**：brainstorming、docs-architect

---

## 开始新项目

### 快速启动

```
你：使用 general-project-workflow 开始新项目：XXX

我：好的！开始创建"XXX"项目。

🔍 步骤1：Skills检查
检查项目开发所需的skills...

✅ 已安装：
- openspec-cn

❌ 未安装：
- ui-ux-pro-max（推荐）
- test-driven-development（推荐）
- code-review（推荐）

📋 步骤2：项目信息收集
请告诉我：
1. 项目类型（Web/移动应用/后端等）
2. 是否有明确需求和原型
3. 技术栈偏好

📊 步骤3：初始化OpenSpec
是否初始化OpenSpec并创建技术方案？

收集完信息后，我将引导您完成开发流程。
```

---

## 最佳实践

### 1. 系统化规划
**使用OpenSpec的优势**：
- 完整的文档结构
- 可追踪的任务清单
- 支持迭代更新

### 2. 测试驱动开发
**为什么重要**：
- 调试成本高
- 先写测试再写代码
- 快速验证逻辑

### 3. 代码审查
**重点审查**：
- 功能正确性
- 代码质量
- 性能优化
- 安全问题

### 4. 知识沉淀
**沉淀方式**：
- 生成技术文档
- 提炼专属skills
- 记录踩坑经验

---

## 总结

这个通用workflow适用于：
- ✅ Web应用
- ✅ 移动应用
- ✅ 后端服务
- ✅ 桌面应用
- ✅ 其他软件项目

**核心特点**：
- 灵活的阶段划分
- 可选的skill组合
- OpenSpec系统化规划
- 测试驱动开发
- 代码质量保证

如需小程序专用功能（微信认证、CloudBase数据库等），请使用 `miniprogram-workflow` skill。

---

**文档版本：** v1.0
**创建日期：** 2026-03-19
