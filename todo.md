# AgentSkill协议支持 - 开发任务清单

## 项目概述
为Agent平台新增AgentSkill协议支持，遵循agentskills.io开放标准。

---

## Phase 1: 基础设施搭建 (第1周)

### 1.1 数据库设计
- [ ] 创建AgentConsole数据库DDL脚本
  - [ ] `ac_skill` Skill主表
  - [ ] `ac_skill_file` Skill文件表
  - [ ] `ac_agent_skill_binding` Agent-Skill绑定表
- [ ] 创建AgentMgmt数据库DDL脚本
  - [ ] `am_skill_review` Skill审核主表
  - [ ] `am_skill_review_log` 审核日志表
- [ ] 创建SkillStore数据库DDL脚本
  - [ ] `ss_skill` Skill商店主表
  - [ ] `ss_skill_download_log` Skill下载记录表

### 1.2 对象存储集成
- [ ] 集成OSS/MinIO SDK
- [ ] 实现文件上传接口
- [ ] 实现签名URL生成
- [ ] 配置存储桶策略

---

## Phase 2: AgentConsole Skill管理 (第2-3周)

### 2.1 Skill上传功能
- [ ] 实现文件上传接口 `POST /api/v1/console/skills`
- [ ] 实现SKILL.md解析器
- [ ] 实现协议规范校验
  - [ ] name字段格式校验
  - [ ] description必填校验
  - [ ] 目录结构校验
- [ ] 实现安全扫描
  - [ ] 敏感信息检测
  - [ ] 恶意代码扫描
  - [ ] 路径遍历检测

### 2.2 Skill管理功能
- [ ] 获取Skill列表 `GET /api/v1/console/skills`
- [ ] 获取Skill详情 `GET /api/v1/console/skills/{skillId}`
- [ ] 更新Skill元数据 `PUT /api/v1/console/skills/{skillId}`
- [ ] 删除Skill `DELETE /api/v1/console/skills/{skillId}`
- [ ] 提交审核 `POST /api/v1/console/skills/{skillId}/submit`

### 2.3 Agent-Skill绑定
- [ ] 获取Agent绑定的Skill列表 `GET /api/v1/console/agents/{agentId}/skills`
- [ ] 绑定Skill到Agent `POST /api/v1/console/agents/{agentId}/skills`
- [ ] 更新绑定配置 `PUT /api/v1/console/agents/{agentId}/skills/{skillId}`
- [ ] 解绑Skill `DELETE /api/v1/console/agents/{agentId}/skills/{skillId}`

---

## Phase 3: 服务间同步机制 (第4周)

### 3.1 AgentConsole -> SkillStore同步
- [ ] 实现导入后同步逻辑
- [ ] 调用 `POST /api/v1/store/internal/skills/sync`
- [ ] 实现同步状态追踪
- [ ] 实现重试机制

### 3.2 AgentConsole -> AgentMgmt同步
- [ ] 实现提交审核时同步逻辑
- [ ] 调用 `POST /api/v1/mgmt/internal/skills/sync`
- [ ] 实现同步状态追踪

### 3.3 回调接口
- [ ] 实现同步状态回调 `POST /api/v1/console/internal/skills/{skillId}/sync-callback`
- [ ] 处理AgentMgmt审核结果回调

---

## Phase 4: AgentMgmt审核功能 (第5周)

### 4.1 审核管理接口
- [ ] 获取待审核列表 `GET /api/v1/mgmt/skills/pending`
- [ ] 获取审核详情 `GET /api/v1/mgmt/skills/{skillId}`
- [ ] 审核通过 `POST /api/v1/mgmt/skills/{skillId}/approve`
- [ ] 审核拒绝 `POST /api/v1/mgmt/skills/{skillId}/reject`
- [ ] 下架Skill `POST /api/v1/mgmt/skills/{skillId}/takedown`
- [ ] 获取审核历史 `GET /api/v1/mgmt/skills/{skillId}/logs`

### 4.2 内部同步接口
- [ ] 接收Skill审核数据 `POST /api/v1/mgmt/internal/skills/sync`
- [ ] 审核通过后更新SkillStore状态
- [ ] 回调通知AgentConsole

---

## Phase 5: SkillStore商店功能 (第6周)

### 5.1 公开接口
- [ ] 浏览公有Skill列表 `GET /api/v1/store/skills`
- [ ] 获取Skill详情 `GET /api/v1/store/skills/{skillId}`
- [ ] 搜索Skill `GET /api/v1/store/skills/search`
- [ ] 获取下载URL `GET /api/v1/store/skills/{skillId}/download-url`

### 5.2 内部接口
- [ ] 接收Skill同步数据 `POST /api/v1/store/internal/skills/sync`
- [ ] 更新Skill状态 `PUT /api/v1/store/internal/skills/{skillId}/status`
- [ ] 批量查询Skill `POST /api/v1/store/internal/skills/query-by-ids`

---

## Phase 6: Runtime集成 (第7周)

### 6.1 Runtime获取Skill流程
- [ ] Runtime从AgentConsole获取绑定关系
- [ ] Runtime从SkillStore批量查询Skill信息
- [ ] Runtime获取签名下载URL
- [ ] Runtime下载Skill包

### 6.2 Skill激活与执行
- [ ] 实现Skill加载机制
- [ ] 实现Skill匹配逻辑
- [ ] 实现Skill激活流程
- [ ] 实现Skill执行框架

---

## Phase 7: 集成测试与联调 (第8周)

### 7.1 单元测试
- [ ] AgentConsole服务单元测试
- [ ] AgentMgmt服务单元测试
- [ ] SkillStore服务单元测试

### 7.2 集成测试
- [ ] Skill上传->同步->审核->发布全流程测试
- [ ] Agent绑定->Runtime获取->下载全流程测试
- [ ] 错误处理和边界条件测试

### 7.3 性能测试
- [ ] 并发上传测试
- [ ] 批量查询性能测试
- [ ] 下载URL生成性能测试

---

## 交付物清单

| 阶段 | 交付物 |
|------|--------|
| Phase 1 | DDL脚本、存储服务SDK |
| Phase 2 | 上传接口、协议解析器 |
| Phase 3 | 同步接口、回调接口 |
| Phase 4 | 审核接口、审核日志 |
| Phase 5 | 浏览、搜索、下载接口 |
| Phase 6 | 绑定接口、绑定管理 |
| Phase 7 | 测试报告、问题修复 |

---

## 关键里程碑

- [ ] **M1**: 数据库和存储基础设施就绪 (第1周末)
- [ ] **M2**: AgentConsole Skill上传功能可用 (第3周末)
- [ ] **M3**: 服务间同步机制打通 (第4周末)
- [ ] **M4**: 审核流程端到端可用 (第5周末)
- [ ] **M5**: SkillStore商店功能上线 (第6周末)
- [ ] **M6**: Runtime可获取并执行Skill (第7周末)
- [ ] **M7**: 全功能集成测试通过 (第8周末)