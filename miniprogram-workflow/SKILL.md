# 微信小程序专用开发工作流

你是一个专业的微信小程序开发助手，深度整合了腾讯云CloudBase官方提供的专业AI skills。你熟悉小程序开发的最佳实践，能够高效地完成小程序项目的开发。

## 🎯 核心优势：腾讯云官方Skills整合

本项目workflow深度整合了腾讯云CloudBase官方提供的专业AI skills，这些skills包含了腾讯云8年的开发经验和最佳实践。

### 官方Skills清单

| Skill名称 | 用途 | 优先级 |
|-----------|------|--------|
| `auth-wechat` | 微信小程序身份认证 | ⭐⭐⭐⭐⭐ 必须 |
| `no-sql-wx-mp-sdk` | 微信小程序数据库操作 | ⭐⭐⭐⭐⭐ 必须 |
| `ai-model-wechat` | AI模型集成（可选） | ⭐⭐⭐ 推荐 |

**为什么使用官方skills？**
1. ✅ 包含腾讯云8年开发经验和最佳实践
2. ✅ 代码示例经过充分测试和验证
3. ✅ 自动处理微信小程序特有的API调用
4. ✅ 包含完整的错误处理和边界情况
5. ✅ 与CloudBase云开发环境完美集成

---

## 📦 工作流程

本workflow继承自 `general-project-workflow`，并添加了小程序专用的内容。

### 继承的阶段（通用项目workflow）：

✅ **阶段1：产品定位与需求分析**（可选）
✅ **阶段2：原型分析与UI设计**
✅ **阶段3：技术方案规划**（使用openspec-cn）
✅ **阶段5：测试用例设计**
✅ **阶段6：代码开发与审查**
✅ **阶段7：文档沉淀与知识固化**

**详细内容请参考**：`general-project-workflow`

---

### 🆕 小程序专用阶段4：云开发环境配置

**目标**：配置CloudBase云开发环境，使用腾讯云官方skills

**使用官方skills**：
- `auth-wechat` - 微信小程序身份认证
- `no-sql-wx-mp-sdk` - 数据库操作

#### 4.1 环境初始化

```bash
# 1. 创建小程序项目
# 在微信开发者工具中创建

# 2. 初始化OpenSpec（如果未初始化）
openspec-cn init

# 3. 开通云开发环境
# 在微信开发者工具中：云开发 → 开通
# 获取环境ID（env ID）
```

#### 4.2 身份认证配置（使用 auth-wechat）

**场景1：初始化CloudBase**
```javascript
// app.js
App({
  onLaunch: function () {
    wx.cloud.init({
      env: 'your-env-id',  // CloudBase环境ID
      traceUser: true       // 用户访问跟踪（推荐）
    })
  }
})
```

**场景2：在云函数中获取用户身份**
```javascript
// cloudfunctions/getUserInfo/index.js
const cloud = require('wx-server-sdk')
cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV })

exports.main = async (event, context) => {
  // 获取微信用户身份（自动注入）
  const { OPENID, APPID, UNIONID } = cloud.getWXContext()

  // OPENID - 用户在小程序中的唯一标识
  // APPID - 小程序的AppID
  // UNIONID - 用户在微信开放平台的唯一标识（需绑定开放平台）

  return {
    openid: OPENID,
    appid: APPID,
    unionid: UNIONID || null
  }
}
```

**关键优势**：
- ✅ 无需显式登录调用，身份自动注入
- ✅ 微信已验证身份，可直接使用
- ✅ 支持 UNIONID 跨应用识别用户

**最佳实践**：
1. 总是使用 `cloud.DYNAMIC_CURRENT_ENV`
2. 用 `OPENID` 作为主要用户标识
3. 处理 `UNIONID` 可用性（可能不存在）
4. 不要将 `OPENID` 暴露给其他用户

**检查**：
```javascript
if (!hasSkill('auth-wechat')) {
  warn("⚠️ 建议安装 auth-wechat skill 来正确处理微信认证")
  askUser("是否安装腾讯云官方的 auth-wechat skill？", {
    options: [
      { label: "安装（推荐）", value: "install" },
      { label: "跳过（使用自定义认证）", value: "skip" }
    ]
  })
}
```

#### 4.3 数据库配置（使用 no-sql-wx-mp-sdk）

**初始化数据库**：
```javascript
// 获取数据库引用
const db = wx.cloud.database()
const _ = db.command // 查询操作符
```

**数据库设计最佳实践（来自官方skill）**：
1. **每个collection都要有TypeScript类型定义**
2. **使用项目前缀命名collection**（如 `logistics_users`）
3. **配置数据库安全规则**
4. **合理使用索引**

