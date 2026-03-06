# 🎬 电影预订系统 (Movie Reservation System)

> 💼 **面试项目作品** | 全栈开发 | Go + Vue3 + PostgreSQL + Redis

一个基于 **Go + Vue3** 的全栈电影票预订平台，采用分层架构设计，实现了完整的影院业务场景。

本项目重点展示后端代码和架构能力, 前端部分主要使用ai完成

![Go](https://img.shields.io/badge/Go-1.24+-00ADD8?style=flat&logo=go)
![Vue](https://img.shields.io/badge/Vue-3.5+-4FC08D?style=flat&logo=vue.js)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-14+-4169E1?style=flat&logo=postgresql)
![Redis](https://img.shields.io/badge/Redis-7+-DC382D?style=flat&logo=redis)

---

## 🎯 项目亮点

### 架构设计
- ✅ **标准分层架构**: Handler → Service → Repository，职责清晰，易于测试和维护
- ✅ **依赖注入**: 通过 `app.App` 统一管理核心依赖（DB、Cache、Logger）
- ✅ **中间件设计**: 统一的认证、日志、错误处理中间件
- ✅ **DTO 模式**: 规范的数据传输对象设计

### 技术深度
- ✅ **JWT 无状态认证**: 基于 Token 的身份验证，支持角色权限控制
- ✅ **Redis 缓存**: 验证码存储、热点数据缓存
- ✅ **GORM ORM**: 优雅的数据访问层设计
- ✅ **HTTPS 加密**: 支持 TLS 安全传输
- ✅ **Zap 日志**: 高性能结构化日志

---

## ✨ 核心功能

| 模块 | 功能 | 技术点 |
|------|------|--------|
| **认证模块** | 注册/登录/验证码 | JWT、bcrypt 密码加密、图形验证码 |
| **电影模块** | 电影 CRUD、场次关联 | 一对多关系、级联查询 |
| **场次模块** | 场次管理、座位状态 | 时间处理、并发控制 |
| **预订模块** | 选座、下单、取消 | 事务处理、唯一约束防重 |
| **影厅模块** | 影厅 CRUD、座位生成 | 批量插入、座位生成 |

---

## 🏗️ 技术架构

```
┌─────────────────────────────────────────────────────────────┐
│                         Frontend                             │
│  Vue3 + Vite + Pinia + Element Plus + Vue Router            │
└─────────────────────────────────────────────────────────────┘
                              │
                              │ HTTPS / JSON
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                         Backend                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Handler Layer (HTTP 请求处理)                        │   │
│  │  - AuthHandler / MovieHandler / ReservationHandler   │   │
│  └──────────────────────────────────────────────────────┘   │
│                              │                                │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Service Layer (业务逻辑)                             │   │
│  │  - AuthService / MovieService / ReservationService   │   │
│  └──────────────────────────────────────────────────────┘   │
│                              │                                │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Repository Layer (数据访问)                          │   │
│  │  - UserRepo / MovieRepo / ReservationRepo            │   │
│  └──────────────────────────────────────────────────────┘   │
│                              │                                │
│  ┌──────────────────────────────────────────────────────┐   │
│  │  Middleware (认证/日志/错误处理)                      │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
              ┌───────────────┴───────────────┐
              ▼                               ▼
┌─────────────────────┐           ┌─────────────────────┐
│    PostgreSQL       │           │       Redis         │
│  (主数据存储)        │           │  (缓存/验证码)      │
└─────────────────────┘           └─────────────────────┘
```

---

## 📦 项目结构

```
movie-reservation/
├── cmd/
│   └── api/
│       └── main.go              # 应用入口、依赖初始化
├── config/
│   └── config.go                # 配置加载（环境变量）
├── frontend/                    # Vue3 前端项目
│   └── src/
│       ├── api/                 # API 封装
│       ├── router/              # 路由配置（含权限守卫）
│       ├── stores/              # Pinia 状态管理
│       └── views/               # 页面组件
├── internal/                    # 后端核心代码
│   ├── app/
│   │   └── app.go               # 应用容器（依赖注入）
│   ├── cache/
│   │   └── redis.go             # Redis 客户端封装
│   ├── dto/                     # 数据传输对象
│   ├── handler/                 # HTTP 处理器层
│   ├── middleware/              # 中间件（Auth/Logger）
│   ├── model/                   # GORM 模型定义
│   ├── repository/              # 数据访问层
│   ├── security/                # 安全模块（JWT/密码）
│   ├── service/                 # 业务逻辑层
│   └── util/                    # 工具函数
├── interfaces/
│   └── web/
│       └── router.go            # Gin 路由配置
├── Makefile                     # 构建脚本
└── go.mod / package.json        # 依赖管理
```

---

## 💻 核心代码展示

### 分层架构示例（Service 层）

```go
// internal/service/reservation_service.go
type ReservationService struct {
    db    *gorm.DB
    cache *cache.RedisCache
}

func (s *ReservationService) CreateReservation(
    ctx context.Context, 
    userID uint, 
    showtimeID, seatID uint,
) (*model.Reservation, error) {
    // 1. 检查场次是否存在
    // 2. 检查座位是否可用
    // 3. 开启事务
    // 4. 创建预订记录
    // 5. 更新座位状态
    // 6. 提交事务
}
```

### JWT 认证中间件

```go
// internal/middleware/auth.go
func RequireAuth() gin.HandlerFunc {
    return func(c *gin.Context) {
        token := c.GetHeader("Authorization")
        claims, err := security.ParseToken(token)
        if err != nil {
            c.AbortWithStatusJSON(401, gin.H{"error": "unauthorized"})
            return
        }
        c.Set("userID", claims.UserID)
        c.Next()
    }
}
```

### 数据库模型设计

```go
// internal/model/model.go
type Reservation struct {
    ID         uint `gorm:"primaryKey"`
    ShowtimeID uint `gorm:"not null;index;uniqueIndex:idx_unique_ticket"`
    SeatID     uint `gorm:"not null;index;uniqueIndex:idx_unique_ticket"`
    UserID     uint `gorm:"not null;index"`
    // 唯一约束防止重复预订同一场次的同一座位
}
```

---

## 🚀 快速开始

### 环境要求

```
Go 1.24+      | Node.js 20+      | PostgreSQL 14+      | Redis 7+
```

### 安装步骤

```bash
# 1. 克隆项目
git clone https://github.com/qs-lzh/movie-reservation.git
cd movie-reservation

# 2. 配置环境变量
cp .env.example .env
# 编辑 .env 配置数据库连接等

# 3. 启动后端
go mod download
make backend

# 4. 启动前端（新终端）
cd frontend && npm install && npm run dev
```

### 访问应用

- 前端：http://localhost:5173
- 后端 API: https://localhost:8080

---

## 📖 API 接口

| 模块 | 接口 | 方法 | 权限 |
|------|------|------|------|
| 认证 | `/users/register` | POST | 公开 |
| 认证 | `/users/login` | POST | 公开 |
| 电影 | `/movies` | GET | 公开 |
| 电影 | `/movies` | POST | 管理员 |
| 场次 | `/showtimes/:id/availability` | GET | 公开 |
| 预订 | `/reservations` | POST | 登录用户 |
| 预订 | `/reservations/me` | GET | 登录用户 |

完整 API 文档见 [API 文档](./docs/api.md)

---

## 📝 技术栈详解

### 后端
| 技术 | 用途 | 选型理由 |
|------|------|----------|
| Gin | Web 框架 | 高性能、中间件生态丰富 |
| GORM | ORM | 开发效率高、支持复杂查询 |
| PostgreSQL | 数据库 | ACID 保证、约束丰富 |
| Redis | 缓存 | 高性能、支持多种数据结构 |
| Zap | 日志 | 高性能结构化日志 |
| JWT | 认证 | 无状态、易于扩展 |

### 前端
| 技术 | 用途 | 选型理由 |
|------|------|----------|
| Vue3 | 框架 | 组合式 API、性能好 |
| Pinia | 状态管理 | Vue3 官方推荐、TypeScript 友好 |
| Element Plus | UI 库 | 组件丰富、文档完善 |
| Axios | HTTP | 拦截器、取消请求等特性 |

---

## 🔮 后续优化方向

- [ ] 接入支付接口（支付宝/微信）
- [ ] 添加退票/退款流程
- [ ] 实现电影评价系统
- [ ] 座位锁定机制（防止超卖）
- [ ] 邮件/短信通知
- [ ] 数据统计看板（上座率、票房）
- [ ] 单元测试覆盖

---

## 📧 联系方式

- **GitHub**: [@qs-lzh](https://github.com/qs-lzh)
- **Email**: [your-email@example.com]

---

> 💡 **本项目为个人学习作品，用于展示全栈开发能力。欢迎面试官查阅！**
