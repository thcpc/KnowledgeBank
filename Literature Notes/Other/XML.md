# DTD
# XSD

## 目标

在 XML 中 添加复杂的语义约束

## XML 中引入

- 引入无命名空间的XSD
```XML
<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:noNamespaceSchemaLocation="http://www.xxxx/xxx.xsd">
	  <!--
	  1. 第一步，添加xmlns:xsi 属性，引入xsi命名空间（固定套路）
	  2. 第二步，通过xsi命名空间下的属性noNamespaceSchemaLocation 指定 XSD 文件的 URI
	  -->
	  <study></study>
</root>
```

使用场景 - 只有一个 xsd 需要引用的时候

- 引入命名空间的XSD
```XML
<root xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	  xsi:schemaLocation="http://www.xxxx/xxx.xsd
						  http://www.yyyy/yyy.xsd"
	  xmlns:a="http://www.xxxx"
	  xmlns:b="http://www.yyyy"
	  >
	  <!--
	  1. 第一步，添加xmlns:xsi 属性，引入xsi命名空间（固定套路）
	  2. 第二步，添加多个命名空间
	  3. 第三不，
	  -->
	  <a:study>...</a:study>
	  <b:study>...</b:study>
</root>
```
使用场景 - 当有多个xsd需要分别引用的时候

