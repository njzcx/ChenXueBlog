在 Java 7 之前，程序中如果有需要关闭的资源，例如 `java.io.InputStream`、`java.sql.Connection` 等，通常会在 finally 中关闭，例如：

```
InputStream inputStream = null;
try {
    inputStream = new FileInputStream("/my/file");
    // ...
} catch (Exception e) {
    e.printStackTrace();
} finally {
    if (inputStream != null) {
        try {
            inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



在 Java 7 以及后续版本中，支持 try-with-resources，任何实现 `java.lang.AutoCloseable` 接口的类，包括 `java.io.Closeable` 的实现类，都可以通过 try-with-resources 来关闭。

上面代码通过 try-with-resources 可以简化为：

```
try (InputStream inputStream = new FileInputStream("/my/file")) {
    // ...
} catch (Exception e) {
    e.printStackTrace();
}
```



### 支持定义多个 resources

通过 JDBC 查询数据库时，会依次创建 `Connection`、`Statment`、`ResultSet`，并且这三个资源都需要关闭，那么可以这样写：

```
try (Connection connection = DriverManager.getConnection(url, user, password);
     Statement statement = connection.createStatement();
     ResultSet resultSet = statement.executeQuery("SELECT ...")) {
    // ...
} catch (Exception e) {
    // ...
}
```



### 多个 resources 的关闭顺序

如果在 try 中定义了多个 resources，那么它们关闭的顺序和创建的顺序是相反的。上面的例子中，依次创建了 `Connection`、`Statment`、`ResultSet` 对象，最终关闭时会依次关闭 `ResultSet`、`Statment`、`Connection`，所以不用担心 `Connection` 会先 close。

官方文档：

> Note that the close methods of resources are called in the opposite order of their creation.

### catch、finally 和 close 的先后顺序

官方文档：

> In a try-with-resources statement, any catch or finally block is run after the resources declared have been closed.

在 try-with-resources 中，catch 和 finally 中的代码在资源关闭之后运行。

以下是一种错误写法：

```
Connection connection = DriverManager.getConnection(url, user, password);
try (Connection connection2 = connection) {
    // ...
} catch (SQLException e) {
    e.printStackTrace();
} finally {
    try {
        Statement statement = connection.createStatement(); // 异常，此时 Connection 已关闭
        // ...
    } catch (SQLException ex) {
        ex.printStackTrace();
    }
}
```



### 创建和关闭 resources 时的异常处理

在 try-with-resources 中，如果创建资源发生异常，即 `try (...)` 中小括号里的代码出现异常，以及 close 时发生异常，都是会在 catch 中捕捉到的。例如：

```
try (InputStream inputStream = new FileInputStream("/not/exist/file")) {
    // ...
} catch (Exception e) {
    e.printStackTrace(); // 如果文件不存在，这里会捕获异常
}
```



这段代码中，如果 `new FileInputStream("/not/exist/file")` 对应的文件不存在，就会抛出异常，这个异常会在下面的 catch 中捕获到。

### Suppressed Exceptions

在 try-with-resources 中，如果 try block（即 try 后面大括号中的代码）抛出异常，会触发资源的 close，如果此时 close 也发生了异常，那么 catch 中会捕获到哪一个呢？

由于 close 抛出异常不是很常见，所以自己实现一个 `AutoCloseable` 实现类：

```
public class MyResource implements AutoCloseable {

    public void doSomething() throws Exception {
        throw new Exception("doSomething exception");
    }

    @Override
    public void close() throws Exception {
        throw new Exception("close exception");
    }
}
```



测试代码：

```
public class Test {

    public static void main(String[] args) {
        try (MyResource myResource = new MyResource()) {
            myResource.doSomething();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```



运行结果：

```
java.lang.Exception: doSomething exception
	at com.xxg.MyResource.doSomething(MyResource.java:6)
	at com.xxg.Test.main(Test.java:12)
	Suppressed: java.lang.Exception: close exception
		at com.xxg.MyResource.close(MyResource.java:11)
		at com.xxg.Test.main(Test.java:13)
```



可以看到 catch 中捕获的是 `doSomething()` 方法抛出的异常，同时这个异常会包含一个 Suppressed Exception，即 `close()` 方法抛出的异常。

如果想直接拿到 `close()` 方法抛出的异常，可以通过 `Throwable.getSuppressed()` 方法获取：

```
try (MyResource myResource = new MyResource()) {
    myResource.doSomething();
} catch (Exception e) {
    // e 是 doSomething 抛出的异常，想要拿到 close 方法抛出的异常需要通过 e.getSuppressed 方法获取
    Throwable[] suppressedExceptions = e.getSuppressed();
    Throwable closeException = suppressedExceptions[0];
    closeException.printStackTrace();
}
```



### Closeable 和 AutoCloseable

`AutoCloseable` 是 Java 7 新增的接口，`Closeable` 早就有了。二者的关系是 `Closeable extends AutoCloseable`。二者都仅包含一个 `close()` 方法。那么为什么 Java 7 还要新增 `AutoCloseable` 接口呢？

`Closeable` 在 `java.io` 包下，主要用于 IO 相关的资源的关闭，其 `close()` 方法定义了抛出 `IOException` 异常。其实现类实现 `close()` 方法时，不允许抛出除 `IOException`、 `RuntimeException` 外其他类型的异常。

而 `AutoCloseable` 位于 `java.lang` 包下，使用更广泛。其 `close()` 方法定义是 `void close() throws Exception`，也就是它的实现类的 `close()` 方法对异常抛出是没有限制的。

### 参考文档

- https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html