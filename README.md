# 网易七鱼(Qiyu)开发助手 Skill

帮助开发者快速完成网易七鱼客服平台的**系统开发、联调测试与上线验证**。

本 Skill 融合两本官方手册的内容：

| 来源 | 说明 |
|------|------|
| **手册A：官方开发者文档中心** | https://b.163.com/docs/qiyu — 现行完整文档，含服务端API、各端SDK、合规指南 |
| **手册B：云商七鱼智能客服开发指南知识库** | https://b.163.com/knowledge/public/WXjbs9n3GC/knowdetail?docId=X9loH09Ibg&pid=431609 — 含各渠道详细接入步骤、服务端登录接口、接口频率限制 |

## 能力覆盖

- **服务端 API**：4 种签名校验方式、checksum 计算（SHA1/SHA256）、服务端登录免登接口、接口限流规则、完整错误码
- **客户端 SDK**：Web/H5（npm + 脚本接入、`ysf('config')` 全参数表、企业微信 AES 加密）、iOS、Android、鸿蒙、Uniapp、微信小程序
- **业务系统**：CRM 对接（轻量+接口）、在线消息（消息类型 msgType）、工单系统（20+ API、回调 eventType）、呼叫系统 IPCC（13 种 eventtype）、短信系统
- **三方渠道**：抖音、百度营销、小红书、Facebook、Line、Discord、WhatsApp、Twitter、Instagram、钉钉、飞书 — 全部含**详细授权/绑定步骤**
- **知识库 API**：外部开放接口 + Agent Tool 接口
- **合规指南**：Android/iOS/鸿蒙权限配置、隐私政策披露模板、可选个人信息配置
- **测试验证**：12 大类开发测试验证清单、常见问题排查、快速开发流程

## 目录结构

```
qiyu-dev-assistant/
├── SKILL.md                        # Skill 主文件（YAML frontmatter + 全部内容）
├── README.md                       # 本说明
├── LICENSE                         # MIT 许可证
└── references/
    └── qiyu-knowledge-manual.md    # 手册B知识库原文（20个文档全文，供溯源）
```

## 安装到 DSH

将 `qiyu-dev-assistant` 目录放入任一 skill 扫描根目录：

| 位置 | 路径 | 说明 |
|------|------|------|
| 用户级（推荐） | `~/.dsh/skills/qiyu-dev-assistant/` | 全局可用 |
| 项目级 | `<projectRoot>/.dsh/skills/qiyu-dev-assistant/` | 仅该项目可用 |
| 自定义 | 在 DSH 配置 `customSkillDirs` 中指向该目录 | 任意位置 |

或直接：
```bash
git clone <本仓库地址> ~/.dsh/skills/qiyu-dev-assistant
```

## 使用方式

在 DSH 会话中，当任务涉及七鱼开发时，模型会通过 skill 目录发现并加载本 Skill，然后按「平台概览 → 选择接入方式 → 对应 SDK/API 章节 → 开发测试清单 → 合规检查」的顺序辅助完成开发。

也可手动要求：*"使用 qiyu-dev-assistant skill 帮我接入七鱼 Web SDK"* 等。

## 文档来源

- 手册A：https://b.163.com/docs/qiyu （七鱼官方开发者文档中心）
- 手册B：https://b.163.com/knowledge/public/WXjbs9n3GC/knowdetail?docId=X9loH09Ibg&pid=431609 （网易云商七鱼智能客服开发指南知识库）

> 注：手册B 标注为【已废弃】，部分内容（如渠道接入步骤、服务端登录接口、限流规则）仍具参考价值，已在 SKILL.md 中与手册A穿插融合。

## 许可证

[MIT](LICENSE)
