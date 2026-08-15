# DMSQL **- Database Manager SQL Tool**

面向开发者的 VS Code 全能数据库客户端扩展，**支持 MySQL、MariaDB、Oracle、PostgreSQL、人大金仓、GaussDB、openGauss、SQL Server、SQLite、DuckDB、ClickHouse、Doris、达梦 (Dameng)、Hive** 等 14 种数据库。纯本地运行，隐私安全无遥测，密码采用 SecretStorage 加密存储。连接管理、SQL 开发、可视化表设计、结果集分析、数据导入导出、模拟数据生成与性能监控一站式完成。

- **官网**：[https://dmsql.top](https://dmsql.top)
- **操作文档**：[https://dmsql.top/docs](https://dmsql.top/docs)
- **English docs**: [https://dmsql.top/docs?lang=en](https://dmsql.top/docs?lang=en)
- **问题反馈 / 功能需求**：请到 GitHub 提交 [Issue](https://github.com/dmsql123/dmsql/issues)

---

## 核心亮点

- **多数据库支持**：支持 MySQL、MariaDB、Oracle、PostgreSQL、人大金仓、GaussDB、openGauss、SQL Server、SQLite、DuckDB、ClickHouse、Doris、达梦 (Dameng)、Hive。
- **纯本地安全**：零云端依赖，密码通过 VS Code SecretStorage 硬件/系统级安全加密。
- **可视化表结构设计器**：支持可视化新建/修改表结构，字段类型、主键、自增、索引、外键约束一站式图形化配置与 DDL 预检。
- **智能模拟数据生成器**：支持基于字段语义的 Faker 模拟数据生成（42+ 内置语义类型），并支持配置 OpenAI/DeepSeek 协议通过 AI 生成逼真测试数据。
- **高级数据分析网格**：基于 AG Grid 构建，支持行列数据转置 (Transpose)、透视模式 (Pivot)、单元格/行深度检视器、DBeaver 风格选择与在线数据编辑持久化。
- **数据迁移与导入导出**：支持 CSV、Excel (xlsx)、JSON、SQL 脚本导出；支持跨库/跨表数据抽取及本地文件批量导入（含智能列映射与主键冲突策略）。
- **实时监控与历史审计**：提供活动会话监控（Session Monitor）与 Kill 连接功能；全量 SQL 执行历史可追溯、可搜索与一键复用。
- **安全护栏与连接保活**：只读连接保护拦截写 SQL、高危指令 (DROP/TRUNCATE/无 WHERE 的 DELETE/UPDATE) 强提醒红色确认；定时心跳保活防止断线。

---

## 快速开始

1. 点击左侧活动栏的 **DMSQL** 图标打开数据库浏览器。
2. 点击 **➕ 新建连接**（或在命令面板输入 `DMSQL: 新建连接`），选择目标数据库类型并按向导填写配置。
3. 在树中展开连接节点（自动连接），支持通过文件夹分组管理多个连接，按 **连接 → 数据库/Schema → 表/视图/存储过程/函数/定时任务** 浏览。
4. 打开或新建 `.sql` 文件，选中要执行的 SQL 语句（未选中则执行全文），点击编辑器右上角 ▶ 或按 `Ctrl+Enter` / `Cmd+Enter` 运行。
5. 执行结果将在结果面板分栏中呈现；表/视图节点右键可一键查看数据、可视化修改结构、生成模拟数据、导入导出等。

---

## 问题反馈与需求

遇到 Bug 或希望增加功能，请到 GitHub 仓库提交 Issue，**不要**在其他渠道重复提交。

- 提交入口：[https://github.com/dmsql123/dmsql/issues/new/choose](https://github.com/dmsql123/dmsql/issues/new/choose)
- 仓库地址：[https://github.com/dmsql123/dmsql](https://github.com/dmsql123/dmsql)
- **Bug**：请选择 Bug 模板，写明 IDE（VS Code / Cursor / Windsurf / Trae 等）、扩展版本、数据库类型，以及可复现步骤。
- **需求**：请选择 Feature 模板，写明使用场景和期望行为。

完整操作文档请看官网 [https://dmsql.top/docs](https://dmsql.top/docs)（[English docs](https://dmsql.top/docs?lang=en)）。本 GitHub 仓库仅提供发布说明与问题跟踪，**不包含源代码**。

---

## 数据库浏览器 (Explorer)

- **完整对象树**：连接 → 数据库/Schema → 数据表 / 视图 / 存储过程 / 函数 / 定时任务 → 字段 (标记主键/注释) / 索引 / 外键。
- **分组与筛选**：支持连接文件夹分组 (Folder Group)，节点支持名称搜索/增量筛选，实时显示各节点子对象数量。
- **右键丰富指令**：
  - **连接节点**：重新连接、设为活动连接、编辑/复制连接、复制连接串、移动到文件夹、新建数据库、查看历史、筛选节点。
  - **数据库节点**：新建 SQL 文件、备份数据库、还原数据库、批量导出表信息、删除数据库。
  - **表/视图节点**：查看数据 (Preview)、查看表信息、查看 DDL、**新建/编辑表结构**、**生成模拟数据**、数据导入/导出、**生成 SQL 脚本** (SELECT/INSERT/UPDATE/DELETE/DDL)、截断/删除表。
  - **存储过程/函数/任务**：查看定义 SQL、生成运行 SQL、启用/禁用定时任务、删除。

---

## 可视化表结构设计器 (Table Designer)

- **可视化建表/改表**：图形化编辑字段名、数据类型、长度/精度、Null 可空、默认值与列注释。
- **主键与自增**：一键勾选设置单列/复合主键与自增属性。
- **索引管理 (Indexes)**：可视化添加/修改/删除索引，选择单列或联合字段，支持 Primary、Unique、Normal、FullText 等索引类型。
- **外键约束 (Foreign Keys)**：可视化配置主从外键关联及 `ON DELETE` / `ON UPDATE` 级联行为 (CASCADE, SET NULL, RESTRICT, NO ACTION)。
- **DDL 预检与执行**：修改完成后自动对比差异生成准确的 `CREATE TABLE` 或 `ALTER TABLE` DDL 代码，确认后一键应用。

---

## SQL 编辑器与智能辅助

- **执行与切分**：`Ctrl+Enter` / `Cmd+Enter` 运行当前语句或选区，智能切分多条 SQL，支持在独立新标签页运行。
- **CodeLens 运行按钮**：在 SQL 语句上方动态呈现「运行语句 / 选择连接 / 选择数据库」交互条。
- **Schema 智能补全**：基于当前连接元数据实现纯本地表名、视图名、列名智能自动补全。
- **关键字自动大写**：输入 `select` 自动转为 `SELECT`，补全候选词保持标准方言大写。
- **SQL 格式化**：`Ctrl+Alt+F` / `Cmd+Alt+F` 一键格式化，智能适应 MySQL / Oracle PL/SQL / PG / SQLite 方言。
- **语句块高亮**：浅色背景动态跟随标注光标当前所在 SQL 块。
- **危险 SQL 拦截**：针对 `DROP`、`TRUNCATE` 以及无 `WHERE` 条件的 `DELETE` / `UPDATE` 弹出红色强提醒二次确认。
- **只读连接保护**：标记为只读的连接全自动阻断任何 `INSERT` / `UPDATE` / `DELETE` / `DDL` 写操作。

---

## 结果面板 (Result View)

- **高性能 AG Grid**：支持排序、多列筛选、自适应列宽、物理服务端分页 (`pageSize`) 与按需懒加载。
- **数据编辑与持久化**：双击单元格在线修改、新增行、删除行；支持按主键精确回写或无主键全行比对提交（带风险提示）。
- **单元格/行深度检视器**：支持针对长文本、JSON、BLOB 等大字段弹窗专门检视与格式化展示。
- **行列转置 (Data Transpose)**：按快捷键 `Tab` 实现行列互换，支持选中局部区域转置。
- **透视模式 (Pivot Mode)**：支持按列分组与聚合计算 (`count` / `sum` / `avg` / `min` / `max`)。
- **列管理侧边栏**：动态勾选显示/隐藏指定列、拖拽调整列顺序。
- **多结果集 Tab**：批量执行多条 SQL 自动分栏呈现各个结果集。
- **数据复制与导出**：TSV 快捷复制选中数据（显式标注 `[NULL]`）；支持直接导出 CSV、Excel (xlsx)、JSON 或 SQL 脚本。

---

## 数据导入与跨库迁移

- **多格式本地文件导入**：支持导入 CSV、XLSX、JSON、SQL 文件，智能自动解析文件首行列头。
- **跨库/跨表抽取**：选择任意已有连接中的库表作为源数据，无缝抽取导入到目标表中。
- **智能列映射**：源字段与目标字段自动模糊匹配（忽略大小写、下划线及空格），支持手动调整映射规则。
- **主键冲突处理策略**：支持选择「报错终止」、「跳过重复行」或「替换覆盖」。导入前自动预检冲突并展示进度与结果统计。

---

## 智能模拟数据生成器 (Mock Data)

- **语义推断与 Faker 规则**：根据列名与字段类型自动匹配 Faker 规则，覆盖 42+ 内置语义类型（如姓名、身份证、手机号、公司、地址、IP、金额、日期等）。
- **自定义映射配置**：支持在 VS Code `settings.json` 中配置固定列名的语义映射规则。
- **AI 智能生成模式**：可配置 OpenAI / DeepSeek 等标准 API 接口，通过 AI 大语言模型生成符合现实业务上下文的复杂高质测试数据。
- **安全分批写入**：支持配置每批插入条数及硬上限保护 (最高 10,000 条)。

---

## 实时监控与历史审计

- **数据库会话监控 (Monitoring View)**：实时查看当前连接的所有活动进程与会话，展示 Client IP、User、Database、Command、运行时间与 SQL 详情，支持一键终止 (Kill) 连接。
- **SQL 执行历史 (History View)**：全量记录 SQL 执行历史、执行耗时、影响行数与状态；支持按连接、时间段、关键词筛选并一键复制或在编辑器中重新运行。

