- 查询依赖树，是否包含指定parent的包

```shell
mvn dependency:tree -Dincludes=org.apache.logging.log4j
```

- 初始化maven工程

```shel
mvn archetype:generate \
    -DgroupId=com.chenxue.mysql \
    -DartifactId=mysql-comparison \
    -DarchetypeArtifactId=maven-archetype-quickstart \
    -DinteractiveMode=false
```

