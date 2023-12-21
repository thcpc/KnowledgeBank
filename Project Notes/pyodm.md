# ODM Class
ODM Class的定义是根据 [ODM V2](https://wiki.cdisc.org/display/ODM2/ODM+v2.0)
## ODM Class 定义方式
### 1. 静态方式(Specification)
 - step1. 定义 ODM Class的代码文件
 - step2. 定义 ODM 元素和ODM Class的映射文件
[示例](https://github.com/thcpc/pyodm/tree/master/example/custom_odm_factory)

### 2. 动态方式(Xsd)
 - step1. 定义 XSD 文件
 - Factory 调用时会动态生成
 
## 成员变量
成员变量主要有三种类型
- Attribute
- OneElement
- ManyElements
### Attribute()
#### 定义
定义改成员变量为属性
![[Pasted image 20231121102155.png]]
#### API
| Name | 成员类型 | 含义 |
| --- | ----- | ---- |
| name | property | Attribute 名 |
| value | property | Attribute 的值 |

### OneElement()
#### 定义
定义该成员变量为 该子元素只有一个
![[Pasted image 20231124143755.png]]
- ? (meaning optional, with zero or one occurrence)
#### API
| Name | 成员类型 | 含义 |
| --- | ----- | ---- |
| name | property | Element 名 |
| value | property | 如果 Element 有文本值 |
| is_blank | method | 返回该元素是否有文本 |
### ManyElements()
#### 定义
定义该成员变量的子元素可能有多个
![[Pasted image 20231124143855.png]]
- * (meaning optional, with zero or more occurrences)
- + (meaning required, with one or more occurrences)
#### API
| Name | 成员类型 | 参数 |含义 |
| --- | ----- | ---- | --- |
| name | property | 无 |Element 名 |
| value | property | 无 |如果 Element 有文本值 |
| array | property| 无 |返回该元素的列表|
| count | property| 无 | 元素个数 |
| index| method | int i |返回 指定位置的元素 |
| first| method | 无 | 返回 第一个元素 |
| find | method | \*\*attributes | 返回符合属性的第一个 | 

# 主要的功能类说明

## Factory
```plantuml
@startuml
package pyodm.factory{
	class CdiscRegistry{
		 void get(name)
		 void registry_cdisc(name, cdisc_class)
	}
	note left of CdiscRegistry
		储存注册 ODM 元素的 Class 类型
		get(name): 根据 ODM 的元素名, 获取 ODM Class
		registry_cdisc(name, cdisc_class): 根据 ODM 元素名, 注册 ODM Class
	end note
	
	abstract class AbstractCdiscFactory extends CdiscRegistry{
		{abstract} void clazz_reader()
		{abstract} AbstractDataReader data_reader()
		AbstractDataLoader data_loader
		# ODM _odm_process(refresh)
		+ ODM odm(refresh)
	}
	note right of AbstractCdiscFactory 
		生成 ODM 的抽象工厂, 定义类生成 ODM 实例的模板方法
		clazz_reader: 指定 生成 ODM Class 的方式
		data_reader: 指定 解析 ODM Data 的方式
		data_loader: 读取 ODM 数据
		_odm_process: 模板方法，解析 ODM 数据，生成 ODM 对象实例
					  refresh: 是否重新读取
		odm: 返回 ODM 对象实例
			 refresh: 是否重新读取
	end note
	abstract class AbstractHierarchyFactory extends AbstractCdiscFactory
	note right: 解析 层级结构数据 抽象工厂,生成 ODM 实例
	abstract class AbstractCdiscXMLFactory extends AbstractHierarchyFactory{
		XMLDataReader data_reader()
	}
	note right of AbstractCdiscXMLFactory
	 	解析XML数据(XML数据为层级结构)抽象工厂, 生成 ODM 实例
	 	data_reader: 指定了解析 ODM XML Data 的数据方式 XMLDataReader
	end note
	abstract class AbstractAssembleFactory extends AbstractCdiscFactory{
		reader_class
		void class_reader()
	}
	note left of AbstractAssembleFactory
		可设置解析 ODM Class 的方式抽象工厂
		reader_class: 设置 解析 ODM Class的方式
		clazz_reader: 抽象方法 clazz_reader的实现，
		根据 reader_class 生成 解析 ODM Class 类，并注册 ODM Class
	end note
	class CdiscListableFactory extends AbstractAssembleFactory{
		ForestDataReader data_reader()
	}
	note left of CdiscListableFactory
		解析 平行结构数据 ,生产 ODM 实例
		data_reader: 指定了解析 ODM XML Data 的数据方式 ForestDataReader
	end note
	class CdiscXMLSpecificationFactory extends AbstractCdiscXMLFactory{
		void class_reader()
	}
	note right of CdiscXMLSpecificationFactory
		根据 Specification 方式，解析XML数据，生成XML对象
		class_reader(): 使用 Specification 的方式 解析 ODM Class 信息
	end note

	class CdiscXMLXsdFactory extends AbstractCdiscXMLFactory{
		void class_reader()
	}
	note left of CdiscXMLXsdFactory
		根据 Xsd 方式，解析XML数据，生成XML对象
		class_reader(): 使用 XSD 的方式 解析 ODM Class 信息
	end note

}

@enduml
```


### odm_process 方法流程图
```plantuml
@startuml
start
partition "_odm_process"{
if (self._cdisc_odm is None?) then(Yes)
label label1
group class_reader
	:AbstractConfigurationReader.load_cdisc_definition;
end group
group data_reader
	:AbstractDataReader.read;
end group
elseif(refresh) then(Yes)
label label2
goto label1
else(No)
endif
}
:Return self._cdisc_odm;
@enduml
```

## Loader
```plantuml
@startuml
package pyodm.core.support{
	class AbstractDataLoader{
		{abstract} DataObject load()
	}
	note top of AbstractDataLoader
		数据读取的抽象类, 把读取的源数据传化为期望的数据结构
		load(): 读取数据源，转化为期望的数据结构
	end note
}

package pyodm.core.data_loader{
	
	class ForestDataLoader extends pyodm.core.support.AbstractDataLoader{
		Forest load()
	}
	note bottom of ForestDataLoader
		把 list[list[dict]] 数据转化为 Forest 数据结构
		load(): 把多维列表转为 Forest 层级结构
	end note
	class XMLPathLoader extends pyodm.core.support.AbstractDataLoader{
		Element load()
	}
	note bottom of XMLPathLoader
		把 路径文件 转化为 eTree
		load()：把文件路径转为 eTree
	end note 
		
}

@enduml
```

## Reader
```plantuml
@startuml
class pyodm.core.support.AbstractDataReader{
	+ CdiscRegistry registry
	{abstract} read(AbstractDataLoader loader)
}
note top of AbstractDataReader 
	读取 ODM 数据的抽象类
	read(): 解析 Loader 读取的数据
end note

class pyodm.core.support.AbstractConfigurationReader{
	{abstract} void load_cdisc_definition(CdiscRegistry registry, list[pathlib.Path] files)
}
note top of AbstractConfigurationReader 
	读取 ODM Class 定义的 抽象类
	load_cdisc_definition(): 读取配置文件, 解析ODM Class信息，并注册进CdiscRegistry对象
end note

package pyodm.core.forest.reader{
	class ForestDataReader extends pyodm.core.support.AbstractDataReader{
		Result read(ForestDataLoader data_loader)
	}
	note left of ForestDataReader  
		读取 Forest 类型的数据
		read(ForestDataLoader data_loader): 把Forest转化为 ODM 实例
	end note
}

package pyodm.core.xml{
	package reader{
		package configuration{
			class CdiscConfigurationSpecificationReader extends pyodm.core.support.AbstractConfigurationReader{
				void load_cdisc_definition(CdiscRegistry registry, list[pathlib.Path] files)

			}
			note bottom of CdiscConfigurationSpecificationReader  
				使用 Specification 的方式读取 ODM 定义的 Class信息
				void load_cdisc_definition(CdiscRegistry registry, list[pathlib.Path] files)
			end note
			class CdiscConfigurationXsdReader extends pyodm.core.support.AbstractConfigurationReader{
				void load_cdisc_definition(CdiscRegistry registry, list[pathlib.Path] files)

			}
			note bottom of CdiscConfigurationXsdReader
				使用 XSD 的方式读取 ODM 定义的 Class信息
				void load_cdisc_definition(CdiscRegistry registry, list[pathlib.Path] files)
			end note
		}
		package data{
			class XMLDataReader extends pyodm.core.support.AbstractDataReader{
				Result read(AbstractDataLoader loader)
			}
			note bottom of XMLDataReader 
				读取 XML 的数据
				Result read(AbstractDataLoader loader)
			end note
		}
		package mapper{
			class XMLMapperReader extends pyodm.core.support.AbstractDataReader{
				Result read(AbstractDataLoader loader)
			}
			note bottom of XMLMapperReader 
				读取 Mapper 配置的数据
				Result read(AbstractDataLoader loader)
			end note
		}
	}
}

@enduml
```

## Source
```plantuml
@startuml
package pyodm.core.support{
	abstract class AbstractDataSource{
		{abstract} Result read()
	}
	note top of AbstractDataSource  
		数据源读取原始数据的抽象类
		Result read(): 
	end note
	abstract class AbstractRegistration{
		+ CdiscRegistry registry 
	}
	note top of AbstractRegistration 
		注册 Class 信息抽象类，如果数据源读取的时候需要获取ODM Class 信息，继承此类
	end note
}

package pyodm.core.source{
	
	class PathSource extends pyodm.core.support.AbstractDataSource{
		 pathlib.Path read()
	}
	note bottom: 数据源为文件路径，转化为 pathlib.Path
	abstract class AbstractDataBaseSource extends pyodm.core.support.AbstractDataSource,pyodm.core.support.AbstractRegistration{
		list[list[dict]] read()
	}
	note bottom: 数据源为数据库	
}

@enduml
```

## 依赖关系
```plantuml
@startuml

pyodm.factory.CdiscXMLSpecificationFactory ..> XMLDataReader
pyodm.factory.CdiscXMLSpecificationFactory ..> CdiscConfigurationSpecificationReader
pyodm.factory.CdiscXMLSpecificationFactory ..> XmlPathLoader
pyodm.factory.CdiscXMLXsdFactory ..> XMLDataReader
pyodm.factory.CdiscXMLXsdFactory ..> CdiscConfigurationXsdReader
pyodm.factory.CdiscXMLXsdFactory ..> XmlPathLoader
pyodm.factory.CdiscListableFactory ..> ForestDataReader
pyodm.factory.CdiscListableFactory ..> CdiscConfigurationXsdReader
pyodm.factory.CdiscListableFactory ..> ForestDataLoader
XmlPathLoader ..> PathSource
ForestDataLoader ..> AbstractDataSource
@enduml
```


# 应用场景
## 读取标准的 ODM data 的XML文件
[示例数据 Example1 ](https://wiki.cdisc.org/display/ODM2/ItemGroupData)
- [CdiscXsdFactory](https://github.com/thcpc/pyodm/tree/master/example/xsd_factory)
- [CdiscSpecificationFactory](https://github.com/thcpc/pyodm/tree/master/example/specification_factory)

## 数据库中读取数据，并生成XML文件
[CdiscListableFactory](https://github.com/thcpc/pyodm/tree/master/example/database)
## 自定义结构，并读取 XML 数据
[示例](https://github.com/thcpc/pyodm/tree/master/example/custom_odm_factory)

### Step2. 定义 ODM 对象的配置的XML文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<CDISC>
    <ODM modulePath="pyodm.model.v2.cdisc.ODM" clazz="ODM"/>
    <AuditRecord modulePath="pyodm.model.v2.cdisc.AuditRecord" clazz="AuditRecord"/>
    <ClinicalData modulePath="pyodm.model.v2.cdisc.ClinicalData" clazz="ClinicalData"/>
    <DateTimeStamp modulePath="pyodm.model.v2.cdisc.DateTimeStamp" clazz="DateTimeStamp"/>
    <ItemData modulePath="pyodm.model.v2.cdisc.ItemData" clazz="ItemData"/>
    <ItemGroupData modulePath="pyodm.model.v2.cdisc.ItemGroupData" clazz="ItemGroupData"/>
</CDISC>
```

#### modulePath
class 的 package 路径
#### clazz
class 名

### Step3. CdiscSpecificationFactory 加载ODM
[[pyodm#CdiscSpecificationFactory 生成 ODM 对象| CdiscSpecificationFactory]]
