# OpenClaw Extensions

本文档记录已安装的 OpenClaw 扩展详情，用于备份恢复时参考。

---

## Feishu (飞书)

**包名:** `@m1heng-clawd/feishu`  
**版本:** `0.1.7`  
**来源:** https://github.com/m1heng/clawdbot-feishu.git

### 安装方式

```bash
# 通过 npm 安装
npm install @m1heng-clawd/feishu

# 或通过 OpenClaw CLI
openclaw extension add @m1heng-clawd/feishu
```

### 功能

飞书/Lark 企业消息平台集成，提供：
- Feishu 消息收发
- 飞书文档操作
- 云空间管理
- 知识库访问
- 权限管理

### 包含技能

| 技能 | 路径 | 功能 |
|------|------|------|
| feishu-doc | `skills/feishu-doc/` | 飞书文档读写操作 |
| feishu-drive | `skills/feishu-drive/` | 云存储文件管理 |
| feishu-perm | `skills/feishu-perm/` | 文档权限管理 |
| feishu-wiki | `skills/feishu-wiki/` | 知识库导航 |

### 配置

在主配置文件 `openclaw.json` 中添加：

```json
{
  "feishu": {
    "appId": "cli_xxxxx",
    "appSecret": "xxxxx",
    "encryptKey": "xxxxx"
  }
}
```

### 依赖

- `@larksuiteoapi/node-sdk`: ^1.30.0
- `@sinclair/typebox`: ^0.34.48
- `zod`: ^4.3.6

### 更新时间

2026-02-09 安装
