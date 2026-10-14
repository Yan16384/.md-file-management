# Codex Markdown 规则管理

本私人仓库用于管理 Codex 的专项规范，并备份全局 `AGENTS.md`。本机只长期保留 `C:\Users\Yan\.codex\AGENTS.md`；`用户md/` 中的四份 Markdown 是专项规范的权威来源，按任务通过 GitHub API 读取。

## 当前版本

`v1.0.2`，改为本机只保留 `AGENTS.md`，专项规范从私人仓库按需读取。

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

## 权威来源与本机部署

| 文件 | 权威位置 | 本机长期保存 |
|---|---|---|
| `AGENTS.md` | `C:\Users\Yan\.codex\AGENTS.md` | 是 |
| `用户md/*.md` | 本私人仓库 `main` 分支 | 否 |

Codex 只会自动加载本机 `AGENTS.md`。专项规范不会自动加载；任务触发相应路由后，先确认 GitHub CLI 登录有效和仓库为 `PRIVATE`，再通过 `gh api` 读取准确路径的原始内容。读取结果只用于当前任务，不保存为长期本地副本。网络、认证或远端文件异常时停止并报告。

## 同步触发

当用户明确提出“固化经验”“更新 agents”“同步 agents”“同步规范”、引用“## 4. 经验固化与同步”，或要求发布规范备份新版本时，执行完整同步：

1. 在系统临时目录建立仓库工作副本，从远端当前 `main` 开始。
2. 全局经验先修改并验证本机 `AGENTS.md`；专项规范在临时工作副本中修改。
3. 同步 `AGENTS.md`，更新唯一的根目录 `README.md` 和 `SHA256SUMS.txt`。
4. 检查文件集合、差异和凭证明文。
5. 提交并推送 `main`，禁止强推。
6. 递增版本号，创建 GitHub Release，不上传自定义压缩包或其他附件。
7. 回读核验远端提交、文件校验值、tag、Release 和私人属性。
8. 核验成功后删除临时工作副本，不在本机长期保留仓库或 `用户md`。

## 版本规则

- 首次完整备份：`v1.0.0`。
- 常规规则更新或同步：递增补丁号，例如 `v1.0.1`。
- Release 说明记录该版本的实际变化。
- 历史 Release 和备份资产保留，用于审计和恢复。
- 仓库中只保留一个根目录 `README.md`，每次更新原文件。GitHub 在文件列表下方展示其正文，是同一个文件的预览。
- Markdown 直接保存在仓库中。GitHub 自动显示的 Source code (zip/tar.gz) 是平台生成的下载入口，不是额外上传的备份包。

## 校验与恢复

`SHA256SUMS.txt` 记录本机 `AGENTS.md` 备份和全部远端专项规范的 SHA-256。恢复前应先核对校验值，并逐项确认文件名和目标位置。

`.gitattributes` 禁止 Git 自动转换 Markdown 和校验清单的换行符，保证仓库内容与本机权威文件按字节一致。

恢复本机全局规则时，只恢复根目录 `AGENTS.md` 到 `C:\Users\Yan\.codex\AGENTS.md`。专项规范正常情况下不恢复到本机，直接从仓库读取；需要恢复旧版本时检出对应 Release 的 tag 并核对校验清单。`v1.0.0` 的历史 ZIP 附件保留，后续版本不再制作或上传自定义压缩包。

## 安全边界

- 仓库必须保持 `PRIVATE`。
- 不提交 API Key、token、密码、cookie 或其他凭证明文。
- 不提交 `sessions`、`archived_sessions`、缓存、数据库、运行日志或未授权文件。
- 上传前以实际文件清单和 SHA-256 为准，不能只依赖命令无报错。
