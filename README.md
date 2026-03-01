# XiaoYi Skill Management System

AgentSkill 协议支持系统 - 为 Agent 平台提供 Skill 的上传、审核、发布和管理能力。

## 项目概述

本项目遵循 [agentskills.io](https://agentskills.io) 开放标准，为类似 coze.cn 的低代码大模型应用平台新增 AgentSkill 协议支持。

### 核心功能

- **Skill 上传管理** - 开发者可上传、管理和版本控制 Skill
- **审核发布流程** - 完整的 Skill 审核机制，确保内容质量和安全
- **Skill 商店生态** - 提供 Skill 浏览、搜索和下载能力
- **Agent-Skill 绑定** - 灵活的 Agent 与 Skill 关联机制

## 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                        开发者侧                              │
│  ┌──────────────────┐                                       │
│  │  AgentConsole   │  ← 开发者 IDE，上传/管理 Skill         │
│  └────────┬─────────┘                                       │
└───────────┼─────────────────────────────────────────────────┘
            │
            ▼
┌─────────────────────────────────────────────────────────────┐
│                        管理侧                                │
│  ┌──────────────────┐      ┌──────────────────┐            │
│  │   HAGManage      │ ←──→ │   SkillStore     │            │
│  │  (审核后台)       │      │  (Skill 商店)     │            │
│  └──────────────────┘      └────────┬─────────┘            │
└─────────────────────────────────────┼───────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────┐
│                        运行时                                │
│  ┌──────────────────┐                                       │
│  │ AgentController   │  ← 获取并执行 Skill                  │
│  └──────────────────┘                                       │
└─────────────────────────────────────────────────────────────┘
```

## 微服务组件

| 服务 | 职责 |
|------|------|
| **AgentConsole** | 开发者工作台，Skill 上传、管理、Agent 绑定 |
| **HAGManage** | 管理后台，Skill 审核、发布、下架 |
| **SkillStore** | Skill 商店，公开浏览、搜索、下载 |

## 目录结构

```
.
├── architecture-design.md       # 架构设计文档
├── openapi-agent-console.yaml   # AgentConsole API 规范
├── openapi-agent-mgmt.yaml      # AgentMgmt API 规范
├── openapi-skill-store.yaml     # SkillStore API 规范
├── todo.md                      # 开发任务清单
└── README.md                    # 项目说明
```

## API 规范

项目采用 OpenAPI 3.0 规范定义接口：

- `openapi-agent-console.yaml` - 开发者侧 API（Skill 管理、Agent 绑定）
- `openapi-agent-mgmt.yaml` - 管理侧 API（审核流程）
- `openapi-skill-store.yaml` - 商店公开 API（浏览、搜索、下载）

## 开发进度

详见 [todo.md](./todo.md)，项目分为 7 个阶段，预计 8 周完成：

1. **Phase 1**: 基础设施搭建
2. **Phase 2**: AgentConsole Skill 管理
3. **Phase 3**: 服务间同步机制
4. **Phase 4**: AgentMgmt 审核功能
5. **Phase 5**: SkillStore 商店功能
6. **Phase 6**: Runtime 集成
7. **Phase 7**: 集成测试与联调

## 技术栈

- **后端框架**: Spring Boot
- **数据库**: MySQL
- **缓存**: Redis
- **对象存储**: OBS (兼容 S3)
- **API 规范**: OpenAPI 3.0

## 相关资源

- [agentskills.io](https://agentskills.io) - AgentSkill 开放标准
- [架构设计文档](./architecture-design.md) - 详细系统设计

## License

MIT License