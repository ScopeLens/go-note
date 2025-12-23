# GOLearning
go语言学习仓库

[仓库地址](https://github.com/golang/go)

- [GOLearning](#golearning)
  - [logrus](#logrus)
  - [viper](#viper)
  - [Gin](#gin)
  - [Gorm](#gorm)
    - [类型对应](#类型对应)

## logrus

[仓库地址](https://github.com/sirupsen/logrus)

## viper

[仓库地址](https://github.com/spf13/viper)

## Gin

[仓库地址](https://github.com/gin-gonic/gin)

## Gorm

[中文文档](https://gorm.io/zh_CN/docs/index.html)  
[仓库地址](https://github.com/go-gorm/gorm)

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

**需要 import "gorm.io/datatypes" 才能使用 datatypes.JSON,本质还是比特流**

GORM标签示例:`gorm:"column:xxx;type:[类型:int,bigint...]"`