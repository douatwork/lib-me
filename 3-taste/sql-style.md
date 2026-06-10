---
category: taste
tags: [sql, styling, linting, formatting, dialect]
last_updated: 2026-06-09
agent_action: "Read this file before generating or modifying any SQL scripts."
---

# SQL Style

## Core Rules

- Lowercase all keywords: `select`, `from`, `where`, `join`, `left join`, `inner join`, `on`, `and`, `or`, `case`, `when`, `then`, `else`, `end`, `with`, `as`, `group by`, `order by`, `over`, `partition by`, `window`, `having`, `insert`, `create`, `drop`, `table`, `temp`
- Leading commas in SELECT lists — first column no comma, subsequent start with `, `
- 4-space indentation, never tabs
- Space before/after operators: `=`, `<>`, `!=`, `>`, `<`, `>=`, `<=`, `+`, `-`, `*`, `/`
- No space after function names: `max(column)` not `max (column)`

## SELECT — Leading Commas

```sql
select
    first_column
    , second_column
    , third_column
from table_name
```

## CTEs

Opening `with cte_name as (` on one line. `)` on its own line, no indent. Subsequent CTEs start with `, cte_name as (`.

```sql
with cte_name as (
    select column1, column2 from table_name
)
, another_cte as (
    select column1, column2 from cte_name
)
select column1, column2 from another_cte
```

## JOINs

`join` at same level as `from`. `on` indented +4. Multiple conditions on own lines with `and`/`or` at the start.

```sql
from table1 t1
left join table2 t2
    on t1.id = t2.id
    and t1.date >= t2.start_date
```

## WHERE

Same level as `from`/`join`. Each condition on its own line, `and`/`or` at the start.

```sql
where
    column1 = 'value'
    and column2 > 100
```

## CASE

Simple (fits one line): `, case when condition then value end as column_alias`

Complex (multi-line): `case` at column level, `when`/`else` indented +4, `end` at same level as `case`.

```sql
, case
    when b.amount > 100 then 'high'
    when b.amount > 50 then 'medium'
    else 'low'
    end as category
```

## Window Functions

Simple: `, sum(amount) over (partition by customer_id order by date) as running_total`

Complex:
```sql
, row_number()
    over (partition by vehicle_id
    order by effective_at rows unbounded preceding) as row_num
```

## CREATE TABLE

```sql
create table schema.table_name (
    column1 varchar(40)
    , column2 integer
)
distkey(column1)
sortkey(column2)
;
```

## INSERT

```sql
insert into table_name (column1, column2, column3)
select value1, value2, value3 from source_table
```

## Transformation Reference

| Pattern | Action |
|---------|--------|
| Uppercase keywords | Lowercase |
| Trailing commas in SELECT | Leading commas |
| Tab indentation | 4-space indentation |
| Inline multi-column SELECT | Multi-line |
| Short CASE (fits one line) | Keep inline |
| Complex CASE | Multi-line format |
