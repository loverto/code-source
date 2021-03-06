## datax web


```yaml
version: '3.7'
services:
  mysql:
    image: mysql:8.0.22
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_USER: root
      MYSQL_DATABASE: dataxweb
      TZ: Asia/Shanghai
      character-set-server:  utf8mb4
      collation-server: utf8mb4_unicode_ci
    ports:
      - 3307:3306
    volumes:
      - ./mysql/data:/var/lib/mysql
      - ./mysql/conf:/etc/mysql/conf.d
      - ./mysql/logs:/logs
      - ./mysql/db:/docker-entrypoint-initdb.d/
    container_name: datax_web_mysql
  
  datax_admin:
    image: 1045438139/datax-admin-alpine:v2.1.2_1_beta1
    restart: always
    ports:
      - 9527:9527
    environment:
      SPRING_DATASOURCE_URL: jdbc:mysql://datax_web_mysql:3306/dataxweb?serverTimezone=Asia/Shanghai&useLegacyDatetimeCode=false&useSSL=false&nullNamePatternMatchesAll=true&useUnicode=true&characterEncoding=UTF-8&allowPublicKeyRetrieval=true
      SERVER_PORT: 9527
      DB_USERNAME: root
      DB_PASSWORD: root
      DB_HOST: datax_web_mysql
      DB_PORT: 3306
      DB_DATABASE: dataxweb
      MAIL_USERNAME: email_name
      MAIL_PASSWORD: email_password
    depends_on:
      - mysql
    container_name: dataxWebAdmin
  
  
  datax_executor:
    image: 1045438139/datax-executor-alpine:v2.1.2_1_beta1
    restart: always
    volumes:
      - ./datax_web/executor/python:/opt/datax-executor/python
      - ./datax_web/executor/json:/opt/datax-executor/json
      - ./datax_web/executor/data:/opt/datax-executor/data
    ports:
      - 8085:8081
    environment:
      SERVER_PORT: 8081
      DATAX_ADMIN_HOST: dataxWebAdmin
      DATAX_ADMIN_PORT: 9527
      EXECUTOR_PORT: 9999
      DATAX_JOB_ADMIN_ADDRESSES: http://dataxWebAdmin:9527
      PYTHON_PATH: /opt/datax-executor/python/datax/bin/datax.py
      JSON_PATH: /opt/datax-executor/json
      DATA_PATH: /opt/datax-executor/data
      LOGGING_LEVEL_ROOT: INFO
    #此处检测 dataxWebAdmin是否成功启动，如果将container_name和container_name.SERVER_PORT改变，请将下面的命令做响应该改变
    #entrypoint: "/etc/init.d/wait-for-it.sh dataxWebAdmin:9527 --  java com.wugui.datax.executor.DataXExecutorApplication"
    depends_on:
      - mysql
      - datax_admin
    container_name: datax_web_executor
```



## 参考资料

