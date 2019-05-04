---
title: java chain-of-responsibility-pattern
date: 2019.05.04 15:45:34
tags:
  - java
  - pattern
---

## chain-of-responsibility-pattern

责任链模式（Chain of Responsibility Pattern）为请求创建了一个接收者对象的链。这种模式给予请求的类型，对请求的发送者和接收者进行解耦。这种类型的设计模式属于行为型模式。

在这种模式中，通常每个接收者都包含对另一个接收者的引用。如果一个对象不能处理该请求，那么它会把相同的请求传给下一个接收者，依此类推。

[学习地址](https://www.runoob.com/design-pattern/chain-of-responsibility-pattern.html)

## code implement

### 创建抽象的记录器类  

```java
/**
 * abstract class can have construction
 */
public abstract class AbstractLogger {
    public static int INFO =1;
    public static int DEBUG = 2;
    public static int ERROR = 3;

    protected int level;

    public AbstractLogger() {
    }

    public AbstractLogger(int level) {
        this.level = level;
    }

    protected AbstractLogger nextLogger;

    public void setNextLogger(AbstractLogger nextLogger){
        this.nextLogger = nextLogger;
    }

    public void logMessage(int level,String message){
        if(this.level <= level){
            write(message);
        }

        if(nextLogger != null){
            nextLogger.logMessage(level,message);
        }
    }

    abstract protected void write(String message);
}

```

### 创建扩展该记录器类的实体类   
 
```kotlin

class ConsoleLogger(level: Int) : AbstractLogger(level) {
    override fun write(message: String) {
        println("Standard console::Logger: $message")
    }
}

```

```kotlin  

class ErrorLogger(level:Int):AbstractLogger(level) {
    override fun write(message: String?) {
        println("Standard error::ErrorLogger: $message")
    }
}

```

```kotlin  

class FileLogger(level: Int) : AbstractLogger(level) {
    override fun write(message: String) {
        println("Standard file::FileLogger: $message")
    }
}

```


### 调用  

> 创建不同类型的记录器。赋予它们不同的错误级别，并在每个记录器中设置下一个记录器。每个记录器中的下一个记录器代表的是链的一部分  

```kotlin

class AbstractMain {
    companion object{
      fun  getChainOfLoggers():AbstractLogger{
            var errorLogger:AbstractLogger = ErrorLogger(AbstractLogger.ERROR)
            var consoleLogger:AbstractLogger = ConsoleLogger(AbstractLogger.INFO)
            var fileLogger:AbstractLogger = FileLogger(AbstractLogger.DEBUG)

            errorLogger.setNextLogger(fileLogger)
            fileLogger.setNextLogger(consoleLogger)

            return errorLogger
        }
    }
}

fun main() {
    var loggerChain = AbstractMain.getChainOfLoggers()

    loggerChain.logMessage(AbstractLogger.INFO,"this is an information")

    loggerChain.logMessage(AbstractLogger.DEBUG,"this is a debug level information.")

    loggerChain.logMessage(AbstractLogger.ERROR,"this is a error information.")
}

```

### 结果输出

```
Standard console::Logger: this is an information

Standard file::FileLogger: this is a debug level information.
Standard console::Logger: this is a debug level information.

Standard error::ErrorLogger: this is a error information.
Standard file::FileLogger: this is a error information.
Standard console::Logger: this is a error information.

```