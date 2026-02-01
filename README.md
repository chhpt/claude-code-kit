# Claude Code Kit

## 命令

### ask - 需求分析和架构设计

这个命令的作用是把模糊的业务需求转化为技术文档。不是简单的问答，而是系统性的架构分析。

- 使用方法：`@ask.md <技术问题>`
- 功能：提供高层次的架构咨询和设计建议
- 适用场景：系统设计、技术选型、架构优化等
- 参考文件：`.claude/commands/ask.md`

#### 实际使用场景：

```
/ask 设计一个支持千万级用户的电商平台的微服务架构
```

输出会包含：

- 系统边界定义
- 技术栈选择理由
- 非功能性需求分析
- 潜在风险点识别

关键在于它会强制你思考那些平时容易忽略的问题，比如数据一致性、服务间通信、故障恢复等。输出的文档直接保存到 docs 目录，后续所有开发工作都以此为准。

### code - 从文档到代码实现

有了需求文档，/code 命令会基于文档内容生成具体的代码实现。不是那种简单的代码片段，而是完整的、可运行的代码。

- 使用方法：`@code.md <功能描述>`
- 功能：提供完整的实现计划、代码、集成指南和测试策略
- 适用场景：新功能开发、模块实现、代码重构等
- 参考文件：`.claude/commands/code.md`

#### 实际使用场景：

```
/code @/docs/points_system.md 基于技术方案文档生成代码
```

请一定要开启 Plan 模式

#### 工作机制：

- 读取 docs 目录下的需求文档
- 分析现有代码库结构
- 生成符合项目规范的代码
- 确保与现有系统的兼容性

这里有个细节很重要：它不会凭空生成代码，而是基于你的项目上下文。比如你用的是 Node.js，它就会生成 Express 风格的代码。

### test - 测试用例生成

测试驱动开发（TDD）说了这么多年，真正执行的团队不多。主要原因是写测试用例太费时间，而且很多开发者不知道该测什么。

/test 命令解决的就是这个问题：

- 基于需求文档自动生成测试用例
- 覆盖单元测试、集成测试、边界条件测试
- 生成可执行的测试代码，不是伪代码

- 使用方法：`@test.md <组件或功能>`
- 功能：提供全面的测试策略、测试代码和覆盖率分析
- 适用场景：新功能测试、代码重构验证、回归测试等
- 参考文件：`.claude/commands/test.md`

#### 实际使用场景

```
/test @/docs/points_system.md 基于技术方案文档生成单元测试
```

如果你写了一个用户认证模块，它会自动生成：

- 正常登录流程测试
- 密码错误测试
- 账号锁定测
- 并发登录测试
- SQL 注入防护测试

### review - 文档与代码一致性检查

这是整个流程中最关键的一环。很多项目的问题就在于代码和文档不一致，时间长了就没人知道系统到底是怎么设计的。

#### 实际使用场景

```
/review @/docs/points_system.md 基于技术方案文档检查代码是否符合 列出不符合内容以及二次优化方案
```

/review 命令会：

- 对比需求文档和实际代码
- 检查代码质量和安全问题
- 验证性能和可扩展性
- 识别架构偏离

如果发现问题，会明确指出哪里不符合预期，以及具体的修改建议。

### optimize - 性能优化指导

#### 实际使用场景

```
/optimize 用户认证API性能优化
```

/optimize 主要处理性能问题：

- 算法复杂度优化
- 资源使用优化
- 并发处理优化
- 缓存策略调整

### refactor - 代码重构指导

#### 实际使用场景

```
/refactor @/docs/points_system.md 基于技术方案文档优化/重构代码
```

/refactor 主要处理代码结构问题：

- 分析现有代码结构和设计模式
- 识别代码重复和复杂度高的部分
- 提供重构建议和具体实现方案
- 确保重构后代码质量和性能不下降

## 工作流

完整开发工作流

```
# 1. Architecture consultation
@ask.md How to design a microservices architecture for an e-commerce platform handling 10M+ users

# 2. Implement new feature
@code.md Implement user authentication system with login, registration, and password reset

# 3. Code review
@review.md user authentication module

# 4. Generate tests
@test.md user authentication functionality

# 5. Performance optimization
@optimize.md login API response time

# 6. Deployment check
@deploy-check.md production environment
```

问题修复流程

```
# 1. Debug analysis
@debug.md User login returns intermittent 500 errors

# 2. Implement fix
@code.md Fix login service concurrency issues

# 3. Test verification
@test.md login concurrent scenarios

# 4. Deployment preparation
@deploy-check.md hotfix branch
```

架构优化流程

```
# 1. Architecture guidance
@ask.md Should we use event sourcing or traditional CRUD for our order management system

# 2. System design consultation
@ask.md How to handle data consistency across microservices in a distributed transaction

# 3. Technology strategy
@ask.md Comparing GraphQL vs REST API for our mobile-first application

# 4. Implementation planning
@code.md Implement API gateway pattern with rate limiting and circuit breaker
```

重构工作流

```
# 1. Refactoring analysis
@refactor.md user service module with high complexity

# 2. Code review
@review.md refactored user service

# 3. Performance validation
@optimize.md refactored service performance

# 4. Test supplementation
@test.md refactored user service
```

## 架构师（Architect）：

你是架构师智能体。你的职责是分析需求，研究技术方案，规划系统架构，并将任务分解到 MULTI_AGENT_PLAN.md 文件中。你需要确保任务分解清晰、依赖关系明确、优先级合理。在做出重大架构决策时，记录你的理由。

## 构建师（Builder）：

你是构建者智能体。你的职责是根据 MULTI_AGENT_PLAN.md 中分配给你的任务，编写高质量的代码。完成任务后，更新计划文件中的状态。如果遇到架构层面的问题，在计划文件中 @architect 提问。

## 验证者（Validator）：

你是验证者智能体。你的职责是为已实现的功能编写测试，运行测试套件，报告问题，并协助调试。你需要覆盖正常路径和边缘情况。发现 bug 时，在计划文件中详细记录。

## 记录员（Scribe）：

你是记录员智能体。你的职责是为已完成的功能撰写清晰的文档，包括 API 文档、使用指南和代码注释。你还可以对代码进行可读性优化。
