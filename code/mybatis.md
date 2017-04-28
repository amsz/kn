# Mybatis

## 代码生成

使用Maven插件 `mybatis-generator-maven-plugin` 来生成 model、mapper、xml 文件

```shell
# 生成表 sys_notice
mvn mybatis-generator:generate -Dtable='sys_notice'

# 表名通配符，生成 sys_ 开头的所有表  
mvn mybatis-generator:generate -Dtable='sys_%'
```

* 运行时必须指定 -Dtable 参数
* 数据库使用了开发环境的配置：`<properties resource="application-dev.properties"/>`
* 文件生成在 `target/generated-sources/mybatis-generator/model` 目录
* Mapper 文件已经继承了 `tk.mybatis.mapper.common.Mapper` 
* 代码拷贝到项目时注意 `package` 的变动

🍺 Tip: 在Intellij IDEA中，生成的 java 代码 通过文件复制后，粘贴到相应的目录中时，代码中的 `package` 会自动修正为正确的包名。

## FAQ

⚡️org.apache.ibatis.binding.BindingException: Invalid bound statement (not found)

一般的原因是Mapper interface和xml文件的定义对应不上

## 参考

http://www.mybatis.org/generator/index.html

http://git.oschina.net/free/Mapper/blob/master/wiki/mapper3/7.UseMBG.md

