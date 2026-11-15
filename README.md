# DMSQL **- Database Manager SQL Tool**

[中文 README](./README.zh-CN.md)

A VS Code database client for developers. It supports **34 databases**: MySQL, MariaDB, TiDB, OceanBase, PolarDB MySQL, TDSQL MySQL, GoldenDB, Oracle, PostgreSQL, CockroachDB, YugabyteDB, Greenplum, PolarDB PostgreSQL, HighGo, Vastbase, UXDB, Kingbase, GaussDB, openGauss, SQL Server, Azure SQL, SQLite, DuckDB, ClickHouse, Doris, StarRocks, SelectDB, Dameng, Hive, Impala, Redis, Valkey, KeyDB, and Trino. Connection management, SQL development, visual table design, result analysis, import and export, mock data, and performance monitoring are in one extension.

- **Website**: [https://dmsql.top](https://dmsql.top)
- **Docs**: [https://dmsql.top/docs?lang=en](https://dmsql.top/docs?lang=en)
- **中文文档**: [https://dmsql.top/docs](https://dmsql.top/docs)
- **Issues and requests**: open a GitHub [Issue](https://github.com/dmsql123/dmsql/issues) or email [service@dmsql.top](mailto:service@dmsql.top)

---

![DMSQL](https://cdn.jsdelivr.net/gh/dmsql123/dmsql@main/docs/1.png)

## Highlights

- **Chinese and English**: pages and settings follow the editor language, and `Dmsql: Locale` can override it.
- **Privacy**: passwords stay in local VS Code SecretStorage.
- **Visual table designer**: create and alter tables, including types, primary keys, identity, indexes, and foreign keys, with a DDL preview.
- **Mock data**: Faker rules from column meaning (42+ built-in kinds), plus OpenAI or DeepSeek compatible APIs for realistic rows.
- **Result grid**: AG Grid with transpose, pivot, cell and row inspectors, DBeaver-style selection, and inline edit and save.
- **Import and export**: CSV, Excel (xlsx), JSON, and SQL scripts. Pull rows from another connection or table, or import a local file, with column mapping and primary-key conflict handling.
- **Monitoring and history**: live sessions with Kill, plus a searchable SQL history you can rerun.
- **Safety and keep-alive**: read-only connections block writes. DROP, TRUNCATE, and DELETE or UPDATE without WHERE ask for confirmation. A heartbeat keeps idle connections up.
- **Session monitor**: client IP, user, database, command, runtime, and SQL, with one-click Kill.
- **SQL history**: duration, row counts, and status. Filter by connection, time, or keyword, then copy or run again.

---

## Quick start

1. Open the database explorer from the **DMSQL** icon in the activity bar.
2. Click **+ New Connection** (or run `DMSQL: New Connection`), pick the database type, and fill in the wizard.
3. Expand the connection. Group connections in folders, then browse **connection → database/schema → tables, views, procedures, functions, and events**.
4. Open or create a `.sql` file. Select the statements to run, or run the whole file. Use the ▶ button or `Ctrl+Enter` / `Cmd+Enter`.
5. Results open in the result panel. Right-click a table or view to preview data, edit the structure, generate mock data, or import and export.

---

## Database explorer

- **Object tree**: connection → database/schema → tables, views, procedures, functions, and events → columns (primary key and comment), indexes, and foreign keys.
- **Folders and filters**: folder groups for connections, name search, and child counts on each node.
- **Context menu**:
  - **Connection**: connect, disconnect, set active, edit or duplicate, copy the connection string, move to a folder, create a database, open history, filter nodes.
  - **Database**: new SQL file, backup, restore, batch-export table info, drop database.
  - **Table or view**: preview data, table details, DDL, **design table**, **mock data**, import and export, **SQL scripts** (SELECT / INSERT / UPDATE / DELETE / DDL), truncate or drop.
  - **Procedure, function, or event**: show definition, generate run SQL, enable or disable an event, drop.

---

## Visual table designer

- Edit column name, type, length and precision, nullability, default, and comment.
- Set a single or composite primary key, and identity.
- Add, change, or drop indexes on one or more columns: Primary, Unique, Normal, FullText, and others.
- Configure foreign keys and `ON DELETE` / `ON UPDATE` actions (CASCADE, SET NULL, RESTRICT, NO ACTION).
- Review the generated `CREATE TABLE` or `ALTER TABLE` before applying it.

---

## SQL editor

- `Ctrl+Enter` / `Cmd+Enter` runs the current statement or the selection. Multiple statements are split, and a statement can open in its own tab.
- CodeLens above a statement: run, pick a connection, pick a database.
- Local completion for tables, views, and columns from the current connection.
- Typing `select` becomes `SELECT`. Completions stay in the dialect's usual case.
- `Ctrl+Alt+F` / `Cmd+Alt+F` formats SQL for MySQL, Oracle PL/SQL, PostgreSQL, and SQLite.
- The current statement block is highlighted.
- `DROP`, `TRUNCATE`, and `DELETE` / `UPDATE` without `WHERE` ask for confirmation.
- A read-only connection blocks `INSERT`, `UPDATE`, `DELETE`, and DDL.

---

## Result view

- Sort, filter columns, fit column width, server-side paging (`pageSize`), and load more on demand.
- Double-click a cell to edit. Insert and delete rows. Save by primary key, or by comparing the whole row when there is no key.
- Inspect long text, JSON, and BLOB values in a dedicated view.
- Press `Tab` to transpose rows and columns, including a selected region.
- Pivot mode groups columns and aggregates with `count`, `sum`, `avg`, `min`, and `max`.
- Show or hide columns, and drag them to reorder.
- Several statements open as separate result tabs.
- Copy a selection as TSV, with `[NULL]` marked. Export CSV, Excel (xlsx), JSON, or SQL.
- Theme the grid header, row background, text, and grid lines, or follow the editor theme.

![DMSQL](https://cdn.jsdelivr.net/gh/dmsql123/dmsql@main/docs/4.png)

---

## Import and cross-database copy

- Import CSV, XLSX, JSON, and SQL. The first row is treated as headers.
- Use any connected table as the source and load it into the target table.
- Columns are matched loosely (case, underscores, and spaces). You can adjust the mapping.
- On a primary-key conflict, stop with an error, skip the row, or replace it. Conflicts are checked before import, with progress and a summary.

---

## Mock data

![DMSQL](https://cdn.jsdelivr.net/gh/dmsql123/dmsql@main/docs/2.png)

- Column names and types pick a Faker rule from 42+ kinds, such as name, national ID, phone, company, address, IP, money, and date.
- Pin a column name to a semantic rule in VS Code `settings.json`.
- An OpenAI-compatible API can generate rows that fit a business context.

---

**Issues and requests**: open a GitHub [Issue](https://github.com/dmsql123/dmsql/issues) or email [service@dmsql.top](mailto:service@dmsql.top)
