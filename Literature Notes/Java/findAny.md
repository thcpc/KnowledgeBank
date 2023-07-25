#java #stream

```java
public PropertyValue getPropertyValue(String propertyName){
       Optional<PropertyValue> propertyValueOptional = propertyValues.stream().filter(propertyValue -> propertyValue.getName().equals(propertyName)).findAny();
       return propertyValueOptional.orElse(null);
    }
```