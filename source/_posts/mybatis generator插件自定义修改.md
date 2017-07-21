---
title: mybatis generator插件自定义修改
date: 2017-07-19 21:17:29
tags: [mybatsi-generator, maven, tools]
---
为了让前端工程师在swagger API文档中看到各个实体类属性的注释说明，而不用再去单独查看数据字典，同时解决在接收和返回日期格式的数据时需要手动对每个日期格式的字段添加相应注解的问题，修改mybatis-generator插件，让其在生成实体类的时候就自动生成相应注解，而不用一个一个从数据字典中复制，提高工作效率。在这个过程中最大的收获就是弄清楚了获取插件源码、修改相应代码，然后重新打包替换的整个流程，以后再修改插件就是轻车熟路了，最终自动生成实体类的**主要代码** 如下：
<!-- more -->
``` java
/**
 * 实体类对应的数据表为： user_info
 * @author xia
 * @date 2017-07-19 13:58:22
 */
@ApiModel(value = "执法人信息表")
public class UserInfo {
	@ApiModelProperty(value = "ID")
	private Integer id;

	@ApiModelProperty(value = "用户登录账号")
	private String userNo;

	@ApiModelProperty(value = "姓名")
	private String userName;

	@ApiModelProperty(value = "姓名拼音")
	private String spellName;

	@ApiModelProperty(value = "密码")
	private String password;

	@ApiModelProperty(value = "手机号")
	private String userPhone;

	@ApiModelProperty(value = "性别")
	private Integer userGender;

	@ApiModelProperty(value = "头像地址")
	private String userImg;

	@ApiModelProperty(value = "职务")
	private String userDuty;

	@ApiModelProperty(value = "排序号")
	private Integer sortCode;

	@ApiModelProperty(value = "记录创建时间")
	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	private Date createTime;

	@ApiModelProperty(value = "记录修改时间")
	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	private Date updateTime;

	@JsonFormat(locale = "zh", timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
	public Date getCreateTime() {
		return createTime;
	}

	@JsonFormat(locale = "zh", timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
	public Date getUpdateTime() {
		return updateTime;
	}
}
```

### 一、mybatis generator插件安装和使用

#### 1、通过Eclipse Marketplace安装使用

打开Help=>Eclipse Marketplace，搜索mybatis generator，安装即可，如下图：

![通过Eclipse Marketplace安装](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/eclipse-marketplace.png) 

使用方法：在配置文件（generatorConofig.xml）上右键，Run As==>Run Mybatis Generator，即会在配置文件指定的目录下生成相应的文件，配置文件主要的配置会在下文说明，详细说明请查看文末的参考资料。

![使用](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/run-mybatis-generator.png) 

#### 2、通过手动下载插件安装使用