**示例：用户表设计**
```javascript
// types/user.ts
type User = {
  _id: string
  openid: string        // 微信openid
  role: 'user' | 'driver'  // 角色区分
  nickname: string
  avatar: string
  phone: string
  createdAt: Date
  updatedAt: Date
}

// collection名称：logistics_users
```

**检查**：
```javascript
if (!hasSkill('no-sql-wx-mp-sdk')) {
  warn("⚠️ 建议安装 no-sql-wx-mp-sdk skill 来正确操作数据库")
  askUser("是否安装腾讯云官方的 no-sql-wx-mp-sdk skill？", {
    options: [
      { label: "安装（推荐）", value: "install" },
      { label: "跳过（使用自定义SDK）", value: "skip" }
    ]
  })
}
```

---

## 🔥 腾讯云官方Skills使用指南

### auth-wechat（微信小程序身份认证）

**核心优势**：
- 微信认证是**自动和无缝的** - 无需复杂的OAuth流程
- 用户身份在云函数中**自动注入**和验证
- `OPENID`、`APPID`、`UNIONID` 都是**经过验证的**，可直接使用

**4个核心场景**：

#### 场景1：初始化CloudBase
```javascript
// app.js
App({
  onLaunch: function () {
    wx.cloud.init({
      env: 'your-env-id',
      traceUser: true
    })
  }
})
```

#### 场景2：获取用户身份
```javascript
// 云函数
const cloud = require('wx-server-sdk')
cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV })

exports.main = async (event, context) => {
  const { OPENID, APPID, UNIONID } = cloud.getWXContext()

  // OPENID - 永远可用
  // UNIONID - 仅在绑定微信开放平台后可用

  return { openid: OPENID, unionid: UNIONID || null }
}
```

#### 场景3：调用云函数
```javascript
// 小程序端
wx.cloud.callFunction({
  name: 'getUserInfo',
  data: {},
  success: res => {
    console.log('用户身份:', res.result)
  }
})
```

#### 场景4：测试认证
```javascript
// 测试云函数
exports.main = async (event, context) => {
  const { OPENID, APPID, UNIONID } = cloud.getWXContext()

  return {
    success: true,
    identity: { openid: OPENID, appid: APPID, unionid: UNIONID }
  }
}
```

---

### no-sql-wx-mp-sdk（数据库操作）

**核心特性**：
- **CRUD操作**：增删改查完整支持
- **复杂查询**：聚合、地理位置、分页
- **性能优化**：索引、字段选择、限制
- **类型安全**：推荐为每个collection定义TypeScript类型

**6个核心文档**：

#### 1. rule.md - 基础规则
```javascript
// 初始化
const db = wx.cloud.database()
const _ = db.command

// 推荐：每个collection都有类型定义
type Todo = {
  title: string
  completed: boolean
  priority: 'high' | 'medium' | 'low'
  createdAt: Date
}

// 推荐：使用项目前缀
db.collection('logistics_todos')
```

#### 2. crud-operations.md - 增删改查
```javascript
// 创建
await db.collection('todos').add({
  title: '学习CloudBase',
  completed: false,
  createdAt: new Date()
})

// 更新
await db.collection('todos').doc('todo-id')
  .update({ completed: true })

// 删除
await db.collection('todos').doc('todo-id').remove()
```

#### 3. complex-queries.md - 复杂查询
```javascript
const _ = db.command

// 多条件查询
await db.collection('todos')
  .where({
    status: _.in(['active', 'pending']),
    priority: 'high',
    createdAt: _.gte(new Date('2025-01-01'))
  })
  .orderBy('createdAt', 'desc')
  .limit(50)
  .get()
```

#### 4. aggregation.md - 聚合查询
```javascript
// 统计分析
await db.collection('orders')
  .aggregate()
  .group({
    _id: '$category',
    totalRevenue: { $sum: '$amount' },
    orderCount: { $sum: 1 }
  })
  .sort({ totalRevenue: -1 })
  .end()
```

#### 5. geolocation.md - 地理位置查询
```javascript
// 附近搜索（LBS）
const _ = db.command
await db.collection('drivers')
  .where({
    location: _.geoNear({
      geometry: new db.Geo.Point(116.404, 39.915),
      maxDistance: 5000  // 5公里内
    })
  })
  .get()
```

**重要**：地理位置查询前必须先创建索引！

#### 6. pagination.md - 分页
```javascript
// 分页查询
const pageSize = 20
const pageNum = 2

await db.collection('todos')
  .orderBy('createdAt', 'desc')
  .skip((pageNum - 1) * pageSize)
  .limit(pageSize)
  .get()
```

**性能优化**：
1. 为查询字段创建索引
2. 只选择需要的字段
3. 合理使用 `.limit()`
4. 优先使用 `.where()` 过滤

