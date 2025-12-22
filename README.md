# GOLearning
go语言学习仓库

- [GOLearning](#golearning)
  - [Logger](#logger)
  - [Viper](#viper)
  - [Gin](#gin)
  - [Gorm](#gorm)
    - [类型对应](#类型对应)

## Logger

## Viper

## Gin

## Gorm

### 类型对应

|数据库类型|Go类型|
|---------|------|
|INT|int/int32|
|BIGINT|int64|
|TINYINT(1)|bool|
|FLOAT|float32|
|DOUBLE|float64|
|DECIMAL(10,2)|float64/string|
|CHAR(20)|string|
|VARCHAR(255)|string|
|TEXT|string|
|DATE|time.Time|
|DATETIME|time.Time|
|TIMESTAMP|time.Time|
|JSON|datatypes.JSON|
|BLOB|[]byte|

GORM标签示例:`gorm:"column:xxx;type:[类型:int,bigint...]"`