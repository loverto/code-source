# Idea 常见问题


报错内容:

Error running 'ServiceStarter': Command line is too long. Shorten command line for ServiceStarter or also for Application default configuration.

 

解法:

修改项目下 `.idea\workspace.xml`，找到标签 

```xml 
<component name="PropertiesComponent">
``` 
 在标签里加一行  

```xml
<property name="dynamic.classpath" value="true" />
```

