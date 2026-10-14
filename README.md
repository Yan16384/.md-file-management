# Codex Markdown 规则备份

本私人仓库用于备份 Codex 的全局 `AGENTS.md` 和 `用户md` 中的专项规范。仓库只保存经过授权的 Markdown 规则文件、校验清单和版本说明，不保存会话、缓存、配置数据库或凭证。

## 当前版本

`v1.0.0`，首次完整备份。

## 仓库结构

```text
.
├── .gitattributes
├── AGENTS.md
├── README.md
├── SHA256SUMS.txt
└── 用户md/
    ├── 毕业论文格式.md
    ├── 去ai规范.md
    ├── 四六级翻译规范.md
    └── visio格式.md
```

## 权威来源

| 仓库文件 | 本机权威文件 |
|---|---|
| `AGENTS.md` | `C:\Users\Yan\.codex\AGENTS.md` |
| `用户md/*.md` | `C:\Users\Yan\.codex\用户md\` 直属目录中的 Markdown 文件 |

本仓库是备份和版本历史，日常修改应先作用于本机权威文件，再同步到仓库。

## 同步触发

当用户明确提出“固化经验”“更新 agents”“同步 agents”“同步规范”、引用“## 4. 经验固化与同步”，或要求发布规范备份新版本时，执行完整同步：

1. 更新并验证本机权威文件。
2. 将 `AGENTS.md` 和 `用户md` 直属目录中的全部 `.md` 文件同步到本仓库。
3. 更新 `README.md` 和 `SHA256SUMS.txt`。
4. 检查文件集合、差异和凭证明文。
5. 提交并推送 `main`，禁止强推。
6. 递增版本号，创建 GitHub Release，并上传完整 ZIP 备份。
7. 回读核验远端提交、文件校验值、tag、Release 和 ZIP 资产。

## 版本规则

- 首次完整备份：`v1.0.0`。
- 常规规则更新或同步：递增补丁号，例如 `v1.0.1`。
- Release 说明记录该版本的实际变化。
- 历史 Release 和备份资产保留，用于审计和恢复。

## 校验与恢复

`SHA256SUMS.txt` 记录所有权威备份文件的 SHA-256。恢复前应先核对校验值，再将文件复制回对应权威路径；覆盖前保留当前文件，并逐项确认文件名和目标位置。

`.gitattributes` 禁止 Git 自动转换 Markdown 和校验清单的换行符，保证仓库内容与本机权威文件按字节一致。

从 GitHub 恢复时，优先下载最新 Release 中与版本号一致的 ZIP 资产。需要恢复旧状态时，选择对应历史 Release，不要仅凭文件修改时间判断版本。

## 安全边界

- 仓库必须保持 `PRIVATE`。
- 不提交 API Key、token、密码、cookie 或其他凭证明文。
- 不提交 `sessions`、`archived_sessions`、缓存、数据库、运行日志或未授权文件。
- 上传前以实际文件清单和 SHA-256 为准，不能只依赖命令无报错。
