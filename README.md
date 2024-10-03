[![SDF Workspace Compiles](https://github.com/sdf-labs/tests/actions/workflows/compile-workspace.yml/badge.svg)](https://github.com/sdf-labs/tests/actions/workflows/compile-workspace.yml)

# Official SDF Tests Library

This SDF library contains the standard tests available in the default namespace for SDF data tests. As such, tests defined in this workspace do not need to be prefixed with the workspace name, they can be referenced directly. Here's an example:

```yaml
columns:
  - name: a
    tests:
      - expect: unique()
      - expect: not_null()   
```

*Note: SDF is still < v1, as such certain scenarios may result in unexpected behavior. Please follow the [contributing guidelines](./CONTRIBUTING.md) and create an issue in this repo if you find any bugs or issues.*

For an in-depth guide on how to use SDF tests, please see the Tests section of [our official docs](https://docs.sdf.com/guide/data-quality/tests)


## SDF Standard Library Tests

| Test Name                                                            | Type      |
| -------------------------------------------------------------------- | --------- |
| [`not_null()`](#not-null)                                            | Scalar    |
| [`valid_scalar(condition)`](#valid-scalar)                           | Scalar    |
| [`valid_aggregate(condition)`](#valid-aggregate)                     | Aggregate |
| [`unique()`](#unique)                                                | Aggregate |
| [`in_accepted_values([values])`](#in-accepted-values)                | Aggregate |
| [`minimum(value)`](#minimum)                                         | Aggregate |
| [`maxiumum(value)`](#maximum)                                        | Aggregate |
| [`exclusive_minimum(value)`](#exclusive-minimum)                     | Aggregate |
| [`exclusive_maximum(value)`](#exclusive-maximum)                     | Aggregate | 
| [`between(lower, upper)`](#between)                                  | Aggregate |
| [`max_length(value)`](#max-length)                                   | Aggregate |
| [`min_length(value)`](#min-length)                                   | Aggregate |
| [`like(string)`](#like)                                              | Aggregate |
| [`try_cast(type)`](#try-cast)                                        | Aggregate |
| [`primary_key(column)`](#primary-key)                                | Aggregate |
| [`unique_columns([c1, c2])`](#unique-columns)                        | Table     |
| [`fresh(reference_value, date_part, value)`](#fresh)                 | Aggregate |
| [`maximum_count([c1, c2], value)`](#maximum_row_count_by_partitions) | Table     |
| [`minimum_count([c1, c2], value)`](#maximum_row_count_by_partitions) | Table     |

#### Not Null

Asserts that no values of a column elements are null.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: not_null()
```

#### Valid Scalar

Asserts that all values of a column meet a scalar condition.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: valid_scalar("""x == 0 OR x == 1""")
```

#### Valid Aggregate

Asserts that all values of a column meet an aggregate condition.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: valid_aggregate("""SUM(x) < 100""")
```

#### Unique

Asserts that all values of a column are unique.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: unique()
```

#### In Accepted Values

Asserts that all values of a column are in a list of accepted values.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: in_accepted_values([1, 2, 3])
      - expect: in_accepted_values(['nyse', 'nasdaq', 'dow'])
```

#### Minimum

Asserts that no values of a column are less than a given value.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: minimum(0)
```

#### Maximum

Asserts that no values of a column are greater than a given value.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: maximum(100)
```

#### Exclusive Minimum

Asserts that no values of a column are less than or equal to a given value.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: exclusive_minimum(0)
```

#### Exclusive Maximum

Asserts that no values of a column are greater than or equal to a given value.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: exclusive_maximum(100)
```

#### Between

Asserts that all values of a column are between two values.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: between(0, 100)
```

#### Max Length

Asserts that no string values of a column are greater than a given length.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: max_length(100)
```

#### Min Length

Asserts that no string values of a column are less than a given length.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: min_length(10)
```

#### Like

Asserts that all string values of a column contain a given string.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: like('abc')
```

#### Try Cast

Asserts that all values of a column can be cast to a given type.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: try_cast('int')
```

#### Primary Key

Asserts that a column is the Primary Key of a table

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: primary_key()
```

#### Unique Columns

Asserts that a combination of columns are unique across a table

**Example:**
```yaml
table:
  name: a
  tests:
    - expect: unique_columns(['a', 'b'])
```

#### Fresh

Asserts that a column contains values more recent than a given number of interval compared to a reference value.
The column and reference must be of the same data type.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: fresh('a', current_date(), 1)
      - expect: fresh('a', current_date(), 1)
  - name: b
    tests:
      - expect: fresh('b', current_timestamp(), 180, 'minute')
      - expect: fresh('warn', 'b', current_datetime(), 90, 'minute')
```

#### Maximum Count by Partition

Asserts that a table grouped by a list of columns contains less rows than a threshold value.
The column and reference must be of the same data type.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: maximum_row_count_by_partition('a', current_date(), 1)
      - expect: maximum_row_count_by_partition('a', current_date(), 1)
```

#### Minimum Count by Partition

Asserts that a table grouped by a list of columns contains more rows than a threshold value.
The column and reference must be of the same data type.

**Example:**
```yaml
columns:
  - name: a
    tests:
      - expect: maximum_row_count_by_partition('a', current_date(), 1)
      - expect: maximum_row_count_by_partition('a', current_date(), 1)
```
