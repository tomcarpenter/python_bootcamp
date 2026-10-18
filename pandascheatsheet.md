
# Series Methods
- `.map()`
- `.groupby()` --> `.transform()`
- `.value_counts()`

- `.sum()` `.min()` `.max()`
- `.mean()` `.median()` `.mode()[0]` RETURNS SERIES, GRAB 1st

- `.std()` `var()`
- `.size` NO PAREN .. same as `leng(ser)`
- `.count()` OMITS MISSINGS!
- `.nunique()`
- `.quantile(.25)`

- membership: `.isin()` `.between()`
- missings: `.isna()` `.notna()` `.fillna()` `.dropna()`
    -`.fillna()` can accept a value OR a series that matches on index and subs in when missing
- recoding: 
    - `.map(dict)` (anything unnamed is nan) .... `.replace()` (same thing but only modifies named vars)
    - `.astype()` to cast
- duplicates: `.duplicated()`  `.drop_duplicates()` `.unique()`
- numeric: `.round()` `.abs()` `.clip()` (winsorizes)
- cumulative: `.cumsum()` `.cumcount()` `.shift()` (cumcount runs on GROPUED ONLY)
- string: `.str.lower()` `.str.contains()` `.str.strip()`
- conditional: `.where()` keeps if true. `.mask` keeps if false

In general, many series methods like this pattern:
- `d.groupby("class")["fare"].rank(method="first",ascending = False)`
- `d.groupby("class")["fare"].quantile([.25, .50, .75])`
- `d.groupby("class")["fare"].transform("quantile", .75)`
- `d.groupby("class")["fare"].cumcount()`

in general, transform dicts are {"old": "new"}

- `round(df, 2)`
    - `df.round(2)`
    - `df["col"] = df["col"].round(2)`
    - rounding ignores categorical

# Make categorical series
```
pd.Categorical(
    df["a"],
    categories = ["q", "r" "s"],
    ordered=True
)
```

# cut
```
pd.cut(
    df["col"],
    bins=[0, 18, 40, 60, np.inf],
    labels=["Child", "Young Adult", "Middle Aged", "Older"],
    include_lowest=True
)
```

`pd.quct(df["col"], 4)`

# DF Methods

### INSPECT:
- NO PAREN: `.shape`  `.columns`  `.size`  `len(df)`   `.dtypes`
- `.info()` `.describe()`
- `.select_dtypes(include=["number", "object", "category"])`

### SELECT FILTER
- `df["col"]` --> series
- `df["col"].to_frame("colname")` --> df with one frame
- `df[["a", "b"]]` --> df
- `.loc[rows, cols]` select rows/cols by boolean OR LISTS OF LABELS
- `.iloc[rows, cols]` select rows/cols by numeric position OR LISTS OF NUMERIC POSITION

### MUTATE / RECODE
- `.assign(new = lambda d: ...)`
    - DO GROUPING INSIDE ASSIGN, NOT BEFORE IT AS YOU DO IN TIDYVERSE
- `.rename(columns={"old": "new"})`
- `.replace({"old": "new"})` runs for entire df
- `.astype({"col": "category"|"int64"|"float64"|"object"|"string"|"bool"})` cast columns
- `.drop(columns = ["col1", "col2"])`

### Sort/Duplicate/Missing
- `.sort_values(["a", "b"], ascending=[True, False])`
- `.sort_index()` #reset df order using index
- `.nlargest(5, "fare")`  `.nsmallest(5, "fare")`
    - `.nlargest(n=10, columns=["fare", "age"], keep="all")` allows tiebreaker columns and a keep for ties
- `.duplicated(subset=["col", "col2"])` flag duplicated rows overall or on a subset of columns
- `.drop_duplicates(subset=["col1", "col2"])` either overall or subset
- `.isna().sum()`
- `.dropna(subset=["col1", "col2"])`
- `.fillna({"col": "val"})` at df level, must provide a dict of cols

### GROUP SUMMARIZE
- `.groupby(["var", "var2"], observed=True)`
- `.groupby().agg()`
    - `.agg(var = ("col", lambda d: d.mean()))`
- `.groupby("a")["b"].transform(lambda d: d.mean())` 
- `.groupby("a").head(5)` first 5 rows per group
- `.groupby("a").size()` count rows per group
- `.groupby("a")["col"].count()` count nonmissing col
- `.reset_index()`

### SUMMARIZE
- `.agg(["func1", "func2", "func3"])`

### GROUP WINDOW
- `.groupby("group")["column"].rank(method="first"|"min"|"dense", ascending=True)` is the equilvanet of a window function
    - method="first" ROW_NUMBER (one value per row, e.g., 1,2,3)
    - method="min" (RANK() preserves position and skips values when duplicates e.g., 1,1,3)
    - method="dense" (doesn't preserve position e.g., 1,1,2)
    - ascending=False or True if you want to rank from top or bottom
- `.groupby("class")["fare"].rank(pct=True)`
- `.groupby("class")["fare"].shift(1)`
    - 1 = LOOKS BACK 1
    - -1 = LOOKS FORWARD 1
- `.groupby("class").cumcount() + 1` FOR MANUAL ROW NUNMBERS
    - pattern: sort --> assign --> d.gropupby().cumcount()+1


### INDEX / UTIL
## Index / Utilities
- `.set_index("col")` — make column the row index
- `.reset_index(drop=True)` — replace index with 0,1,2...
- `.rename_axis(index=None, columns=None)` — remove axis names
- `.copy()` — independent copy of DF
- `.T` — transpose rows and columns
- `.equals(other_df)` — Boolean equality check

# BIG PATTERNS
Use assign --> groupby --> transform to make a complex series and shove it back in
```
df.assign(
    class_mean_fare=lambda d:
        d.groupby("class", observed=True)["fare"].transform("mean")
)
```

group --> sort --> select top n
```
(
    df
    .sort_values("fare", ascending=False)
    .groupby("class", observed=True)
    .head(5)
)
```


# FEATURE ENG
- pd.cut()
- np.where()
- np.select()


# Reshape

`index` generally refers to rows
`columns` generally refers to cols
`values` is what we are measuring

```
pd.crosstab(
    index=df["row_category"],
    columns=df["column_category"],
    normalize="index"|"columns"|"all"
)
```

```
df.melt(
    id_vars=["cols_identifying_rows"], #current rows
    value_vars=["measure_a", "measure_b"], #current inputs
    var_name = "old_varnames_as_var",   # new place to put intput names
    value_name = "old_values_as_var"    # new place to put value names
)
```   

- both `pivot` and `pivot_table` use `index`, `columns`,`values` format. Just that pivot table has an `aggfunc`
```
df.pivot(
    index="identifier", # keep as rows
    columns="measure", #this will turn into new columns
    values="value"
)
```

```
df.pivot_table()
    index="row_category",       # one analysis column
    columns="column_category",  # one analysis column
    values="measurement",       # the VALUE 
    aggfunc="mean"              # what we are using as the function OR LIST OF FUNCTIONS
```

`.reset_index()`
`.rename_axis(index=None, columns=None)`

# Joins
- `.merge()` or `pd.merge()` joins dataframe using keys
- `.join()` joins dataframe using index values
- `pd.concat([a, b], axis=0|1, ignore_index=True)` 

```
df1.merge(
    df2,
    on="key", #or [key1, key2] for multi-key-joins
    how="left"|"right"|"inner"|"outer",
    validate="X_to_X", #X can be many or one
    indicator=True #column to say if row matched
)
```

if differnet names:
`left_on=` and `right_on=`

indicator gives a `_merge` column indicating which joined ("both" "left_only" "right_only)