# postgresql 导入csv数据

##  切换到 用户

su - postgres

# 使用客户端登陆

psql -h localhost -p 5432 -U NewPathway


## 访问数据库

1、列举数据库：\l
2、选择数据库：\c 数据库名
3、查看该某个库中的所有表：\dt
4、切换数据库：\c interface
5、查看某个库中的某个表结构：\d 表名
6、查看某个库中某个表的记录：select * from apps limit 1;
7、显示字符集：\encoding
8、退出psgl：\q




导入 csv 文件到 custom_template 表，需要用到 COPY 命令


COPY COMPUTER_TYPE FROM '/var/lib/postgresql/COMPUTER_TYPE.csv' DELIMITER ';' CSV HEADER;
COPY CONFIG FROM '/var/lib/postgresql/CONFIG.csv' DELIMITER ';' CSV HEADER;
COPY CUSTOM_STATE FROM '/var/lib/postgresql/CUSTOM_STATE.csv' DELIMITER ';' CSV HEADER;
COPY DATABASECHANGELOG FROM '/var/lib/postgresql/DATABASECHANGELOG.csv' DELIMITER ';' CSV HEADER;
COPY DATABASECHANGELOGLOCK FROM '/var/lib/postgresql/DATABASECHANGELOGLOCK.csv' DELIMITER ';' CSV HEADER;
COPY DESIGN_MATERIAL FROM '/var/lib/postgresql/DESIGN_MATERIAL.csv' DELIMITER ';' CSV HEADER;
COPY FINISHED_CONDITION FROM '/var/lib/postgresql/FINISHED_CONDITION.csv' DELIMITER ';' CSV HEADER;
COPY FONT_TYPE FROM '/var/lib/postgresql/FONT_TYPE.csv' DELIMITER ';' CSV HEADER;
COPY IMAGE_TYPE FROM '/var/lib/postgresql/IMAGE_TYPE.csv' DELIMITER ';' CSV HEADER;
COPY JHI_AUTHORITY FROM '/var/lib/postgresql/JHI_AUTHORITY.csv' DELIMITER ';' CSV HEADER;
COPY JHI_ENTITY_AUDIT_EVENT FROM '/var/lib/postgresql/JHI_ENTITY_AUDIT_EVENT.csv' DELIMITER ';' CSV HEADER;
COPY JHI_PERSISTENT_AUDIT_EVENT FROM '/var/lib/postgresql/JHI_PERSISTENT_AUDIT_EVENT.csv' DELIMITER ';' CSV HEADER;
COPY JHI_PERSISTENT_AUDIT_EVT_DATA FROM '/var/lib/postgresql/JHI_PERSISTENT_AUDIT_EVT_DATA.csv' DELIMITER ';' CSV HEADER;
COPY JHI_USER FROM '/var/lib/postgresql/JHI_USER.csv' DELIMITER ';' CSV HEADER;
COPY JHI_USER_AUTHORITY FROM '/var/lib/postgresql/JHI_USER_AUTHORITY.csv' DELIMITER ';' CSV HEADER;
COPY MATERIAL_TYPE FROM '/var/lib/postgresql/MATERIAL_TYPE.csv' DELIMITER ';' CSV HEADER;
COPY MODEL_TYPE FROM '/var/lib/postgresql/MODEL_TYPE.csv' DELIMITER ';' CSV HEADER;
COPY OFFICIAL_GALLERY FROM '/var/lib/postgresql/OFFICIAL_GALLERY.csv' DELIMITER ';' CSV HEADER;
COPY RECOMMENDED_STATUS FROM '/var/lib/postgresql/RECOMMENDED_STATUS.csv' DELIMITER ';' CSV HEADER;
COPY UPLOAD_SETTINGS FROM '/var/lib/postgresql/UPLOAD_SETTINGS.csv' DELIMITER ';' CSV HEADER;
COPY USER_EXTENDS FROM '/var/lib/postgresql/USER_EXTENDS.csv' DELIMITER ';' CSV HEADER;



COPY DIE_PATTERN FROM '/var/lib/postgresql/DIE_PATTERN.csv' DELIMITER ';' CSV HEADER;

COPY CUSTOM_TEMPLATE FROM '/var/lib/postgresql/CUSTOM_TEMPLATE.csv' DELIMITER ';' CSV HEADER;

COPY FABRIC_DESIGN_MATERIAL FROM '/var/lib/postgresql/FABRIC_DESIGN_MATERIAL.csv' DELIMITER ';' CSV HEADER;

首先，您在COPY关键字之后指定带有列名称的表。列的顺序必须与CSV文件中的顺序相同。如果CSV文件包含表的所有列，则无需显式指定它们，例如：

其次，将CSV文件路径放在FROM关键字之后。由于使用的是CSV文件格式，因此您需要指定DELIMITER和CSV子句。

第三，指定HEADER关键字以指示CSV文件包含标题。当COPY命令导入数据时，它将忽略文件的头。

请注意，该文件必须由PostgreSQL服务器直接读取，而不是由客户端应用程序直接读取。因此，PostgreSQL服务器计算机必须可以访问它。另外，您需要具有超级用户访问权限才能COPY成功执行该语句。

## 创建带初始值的sequence

-- auto-generated definition
create sequence sequence_generator
increment by 50 start with 1550;

alter sequence sequence_generator owner to "NewPathway";

select nextval('sequence_generator')

## 设置最大值

select setval('sequence_generator', 4000000);


## 注意事项

postgres:11.3

1。 COPY 导入数据时发生错误，无法忽略


## 参考资料
[Csv 导入pg](https://www.postgresqltutorial.com/import-csv-file-into-posgresql-table/)
[PostgreSQL操作-psql基本命令](https://www.cnblogs.com/my-blogs-for-everone/p/10226473.html)
