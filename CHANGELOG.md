# 更新日志 (Change Log)

本文档记录 DMSQL 的重大版本变更、新特性与缺陷修复。  
This document logs significant changes, new features, and bug fixes for DMSQL.

如果您在使用过程中遇到任何问题或建议，欢迎前往 [GitHub Issues](https://github.com/dmsql123/dmsql/issues) 提交反馈！  
If you encounter any issues, bugs, or have feature requests, feel free to open an issue at [GitHub Issues](https://github.com/dmsql123/dmsql/issues).

## [0.9.13] - 2026-11-14

### 优化

- **高危 SQL 拦截与确认体验优化**：执行 DROP、TRUNCATE 或无 WHERE 的 DELETE / UPDATE 时，统一采用快捷确认弹窗（支持一键确认/取消，按 Esc 亦可取消）。
- **表详情触发器排版与联动增强**：表详情面板中触发器定义默认单行紧凑展示（超长自动省略截断，列宽自适应避免撑出横向滚动条），支持点击「查看详情」弹窗查看排版美化后的完整 SQL 并快捷复制；在资源树点击触发器或模式下的「表触发器」节点时，精准联动跳转至对应表详情的触发器标签页。(#10)

---

### Improvements

- **Dangerous SQL Guard & Confirmation Polish**: Streamlined confirmation for DROP, TRUNCATE, and unconstrained DELETE / UPDATE statements into a quick action prompt (confirm/cancel in one click, or press Esc to cancel).
- **Table Details Trigger Layout & Navigation Polish**: Trigger definitions in the table details panel now display compactly on a single line with dynamic column width to prevent unintended horizontal scrollbars, featuring a "View Details" modal for formatted SQL preview and quick copying; clicking trigger nodes or "Table Triggers" in the resource tree now seamlessly navigates to the triggers tab of the corresponding table details panel. (#10)

## [0.9.12] - 2026-11-13

### 新增

- **资源树数据表筛选记忆**：新增 `dmsql.tree.rememberTableFilter` 设置项（默认关闭），开启后可跨会话记住每个连接下勾选的数据表筛选状态，重新打开编辑器仍保持生效。(#9)

### 修复

- **结果网格日期时间回写修复**：优化各数据库引擎在结果网格编辑保存日期与时间字段时的 SQL 生成逻辑。Oracle、达梦规范使用 `TO_DATE` / `TO_TIMESTAMP` 避免 `ORA-01861` 格式错误；PostgreSQL、MySQL、SQL Server、ClickHouse、DuckDB 等按列类型生成标准字面量，纯日期列不再写入多余的时间部分。(#8)

---

### Added

- **Remember Table Filter in Resource Tree**: Added `dmsql.tree.rememberTableFilter` setting (disabled by default) to optionally persist checked table filter states per connection across editor restarts. (#9)

### Bug Fixes

- **Result Grid Date/Time Persistence Fix**: Improved SQL generation when saving edited date and datetime cells across database engines. Oracle and Dameng now use `TO_DATE` / `TO_TIMESTAMP` functions to avoid `ORA-01861` format errors; PostgreSQL, MySQL, SQL Server, ClickHouse, and DuckDB format literals strictly according to column types, preventing redundant time components in date-only columns. (#8)

## [0.9.11] - 2026-11-12

### 修复

- **行展开详情字段名排版修复**：优化结果网格行展开详情的字段名列宽与换行逻辑，避免短字段名被异常折行，展开区域高度随内容自适应。

---

### Bug Fixes

- **Row Detail Field Name Layout Fix**: Improved field-name column sizing and wrapping in expanded row details to avoid unintended line breaks on short field names, with panel height adapting to content.

## [0.9.10] - 2026-11-12

### 优化

- **单元格详情编辑器升级**：单元格编辑弹窗的文本页改用与包声明面板相同的编辑器，支持 JSON 语法高亮、行号显示、括号匹配，以及多行格式化后的对象/数组折叠；编辑区高度自适应弹窗大小，文本页仅保留编辑器自身滚动条。
- **结果网格自动列宽与配置优化**：自动列宽默认上限为 `360px`、短列下限为 `120px`（长文本悬停可查看全文，且支持手动拖拽调宽）；底栏设置（齿轮图标）及扩展配置均支持调整这两个阈值（`dmsql.grid.columnMinWidth` / `dmsql.grid.columnMaxWidth`）。

### 修复

- **JSON 单元格原文展示与写回修复**：修复 PostgreSQL（`json` / `jsonb`）与 MySQL（`JSON`）列在结果网格中显示为 `[object Object]` 的问题，现在按数据库原文（保留原始空格与键序，例如 `{"n": 1, "name": "ann"}`）展示，列表头图标使用 `{ }`，编辑后按文本写回。
- **多语句执行记录丢失修复**：修复一次运行建表、插入等多条 SQL 语句且结果页面尚未打开完时，部分执行记录丢失的问题，确保结果页完整列出每一条语句的执行记录。
- **当前库与 Schema 分级切换修复**：针对多层级数据库引擎规范当前库 / Schema 逐级切换流程与会话作用域。

---

### Improvements

- **Cell Inspector Text Editor Upgrade**: Upgraded the Text tab in the cell editor modal to use the same editor as the Package editor, adding JSON syntax highlighting, line numbers, bracket matching, and folding for multi-line formatted objects and arrays. The editor area adapts to modal height with only its own scrollbar.
- **Result Grid Auto Column Width & Settings**: Set default auto-column width bounds to `120px` minimum and `360px` maximum (full content on hover, and manual column resizing remains supported). Both limits can be customized via the bottom-bar settings gear or extension settings (`dmsql.grid.columnMinWidth` / `dmsql.grid.columnMaxWidth`).

### Bug Fixes

- **JSON Cell Raw Display & Text Write-back Fix**: Fixed an issue where PostgreSQL (`json` / `jsonb`) and MySQL (`JSON`) columns displayed as `[object Object]`. Cells now render verbatim raw database text (preserving spacing and key order, e.g., `{"n": 1, "name": "ann"}`), use `{ }` in column headers, and write back as text after editing.
- **Multi-Statement Execution Records Loss Fix**: Fixed an issue where running multiple statements (such as `CREATE TABLE` and `INSERT`) while the results view had not finished opening could drop execution records, ensuring every statement is listed in the results tab.
- **Database & Schema Multi-Level Scope Switching Fix**: Aligned active database and schema multi-level switching flow and session scope across database engines.

## [0.9.9] - 2026-11-10

### 修复

- **Oracle / 达梦列默认值与非空顺序修复**：修复 Oracle、达梦及 OceanBase (Oracle 模式) 在建表、加列与改列时，因默认值生成在非空之后（`NOT NULL DEFAULT`）导致报错 `ORA-00907: missing right parenthesis` 的语法缺陷，纠正为标准的 `DEFAULT ... NOT NULL` 顺序。
- **表结构设计器外键映射与保存校验修复**：修复在可视化表设计器中编辑已有外键的表结构时，因元数据未完整拆解引用列对导致保存被误拦截的问题；全数据库引擎统一规范复合外键「本表列 -> 引用列」列对解析。

---

### Bug Fixes

- **Oracle & Dameng Column Default and NOT NULL Ordering Fix**: Fixed an `ORA-00907: missing right parenthesis` syntax error in Oracle, Dameng, and OceanBase (Oracle mode) when creating or altering columns caused by placing default values after NOT NULL (`NOT NULL DEFAULT`), correcting the syntax order to `DEFAULT ... NOT NULL`.
- **Table Structure Designer Foreign Key Mapping & Save Validation Fix**: Fixed an issue where saving an existing table with foreign keys was falsely blocked due to incomplete referenced column pair parsing; standardized composite FK column-to-referenced-column mapping across all database engines.

## [0.9.8] - 2026-11-10

### 新增

- **Oracle 族程序包（Package）可视化管理面板**：为 Oracle、OceanBase (Oracle 模式) 和达梦数据库新增专属程序包结构化面板，支持查看包头信息（状态、时间等）、子过程、函数、依赖关系，并支持包规范（声明）与包体（主体）独立分栏查看、编辑保存、编译及删除。

### 优化

- **DDL 智能排版美化引擎**：针对 DuckDB、SQLite、Oracle 等建表语句单行紧凑或凌乱的问题，新增方言感知的 DDL 自动换行、缩进美化与安全容错引擎。
- **多数据库建表 DDL 完整度大幅补齐**：
  - **DuckDB & SQLite**：建表 DDL 自动追加表注释、列注释以及独立的二级索引；
  - **Oracle & OceanBase (Oracle 模式)**：在原生 DDL 基础上自动拼接表注释、列注释及独立二级索引；
  - **PostgreSQL 家族（PG / 金仓 Kingbase / GaussDB / openGauss / 瀚高 HighGo）**：DDL 补齐唯一约束、检查约束、表/字段注释及独立二级索引；
  - **SQL Server**：重构原生 T-SQL DDL 提取引擎，完整生成字段类型、自增 IDENTITY、默认值、主键约束、扩展属性说明及普通索引；
  - **达梦 (Dameng)**：自动补齐表与字段的 COMMENT ON 注释。
- **表详情标签视觉细节优化**：优化表详情面板中字段类型、主键、触发时机等徽标标签的底色与字色，文本垂直居中，消除深色模式下对比度不足的问题。

### 修复

- **PL/SQL 存储过程与复合块批量执行修复**：修复包含触发器、存储过程、函数和程序包的 SQL 脚本全选批量执行时，内部的分号被错误截断导致报错的问题，确保 `BEGIN ... END;` 块完整执行。
- **CodeLens 复合块范围识别修复**：修复光标停留在触发器或过程末尾 `END;` 时被错误识别为独立语句块的问题，准确选中整个复合语句块。

---

### Added

- **Visual Package Management Panel for Oracle Family**: Added dedicated structured package editors for Oracle, OceanBase (Oracle mode), and Dameng, featuring header metadata, subprograms (procedures/functions), dependencies, and split-pane viewing, editing, compiling, and dropping of package specification and body.

### Improvements

- **Intelligent DDL Formatter Engine**: Introduced a dialect-aware DDL formatting and indent engine with safe fallback, automatically prettifying compact single-line CREATE TABLE statements across DuckDB, SQLite, Oracle, and others.
- **Comprehensive DDL Metadata & Index Enrichment**:
  - **DuckDB & SQLite**: Automatically appends table/column comments and standalone secondary indexes to table DDL;
  - **Oracle & OceanBase (Oracle mode)**: Supplements native DDL with table comments, column comments, and secondary index definitions;
  - **PostgreSQL Family (PG, Kingbase, GaussDB, openGauss, HighGo)**: Enriches DDL with unique/check constraints, table/column COMMENT ON statements, and secondary indexes;
  - **SQL Server**: Rebuilt native T-SQL DDL generation with full types, IDENTITY(1,1), defaults, PK constraints, MS_Description comments, and indexes;
  - **Dameng**: Automatically appends external COMMENT ON table and column comments.
- **Table Details Badge Visual Refinement**: Standardized badge background colors, font colors, and vertical alignment for field types, PK flags, and trigger timing, improving contrast under dark themes.

### Bug Fixes

- **PL/SQL Composite Block Batch Execution Fix**: Fixed an issue where internal semicolons within triggers, procedures, functions, and package bodies were mistakenly split during batch execution, ensuring full `BEGIN ... END;` blocks run reliably.
- **CodeLens Composite Block Range Detection**: Fixed an issue where placing the cursor on a trailing `END;` of a trigger or procedure isolated it as a standalone statement instead of recognizing the entire composite block.

## [0.9.7] - 2026-11-09

### 优化

- **数据库品牌图标与界面展示优化**：
  - 优化资源树、编辑器 CodeLens 与连接切换弹窗（QuickPick）等多处图标展示；
  - 各类协议兼容库全面改为展示自身正确的官方品牌图标，不再退化显示通用图标；
  - 连接切换弹窗（QuickPick）支持展示彩色品牌图标与更精准的产品名称。

---

### Improvements

- **Database Brand Icons and UI Display Improvements**:
  - Refined database icon displays across the Explorer tree, editor CodeLens, and QuickPick menu;
  - Compatible databases now display their accurate brand logos instead of generic fallback icons;
  - QuickPick connection switcher now displays full-color official logos and precise product names.

## [0.9.6] - 2026-11-09

### 修复

- **Oracle 族结果集过滤语法修复**：修复在 Oracle、达梦及 OceanBase Oracle 模式租户下使用结果集 WHERE 筛选时，因派生子查询别名以下划线开头违背 Oracle 标识符规范导致报 ORA-00911: invalid character 语法错误的问题。

---

### Bug Fixes

- **Oracle Family Result Filter Syntax Fix**: Fixed an ORA-00911: invalid character syntax error when filtering results via WHERE conditions in Oracle, Dameng, and OceanBase Oracle mode, caused by a derived subquery alias starting with an underscore.

## [0.9.5] - 2026-11-08

### 修复

- **Oracle 连接服务名防篡改保护**：修复在资源树 Schema 节点新建 SQL 查询时，错误将 Oracle 服务名（Service Name / SID）覆盖为 Schema 用户名，导致后续重新连接报 `NJS-518` 无法连接的问题。
- **Oracle 超大数值与雪花算法 ID 精度保障**：修复 Oracle 查询 `NUMBER(19)` / Java `Long` 字段及雪花算法 ID 超过 53 位时因 JavaScript 浮点数转换导致精度丢失的问题，原生以高精度文本输出。
- **Oracle 与达梦日期时间本地格式化**：修复 Oracle 和达梦查询结果中的日期/时间字段显示为带 `T` 和 `Z` 的 UTC ISO 字符串问题，统一格式化为符合习惯的本地时间格式 `YYYY-MM-DD HH:mm:ss`。

---

### Bug Fixes

- **Oracle Service Name Overwrite Protection**: Fixed an issue where opening a new SQL query on a Schema node mistakenly overwrote the Oracle Service Name / SID with the schema username, causing subsequent reconnection failures with `NJS-518`.
- **Oracle Large Number & Snowflake ID Precision Fix**: Fixed precision loss on `NUMBER(19)`, Java `Long`, and 64-bit snowflake IDs exceeding 53 bits due to JavaScript IEEE-754 floating-point conversion by returning them as accurate strings.
- **Oracle & Dameng DateTime Local Formatting**: Fixed query results displaying datetime values as UTC ISO strings with `T` and `Z`, standardizing them into the intuitive local format `YYYY-MM-DD HH:mm:ss`.

## [0.9.4] - 2026-11-06

### 新增

- **OceanBase Oracle 兼容模式租户支持**：全面适配 Oracle 模式的连接，元数据管理，数据字典查询，SQL 方言与标识符引用。

### 优化

- **连接表单文件夹选项联动**：优化新建与编辑连接表单中的「文件夹」选项，仅在保存位置选择「项目」时展示，切换为「全局」保存时自动隐藏并重置，界面更清晰。

### 修复

- **编辑连接保存后缓存清理与状态重置**：修复编辑连接保存后未自动断开原连接及未彻底清除该连接内存/磁盘元数据缓存，导致修改 DuckDB 数据库文件路径等配置后刷新表数量显示为 0 的问题；同时补齐连接节点、Schema 顶级库节点与资源树全局刷新时的元数据缓存联动清理。

---

### Added

- **OceanBase Oracle Mode Tenant Support**: Fully adapted connection handling, metadata management, data dictionary queries, SQL dialect, and identifier quotes for Oracle mode.

### Improvements

- **Connection Form Folder Option Visibility**: Refined the "Folder" option in the connection form to display only when the save scope is set to "Project", automatically hiding and resetting it when switched to "Global".

### Bug Fixes

- **Connection Edit Cache Invalidation & Reconnect**: Fixed an issue where saving an edited connection did not disconnect the existing session or purge its in-memory and disk metadata caches, causing DuckDB and file databases to show 0 tables after updating file paths; also aligned metadata cache invalidation across connection nodes, schema nodes, and full explorer refreshes.

## [0.9.3] - 2026-11-03

### 修复

- **【紧急修复】项目连接转全局配置丢失**：修复把一条项目连接改回全局连接时，可能导致项目里其他已有连接被误清空的问题。

---

### Bug Fixes

- **[Hotfix] Project Connections Wipe Fix**: Fixed an issue where changing a project connection back to global could accidentally clear other existing connections in the project.

## [0.9.2] - 2026-11-03

### 新增

- **连接归属目录层级选择**：新建或编辑连接表单新增所属目录选择器，支持自由选择将连接存放在资源树根目录或任意指定文件夹下，便于多连接结构化组织。

### 优化

- **资源树连接节点视觉精简**：移除资源树上项目专属连接的「项目」文字徽标，保持树节点视觉简洁纯净。
- **项目连接隔离与全局自动清理**：将已有连接变更为项目连接保存时，自动从全局连接名单中移除对应记录，避免在其他工作区意外暴露。

### 修复

- **项目连接读写竞态修复**：优化保存项目连接与文件监听读取的执行时序为串行队列，彻底修复刚保存的连接可能被旧文件快照覆盖导致连接丢失的问题。

---

### Added

- **Target Folder Selection for Connections**: Added a folder selector in the connection form, enabling direct placement of new or edited connections into any explorer folder level or root directory.

### Improvements

- **Explorer Tree Label Streamlining**: Removed the "Project" badge from workspace connection nodes in the explorer for a cleaner, unified visual appearance.
- **Project Connection Isolation & Global Cleanup**: Automatically purges identical connection records from the global store when migrated to workspace-scoped storage, preventing cross-workspace leakage.

### Bug Fixes

- **Project Connection Race Condition Fix**: Serialized project connection save and reload operations to eliminate race conditions where freshly saved connections could be overwritten by stale file snapshots.

## [0.9.1] - 2026-11-02

### 优化

- 优化资源树加载速度，部分注册动作在加载展示树完成后进行。

---

### Improvements

- Optimized explorer tree loading performance by deferring non-essential provider registrations until after the connection tree is fully loaded.

## [0.9.0] - 2026-11-01

### 新增

- **Redis 族产品识别**：服务器信息与节点版本展示支持精准识别 Valkey 与 KeyDB 衍生产品，不再仅显示单一底层协议版本号。

### 优化

- **新建连接独立标签页打开**：编辑已有连接状态下点击新建连接支持多标签独立开启，避免无响应。
- **OceanBase / MySQL 族元数据查询超时保护**：展开数据库元数据查询设置 12 秒超时保护并自动重连，彻底避免系统架构（如 OceanBase 的 LBACSYS、SYS 等系统模式）因复杂字典查询长期阻塞整条连接的问题。

### 修复

- **Doris / SelectDB 握手连接修复**：修复连接 SelectDB 与 Doris 时因客户端发送连接属性导致服务端 FE 握手阶段主动断开连接 (`The server closed the connection`) 的问题。
- **StarRocks 同步物化视图支持与列查看修复**：修复 StarRocks 同步物化视图未在表列表中展示的问题（改从 `SHOW MATERIALIZED VIEWS` 动态补齐），并修复查看字段时因 `SHOW FULL COLUMNS` 报错改走 `DESC` 解析的问题。
- **PostgreSQL 族 Schema 刷新与兼容性修复**：
  - 修复多数据库切换后在 Schema 节点（如 public）点击刷新错误指向历史同名节点导致表和视图不刷新的问题；
  - 修复 CockroachDB 缺少 `pg_total_relation_size` 导致展开表列表报错的问题，并具备实例级兼容性自适应记忆；
  - 修复 Greenplum 6 缺少 `prokind` 列导致函数列表、参数及定义查看失败的问题；
  - 修复例程参数数组以文本形式返回时解析多出空白参数的问题；
  - 修复 Yugabyte 等环境查询 `pg_stat_replication` 长期挂起阻塞 Schema 展开的问题（跳过该查询并增加 3 秒语句超时保护），并在连接被服务端断开时支持树节点展开自动恢复重连。

---

### Added

- **Redis Variant Product Identification**: Server info and node badges now accurately identify Valkey and KeyDB fork engines rather than only displaying the underlying redis_version number.

### Improvements

- **Multi-Tab Connection Creation**: Clicking "New Connection" while editing an existing connection now opens in a new tab reliably.
- **MySQL & OceanBase Schema Expansion Timeout Protection**: Added a 12-second timeout protection with auto-reconnection for database metadata queries, preventing system schema inspections (e.g. LBACSYS / SYS on OceanBase) from hanging the entire connection.

### Bug Fixes

- **Doris & SelectDB Handshake Connection Fix**: Fixed connection failures with Doris and SelectDB caused by sending client connection attributes during handshake that triggered FE to disconnect (`The server closed the connection`).
- **StarRocks Synchronous Materialized Views & Column Inspection Fix**: Supplemented missing StarRocks synchronous materialized views from `SHOW MATERIALIZED VIEWS` and supported `DESC` fallback when `SHOW FULL COLUMNS` fails.
- **PostgreSQL-family Schema Refresh & Compatibility Fixes**:
  - Fixed an issue where refreshing a Schema node (e.g. public) after switching databases incorrectly targeted the previous database's node without updating tables/views;
  - Added graceful fallback and instance-level caching when CockroachDB lacks `pg_total_relation_size`;
  - Fixed function list, parameter, and definition errors in Greenplum 6 when `prokind` is absent;
  - Fixed routine parameter parsing when array returns as text, eliminating ghost parameters;
  - Fixed Schema expansion hanging on Yugabyte caused by blocked `pg_stat_replication` queries, adding a 3-second query timeout and auto-reconnection recovery for disconnected sessions.

## [0.8.45] - 2026-10-30

### 新增

- **MariaDB 序列分组支持**：MariaDB 数据库节点下新增「序列 (Sequences)」独立对象分组，便于直观浏览与管理序列。
- **数据库级系统内置类型展示**：支持 Schema 的数据库将系统内置数据类型提升至数据库节点下展示（与 Schema 并列），用户自定义类型保留在各 Schema 下，层级更加直观。

### 优化

- **资源树层级与聚合函数展示优化**：
  - 支持 Schema 的数据库将「性能监控」统一挂载在数据库节点上，与 Schema 保持并列；
  - PostgreSQL 族聚合函数严格按聚合元数据登记归类，仅出现在「聚合函数」分组，不再与普通函数重复混杂。

### 修复

- **openGauss / GaussDB 兼容性修复**：
  - 修复系统字典缺少 `attidentity` 字段导致表、视图和物化视图无法列出字段的问题；
  - 修复会话视图缺少 `backend_type` 字段导致性能监控无法查看活动会话的问题；
  - 修复查看存储过程和函数定义时包含冗余杂质的问题，精准提取并保留 `CREATE` 语句主体。

---

### Added

- **MariaDB Sequence Group**: Added a dedicated "Sequences" object group under MariaDB database nodes for intuitive sequence browsing and management.
- **Database-Level Built-in Types**: For databases with schemas, system built-in data types are now placed directly under the database node (alongside schemas), while user-defined types remain within their respective schemas.

### Improvements

- **Explorer Tree Hierarchy & Aggregate Functions Clean-up**:
  - Placed the "Performance Monitor" node at the database root level alongside schemas for cleaner navigation;
  - Categorized PostgreSQL-family aggregate functions strictly from `pg_aggregate`, displaying them exclusively under the aggregates group without mixing with regular functions.

### Bug Fixes

- **openGauss & GaussDB Compatibility Fixes**:
  - Fixed an issue where tables, views, and materialized views failed to list columns on systems without `attidentity`;
  - Fixed session monitor failures when `pg_stat_activity` lacks `backend_type`, smoothly displaying all active sessions;
  - Refined procedure and function definition inspection to cleanly retain only the `CREATE` statement.

## [0.8.44] - 2026-10-29

### 新增

- **Doris 异步物化视图独立分组与刷新**：Doris 异步物化视图单独归入「物化视图」分组展示（不再与基表混在一起），支持专属的完整刷新 (`REFRESH MATERIALIZED VIEW ... COMPLETE`) 操作。

### 修复

- **Doris 无可用 Backend (BE) 时展开库报错修复**：修复在 Doris 仅有前端 (FE) 存活、无可用 BE 时因底层元数据字典查询报错导致展开数据库失败的问题；增加平滑降级容错，确保表与视图正常列出。

---

### Added

- **Doris Asynchronous Materialized Views Group & Refresh**: Doris async materialized views are now grouped under "Materialized Views" instead of being mixed with base tables, with support for complete refresh (`REFRESH MATERIALIZED VIEW ... COMPLETE`).

### Bug Fixes

- **Doris Expansion Fix Without Active Backends**: Fixed an issue where expanding a Doris database failed when only frontends (FE) were active without available Backends (BE); added smooth fallback to ensure tables and views are listed reliably.

## [0.8.43] - 2026-10-28

### 新增

- **PostgreSQL / SQL Server 全对象元数据浏览**：
  - PostgreSQL：模式下补齐外部表、聚合函数与独立索引，数据库节点支持浏览扩展插件 (Extensions)、事件触发器、表空间、外部数据封装器 (FDW) 与外部服务器。
  - SQL Server：架构下补齐外部表、独立索引、序列、同义词、触发器与用户定义类型 (UDT)，数据库节点支持浏览数据库级触发器；展开过程与函数支持查看参数及返回值。
- **ClickHouse 系统函数分组浏览**：数据库节点下新增函数分组，可直接查阅 `system.functions` 系统函数及说明，自动隐藏空的存储过程分组。

### 优化

- **连接失败诊断与错误提示增强**：优化数据库连接异常捕获与格式化展示，清晰呈现具体报错根因（如端口拒绝 `ECONNREFUSED`、鉴权失败等），避免泛化的空白提示。

### 修复

- **SQL Server 动态驱动下载 404 修复**：修复 SQL Server 驱动下载时请求不存在的 `tedious@18.7.1` 导致 404 失败的问题，升级为 `tedious@19.2.2` 并补齐启动依赖。
- **SQL Server 过程与函数定义查看修复**：修复查看 SQL Server 函数和存储过程定义时误用 `SHOW CREATE` 报语法错误的问题，改为适配原生元数据解析。

---

### Added

- **PostgreSQL & SQL Server Comprehensive Object Catalogs**:
  - PostgreSQL: Added foreign tables, aggregate functions, and indexes under schemas; added extensions, event triggers, tablespaces, foreign data wrappers (FDW), and foreign servers under databases.
  - SQL Server: Added foreign tables, indexes, sequences, synonyms, triggers, and user-defined types (UDT) under schemas, plus database triggers under databases; expanding routines now details parameters and return types.
- **ClickHouse System Functions Group**: Added a functions group under ClickHouse database nodes to inspect system functions and descriptions from `system.functions`, automatically hiding empty procedure groups.

### Improvements

- **Connection Failure Diagnostics & Error Details**: Improved database connection error reporting to explicitly present underlying causes (such as port `ECONNREFUSED` or authentication rejections) instead of blank or generic failure notices.

### Bug Fixes

- **SQL Server Driver Download 404 Fix**: Fixed a 404 error caused by requesting non-existent `tedious@18.7.1`, upgrading to `tedious@19.2.2` with all necessary runtime dependencies bundled.
- **SQL Server Routine Definition Syntax Fix**: Fixed a syntax error where viewing SQL Server function or procedure definitions incorrectly sent MySQL-style `SHOW CREATE` statements, replacing it with native catalog queries.

## [0.8.42] - 2026-10-27

### 新增

- **支持 Apache Impala 数据库**：连接图库新增 Apache Impala 原生数据库卡片，预置默认端口 21050、NOSASL 认证模式及 `impala` Kerberos 服务名。

### 修复

- **HiveServer2 握手卡死修复**：修复 Hive 会话协议版本不匹配导致连接卡死超时的问题，增加协议版本向下自适应协商与重连机制，全部失败时提示核对 NOSASL 与 HTTP 端口。
- **Trino 动态驱动依赖缺失修复**：修复 Trino 动态驱动因缺少 `form-data` 依赖导致已安装却报找不到 `trino-client` 的问题；补齐安装依赖并为损坏的驱动缓存提供自动重新下载机制。
- **Trino 视图与特殊表名打开失败修复**：修复 Trino 视图检索以及包含点号的特殊表名（如 JMX 指标表）因未严格分段转义导致无法打开浏览的问题。

---

### Added

- **Apache Impala Support**: Added native Apache Impala database card in the connection gallery, preset with default port 21050, NOSASL authentication mode, and `impala` Kerberos service name.

### Bug Fixes

- **HiveServer2 Handshake Hang Fix**: Fixed connection timeout and hang issues caused by Hive protocol version mismatches; added automatic downward protocol version negotiation and reconnection fallback with clear troubleshooting hints.
- **Trino Dynamic Driver Dependency Fix**: Fixed an issue where the Trino driver failed to load `trino-client` due to missing `form-data` dependency; bundled required dependencies and added automatic re-download recovery for broken driver caches.
- **Trino Views & Special Table Open Fix**: Fixed an issue where Trino views or dotted table names (such as JMX metrics tables) failed to open due to improper identifier quoting.

## [0.8.41] - 2026-10-26

### 新增

- **Oracle / 达梦表约束与触发器展示**：展开表节点支持展示唯一约束、检查约束、外键以及表级触发器，达梦自动解析 `TRIGGERING_TYPE`，并智能过滤 `SYS_C` 系统非空检查约束。
- **Oracle 全对象元数据浏览**：模式下完整支持物化视图、队列、类型、包、同义词、Java、作业、调度器、数据库连接及回收站等丰富对象分组浏览；字典无权限时自动隐藏。
- **过程与函数参数列表展示**：达梦与 Oracle 展开存储过程和函数支持查看参数名称、出入参方向（IN / OUT / IN OUT）与数据类型，函数独立呈现返回值定义。
- **达梦模式表触发器分组**：达梦模式节点下新增「表触发器」对象分组，集中列出当前模式的全部表级触发器。

### 修复

- **清除错误右键菜单**：修复表、数据库、对象分组等非连接节点右键错误显示「断开连接」菜单的问题，仅保留在已连接的连接根节点上。
- **Oracle 连接串服务名解析修复**：解决仅填写主机与端口时可能误连入监听默认库（如多租户实例根容器 CDB）的问题，确保配置的服务名 (Service Name) 或 SID 正确写入连接串。

---

### Added

- **Oracle & Dameng Table Constraints and Triggers**: Expanding table nodes now displays unique constraints, check constraints, foreign keys, and triggers; Dameng resolves `TRIGGERING_TYPE` correctly and system-generated `SYS_C` non-null constraints are filtered out.
- **Oracle Comprehensive Object Catalog**: Expanding schemas now supports materialized views, queues, types, packages, synonyms, Java, jobs, scheduler, database links, and recycle bin; groups are gracefully hidden when dictionary permissions are missing.
- **Procedure and Function Parameters Display**: Expanding procedures and functions in Dameng and Oracle displays parameter names, directions (IN / OUT / IN OUT), and data types, with functions explicitly showing return types.
- **Dameng Schema Table Triggers Group**: Added a "Table Triggers" group under Dameng schema nodes to display all table triggers owned by the current user.

### Bug Fixes

- **Remove Erroneous Context Menus**: Fixed an issue where the "Disconnect" context menu incorrectly appeared on non-connection nodes (such as tables, databases, and object groups), ensuring it only displays on connected root nodes.
- **Oracle Connection String Service Name Resolution**: Fixed an issue where connecting with only host and port could route to the listener's default database (often CDB$ROOT in multitenant instances), ensuring the configured Service Name or SID is properly included in the connection string.

## [0.8.40] - 2026-10-25

### 优化

- **DuckDB 驱动升级至 2.0**：升级 DuckDB 驱动至 2.0，原生支持打开 2.0 新格式数据库文件；已安装环境自动平滑升级且向后兼容旧版库文件。
- **SQL Server 表统计性能优化**：展开 Schema 时通过分区元数据一次性带出基表行数与存储占用，无需逐表计数。

### 修复

- **带注释 SQL 查询识别修复**：修复 DuckDB、ClickHouse、Hive、Trino 在执行带前置注释的 SELECT 语句时误判为非查询（显示 0 行受影响）的问题。

---

### Improvements

- **DuckDB Driver Upgrade to 2.0**: Upgraded DuckDB driver to 2.0 with native support for opening DuckDB 2.0 database files; existing setups upgrade automatically with backward compatibility for older files.
- **SQL Server Table Stats Batching**: Retrieved base table row counts and storage sizes in a single query via partition metadata when expanding schemas, eliminating per-table query overhead.

### Bug Fixes

- **Comment-Prefixed Query Detection Fix**: Fixed an issue where SELECT statements prefixed with comments were misidentified as non-queries (showing 0 rows affected) in DuckDB, ClickHouse, Hive, and Trino.

## [0.8.39] - 2026-10-25

### 优化

- SQLite 表节点统计显示优化。

---

### Improvements

- Optimized table statistics display for SQLite table nodes in the explorer.

## [0.8.38] - 2026-10-23

### 新增

- **结果网格连续加载模式**：结果集网格底栏新增「连续加载」模式（可与传统分页随时切换）；滚到底部自动追加下批数据，并提供精确单行步进控制（上一行/下一行/首行/末行）与末行微览；齿轮菜单支持配置默认加载方式及每页行数联动。
- **连接节点版本悬停提示**：资源树连接根节点在悬停提示 (Tooltip) 中增加数据库版本号展示。

### 优化

- **网格行号列宽自适应**：行号列宽度依据当前最大行号位数自动计算加宽，确保高行数完整呈现不被遮挡。

---

### Added

- **Continuous Loading (Infinite Scroll) for Result Grid**: Added a "Continuous Loading" mode alongside traditional pagination in the grid footer; automatically appends next pages on scroll to bottom with row-by-row navigation buttons (Previous/Next/First/Last Row); settings gear menu supports toggling default paging mode and configuring page size.
- **Connection Tooltip Version Display**: Displays the detected database version in the connection node tooltip within the database explorer.

### Improvements

- **Dynamic Row Number Column Width**: Automatically widens the grid row index column based on the number of digits of the maximum loaded row.

## [0.8.37] - 2026-10-22

### 新增

- **MySQL 9+ 向量列支持 (VECTOR)**：可视化建表与结构编辑新增 ector(n) 类型；结果集网格按浮点数组只读展示向量数据。
- **MySQL JSON Duality View 支持**：资源树新增 JSON Duality View 独立分组，支持数据只读预览与基表映射权限检视。
- **MySQL 9+ 程序库与四段版本识别**：新增程序库 (Libraries) 对象分组与定义查看；JavaScript 存储过程标注语言类型；支持解析展示 9.7.4+ 四段版本号。

### 优化

- **动态列类型识别与筛选优化**：无元数据类型的结果列按内容自适应推断数值与日期，修复数值/日期筛选退化为文本列表与跨列串值问题；筛选弹窗支持按 Esc 或点击外部自动收起。
- **分页与单次查询上限逻辑优化**：`dmsql.maxRows` 默认调整为 0（不限制）；数据快速查看在启用分页时翻页 SQL 不再夹带单次最大行数限制，便于大表后翻。
- **MySQL 执行计划树展开兼容**：优化 `EXPLAIN FORMAT=JSON` 解析，兼容带 `query_plan` 根节点的执行计划树展示。

### 修复

- **SQLite 注释前缀查询解析修复**：修复 SQLite 执行带注释前缀的 SELECT 语句被误识别为非查询（显示 0 行受影响）且无法分页的问题。

---

### Added

- **MySQL 9+ VECTOR Column Support**: Added ector(n) data type in visual table designer; result grid displays vector embeddings as read-only float arrays.
- **MySQL JSON Duality View Support**: Added dedicated tree grouping for JSON Duality Views, supporting read-only previews and underlying base table operation inspection.
- **MySQL 9+ Libraries & 4-Part Version Display**: Added Libraries object grouping in explorer; added language badge for JavaScript routines; supported 4-part version strings for MySQL 9.7.4+.

### Improvements

- **Dynamic Column Type Inference & Filter Polish**: Inferred numeric and datetime types from cell contents when column types are missing, fixing filter dropdown issues and column value leakage; filter panels now close on outside click or Esc.
- **Pagination & Query Row Limit Polish**: Changed default `dmsql.maxRows` to 0 (unlimited); data preview pagination no longer appends maxRows to pagination SQL, enabling deep pagination on large tables.
- **MySQL JSON Explain Plan Compatibility**: Enhanced `EXPLAIN FORMAT=JSON` parser to render plan trees for statements with `query_plan` roots.

### Bug Fixes

- **SQLite Comment-Prefixed SELECT Fix**: Fixed an issue where SQLite SELECT queries prefixed with SQL comments were misidentified as non-queries (showing 0 rows affected) and failed to paginate.

## [0.8.36] - 2026-10-20

### 优化

- 部分图标以及文字菜单显示优化。

---

### Improvements

- Optimized display for selected icons and text menus.

## [0.8.35] - 2026-10-19

### 新增

- **SQL 笔记本单元格连接锁定**：支持将代码单元格锁定到当前数据库连接。

---

### Added

- **SQL Notebook Cell Connection Locking**: Added support for locking individual code cells to their current database connection.

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

