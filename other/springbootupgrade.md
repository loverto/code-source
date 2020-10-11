# popay 升级Spring Boot

1.5.10.RELEASE 升级到 2.3.4.RELEASE


1. druid数据源连接池优化

```java
DataSourceBuilder.create().build(); 改为 new DruidDataSource();
```

1. SqlSessionFactoryBean @ConfigurationPropertes注解修改

```java

    @ConfigurationProperties(prefix = "mybatis")
    public SqlSessionFactoryBean sqlSessionFactoryBean() {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        // 配置数据源，此处配置为关键配置，如果没有将 dynamicDataSource 作为数据源则不能实现切换
        sqlSessionFactoryBean.setDataSource(dynamicDataSource());
        return sqlSessionFactoryBean;
    }


    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(MybatisProperties properties, ResourceLoader resourceLoader) {
        SqlSessionFactoryBean factory = new SqlSessionFactoryBean();
        // 配置数据源，此处配置为关键配置，如果没有将 dynamicDataSource 作为数据源则不能实现切换
        factory.setDataSource(dynamicDataSource());
        if (StringUtils.hasText(properties.getConfigLocation())) {
            factory.setConfigLocation(resourceLoader.getResource(properties.getConfigLocation()));
        }
        if (properties.getConfigurationProperties() != null) {
            factory.setConfigurationProperties(properties.getConfigurationProperties());
        }

        if (StringUtils.hasLength(properties.getTypeAliasesPackage())) {
            factory.setTypeAliasesPackage(properties.getTypeAliasesPackage());
        }
        if (properties.getTypeAliasesSuperType() != null) {
            factory.setTypeAliasesSuperType(properties.getTypeAliasesSuperType());
        }
        if (StringUtils.hasLength(properties.getTypeHandlersPackage())) {
            factory.setTypeHandlersPackage(properties.getTypeHandlersPackage());
        }

        if (!ObjectUtils.isEmpty(properties.resolveMapperLocations())) {
            factory.setMapperLocations(properties.resolveMapperLocations());
        }
        return factory;
    }


```

配置文件修改

```yaml
spring:
  http:
    encoding:
      charset: utf-8

```
改为下面配置

```yaml
server:
  servlet:
    encoding:
      charset: UTF-8
```

```yaml
server:
  context-path: /popay
```

```yaml
server:
  servlet:
    context-path: /popay
```

修改mysql数据库驱动

```
com.mysql.jdbc.Driver -> com.mysql.cj.jdbc.Driver
```


升级后 mockito 包中无以下类，用spring替代

org.mockito.cglib.beans.BeanMap; 

升级后web不在引入validation配置，需要手动引入

spring-boot-starter-validation

新加

升级druid jar包版本

1.1.10 -> 1.2.1

升级mybatis版本
mybatis-springboot.version

1.3.2 -> 2.1.3