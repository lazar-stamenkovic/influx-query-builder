# Influx Query Builder

[![Build Status](https://travis-ci.org/benjamin658/influx-query-builder.svg?branch=master)](https://travis-ci.org/benjamin658/influx-query-builder.svg?branch=master)

> The super lightweight InfluxDB query builder.

This project is still under active development.

## Installation

`go get -u github.com/benjamin658/influx-query-builder`

## Query Builder Usage

### Simple query

```go
builder := New()
query := builder.
  Select([]string{"temperature", "humidity"}).
  From("measurement).Build()
```

Output:

```sql
SELECT "temperature","humidity" FROM "measurement"
```

### Function query

```go
builder := New()
query := builder.
  Select([]string{`MEAN("temperature")`, `SUM("humidity")`}).
  From("measurement").
  Build()
```

Output:

```sql
SELECT MEAN("temperature"),SUM("humidity") FROM "measurement"
```

### Query with criteria

```go
builder := New()
query := builder.
  Select([]string{"temperature", "humidity"}).
  From("measurement").
  Where("time", ">", "2018-11-01T06:33:57.503Z").
  And("time", "<", "2018-11-02T09:35:25Z").
  Or("tag", "=", "t").
  Build()
```

Output:

```sql
SELECT "temperature","humidity" FROM "measurement" WHERE "time" > '2018-11-01T06:33:57.503Z' AND "time" < '2018-11-02T09:35:25Z' OR "tag" = 't'
```

### Group By time

```go
builder := New()
query := builder.
  Select([]string{"temperature", "humidity"}).
  From("measurement").
  GroupBy("10m").
  Build()
```

Output:

```sql
SELECT "temperature","humidity" FROM "measurement" GROUP BY time(10m)
```

### Order By time

```go
builder := New()
query := builder.
  Select([]string{"temperature", "humidity"}).
  From("measurement").
  Desc().
  Build()
```

Output:

```sql
SELECT "temperature","humidity" FROM "measurement" ORDER BY time DESC
```

### Limit and Offset

```go
builder := New()
query := builder.
  Select([]string{"temperature", "humidity"}).
  From("measurement").
  Limit(10).
  Offset(5).
  Build()
```

Output:

```sql
SELECT "temperature","humidity" FROM "measurement" LIMIT 10 OFFSET 5
```

### Reset builder and get a new one

```go
builder := New()
// some code...
builder = builder.Clean()
```

License
-------

© Ben Hu (benjamin658), 2018-NOW

Released under the [MIT License](https://github.com/benjamin658/influx-query-builder/blob/master/LICENSE)