在 [mybatis-generator-core 1.3.5官方下载地址](https://github.com/mybatis/generator/releases)  ,下载eclipse插件，如下图：

![下载mybatis-generator-core 1.3.5插件](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/downloads-mybatis-generator.png)  

解压，将`features` 和 `plugins` 两个文件夹拷贝到eclipse安装目录下的`dropins` 中 ，重启eclipse，使用方法同上；

![plugins](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/mybatis-generator-plugins.png)  

#### 3、通过maven管理插件

具体的配置方法这里不细说，主要说说使用的方法：

* 安装`EasyShell` 插件：

  ![通过EasyShell插件运行命令生成文件](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/run-easy-shell.png) 

  然后运行如下命令： 

  ~~~ shell
  mvn mybatis-generator:generate
  ~~~

* 通过maven builder运行

  在项目上右键，Run As ==> Maven build...，在Goals中输入`mybatis-generator:generate` ，点击run运行，**刷新整个项目才能看到生成的文件** ，如下图：

  ![通过maven build 运行](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/maven-build.png) 

  ​

### 二、获取插件源码

Window ==> Show view ==> other，打开`Plug-ins` 视图，然后将插件导出成项目，如下图：

![添加插件视图](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/show-view.png) 

![获取插件源码](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/source-project.png) 

生成的源码结构如下图：

![源码结构](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/source-project-code.png) 

### 三、根据实际需求修改源码

获取了源码之后就可以根据自己的实际需求来进行修改了，这次修改主要是通过调整注释的生成达到自动生成swagger注解和处理日期格式注解的目的，控制注释生成的文件是`DefaultCommentGenerator.java` ，剩下的就是弄清楚类注释、字段注释、方法注释具体是由哪个方法控制的，需要在哪进行修改，然后就是进行字符串的拼接，[我修改后的源码](https://github.com/xiachengwei5/org.mybatis.generator.core-1.3.5) ，[源码详细剖析](https://github.com/orange1438/mybatis-generator-core-chinese-annotation-1.3.5) 。

### 四、重新打包并替换

代码修改完成后只需重新打包替换原先的包就行了，为解决打包出的插件中有中文乱码的问题，须在`build.properties` 中配置：

``` properties
javacDefaultEncoding.. = UTF-8
```

![解决中文乱码问题](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/modify-build.png) 

为了打出的jar包可以直接被运行，在`MANIFEST.MF` 中添加运行入口类的配置，否则会报`jar中没有主清单属性` 的错：

``` properties
Main-Class: org.mybatis.generator.api.ShellRunner
```

![配置运行入口类](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/modify-manifest.png) 

配置完成之后操作：项目 ==> 右键 ==> Export == > Deployable plug-ins and fragments：

![重新打包-1](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/Export.png) 

![配置相关参数](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/Export-options.png) 

将重新生成的jar包如下：

![重新生成的jar包](http://olywxnzqu.bkt.clouddn.com/img/mybatis_generator/jar.png) 

#### 1、通过Eclipse Marketplace安装的替换

*待测试* 

#### 2、通过手动下载插件安装的替换

替换eclipse安装目录下dropins ==> plugins中jar包，替换后需要重启eclipse才能生效；

#### 3、通过maven管理插件的替换

查看pom.xml中引用插件的信息：

``` xml
<plugin>
	<groupId>org.mybatis.generator</groupId>
	<artifactId>mybatis-generator-maven-plugin</artifactId>
	<version>1.3.5</version>
	<dependencies>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.38</version>
		</dependency>
	</dependencies>
</plugin>
```

此处引用的是`mybatis-generator-maven-plugin` ，在本地maven仓库（Repository）中找到`mybatis-generator-maven-plugin` jar包，解压打开*plugin.xml*  查看其引用的核心包：

``` xml
<dependency>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-core</artifactId>
  <type>jar</type>
  <version>1.3.5</version>
</dependency>
```

从配置文件中可以看到插件引用的核心包是`org.mybatis.generator.1.3.5` ，在本地maven仓库（Repository）中找到此jar包所在的路径，用新生成的jar包修改成同名（名称对插件的功能没有影响）后替换即可；

### 五、配置generatorConfig.xml

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>
	<!-- 数据库驱动 注意：这个 location要指明\MySQL-connector-Java jar包的绝对路径 -->
	<classPathEntry
		location="E:\software\eclipse\Repository\mysql\mysql-connector-java\5.1.42\mysql-connector-java-5.1.42.jar" />

	<context id="marketing" targetRuntime="MyBatis3"
		defaultModelType="flat">
		<property name="javaFileEncoding" value="UTF-8" />

		<!-- 配置生成toString()方法 -->
		<plugin type="org.mybatis.generator.plugins.ToStringPlugin" />

		<commentGenerator>
			<!-- 是否禁止显示日期 true：是 ： false:否 -->
			<property name="suppressDate" value="false" />
			<!-- 是否去除自动生成的所有注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="false" />
			<!-- 是否添加字段注释 true:是 false：否 -->
			<property name="addRemarkComments" value="true" />
			<!-- 自定义属性 作者名称 -->
			<property name="author" value="Jeff" />
		</commentGenerator>

		<!--数据库链接URL，用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/test" 
			userId="root"
			password="root">
		</jdbcConnection>

		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- 配置生成实体类的包名和位置 -->
        <!-- 通过maven管理插件时targetProject的路径为绝对路径：D:\workspace_oxygen\mybatis3 -->
        <!-- 通过自安装插件生成时的地址：mybatis3 -->
		<javaModelGenerator targetPackage="model"
			targetProject="mybatis3">
			<property name="enableSubPackages" value="true" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		
		<!-- 配置生成映射文件的包名和位置 -->
        <!-- 通过maven管理插件时targetProject的路径为绝对路径：D:\workspace_oxygen\mybatis3 -->
        <!-- 通过自安装插件生成时的地址：mybatis3 -->
		<sqlMapGenerator targetPackage="mapperXml"
			targetProject="mybatis3">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>
		
		<!-- 配置生成mapper文件的包名和位置 -->
        <!-- 通过maven管理插件时targetProject的路径为绝对路径：D:\workspace_oxygen\mybatis3 -->
        <!-- 通过自安装插件生成时的地址：mybatis3 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="mapper" targetProject="mybatis3">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>
		
		<!-- 配置需要反向生成表的信息 -->
		<table tableName="user_info" domainObjectName="UserInfo"
			enableCountByExample="false" enableUpdateByExample="false"
			enableDeleteByExample="false" enableSelectByExample="false"
			selectByExampleQueryId="false">
			<property name="ignoreQualifiersAtRuntime" value="true" />
			<generatedKey column="id" sqlStatement="MySql" identity="true" />
		</table>
	</context>
</generatorConfiguration>
```

**配置文件中需要注意的几点：** 

> * 只有将`suppressAllComments` 设置为`false` 才能生成注释；
> * 只有将`addRemarkComments` 设置为`true` 才能生成字段的注释；
> * 只有将`suppressDate` 设置为`false` 才能显示时间；
> * 可以通过`author` 指定生成的实体类中显示的作者名称；
> * **通过手动安装的插件配置路径需要写*相对路径* ，而通过maven管理的插件需要写绝对路径，如果路径配置有误，则不能正常生成** ；

### 六、资料下载

[修改后的源码](https://github.com/xiachengwei5/org.mybatis.generator.core-1.3.5) 

[mybatis-generator-core 1.3.5官方下载地址](https://github.com/mybatis/generator/releases)  

[所有资料下载](http://pan.baidu.com/s/1jIDVV6m) 

### 七、参考资料

[MyBatis Generator中文文档](http://mbg.cndocs.tk/) 

[mybatis-generator-core-chinese-annotation-1.3.5](https://github.com/orange1438/mybatis-generator-core-chinese-annotation-1.3.5) 

[修改mybatis-generator-1.3.2源码实现自定义代码生成详解（一）](http://www.blogjava.net/bolo/archive/2015/03/20/423683.html) 

[Mybatis generator 生成domain字段带数据库注释](http://www.cnblogs.com/sz-zzm/p/5815834.html) 

[mybatis-generator 代码自动生成工具（maven方式）](http://www.cnblogs.com/JsonShare/p/5521901.html) 

[Eclipse插件开发 导出插件项目为jar包时报错 出现中文乱码解决办法](http://www.iteye.com/topic/1135959) 

[MANIFEST.MF 文件内容完全详解](http://blog.csdn.net/zhifeiyu2008/article/details/8829637) 