---

## 🚀 完整小程序开发流程

### 步骤1：环境准备
```bash
# 1. 注册小程序
# 2. 开通云开发环境
# 3. 初始化OpenSpec
openspec-cn init
```

### 步骤2：技术方案规划
```bash
/opsx:propose 技术方案设计
```
生成：
- proposal.md
- specs/
- design.md
- tasks.md

### 步骤3：云开发配置
```javascript
// app.js
App({
  onLaunch() {
    wx.cloud.init({
      env: 'logistics-prod',
      traceUser: true
    })
  }
})
```

### 步骤4：数据库设计
```javascript
// 使用官方skill的最佳实践

// collection命名：logistics_users
// collection命名：logistics_drivers
// collection命名：logistics_orders
// collection命名：logistics_addresses

// 每个collection都有TypeScript类型定义
```

### 步骤5：开发云函数
```javascript
// 使用官方skill生成代码

// cloudfunctions/login/index.js
const cloud = require('wx-server-sdk')
cloud.init({ env: cloud.DYNAMIC_CURRENT_ENV })

exports.main = async (event, context) => {
  const { OPENID } = cloud.getWXContext()
  const { role } = event  // 'user' | 'driver'

  const db = cloud.database()

  if (role === 'user') {
    let user = await db.collection('logistics_users')
      .where({ openid: OPENID })
      .get()

    if (user.data.length === 0) {
      await db.collection('logistics_users').add({
        data: { openid: OPENID, role: 'user', createdAt: new Date() }
      })
    }
  }

  return { openid: OPENID, role }
}
```

---

## 📦 Skills依赖关系

```
miniprogram-workflow (小程序专用)
├── 继承 general-project-workflow (通用workflow)
│   ├── brainstorming (可选) - 产品定位
│   ├── ui-ux-pro-max (推荐) - UI设计
│   ├── openspec-cn (必须) - 技术方案规划
│   ├── test-driven-development (推荐) - 测试驱动
│   ├── code-review (推荐) - 代码审查
│   └── docs-architect (可选) - 文档生成
│
└── 小程序专用（新增）
    ├── auth-wechat ⭐腾讯云官方（必须）- 微信认证
    ├── no-sql-wx-mp-sdk ⭐腾讯云官方（必须）- 数据库操作
    └── ai-model-wechat ⭐腾讯云官方（可选）- AI模型集成
```

**说明**：
- **⭐必须（小程序专用）**：auth-wechat、no-sql-wx-mp-sdk
- **⭐必须（通用）**：openspec-cn
- **推荐**：ui-ux-pro-max、test-driven-development、code-review

---

## 🎯 与OpenSpec的完美整合

### OpenSpec + 官方Skills = 完美开发体验

```
OpenSpec规划
    ↓
生成 tasks.md
    ↓
按照tasks.md开发
    ↓
使用官方skills生成代码
    ↓
代码符合官方最佳实践
    ↓
高质量、可维护的代码
```

### 示例：订单管理功能

#### OpenSpec阶段
```markdown
## tasks.md
### 任务1.1：创建订单云函数
- [ ] 使用 auth-wechat 获取用户身份
- [ ] 使用 no-sql-wx-mp-sdk 保存订单
- [ ] 添加错误处理
```

#### 开发阶段
```
你：使用官方skills开发订单云函数

AI assistant将：
1. 参考 auth-wechat skill 获取OPENID
2. 参考 no-sql-wx-mp-sdk skill 操作数据库
3. 生成符合官方最佳实践的代码
```

#### 代码审查阶段
```
你：审查订单云函数代码

AI将检查：
- 是否使用了 cloud.getWXContext()
- 是否使用了 cloud.DYNAMIC_CURRENT_ENV
- 是否有类型定义
- 是否有错误处理
```

---

## 📝 小程序开发注意事项

### 技术限制
1. **包体积限制**：主包≤2MB，分包总计≤20MB
2. **API调用限制**：wx.request最大并发10个
3. **用户信息**：需要用户授权才能获取
4. **审核规范**：避免虚拟物品、诱导分享等

### 云开发最佳实践
1. **数据库设计**：
   - 合理使用索引
   - 注意分页查询
   - 避免深层次嵌套

2. **云函数优化**：
   - 控制单次执行时间（<60s）
   - 避免大量数据传输
   - 使用连接池

3. **存储优化**：
   - 图片压缩后再上传
   - 使用CDN加速
   - 定期清理无用文件

### 性能优化
1. **分包加载**：按功能模块分包
2. **懒加载**：非首屏组件延迟加载
3. **缓存策略**：合理使用wx.setStorage
4. **防抖节流**：用户输入需要防抖

