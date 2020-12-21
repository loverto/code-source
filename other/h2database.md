## 导出CSV

curl http://www.h2database.com/h2-2019-10-14.zip -o h2.zip \
&& unzip h2.zip -d . \
&& rm h2.zip

dd if=/data/xintonglu/newpathway.mv.db of=/data/docker/newpathway.mv.db

## 进入控制台

java -cp /data/docker/h2/bin/h2-*.jar org.h2.tools.Shell

jdbc:h2:/data/docker/newpathway;IFEXISTS=TRUE

call CSVWRITE('/data/docker/bak/COMPUTER_TYPE.csv','select * from COMPUTER_TYPE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/CONFIG.csv','select * from CONFIG ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/CUSTOM_STATE.csv','select * from CUSTOM_STATE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/CUSTOM_TEMPLATE.csv','select * from CUSTOM_TEMPLATE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/DATABASECHANGELOG.csv','select * from DATABASECHANGELOG ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/DATABASECHANGELOGLOCK.csv','select * from DATABASECHANGELOGLOCK ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/DESIGN_MATERIAL.csv','select * from DESIGN_MATERIAL ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/DIE_PATTERN.csv','select * from DIE_PATTERN ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/FABRIC_DESIGN_MATERIAL.csv','select * from FABRIC_DESIGN_MATERIAL ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/FINISHED_CONDITION.csv','select * from FINISHED_CONDITION ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/FONT_TYPE.csv','select * from FONT_TYPE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/IMAGE_TYPE.csv','select * from IMAGE_TYPE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_AUTHORITY.csv','select * from JHI_AUTHORITY ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_ENTITY_AUDIT_EVENT.csv','select * from JHI_ENTITY_AUDIT_EVENT ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_PERSISTENT_AUDIT_EVENT.csv','select * from JHI_PERSISTENT_AUDIT_EVENT ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_PERSISTENT_AUDIT_EVT_DATA.csv','select * from JHI_PERSISTENT_AUDIT_EVT_DATA ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_USER.csv','select * from JHI_USER ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/JHI_USER_AUTHORITY.csv','select * from JHI_USER_AUTHORITY ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/MATERIAL_TYPE.csv','select * from MATERIAL_TYPE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/MODEL_TYPE.csv','select * from MODEL_TYPE ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/OFFICIAL_GALLERY.csv','select * from OFFICIAL_GALLERY ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/RECOMMENDED_STATUS.csv','select * from RECOMMENDED_STATUS ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/UPLOAD_SETTINGS.csv','select * from UPLOAD_SETTINGS ','charset=UTF-8 fieldSeparator=;');
call CSVWRITE('/data/docker/bak/USER_EXTENDS.csv','select * from USER_EXTENDS ','charset=UTF-8 fieldSeparator=;');