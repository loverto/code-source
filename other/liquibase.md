docker run --rm -v /e/workspace/jhipster/new-pathway/build/h2db/db/:/liquibase/changelog -v /e/workspace/jhipster/new-pathway/build/h2db/db/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url="jdbc:sqlserver://<IP OR HOSTNAME>:1433;database=<DATABASE>;" --changeLogFile=com/example/changelog.xml --username=<USERNAME> --password=<PASSWORD> --liquibaseProLicenseKey="<PASTE LB PRO LICENSE KEY HERE>" updat

## 比较两个库差异,只包含表结构
docker run --rm -v /e/workspace/jhipster/new-pathway/build/h2db/db/:/liquibase/changelog -v /e/workspace/jhipster/new-pathway/build/h2db/db/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/changelog.xml diffChangeLog   --referenceUrl=jdbc:postgresql://192.168.56.1:5432/NewPathway --referenceUsername=NewPathway --referencePassword=

## 生成差异语句,带数据
docker run --rm -v /e/workspace/jhipster/new-pathway/build/h2db/db/:/liquibase/changelog -v /e/workspace/jhipster/new-pathway/build/h2db/db/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://192.168.56.1:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl=jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1; --referenceUsername=NewPathway --referencePassword=
docker run --rm -v /e/wsl/:/liquibase/changelog -v /e/wsl/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://192.168.56.1:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl=jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1; --referenceUsername=NewPathway --referencePassword=
docker run --rm -v /e/wsl/:/liquibase/changelog -v /e/wsl/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://192.168.56.1:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl=jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1; --referenceUsername=NewPathway --referencePassword=  --excludeObjects="table:JHI_PERSISTENT_AUDIT_EVENT,table:JHI_ENTITY_AUDIT_EVENT,table:JHI_PERSITENT_AUDIT_EVT_DATA,table:CUSTOM_TEMPLATE"

## 更新语句
docker run --rm -v /e/workspace/jhipster/new-pathway/build/h2db/db/:/liquibase/changelog -v /e/workspace/jhipster/new-pathway/build/h2db/db/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://192.168.56.1:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=pgchangelog.xml  update


liquibase \
--url=jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/changelog.xml diffChangeLog   --referenceUrl=jdbc:postgresql://localhost:5432/NewPathway --referenceUsername=NewPathway --referencePassword=


## Linux 中文件权限问题

https://www.cnblogs.com/woshimrf/p/understand-docker-uid.html

指定用户解决,容器中的用户是liquibase,实际用户是root

## NewPathway online
docker run  -e JAVA_OPTS='-Xmx25G'  --rm -u 0:0 -v /data/xintonglu/minio/data/admin-app/:/liquibase/changelog -v /data/xintonglu/minio/data/admin-app/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl="jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;" --referenceUsername=NewPathway --referencePassword= --excludeObjects="table:JHI_PERSISTENT_AUDIT_EVENT,table:JHI_ENTITY_AUDIT_EVENT,table:JHI_PERSITENT_AUDIT_EVT_DATA,table:CUSTOM_TEMPLATE"
docker run --rm -u 0:0 -v /data/xintonglu/minio/data/admin-app/:/liquibase/changelog -v /data/xintonglu/minio/data/admin-app/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  diffChangeLog   --referenceUrl="jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;" --referenceUsername=NewPathway --referencePassword= --excludeObjects="table:JHI_PERSISTENT_AUDIT_EVENT,table:JHI_ENTITY_AUDIT_EVENT,table:JHI_PERSITENT_AUDIT_EVT_DATA,table:CUSTOM_TEMPLATE"

docker run  -e JAVA_OPTS='-Xmx25G' --rm -u 0:0 -v /data/xintonglu/:/liquibase/changelog -v /data/xintonglu/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl="jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;" --referenceUsername=NewPathway --referencePassword= --excludeObjects="table:JHI_PERSISTENT_AUDIT_EVENT,table:JHI_ENTITY_AUDIT_EVENT,table:JHI_PERSITENT_AUDIT_EVT_DATA"
docker run  -e JAVA_OPTS='-Xmx25G' --rm -u 0:0 -v /data/xintonglu/:/liquibase/changelog -v /data/xintonglu/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --logLevel=debug --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  --dataOutputDirectory=/liquibase/changelog diffChangeLog   --referenceUrl="jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;" --referenceUsername=NewPathway --referencePassword=
docker run  -e JAVA_OPTS='-Xmx25G' --rm -u 0:0 -v /data/xintonglu/:/liquibase/changelog -v /data/xintonglu/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=/liquibase/changelog/pgchangelog.xml  diffChangeLog   --referenceUrl="jdbc:h2:file:/liquibase/newpathway;DB_CLOSE_DELAY=-1;" --referenceUsername=NewPathway --referencePassword=

docker run --rm -v /data/xintonglu/:/liquibase/changelog -v /data/xintonglu/newpathway.mv.db:/liquibase/newpathway.mv.db liquibase/liquibase:4.2 --url=jdbc:postgresql://172.17.189.23:5432/NewPathway  --username=NewPathway  --password=  --changeLogFile=pgchangelog.xml  update

## Liquibase 使用经验

https://zhuanlan.zhihu.com/p/55962347

https://www.cnblogs.com/harbin1900/p/10976421.html