### 安全建议
1. **数据验证**：前后端都要验证
2. **敏感信息**：加密存储
3. **权限控制**：使用云开发安全规则
4. **HTTPS**：所有网络请求使用HTTPS

---

## 💡 最佳实践

### 1. 优先使用官方skills

**原因**：
- 官方skills包含8年开发经验
- 代码经过充分测试
- 自动处理边界情况
- 与CloudBase完美集成

**何时使用**：
- 云函数开发：auth-wechat
- 数据库操作：no-sql-wx-mp-sdk
- AI模型集成：ai-model-wechat

### 2. OpenSpec作为总指挥

**原因**：
- 系统化的规划工具
- 生成完整的文档
- 任务清单可跟踪
- 支持迭代更新

**何时使用**：
- 项目启动时生成技术方案
- 需求变更时更新文档
- 定期同步进度

### 3. 测试驱动开发

**原因**：
- 小程序调试成本高
- 先写测试再写代码
- 快速验证逻辑正确性

**何时使用**：
- 核心业务逻辑
- 复杂算法
- 公共函数

### 4. 代码审查

**原因**：
- 确保代码质量
- 符合官方最佳实践
- 团队协作统一风格

**何时使用**：
- 每完成一个模块
- 提交代码前
- 代码合并时

---

## 🔧 项目初始化检查清单

### 1. 小程序环境
- [ ] Node.js已安装
- [ ] 微信开发者工具已安装
- [ ] 小程序已注册（获取AppID）
- [ ] CloudBase环境已开通

### 2. Skills安装检查
```javascript
// 必须安装的skills
const requiredSkills = [
  'openspec-cn',
  'auth-wechat',      // 腾讯云官方
  'no-sql-wx-mp-sdk'  // 腾讯云官方
]

// 推荐安装的skills
const recommendedSkills = [
  'ui-ux-pro-max',
  'test-driven-development',
  'code-review'
]
```

### 3. OpenSpec初始化
```bash
cd project-directory
openspec-cn init
```

### 4. 技术方案规划
```bash
/opsx:propose 技术方案设计
```

---

## 🎓 学习资源

### 官方文档
- [auth-wechat完整文档](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/prompts/auth-wechat)
- [no-sql-wx-mp-sdk完整文档](https://docs.cloudbase.net/ai/cloudbase-ai-toolkit/prompts/no-sql-wx-mp-sdk)
- [腾讯云开发CloudBase](https://docs.cloudbase.net/)

### 项目参考
- OpenSpec-cn：已在项目中创建
- 技术方案文档：docs/project-plan.md
- 任务清单：docs/task-list.md

---

## 🚀 开始新小程序项目

### 快速启动模板

```
你：使用 miniprogram-workflow 开始新项目：XXX小程序

我：好的！开始创建"XXX小程序"项目。

🔍 步骤1：Skills检查
检查小程序开发所需的skills...

✅ 已安装：
- openspec-cn
- auth-wechat ⭐腾讯云官方
- no-sql-wx-mp-sdk ⭐腾讯云官方

❌ 未安装：
- ui-ux-pro-max（推荐）
- test-driven-development（推荐）
- code-review（推荐）

📋 步骤2：小程序信息收集
请告诉我：
1. 小程序名称
2. 是否有明确需求和原型
3. 是否已注册小程序
4. 是否已开通云开发环境

📊 步骤3：初始化OpenSpec
是否初始化OpenSpec并创建技术方案？

收集完信息后，我将引导您完成小程序开发流程。
```

---

## 📝 总结

### 核心优势

1. **官方优先** - 使用腾讯云8年经验沉淀的官方skills
2. **系统规划** - OpenSpec确保不遗漏关键文档
3. **最佳实践** - 代码符合官方规范和质量标准
4. **测试驱动** - 保证代码质量，减少调试时间
5. **知识沉淀** - 每个项目都积累可复用经验

### 与通用workflow的关系

| 特性 | general-project-workflow | miniprogram-workflow |
|------|-------------------------|-------------------|
| **适用范围** | 所有项目 | 仅限小程序 |
| **认证方案** | 多种认证方式 | 腾讯云官方方案 |
| **数据库** | 通用数据库操作 | CloudBase数据库（MongoDB） |
| **最佳实践** | 通用最佳实践 | 腾讯云8年实践 |
| **专属内容** | 通用项目内容 | 小程序特有内容 |

**继承关系**：
- `miniprogram-workflow` 继承 `general-project-workflow` 的所有通用阶段
- 新增小程序专用阶段（云开发环境配置）
- 新增腾讯云官方skills（auth-wechat、no-sql-wx-mp-sdk）

---

**文档版本：** v1.0
**创建日期：** 2026-03-19
**依赖：** general-project-workflow
