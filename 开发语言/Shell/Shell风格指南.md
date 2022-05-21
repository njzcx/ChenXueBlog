### 1、什么时候使用shell

> Shell应该仅仅被用于小功能或者简单的包装脚本。

- 如果你主要是在调用其他的工具并且做一些相对很小数据量的操作，那么使用shell来完成任务是一种可接受的选择。
- 如果你在乎性能，那么请选择其他工具，而不是使用shell。
- 如果你发现你需要使用数据而不是变量赋值（如 `${PHPESTATUS}` ），那么你应该使用Python脚本。
- 如果你将要编写的脚本会超过100行，那么你可能应该使用Python来编写，而不是Shell。请记住，当脚本行数增加，尽早使用另外一种语言重写你的脚本，以避免之后花更多的时间来重写。

### 2、文件扩展名

- Shell可执行文件应该没有扩展名（强烈建议）或者使用.sh扩展名。

- 库文件必须使用.sh作为扩展名，而且应该是不可执行的。

### 3、异常处理

所有的错误信息都应该被导向STDERR，这使得异常查看变得易读，推荐使用类似如下函数：

```shell
err() {
    echo "[$(date +'%Y-%m-%dT%H:%M:%S%z')]: $@" >&2
}

if ! do_something; then
    err "Unable to do_something"
    exit "${E_DID_NOTHING}"
fi
```

### 引用

- 《[ Google 开源项目风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/contents/#)》
- 《[Google Shell 风格指南](https://www.bookstack.cn/books/google-shell-style)》书栈网

