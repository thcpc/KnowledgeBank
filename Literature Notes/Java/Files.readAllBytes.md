#java  #file 

# 应用场景
小文件一次性加载

```Java
String fileName = "xxx\\xxx";
byte[] bytes = Files.readAllBytes(Paths.get(fileName));
String content = new String(bytes, StandardCharsets.UTF_8);
```