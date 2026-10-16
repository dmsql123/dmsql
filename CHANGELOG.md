# 更新日志 (Change Log)

本文档记录 DMSQL 的重大版本变更、新特性与缺陷修复。  
This document logs significant changes, new features, and bug fixes for DMSQL.

如果您在使用过程中遇到任何问题或建议，欢迎前往 [GitHub Issues](https://github.com/dmsql123/dmsql/issues) 提交反馈！  
If you encounter any issues, bugs, or have feature requests, feel free to open an issue at [GitHub Issues](https://github.com/dmsql123/dmsql/issues).

---

## [0.8.34] - 2026-10-16

### 优化

- **自定义主题滚动条自适应**：自定义主题下滚动条根据主题自定义配置色适应，反差主题情况下整体性更强。

---

### Improvements

- **Custom Theme Scrollbar Adaptation**: Scrollbars now dynamically adapt to custom theme color configurations, providing enhanced visual unity in high-contrast theme setups.

## [0.8.33] - 2026-10-15

### 优化

- **SSH 隧道主机指纹核对**：首次连接 SSH 隧道或主机指纹变更时，在发送登录凭据前主动提示核对主机指纹；支持「记住并继续」、「仅本次继续」或「拒绝」，指纹独立存储不修改系统 `known_hosts`。
- **代理协议类型引导提示**：当系统代理为 SOCKS 协议时，提供清晰引导提示改用 Socks 代理配置。

### 修复

- **HTTP 代理数据与连接稳定性修复**：修复 HTTP CONNECT 握手后紧邻的首截数据可能丢失的问题，并解决空闲连接被超时错误误掐断的问题。

---

### Improvements

- **SSH Tunnel Host Fingerprint Verification**: Prompts to verify host key fingerprints before sending authentication credentials on first connection or when host key changes; supports "Remember", "This time only", or "Reject" without modifying the system `known_hosts` file.
- **Proxy Protocol Guidance**: Displays clear actionable guidance to switch to Socks proxy when the detected system proxy uses SOCKS.

### Bug Fixes

- **HTTP Proxy Data & Connection Stability**: Fixed an issue where the initial chunk of data following the HTTP CONNECT handshake could be dropped, and resolved an issue where idle connections were prematurely terminated by timeouts.

## [0.8.32] - 2026-10-15

### 新增

- **数据库接入扩展至 33 种**：新建连接向导（图库）新增 17 款常用协议兼容数据库，配备专属品牌图标与默认端口：
  - MySQL 系：TiDB、OceanBase（MySQL 模式）、PolarDB MySQL、TDSQL MySQL、GoldenDB；
  - PostgreSQL 系：CockroachDB、YugabyteDB、Greenplum、PolarDB PostgreSQL、瀚高数据库 (HighGo)、海量数据库 (Vastbase)、优炫数据库 (UXDB)；
  - SQL Server 系：Azure SQL；
  - Doris 系：StarRocks、SelectDB；
  - Redis 系：Valkey、KeyDB。

### 优化

- **连接节点品牌识别**：资源树根节点图标与悬停提示精准跟随所选具体数据库产品；图库收藏支持按具体数据库卡片独立加星。

---

### Added

- **Expanded Database Catalog (33 Databases)**: Added 17 protocol-compatible database options to the connection wizard gallery, each with dedicated branding icons and default ports:
  - MySQL family: TiDB, OceanBase (MySQL mode), PolarDB MySQL, TDSQL MySQL, GoldenDB;
  - PostgreSQL family: CockroachDB, YugabyteDB, Greenplum, PolarDB PostgreSQL, HighGo, Vastbase, UXDB;
  - SQL Server family: Azure SQL;
  - Doris family: StarRocks, SelectDB;
  - Redis family: Valkey, KeyDB.

### Improvements

- **Connection Node Brand Identification**: Explorer root connection node icons and tooltips accurately reflect the specific database product; gallery favorites now support starring individual product cards independently.

## [0.8.31] - 2026-10-14

### 新增

- **SQL 笔记本（.dmsnb）**：全新支持交互式 SQL 笔记本，可在同一文档内混合编写 Markdown 说明与 SQL 语句。支持逐格顺序执行并在单元格下方直接渲染数据网格（可一键展开至主结果面板）；支持运行前参数变量配置；支持通过数据库节点右键菜单或快捷按钮一键创建。

### 优化

- **资源树右键菜单**：优化资源树右键菜单顺序以及显示逻辑。
- **SQL 执行错误提示精简**：优化执行失败提示，突出展示核心错误摘要与数据库错误码，剔除冗长底层调用堆栈。
- **单连接并发执行保护**：增加同连接运行状态保护，防止同一数据库连接并发执行导致的连接冲突。

---

### Added

- **Interactive SQL Notebook (.dmsnb)**: Native support for interactive SQL notebooks, allowing Markdown notes and executable SQL cells in a single document. Features sequential cell execution with inline grid results (expandable to the main result panel), pre-execution variable prompts, and dedicated creation actions from database context menus or inline buttons.

### Improvements

- **Explorer Context Menu**: Reorganized explorer context menu ordering and visibility logic.
- **Streamlined SQL Error Messages**: Improved error banners by highlighting the core error summary and error code while eliminating cluttered internal stack traces.
- **Connection Concurrency Protection**: Added run locks per connection to prevent command collisions during concurrent execution on the same database session.

## [0.8.30] - 2026-10-13

### 改进与优化

- **连接节点交互与菜单优化**：未连接的节点右侧新增快捷连接图标（连接成功后自动隐藏）；连接节点右键菜单新增「断开连接」选项。

### 修复

- **单连接独立重连修复**：修复点击单个连接时误触发其他失败连接一并重连的问题，并精简连接失败提示。

---

### Improvements & Enhancements

- **Connection Node Actions & Context Menu Polish**: Added an inline quick-connect icon on disconnected nodes (auto-hidden when connected), and added a "Disconnect" option to the connection context menu.

### Bug Fixes

- **Isolated Single Connection Retry**: Fixed an issue where connecting a single node unintentionally triggered reconnection attempts on other failed connections, and streamlined connection failure messages.

## [0.8.29] - 2026-10-13

### 新特性与改进

- **SQL 收藏夹**：新增独立侧边栏 SQL 收藏夹，支持将选中 SQL 加入收藏、多级文件夹管理、拖拽归类，以及一键打开、复制或插入到当前光标处。
- **表名悬停预览增强**：SQL 编辑器表名悬停提示支持完整展示全部字段，不再限制显示列数。

---

### Features & Improvements

- **SQL Favorites**: Added a dedicated sidebar view for SQL Favorites, supporting saving selected SQL, multi-level folder organization, drag-and-drop grouping, and quick actions to open, copy, or insert at the cursor.
- **Full Table Hover Preview**: SQL editor table hover tooltip now displays all columns in full without truncation limits.

## [0.8.28] - 2026-10-12

### 改进与优化

- **透视模式体验升级**：透视模式支持拖拽调整列顺序，支持独立快速搜索。
- **数据查询加载增强**：查询数据新增全部加载功能。

### 修复

- **列筛选菜单滚动修复**：解决列筛选菜单鼠标滚动导致菜单关闭的问题。

---

### Improvements & Enhancements

- **Pivot Mode Usability**: Supports drag-and-drop column reordering and independent quick search.
- **Load All Rows**: Added one-click action to load all query rows.

### Bug Fixes

- **Column Filter Menu**: Fixed unexpected menu closure when scrolling through the filter value list.

## [0.8.27] - 2026-10-11

### 改进与优化

- **资源树子节点操作净化**：全面清理数据表子节点无效冗余操作图标以及右键菜单。
- **资源树索引计数准确度提升**：资源树索引节点数量统一改为读取各库实际索引列表统计，准确反映二级索引及复合索引数量。

---

### Improvements & Enhancements

- **Resource Tree Child Node Cleanup**: Cleaned up invalid and redundant action icons and context menus on table child nodes.
- **Accurate Index Count in Resource Tree**: Index count badges on resource tree nodes now accurately reflect the real index list count across all databases, including secondary and composite indexes.

## [0.8.26] - 2026-10-10

### 新特性与改进

- **透视模式交互与导出升级**：优化透视模式交互，避免与基础数据查询互相干扰；数据导出支持直接导出透视分析结果。
- **资源树行数统计与缓存优化**：优化数据表行数统计刷新逻辑与会话缓存机制，提升行数准确度与浏览性能。

### 修复

- **透视筛选汇总数据修复**：修复在数据筛选状态下透视模式汇总数据计算错误的问题。

---

### Features & Improvements

- **Pivot Mode Usability & Export**: Refined pivot mode interactions to prevent interference with basic data querying, and added support for exporting pivot table results directly.
- **Resource Tree Row Count & Cache Polish**: Optimized table row count calculation and session caching mechanisms for improved accuracy and performance.

### Bug Fixes

- **Pivot Aggregation with Filter Fix**: Fixed incorrect summary aggregation in pivot mode when data filtering is applied.

## [0.8.25] - 2026-10-09

### 改进与优化

- **结果集顶部栏与表数据预览标题优化**：优化结果集顶部栏信息排版，左侧动态显示当前 SQL（超长省略，悬停查看全文），右侧统一标注连接与数据库；精简表数据预览标题及耗时呈现。

---

### Improvements & Enhancements

- **Result Grid Header & Table Preview Polish**: Refined result grid header bar with dynamic SQL preview (truncated with hover tooltip) and connection info, and streamlined table preview titles and duration display.

## [0.8.24] - 2026-10-08

### 新特性与改进

- **SQL 动态变量绑定**：新增支持在 SQL 中使用 `${变量}` 占位符语法，弹窗提示填入相应变量值，确认后运行SQL。

---

### Features & Improvements

- **Dynamic SQL Variable Binding**: Added support for `${variable}` placeholders in SQL queries, prompting for variable values before executing the SQL.

## [0.8.23] - 2026-10-08

### 改进与优化

- 优化结果网格行悬停联动高亮与展开箭头视觉细节。

---

### Improvements & Enhancements

- Refined result grid row hover highlight synchronization and expand arrow visuals.

## [0.8.22] - 2026-10-07

### 新特性与改进

- 优化网格展示。
- 新增设置项「展开高度」（`dmsql.grid.expandHeight`，默认 0 不限高），支持自定义详情最大高度上限。

---

### Features & Improvements

- Refined grid display.
- Added "Expand Height" setting (`dmsql.grid.expandHeight`, default 0 for unlimited) to allow configuring a max height cap for expanded rows.

## [0.8.21] - 2026-10-06

### 新特性与改进

- **离线驱动指引**：连接表单新增驱动离线安装提示与文档跳转链接，方便离线/内网环境下快速配置。
- **视图操作体验优化**：数据库资源管理器标题栏按钮改为常驻显示，操作更直观便捷。

### 修复

- **连接状态提示修复**：修复重新连接时偶发重复弹出多条「正在连接」通知的问题。

---

### Features & Improvements

- **Offline Driver Guidance**: Added offline driver installation instructions and documentation links to the connection form for restricted/intranet environments.
- **View Header Usability**: Made action buttons in the Database Explorer header persistently visible for quicker access.

### Bug Fixes

- **Connection Status Notification**: Fixed duplicate "Connecting..." notifications when reconnecting.

## [0.8.20] - 2026-09-30

### 修复与优化

- **达梦 (DM) 数据库连接优化**：增强空闲连接心跳保活，优化断开会话的及时释放与回收，解决闲置超时断开及后续重连失败的问题。

---

### Bug Fixes & Improvements

- **Dameng (DM) Connection Stability**: Enhanced idle heartbeat keep-alive and improved session cleanup, resolving idle timeout disconnections and reconnection failures.

## [0.8.19] - 2026-09-30

### 修复

- **【重要修复】达梦 (DM) 表定义与 DDL 获取导致实例卡死问题**：彻底移除对 `DBMS_METADATA.GET_DDL` 的依赖调用，解决特定版本达梦数据库实例在获取表 DDL 时被意外挂起、卡死（严重时需重启数据库实例）的重大缺陷；改用系统过程与元数据安全读取表定义。
- **表详情面板生命周期与交互 Bug 修复**：
  - 修复关闭表详情面板或性能监控面板后，再次点击无法重新打开的问题（纠正面板销毁与缓存清理时序）；
  - 修复面板已打开时再次点击「查看建表 DDL」或「表详情」无法精准激活并切换对应页签的问题（统一达梦与 Hive 页签切换逻辑）。

### 改进与优化

- **表详情界面排版优化**：主键标识标签（PK）优化为强制单行展示，避免列宽收窄时被挤压换行，提升字段列表阅读体验。

---

### Bug Fixes

- **[Critical Fix] Dameng (DM) Table DDL Hanging & Instance Lockup**: Completely removed calls to `DBMS_METADATA.GET_DDL`, eliminating a critical issue where certain Dameng database instances would hang or freeze (requiring a service restart) during DDL generation; now safely retrieves table definitions via system procedures and metadata views.
- **Table Details Panel Lifecycle & Tab Interaction Bug Fixes**:
  - Fixed an issue where reopening Table Details or Performance Monitoring panels failed after closing (corrected disposal order and cache invalidation);
  - Fixed tab switching so clicking "View Table DDL" or "Table Details" seamlessly jumps to and activates the target tab when the panel is already open (unified Dameng and Hive tab handling).

### Improvements & Enhancements

- **Table Details UI Layout Polish**: Enforced single-line rendering for Primary Key (PK) badges, preventing awkward wraps on narrow columns and improving field readability.

## [0.8.18] - 2026-09-30

### 新特性与改进

- **SQL 编辑器自定义列注释**：SQL 编辑器右键菜单新增「自定义列注释」命令，自动在查询语句上方生成/更新 `/* @columns */` 注释块；执行后自定义注释将显示在网格列名下方并覆盖数据库默认注释（自动排除通配符）。
- **结果网格列宽与注释展示优化**：列头注释不再强行撑开列宽，默认自适应列名与数据长度，注释过长时优雅省略截断，支持鼠标悬停浮窗查看完整注释。

### 修复

- **达梦 (DM) 数据库驱动依赖修复**：驱动下载流程补充安装缺失的 `safer-buffer` 依赖，解决此前因依赖不全导致“驱动已就绪但测试连接提示未安装”的异常，并支持测试连接时自动检测并补齐。 (#1)

---

### Features & Improvements

- **SQL Editor Custom Column Comments**: Added "Custom Column Comments" to the editor context menu to insert/refresh `/* @columns */` annotations above queries; defined comments display below column headers in the grid and override database comments (wildcards excluded).
- **Result Grid Column Width & Comment Truncation**: Header comments no longer stretch default column widths; column width automatically fits column names and data, with long comments cleanly truncated and full text viewable on hover.

### Bug Fixes

- **Dameng (DM) Driver safer-buffer Dependency Fix**: Included missing `safer-buffer` dependency in Dameng driver downloads, resolving an issue where the driver was marked ready but connection tests reported it missing; automatically repairs existing installations during connection test. (#1)

## [0.8.17] - 2026-09-30

### 改进与优化

- 优化网格显示细节。

---

### Improvements & Enhancements

- Refined grid display details.

## [0.8.16] - 2026-09-30

### 改进与优化

- **结果网格多条件 WHERE 筛选重构升级**：
  - 工具栏 WHERE 筛选支持多条件管理池：支持通过「+」号将当前条件收入列表，支持在 AND / OR 之间自由切换连接逻辑；
  - 支持直接以 `and ` 或 `or ` 开头输入条件并自动识别提取连接词；支持将已有条件一键回填至输入框重新编辑；
  - 条件列表支持拖拽灵活调整优先级排序，离开首位自动恢复连接词；回车或点击运行按钮自动收纳输入框并立即执行；
  - 工具栏筛选徽标仅在条件生效时常驻展示筛选计数，视觉更清爽。

---

### Improvements & Enhancements

- **Result Grid Multi-Condition WHERE Filter Upgrade**:
  - Upgraded toolbar WHERE filtering with a multi-condition list: use "+" to collect clauses into the list, with flexible AND / OR toggles on subsequent items;
  - Supports typing conditions prefixed with `and ` or `or ` to automatically assign logical connectors; allows quick round-trip editing by restoring items back to the input box;
  - Supports drag-and-drop reordering of filter conditions with automatic connector adjustments; pressing Enter or clicking Run automatically collects input and executes;
  - Filter counter badges now persist cleanly only while filtering is active.

## [0.8.15] - 2026-09-29

### 修复

- **网格选择互斥与清理修复**：
  - 修复单元格框选、行选择、列选择与全选状态冲突残留的问题，点击单元格自动清除行列全选，点击行号/列头自动清除单元格手柄，并支持 Esc、排序或点击空白区域取消选中；
  - 修复左上角全选切换交互（已全选时再次点击取消全选），右键未选中单元格时自动纠正焦点。
- **行内数据编辑行为修复**：
  - 严格限制双击可编辑范围，多表关联查询严格禁止修改，仅支持表数据与单表简单查询；
  - 修复双击单元格进入编辑时选区残留遮挡的问题，并解决鼠标松开时焦点意外丢失的缺陷。
- **修改状态与撤销重做修复**：
  - 修复未保存修改在工具栏保存按钮的视觉展示，采用图标高亮并于浮动提示中清晰标注变更计数；
  - 修复撤销修改时单元格未正确显示修改前原值的问题，并将撤销/重做粒度精准修复为按单步编辑逐步回退，避免误丢全部修改。

---

### Bug Fixes

- **Grid Selection Exclusivity & Cleanup**:
  - Fixed conflicting selection states between cell range, row, column, and select-all modes; selecting cells now clears row/column selections, while selecting row/column headers removes cell ranges and drag handles; Esc, sorting, or clicking outside/blank areas clears selections;
  - Fixed corner select-all toggle interaction (clicking when fully selected now deselects), and right-clicking an unselected cell correctly focuses it.
- **Inline Cell Editing Constraints & Focus**:
  - Restricted inline editing strictly to table data browsing and single-table SELECT statements, rejecting multi-table joins;
  - Fixed cell selection overlays obstructing the active editor upon double-click, and prevented unexpected focus loss upon mouse release.
- **Dirty State Indicator & Undo/Redo Granularity**:
  - Refined toolbar save button indicator for unsaved changes by highlighting the icon and displaying change counts within tooltips;
  - Fixed cells failing to revert to original values upon undo, and corrected undo/redo granularity to single-step history instead of wiping all unsaved changes simultaneously.

## [0.8.14] - 2026-09-28

### 改进与优化

- 优化结果网格列头排序箭头与筛选图标视觉颜色对齐。

### 修复

- 修复自定义主题下浅色行单击单元格未生效选中底色的问题。

---

### Improvements & Enhancements

- Aligned column sort arrow colors with filter icons in result grids.

### Bug Fixes

- Fixed cell selection background colors not applying to alternating light rows under custom themes.

## [0.8.13] - 2026-09-28

### 改进与优化

- 结果网格全面对齐各宿主（Trae/CodeBuddy/Cursor）角标选中主题色与单元格选区手柄，优化新建连接浅色视觉层次。

### 修复

- 修复深色主题下模拟数据下拉菜单文字看不清及部分主题下排序箭头对比度不足的问题。

---

### Improvements & Enhancements

- Aligned grid selections and cell range handles with host badge themes (Trae, CodeBuddy, Cursor), and refined connection view styling.

### Bug Fixes

- Fixed unreadable dropdown text in dark mock data views and improved sort arrow contrast under specific host themes.

## [0.8.12] - 2026-09-27

### 改进与优化

- **SQL 编辑器括号块快捷选中**：
  - 光标在左括号后双击位置或紧邻空白，可一键选中该对括号内的完整 SQL 内容，大幅提升子查询局部执行与调试效率（兼容 VS Code / Cursor / Windsurf / Trae 等环境）。
- **深色主题与图标视觉体验优化**：
  - 资源树表、函数、事件、数据库等节点图标在深色模式下自适应提亮，视觉对比更清晰；
  - 优化新建连接页面深色主题效果：移除数据库图标突兀的灰底，增强分类选中项可读性与卡片悬停边框质感。

### 修复

- 修复新建连接在 Trae 等浅色主题下测试连接成功提示文字对比度不足、难以看清的问题。

---

### Improvements & Enhancements

- **Quick Enclosed SQL Block Selection**:
  - Double-clicking immediately after an opening parenthesis or adjacent whitespace automatically selects the entire enclosed SQL block, speeding up partial subquery execution (compatible with VS Code, Cursor, Windsurf, Trae, etc.).
- **Dark Theme & Icon Visual Enhancements**:
  - Explorer tree icons for tables, functions, events, and databases now dynamically adapt with brighter contrast in dark mode;
  - Refined the New Connection view in dark themes by removing harsh grey backgrounds on database badges, improving category selection legibility, and sharpening card hover borders.

### Bug Fixes

- Fixed low contrast and illegible "Connection Successful" toast text under light themes such as Trae.

## [0.8.11] - 2026-09-27

### 改进与优化

- 结果网格新增多选列批量复制列名与多列固定支持，优化列菜单展示。

### 修复

- 修复排序后斑马纹底色未按视觉顺序更新及子菜单靠边溢出的问题。

---

### Improvements & Enhancements

- Added batch column name copying and multi-column pinning in result grids, with streamlined header menus.

### Bug Fixes

- Fixed alternating row colors not updating by visual order after sorting, and resolved submenu viewport overflow issues.

## [0.8.10] - 2026-09-27

### 改进与优化

- **历史记录交互与列头体验全面升级**：
  - 支持单击列头快速排序（升序 ↑ / 降序 ↓ / 取消排序三态循环）；
  - 耗时与行数严格按真实数值排序，类型、来源、动作按本地化展示文案排序；
  - 执行计划与分析树表保护层级顺序，避免误排序打乱算子父子结构。
- **结果网格列头交互与排序体验重构**：
  - 单击列头直接切换排序状态，操作更直观顺畅；
  - 支持快捷选列：`Ctrl+单击` 切换选中单列，`Shift+单击` 进行连续列区间多选。
- **执行计划面板精简优化**：
  - 执行计划面板移除试验阶段的图表视图，聚焦于层次化计划树与原始结果网格的稳定呈现。

### 修复

- 修复列头上下文菜单中「复制列名」偶发误选整列的问题。
- 修复结果网格排序后展开行错位掉落至表格最底部的问题，现切换排序前自动收起展开项，重排后展开明细紧邻当前行。
- 修复历史记录列筛选列表显示为内部代号的问题，现已全面对齐为易懂的本地化标签。

---

### Improvements & Enhancements

- **Execution History Grid & Sorting Enhancements**:
  - Click column headers directly to toggle sorting (Ascending ↑ → Descending ↓ → None);
  - Numeric columns (duration, row count) sort by actual numerical values, while types, sources, and actions sort by localized display labels;
  - Guarded plan explorer tree grids from unintended sorting to preserve parent-child operator hierarchies.
- **Result Grid Column Header & Sorting Enhancements**:
  - Click column headers to toggle sorting state smoothly;
  - Support flexible column selection: `Ctrl+Click` toggles single column selection, while `Shift+Click` performs multi-column range selections.
- **Query Plan Panel Refinements**:
  - Streamlined the execution plan panel by removing experimental chart visualizations, focusing on the stability of the hierarchical plan tree and raw result grid.

### Bug Fixes

- Fixed an issue where "Copy Column Name" from the header menu could unintentionally select the entire column.
- Fixed an issue where expanded row details could detach and drop to the bottom of the table after sorting; details are now collapsed prior to sorting and correctly anchored under the current row when expanded.
- Fixed an issue where execution history column filters displayed internal code names instead of user-friendly localized labels.

## [0.8.9] - 2026-09-27

### 新增与改进

- **全新可视化执行计划树表（EXPLAIN）**：
  - PostgreSQL 族（PostgreSQL / 金仓 / GaussDB / openGauss）及 MySQL 8.0+ 右键「解释」全新升级为层次化计划树表，清晰直观展示算子层级、详细谓词、估算行数与代价占比，并支持随时切回原始格式；
  - SQLite 与 DuckDB 全面升级为树形查询计划，告别晦涩的底层字节码展示。
- **安全高效的查询实际分析（EXPLAIN ANALYZE）**：
  - 右键新增「分析」功能，支持安全执行只读查询并捕获真实耗时与实际扫描行数（执行前弹窗确认，严格拦截写操作）；
  - 计划树面板全面复用结果网格体系，主题配色、单元格复制与列宽体验高度统一。
- **智能体慢查询诊断上下文增强**：
  - 内置 AI 智能体在解释 SQL 时自动注入精简计划树结构，大幅提升索引优化与性能调优建议的精准度。
- **资源树库表即时刷新**：
  - 刷新数据库节点或表分组时立即清除本地表缓存并重查，彻底解决新建/删表后偶发仍显示旧数量的问题。

---

### Improvements & Enhancements

- **Visual Query Plan Explorer (EXPLAIN Tree)**:
  - Upgraded "Explain" for PostgreSQL families and MySQL 8.0+ into hierarchical plan tree grids, displaying operator details, filter predicates, estimated rows, and optimizer cost percentages, with seamless toggling back to raw results;
  - Added structured plan tree support for SQLite and DuckDB, replacing raw bytecode outputs.
- **Safe Actual Query Execution Analysis (EXPLAIN ANALYZE)**:
  - Added "Analyze" action to safely measure actual execution durations and real row counts on read-only statements (guarded by confirmation prompts; write queries strictly rejected);
  - Plan explorer panels now share the unified Result Grid infrastructure for consistent theming and cell copy interactions.
- **AI Agent Query Diagnostics Enhancement**:
  - Automatically incorporates compact plan tree context when agents explain queries, enabling more accurate index and performance tuning recommendations.
- **Instant Explorer Cache Invalidation**:
  - Invalidates database table caches immediately upon explorer refresh, eliminating delayed table counts after creating or dropping tables.

## [0.8.8] - 2026-09-26

### 改进与优化

- 全面升级网格交互与历史记录：新增单元格多选拖拽框选、列头连选、高级复制格式（带表头/逗号分隔列表），并统一历史记录网格交互与稳定性。

---

### Improvements & Enhancements

- Upgraded grid interactions and execution history: added multi-cell range dragging, column selections, advanced Copy As formats, and unified the history grid experience.

## [0.8.7] - 2026-09-23

### 改进与优化

- 新增存储过程多结果集分 Tab 展示支持（涵盖 MySQL/MariaDB/Doris/SQL Server），并新增最大结果集上限配置项。

---

### Improvements & Fixes

- Added tabbed multi-resultset rendering for stored procedures across MySQL, MariaDB, Doris, and SQL Server, with a configurable result set limit.

## [0.8.6] - 2026-09-21

### 改进与优化

- 官网主题配置优化。
- 修复网格背景色显示问题。

---

### Improvements & Fixes

- Website theme configuration optimizations.
- Fixed grid background color display issues.

## [0.8.5] - 2026-09-21

### 改进与优化

- 自定义主题新增选中行自定义背景色、字体颜色，优化网格显示，解决已知问题。

---

### Improvements & Fixes

- Added customizable selected row background and text colors for custom themes, optimized grid display, and resolved known issues.
