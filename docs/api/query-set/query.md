<a name="queryset.query"></a>
# queryset.query

<a name="queryset.query.Query"></a>
## Query Objects

```python
class Query()
```

<a name="queryset.query.Query._init_sorted_orders"></a>
#### \_init\_sorted\_orders

```python
 | _init_sorted_orders() -> None
```

Initialize empty order_by dict to be populated later during the query call

<a name="queryset.query.Query.apply_order_bys_for_primary_model"></a>
#### apply\_order\_bys\_for\_primary\_model

```python
 | apply_order_bys_for_primary_model() -> None
```

Applies order_by queries on main model when it's used as a subquery.
That way the subquery with limit and offset only on main model has proper
sorting applied and correct models are fetched.

<a name="queryset.query.Query._apply_default_model_sorting"></a>
#### \_apply\_default\_model\_sorting

```python
 | _apply_default_model_sorting() -> None
```

Applies orders_by from model Meta class (if provided), if it was not provided
it was filled by metaclass so it's always there and falls back to pk column

<a name="queryset.query.Query._pagination_query_required"></a>
#### \_pagination\_query\_required

```python
 | _pagination_query_required() -> bool
```

Checks if limit or offset are set, the flag limit_sql_raw is not set
and query has select_related applied. Otherwise we can limit/offset normally
at the end of whole query.

**Returns**:

`bool`: result of the check

<a name="queryset.query.Query.build_select_expression"></a>
#### build\_select\_expression

```python
 | build_select_expression() -> Tuple[sqlalchemy.sql.select, List[str]]
```

Main entry point from outside (after proper initialization).

Extracts columns list to fetch,
construct all required joins for select related,
then applies all conditional and sort clauses.

Returns ready to run query with all joins and clauses.

**Returns**:

`sqlalchemy.sql.selectable.Select`: ready to run query with all joins and clauses.

<a name="queryset.query.Query._build_pagination_condition"></a>
#### \_build\_pagination\_condition

```python
 | _build_pagination_condition() -> Tuple[
 |         sqlalchemy.sql.expression.TextClause, sqlalchemy.sql.expression.TextClause
 |     ]
```

In order to apply limit and offset on main table in join only
(otherwise you can get only partially constructed main model
if number of children exceeds the applied limit and select_related is used)

Used also to change first and get() without argument behaviour.
Needed only if limit or offset are set, the flag limit_sql_raw is not set
and query has select_related applied. Otherwise we can limit/offset normally
at the end of whole query.

The condition is added to filters to filter out desired number of main model
primary key values. Whole query is used to determine the values.

<a name="queryset.query.Query._apply_expression_modifiers"></a>
#### \_apply\_expression\_modifiers

```python
 | _apply_expression_modifiers(expr: sqlalchemy.sql.select) -> sqlalchemy.sql.select
```

Receives the select query (might be join) and applies:
* Filter clauses
* Exclude filter clauses
* Limit clauses
* Offset clauses
* Order by clauses

Returns complete ready to run query.

**Arguments**:

- `expr` (`sqlalchemy.sql.selectable.Select`): select expression before clauses

**Returns**:

`sqlalchemy.sql.selectable.Select`: expresion with all present clauses applied

<a name="queryset.query.Query._reset_query_parameters"></a>
#### \_reset\_query\_parameters

```python
 | _reset_query_parameters() -> None
```

Although it should be created each time before the call we reset the key params
anyway.

