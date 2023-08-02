#java #jar
build.gradle
```
plugins {  
    id 'java'  
    id 'com.github.johnrengelman.shadow' version '7.0.0'  
}
shadowJar {  
    configurations = [project.configurations.runtimeClasspath]  
    manifest {  
        attributes 'Main-Class': 'edetek.Main'  
    }  
}
```

运行
```
 .\gradlew shadowjar
```
