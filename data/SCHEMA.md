# data/SCHEMA.md — data dictionary

This is the reference an agent needs to write code that **runs against the data**. It
describes the tables, their grain, their key columns, and how to read them efficiently.
It does **not** contain the data, and it deliberately omits research framing — the full
dataset narrative lives with the Runner. (What the data *is* and under what terms is
in [`PROJECT.md`](../PROJECT.md)'s data statement.)

> **Template note:** this file ships as a skeleton. Filling it in is SETUP step 4 —
> and it is load-bearing: Proposers' code is written *only* from what this file says.

## The `DATA_ROOT` contract

Your code does not know where the data physically is. It reads one environment
variable and joins it with the canonical table names below:

```python
import os
from pathlib import Path

DATA_ROOT = Path(os.environ["DATA_ROOT"])
table_a = DATA_ROOT / "table_a.parquet"
```

The **Runner** sets `DATA_ROOT` to the real location at run time; it is never committed.
The Proposer's code must work for any value of `DATA_ROOT` that contains these files.

## Canonical tables

One row per table. `Scale` is the contract: tables marked **never load whole** must be
read lazily (column projection + predicate pushdown — polars `scan_parquet`, pyarrow
filters, or DuckDB). When you mark a table never-load-whole here, also set
`BIG_TABLES_ERE` in `scripts/scan-round.sh` so the safety scan flags eager loads.

| Canonical name       | Grain (one row =) | Approx. rows | Scale            | Notes |
| -------------------- | ----------------- | ------------ | ---------------- | ----- |
| `<table_a>.parquet`  | <…>               | <…>          | loads directly   | <…>   |
| `<table_b>.parquet`  | <…>               | <…>          | **never load whole** | <…> |

Canonical names are logical. If the on-disk filename differs, the Runner maps it;
the Proposer always refers to the names in this table.

## Key columns

For each table most rounds will touch, list the columns that matter — name, type,
meaning, units/timezone, and any sample-selection flags:

**`<table_a>`**

| Column | Type | Meaning |
| ------ | ---- | ------- |
| <…>    | <…>  | <…>     |

**Sample selection:** <e.g. "the main analysis sample is `<table_a>` with
`exclude_flag == 0`" — spell out the convention so every round uses the same sample.>

## Conventions

- Sampling seed: `42` unless a round's `ASK.md` says otherwise.
- Timestamps: <timezone / epoch convention>.
- <units, identifier formats, join keys…>

## Contributed data

Tables contributed by members (AGENTS.md §15) are registered here as
`contrib_<owner>_<table>`, stored under `DATA_ROOT/contrib/<owner>/`, each with one
line of provenance: who contributed it, when, and the licence note.

| Canonical name | Grain | Contributed by / date | Licence note |
| -------------- | ----- | --------------------- | ------------ |
| —              |       |                       |              |
