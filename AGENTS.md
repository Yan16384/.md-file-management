# AGENTS.md — Codex 全局规则与软件经验

> 本文件位于 `C:\Users\Yan\.codex\AGENTS.md`，供 Codex 自动读取。只保留跨任务硬规则、精确的规范调用入口与已验证的软件操作经验。按任务最小读取，不为简单任务做全库扫描。

## 目录

- [1. 交互与文件安全](#1-交互与文件安全)
- [2. 任务规范路由](#2-任务规范路由精确读取不遍历目录)
- [3. 已验证的软件操作经验](#3-已验证的软件操作经验)
  - [3.1 Origin](#31-origin)
  - [3.2 Fluent-PyFluent](#32-fluent-pyfluent)
  - [3.3 Android-WebView-MediaStore](#33-android-webview-mediastore)
  - [3.4 Office](#34-office)
  - [3.5 Visio](#35-visio)
- [4. 经验固化与同步](#4-经验固化与同步)
- [5. 配置与权限边界](#5-配置与权限边界)

## 1. 交互与文件安全

1. 回答先给结论；一般任务简洁处理，必要时再说明依据。不要编造已执行操作、已验证结果或文件路径。
2. 只操作用户指定的文件与路径；找不到时停止并询问，不向其他目录漫游搜索。文件名称以磁盘实际存在的名称为准，未经授权不重命名或移动。
3. 普通操作产物默认保存到 `D:\desk\codex操作文件\`；修改用户明确指定的权威 `AGENTS.md` / 规范文件时，只有得到明确更新授权才能原位修改。报告实际输出位置。
4. 不在任何 MD 中保存 API Key、token、密码或 cookie 明文；只保存必要的凭证位置与恢复方法。
5. `C:\Users\Yan\.codex\用户md\` 中的文件不得未经授权删除或移出。不要为定位规则而扫描此目录；只读取下一节列出的**准确文件名**，或用户显式指定的文件。
6. 不自动清理旧文件、`.bak`、会话、配置、数据库和用户资料；确需删除前明确确认对象及范围。不在 `D:\desk` 桌面上创建杂乱备份目录。运行时文件与目录用途可能随 Codex 更新，应先核实，不依靠旧清理白名单批量删除。
7. 仅修改格式不得改动事实、数据或正文；软件写操作执行后核查实际结果，而不只看命令是否报错。简单修改只检查受影响部分，交付完整 Word 时执行完整成品验收。

## 2. 任务规范路由（精确读取，不遍历目录）

除下表明确的任务外，不主动读取 `用户md` 的其他文件。只读取与本次任务直接相关的规范；用户明确指定某文件时优先遵从。所有文件均是普通 Markdown，**不会因为名字中有 agents 就被 Codex 自动读取**。

| 任务 | 按需读取的文件 | 触发边界 |
|---|---|---|
| 论文、技术报告、政策报告写作；去 AI；完整 Word 成品验收 | `C:\Users\Yan\.codex\用户md\去ai规范.md` | 撰写/修订正式成果时读取；普通问答、微小文本操作不触发全套验收 |
| 内蒙古大学专业学位硕士论文排版 | `C:\Users\Yan\.codex\用户md\毕业论文格式.md` | **只在相应学位论文或用户明确指定学校模板时**读取；一般 Word、技术指南和政策报告不套用学校封面及论文专用页码 |
| Visio 绘制、修改流程图或框架图 | `C:\Users\Yan\.codex\用户md\visio格式.md` | 需要生成/修改 Visio 图时读取；仅查看文件时不触发完整绘图检查 |
| 中译英训练、逐词翻译或单词学习 | `C:\Users\Yan\.codex\用户md\四六级翻译规范.md` | 仅在明确处于翻译/英语学习任务时读取；普通中文问题不自动翻译 |

文件名如与实际磁盘不符，不猜测重命名，不遍历目录，告知路径差异并确认。上述四份规范互相独立；格式模板与通用写作规范同时适用时，以用户当次指令、指定模板和具体任务职责为准，不把论文专属格式强加给一般 Word。

**Word 全局底线**：正式数据表、附录表和记录表均采用三线表——顶线/底线 **1.5 磅**、表头分隔线 **1 磅**、无竖线及内部横线；用户明确提出例外时按其要求。完整交付前核对正文内容、目录/题注/页码及实际渲染页面；不得将内部“最终版/初稿”等工作标记写入正式正文。

## 3. 已验证的软件操作经验

仅在对应软件任务中应用下列细节；具体项目数值、结果和项目状态不在本文件记录。升级软件或 API 后先验证接口是否仍适用。

### 3.1 Origin

- 连接/隐藏：`New-Object -ComObject Origin.ApplicationSI`；`$origin.Visible=0`；超时卡死 → 杀无窗口 Origin 进程重连。
- 加载/保存：`$origin.Load($path,0)` / `$origin.Save($path)`；中文路径加载易失败，先复制到英文路径；`doc -o` 可能弹保存对话框卡住，用 Load。
- 读 LabTalk 结果：`$origin.Execute('double x = layer.cmap.colorBelow;')` 后 `$origin.LTVar('x')` 读数值、`$origin.LTStr('名')` 读字符串。
- 工作表：`FindWorksheet('A1')`；`$ws.GetData(0,0,-1,-1,2)` 返回 double[,]（GetValue(row,col) 1-based）；`(…,0)` 为显示文本。
- 图表：`FindGraphLayer('页面名')`；先 `Execute('win -a 页面名;')` 激活；DataPlots/GraphObjects。
- 色标外颜色：`layer.cmap.colorBelow/colorAbove/colorMiss`（低于下限默认黑色填充，改 colorBelow 换色）。
- 动态标签：`GetNumProp('label.show')` 开关；格式串 LabTalk：`layer1.plot1.label.form$="\b($(y,.1))"`（COM 读 label.form$ 无效）。
- 静态标签：LabTalk `label -j 1 -n 名 "\b(数值);"` → 设 `名.x/y/fsize/font/color`；最后创建的标签需再执行一条 LabTalk 才定型；删除 `label -ra 前缀;`。
- 导出：`expGraph type:=png filename:="xx.png"` 实际写入 `D:\数据\origin\`（路径压平、约100DPI）；高清用 `expGraph type:=png filename:="<短名>" path:="<英文目录>" tr2.dpi:=300`；失败时改 `expG2img type:=png name:="英文路径";`；误保存可从 `C:\Users\Yan\Documents\OriginLab\Backup\` 恢复。
- 提速：保持实例常开（新会话自动 attach）；Python 可 win32com 连接但 PS COM 更稳。

### 3.2 Fluent-PyFluent

#### 3.2.1 PyFluent 写操作验证原则
- API/TUI 没有报错 ≠ 操作成功：换网格后重新检查 cell count、zone names/IDs；设置参数后 read-back；复制 setup 后 diff 关键字段；创建 surface 后验证其坐标/位置；保存文件后确认文件存在。
- 每个写操作都应定义并断言 post-condition，不能只依赖返回状态。

#### 3.2.2 Case/Data 读取与求解控制
- 读取 data 后，case/data 中的 transient control 可能覆盖此前设置的 time step；瞬态流程固定顺序：read_data → set <requested_dt> → read-back/assert → iterate。断点续算同样执行该检查，禁止假定此前设置仍有效。
- 某些 settings 命令对象（如 run_calculation.iterate）首次调用后可能变为 inactive；可用 TUI（/solve/iterate、/solve/dual-time-iterate）作为稳定替代。
- scheme_eval 是服务对象，需以 s.scheme_eval.scheme_eval(...) 调用；不要把单个 rpvar 当作 flow-time/iteration 的唯一状态来源，应结合 settings、transcript 与输出文件验证。

#### 3.2.3 Setup 迁移与 DPM Injection
- setup.set_state 不能视为完全可靠的 setup 复制：对 DPM 尤其危险，injection 的 mass flow、velocity、cone 参数、particle distribution 等可能丢失或退回默认值。
- 复制后必须逐字段核查上述 injection 关键字段；对关键字段使用显式恢复函数（先设 option 再设子值），并在真正求解前做 read-back 比较。
- DPM 正式运行前增加运行时断言：不只查 settings，还要校验第一时间步实际产生的 parcel 数量、注入质量与按 injection 设置计算的 <expected_injected_mass> 一致；不一致立即停止，禁止继续生成"看起来正常"的无效结果。

#### 3.2.4 Surface 创建与 Report 提取
- 固定坐标平面优先使用 settings API 的确定性接口（如 plane_surface，指定 <surface_coordinate>），避免依赖交互式 /surface/iso-surface——其 prompt 顺序容易错位生成错误位置的面。
- 某些 /report/surface-integrals/... 命令在 execute_tui 后会留下悬空 prompt，可能吞掉下一条命令；必要时在命令串末尾增加额外空行 \n\n 冲刷，并防止命令串错位。
- 创建 surface 后必须验证其实际位置/坐标，再用于 report。

#### 3.2.5 Watertight Meshing 工作流
- 流程：Import Geometry → Generate Surface Mesh（MinSize/MaxSize、GrowthRate、CurvatureNormalAngle）→ Describe Geometry → Update Boundaries → Update Regions → Add Boundary Layers → Generate Volume Mesh → write case；几何导入以项目验证的原始几何格式为准。
- Add Boundary Layers 需要通过 compound child task 设置：add_child=yes → AddChildToTask → InsertCompoundChildTask → 在子任务中设置 BL 控制参数与 zone/region scope；执行后验证层数、层高分布与 zone scope。
- Volume fill 需使用实际 API 接受的值（已验证 "poly"）。
- 网格生成后必须读取 cell count、quality（如 Minimum Orthogonal Quality）与 zones 做 post-condition 检查，不能只看 workflow 显示完成。

#### 3.2.6 长任务、断点与进程管理
- 前台命令宿主超时可能连带杀死 Python/Fluent 子进程；长任务使用后台分离进程（Start-Process -WindowStyle Hidden），并分 chunk 写 case/data 断点。
- 启动新任务前检查旧 Fluent、MPI、Python 孤儿进程，避免 CPU/license 争抢。

#### 3.2.7 Transcript/Autosave/RPVar 可靠性
- autosave 异常可能影响自动化命令链与 transcript 残差抓取；正式自动运行中可关闭自动保存，改为脚本显式保存断点。
- 残差/收敛判定结合 transcript（残差行、converged 消息）与 settings，不要只依赖单个 rpvar。

#### 3.2.8 数值发散排查顺序
- 遇到发散先确认"运行的是否真的是预期设置"：<requested_dt> 是否生效（read-back）、DPM 注入量/parcel 数是否符合理论、双向耦合状态、初始场是否正确。
- 未排除脚本/参数恢复错误前，禁止通过改物理模型（松弛因子、production limiter、湍流模型等）"治发散"。

### 3.3 Android-WebView-MediaStore

- 中文脚本坑：PowerShell 管道 stdin 给 python 按 GBK 解码，中文脚本字面量会变 ??? → 用 \uXXXX 转义，或 PowerShell 写 UTF-8 临时文件再处理；匹配已有中文用 ASCII 锚点或 \u 转义。
- 行号替换坑：按行号改文件，前序替换会让后续行号偏移 → 用唯一字符串锚点（尽量 ASCII）做替换。
- WebView IndexedDB 持久化：后台/低电量可能被系统清 → 启动时 navigator.storage.persist() 申请持久化存储。
- MediaStore 移动照片：ContentResolver.update(uri, {RELATIVE_PATH: 新路径}) = 真移动，免删除授权；copy+delete 在 Android 10+ 删源必须走系统确认弹窗。
- MediaStore 大量数据查询：避免逐项访问（性能瓶颈），优先分批 IN 查询；批大小根据设备、Binder/SQL 限制与实际性能测试确定。
- 大相册前端：真虚拟化（只渲染可视区 + 上下占位符 + 分页）远好于全量渲染/简单分块；缩略图按实际显示尺寸生成低分辨率、适度压缩版本，避免原图解码；具体尺寸与质量根据设备与视觉效果测试确定。
- GitHub 传输：先 `gh auth status` 与 `git fetch` 核对认证、远端分支/tag；二进制走 `gh release`，contents API 对 >1MB 文件不返回 base64（改用 `download_url`），PowerShell 重定向二进制会损坏。Git 443 不通但 `gh api` 可用时，Git Data API 仅作兜底：以现场查询的远端 commit/tree 为父级、逐个校验 blob SHA、`force=false` 更新 ref；禁止用强推覆盖远端。修改 `.github/workflows/` 需要 token 的 `workflow` scope，缺失时 API 可能返回 404。
- 离线测试 WebView 前端：Playwright（has_touch/is_mobile）注入 window.Android mock，可验证渲染/分页/移动/重命名，无 JS 报错。
- GitHub 发布：在 PicaPhoto 等已明确采用滚动单版本发布策略的项目中，每次发布必须同时提供安装/构建产物和完整可构建源码；存在多个平台实现时，每个平台分别提供一套完整源码，通用“完整源码包”不得替代缺失的平台源码。先同步代码、各平台版本元数据、README 与重复资源 → 跑对应回归 → 构建产物并核对包名/版本/签名/内嵌资源；源码包只排除缓存、构建输出和旧二进制，禁止排除必需的平台源码目录，上传前逐包列目录确认平台与构建文件齐全。该类项目更新采用滚动单版本发布：递增版本号后替换上一版 Release 资产、仓库内版本化产物及源码包，并清理被替代的 Release/tag/版本文件，禁止累计多套历史版本；同名资产用 `gh release upload ... --clobber`。最后用 `gh release view --json assets` 比对远端 size/SHA-256/state，并确认仅保留一个当前版本，且 `/releases/latest`、tag、target commit、版本元数据与静态站点资源一致，全部通过才宣布发布完成。

### 3.4 Office

- 办公文件生成/编辑按文件类型使用合适工具；按修改范围与风险执行验证，纯格式修改走下述速度优先流程。
- 办公文件统一放入 D:\desk\codex操作文件\（见第 1 节“交互与文件安全”）。
- Word 排版（docx 直接改 OOXML：unzip → 改 word/document.xml → 重打包 → 校验/渲染）：
  - 速度优先（仅改字体/字号/行距/加粗等格式时）：一次性处理 `word/*.xml` 后先断言所有 `w:t/w:delText` 文字节点逐项一致、段落数一致、目标 `rFonts/sz/spacing/b` 属性达标；优先使用可用的办公 Python 运行时，避免为简单排版临时安装依赖；无分页风险或用户未要求时不启动完整渲染链。新增 OOXML 子节点必须遵守 schema 顺序（如段落 `spacing` 位于 `ind/jc/rPr` 前，样式 `pPr` 不放 `rPr`），写出后先做结构校验，避免返工。
  - 三线表：tblPr 内显式加 `<w:tblBorders>`（顶/底 single sz=12=1.5磅；left/right/insideH/insideV none）覆盖 TableGrid 全框线样式；表头行每个单元格 tcPr 加 `<w:tcBorders><w:bottom single sz=8/></w:tcBorders>`（栏目线 1 磅；如用户指定其他磅值，sz=磅值×8（OOXML 边框线宽以 1/8 磅为单位））；删表头 `<w:shd>` 底色。风险：只删样式引用可能残留旧框线，必须显式写 tblBorders。
  - 需要正文宋体小四、1.5 倍行距时（以本次文档模板为准）：rPr `w:eastAsia="宋体"` + `<w:sz w:val="24"/>` + `<w:szCs w:val="24"/>`；段落 `<w:spacing w:line="360" w:lineRule="auto"/>`；西文/数字惯例保留 Times New Roman（ascii/hAnsi）。sz 单位=半磅（24=12pt=小四），line=360=1.5倍行距。
  - 批量定位与验证：用段落特征值组合（字号/行距/首行缩进，正文与标题/题注/表内文字各不相同）定位目标，替换前统计计数、改后复核 0 残留；涉及分页、图表、页眉页码或用户明确要求时再做完整渲染。需用 validate.py 时 GBK 系统设 PYTHONUTF8=1 并优先使用可用运行时；无 LibreOffice 时用 Word COM 转 PDF（`ExportAsFixedFormat` 格式 17）；Poppler pdftoppm 不支持中文路径 → 复制 PDF 到纯英文临时目录再转图，图片需核验。
- Word 内容修改：先在统一输出目录建立副本并保留源文件；Word COM 编辑必须以 `ReadOnly=$false` 打开，只读打开后写入可能误报“所选内容受保护”。修改后重新打开成品并核对表格数、页数及目标内容，不能只以 `SaveAs` 无报错判定成功。
- Word 修订标记：用户所说“突出显示”应使用 OOXML `<w:highlight w:val="<color>"/>` 或 COM `HighlightColorIndex`（绿色=4），不是改字体颜色；只标记真正新增或替换的 run。替换文本时复制原 `w:rPr` 以保留字体、字号等格式，既有“待补充”等占位符继续沿用其原有红色/加粗规则。
- Word 表格重排：按明确键值稳定排序并移动整行 XML，再重编序号；改前后核对人员/条目集合不变，除序号外所有单元格内容逐项一致。合并单元格可能使 COM 的 Rows/Cells 访问失败，应改用 OOXML 或 python-docx，并按底层唯一 `<w:tc>` 精确定位、写入后回读。
- 证明材料归档：同一源文件可证明多个类别时分别复制；复制前比较 SHA-256 避免重复，只有相应证明实际存在后才移除“缺…”或“没给附件”标记。个人简介中提到成果不等同于证书、任务书、标准正文或合作协议。
- 单位/人员移除审计：同时检查文件名与目录名、Word 正文与表格、PDF 文本、人员姓名、单位简称/全称、已知项目/专利/标准标题及关联叙述和统计；普通关键词检索无法覆盖扫描型 PDF，应把可检索文本命中与需 OCR/识图的扫描件分开核查，并分别报告直接残留与需联动更新的统计/叙述。

### 3.5 Visio

- COM：`New-Object -ComObject Visio.Application`；`$app.Visible=$false`；`$app.UndoEnabled=$false`；**Quit 可能挂起** → 结束/报错后查杀残留 VISIO 进程；下次操作前先杀残留（模具被占会报"该文件已在其他 Visio 实例中打开"）。
- 模具/页面：中文版模具用 8.3 短名（BASIC_M.VSSX=基本形状，路径 <Office16>\Visio Content\<LCID>\）；`OpenEx(路径,8)` 隐藏停靠；**Masters 按中文名遍历匹配**（ItemU 中文名不可靠）；页面尺寸用 `PageWidth/PageHeight` 的 `FormulaU="<n> in"`；坐标 y 向上、原点左下。
- 文字：`shape.Text` 设文本（换行用 LF）；字体用 `doc.Fonts` 查 ID → `Char.Font.ResultIU=ID`（FormulaForceU 写字体名读回 #VALUE!）；**字号用 `Char.Size.FormulaU="<n> pt"`**（ResultIU 赋小数报 Double→Int32 转换错）；整框加粗 `Char.Style.ResultIU=1`；首行加粗：`AddRow(2,-1,0)` + `CellsSRC(2,row,0/1/2/4/5)`=Begin/End/Font/Size/Style。
- **中文引号坑**：PowerShell 7.6+ 把 “” 当字符串定界符 → 含中文引号文本会被截断、参数错位（报 bool 转换错）；用 `$lq=[string][char]0x201C; $rq=[string][char]0x201D` + 文本内 `${lq}...${rq}` 运行时插值（脚本内不写字面中文引号）。
- 隐藏文本框（标题/图注）：`page.DrawRectangle(...)` + `FillPattern=0` + `LinePattern=0`，文字仍渲染；`Para.HorzAlign=1` + `VerticalAlign=1` 居中。
- 箭头：`page.DrawLine` + `EndArrow.ResultIU=4`（实心）+ `LineColor RGB(0,0,0)`。
- 保存：`$doc.SaveAs("<中文路径>.vsdx")` 可用。
- 渲染验证：若 `Page.Export` 输出尺寸异常或 `ExportAsFixedFormat` 报参数无效，可用可见窗口 `SetWindowRect` 大窗口 + 默认适配视图（**禁用 SetViewRect(0,0,w,h)，会视图空白**）+ `GetWindowRect([ref]l,t,w,h)` + `CopyFromScreen` 截图；截图前 `SetWindowPos(hwnd,HWND_TOPMOST,...)` 置顶防被其他窗口遮挡（句柄 `$win.WindowHandle32`）。

════════════════════════════════════════════════════════════════

## 4. 经验固化与同步

只有用户明确提出“固化经验 / 更新 agents / 同步 agents / 同步规范”、引用“## 4. 经验固化与同步”，或要求发布规范备份新版本时，才编辑相应权威文件并执行本节的 GitHub 同步发布流程。该指令已包含同步和发布授权，无需再次确认。

1. 分类：跨项目可复用规则及已验证软件方法 → `AGENTS.md`；专项写作/格式方法 → 对应规范文件；本机环境、项目事实及阶段性状态不在本文件固化；凭证明文不写入。
2. 先定位原条目，按“去重 → 纠错 → 替换过期信息 → 必要新增”的顺序修改。保留高价值的操作、风险、验证方法，不堆积失败探索叙事。未经验证的尝试不得标作已确认经验。
3. 实验数值不提升为通用规律；恢复出的设置标 `Recovered`，实际验证后才标 `Confirmed`；提案区分批准和未批准。
4. 只读取和修改任务相关的文件。**不默认扫描 `sessions`、`archived_sessions` 或整个 `用户md`**；执行本节同步时，可读取 `C:\Users\Yan\.codex\用户md\` 直属目录中的全部 `.md` 文件用于完整备份，不递归读取其他目录。
5. 权威文件修改后按影响范围验证：路径与文件名存在、引用及 Markdown 标题有效、无重复矛盾、无越界改动、无凭证明文、内容真实且文件可读取。失败先修复再同步。
6. 备份仓库固定为 GitHub 私人仓库 `Yan16384/.md-file-management`。同步前确认仓库可访问且 `visibility=PRIVATE`；仓库根目录保存 `AGENTS.md`，`用户md/` 保存该目录下全部 `.md` 文件，根目录只保留一个 `README.md` 用于说明用途、结构、同步与恢复方法，`SHA256SUMS.txt` 记录备份文件校验值。不得上传会话、缓存、配置数据库、凭证或其他未授权文件。
7. 每次同步都创建新版本：首次版本为 `v1.0.0`，后续常规同步递增补丁号。提交信息和 Release 说明应概括实际变化；备份文件通过 Git 逐文件提交，不制作或上传 ZIP 等自定义压缩包，并保留历史 Release 供恢复。GitHub 自动生成的 Source code (zip/tar.gz) 属于平台固定内容，不视为自定义备份资产。
8. 发布顺序：同步最新权威文件 → 更新唯一的根目录 README 和 SHA-256 清单 → 检查 Git diff 与备份文件集合 → 提交并推送 `main`（禁止强推）→ 创建版本 tag 和 GitHub Release。Git HTTPS 不可用时，可按第 3.3 节的 Git Data API 兜底方法以远端当前 commit/tree 为父级、`force=false` 更新。
9. 发布后必须回读核验：远端仓库仍为 PRIVATE、仓库中只有一个根目录 `README.md`、`main` 指向本次提交、远端文件集合和 SHA-256 与本地一致、Release/tag 指向本次提交、Release 没有自定义上传资产且 `/releases/latest` 为本次版本。全部通过后才报告同步发布完成。

## 5. 配置与权限边界

- `AGENTS.md` 由 Codex 自动加载；`用户md` 中其他 `.md` 不会自动加载，必须按第 2 节精确读取或用户指定。
