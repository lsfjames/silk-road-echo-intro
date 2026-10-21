# 权限矩阵 JWT / RBAC

实现依据：`lib/auth.ts`（JWT httpOnly Cookie）+ 各 `/api/**/route.ts` 内 `requireUser` / 角色判断。

## 角色

| role | 含义 |
|------|------|
| `student_intl` | 来华留学生 |
| `student_cn` | 中国学生 |
| `mentor` | 导师 |
| `admin` | 管理员 |

未登录仅可浏览公开只读接口（列表/详情/统计）。

## 矩阵

| 能力 | 未登录 | 学生 | 导师 | 管理员 |
|------|:------:|:----:|:----:|:------:|
| 浏览首页/IP/内容/活动/Feed | ✅ | ✅ | ✅ | ✅ |
| 注册 / 登录 | ✅ | — | — | — |
| 发起结对 / 接受结对 | ❌ | ✅ | ✅ | ✅ |
| 创建/编辑本人项目 | ❌ | ✅ | ✅ | ✅ |
| 编辑任意项目 | ❌ | ❌ | ✅ | ✅ |
| 视频投稿 upload | ❌ | ✅ | ✅ | ✅ |
| 查看审核队列 pending/approved/rejected | ❌ | ❌* | ✅ | ✅ |
| 通过 / 驳回内容 | ❌ | ❌ | ✅ | ✅ |
| 回填站外指标 | ❌ | ✅ | ✅ | ✅ |
| 核验站外指标 verify | ❌ | ❌ | ✅ | ✅ |
| 调用 AI Studio | ❌ | ✅ | ✅ | ✅ |
| 查看本人 AiRecord 溯源 | ❌ | ✅ | ✅ | ✅ |
| 管理 IP 创建 | ❌ | ✅(登录) | ✅ | ✅ |
| 活动报名 | ❌ | ✅ | ✅ | ✅ |

\* 学生可传 `status=mine` 查看自己的投稿列表。

## 技术要点

1. **传输**：JWT HS256，httpOnly Cookie（`sry_token`），7 天有效  
2. **口令**：bcrypt 哈希，不存明文  
3. **接口**：`requireUser()` 401；角色不足 403（`AuthError`）  
4. **审核**：`isStaff = mentor || admin`  
5. **作品修改**：项目成员或 mentor/admin（`/api/projects` PATCH）

## 演示账号（seed 虚构，密码统一 demo1234）

| 角色 | 邮箱 |
|------|------|
| 留学生 | `aina@sry.demo` `ivan@sry.demo` `gulzat@sry.demo` |
| 中国学生 | `siyuan@sry.demo` `xiaoqing@sry.demo` |
| 导师 | `mentor@sry.demo` |
| 管理员 | `admin@sry.demo` |

> 仅为本地演示数据，非真实个人信息。
