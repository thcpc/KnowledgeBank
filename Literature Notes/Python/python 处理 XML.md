
# 通过 XSD 生成 XML

```python
pip install lxml
pip install xmlschema
```

```python
from lxml import etree  
from xmlschema import XMLSchema  
  
# 加载XSD文件并创建Schema对象  
schema = XMLSchema("example.xsd")  
  
# 创建根元素  
root = etree.Element(schema.elements["Root"].tag)  
  
# 创建子元素  
child = etree.SubElement(root, schema.elements["Child"].tag)  
child.text = "Child Text"  
  
# 创建子元素的子元素  
grandchild = etree.SubElement(child, schema.elements["Grandchild"].tag)  
grandchild.text = "Grandchild Text"  
  
# 打印生成的XML  
print(etree.tostring(root, pretty_print=True).decode())
```