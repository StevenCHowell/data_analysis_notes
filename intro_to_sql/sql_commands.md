# SQL Commands

- `SELECT`: instruction to select some data.
- `FROM`: which table to get the data from.
- `DISTINCT`: only select the unique entries.
- `COUNT()`: return the number from the select statement.  If the input is a column label, the results will be the number of non-missing entries.

To combine `DISTINCT` and `COUNT`, the syntax would be

    SELECT COUNT(DISTINCT country) FROM films;

