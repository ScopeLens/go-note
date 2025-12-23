# GOLearning
go语言学习仓库

[仓库地址](https://github.com/golang/go)

- [GOLearning](#golearning)
  - [基础语法](#基础语法)
  - [进阶语法](#进阶语法)
    - [文件嵌入](#文件嵌入)
  - [编译部署](#编译部署)
    - [windows](#windows)
    - [linux](#linux)
  - [常用开源库](#常用开源库)
    - [logrus](#logrus)
    - [viper](#viper)
      - [热更新](#热更新)
      - [最佳实践](#最佳实践)
    - [Gin](#gin)
    - [Gorm](#gorm)
      - [类型对应](#类型对应)
      - [最佳实践](#最佳实践-1)

## 基础语法

## 进阶语法

### 文件嵌入

GO语言在1.16版本中引入了`//go:embed`指令可以将文件嵌入到编译结果的文件中

使用方法也很简单,只需要在一个变量上使用这个注解进行标识

例如:
```
//go:embed config.yaml
var configFile embed.FS

//注意:embed指令使用的时候,目标文件必须要在当前目录或子目录中
```

实际使用一般在配置文件的默认值设定,[项目实践](#viper)

## 编译部署

### windows

可以直接编译成exe文件,在项目根目录中执行`go build -o [文件名].exe`

### linux

**交叉编译:在一个平台上生成另一个平台可运行程序的过程**

go语言的交叉编译非常简单,只需要修改两个环境变量即可

```
#设置目标系统为Linux
SET GOOS=linux
#设置目标CPU架构为arm64
SET GOARCH=arm64
#开始编译
go build main.go
```

## 常用开源库

### [logrus](https://github.com/sirupsen/logrus)

### [viper](https://github.com/spf13/viper)

#### 热更新

普通情况下,配置文件只会在启动时候读入内存,之后配置文件变化不会影响项目中的变量  

但是viper中存在一个热更新机制,利用了操作系统的文件系统通知

>Windwos上是ReadDirectoryChangesW  
>Linux上是Inotify

只需要在配置初始化的时候加上一行`实例.WatchConfig()`,并且注册一个回调函数即可

**只有一份配置文件并且嵌入程序无法热更新**

```
func InitConfig(){
  viper:=viper.New()
  viper.SetConfigFile("config.yaml")

  viper.WatchConfig()

  viper.OnConfigChange(func(e fsnotify.Event){
    //执行相关重启操作,例如断开重连数据库
    ...
  })

  //首次读取
  if err:=viper.ReadInConfig();err!=nil{
    //错误处理
  }
}
```

#### 最佳实践

**嵌入默认值+外部文件覆盖**

1. 内置默认配置,利用`//go:embed`存储一份出厂设置在二进制中,防止误删配置文件,导致程序崩溃
2. 外部配置优先,程序启动的时候检测当前目录中是否存在`config.yaml`
3. 开启WatchConfig:如果使用的是外部文件,则开启热更新


### [Gin](https://github.com/gin-gonic/gin)

### [Gorm](https://github.com/go-gorm/gorm)

[中文文档](https://gorm.io/zh_CN/docs/index.html)  

#### 类型对应

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

#### 最佳实践