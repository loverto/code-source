# gradle

## gradle 中 api 提示无法使用

1. 先确定 gradle 版本是否高于3.4

2. 如果高于3.4 确认以下使用的插件是java还是java-library

替换为java-library解决问题

[参考文档](https://stackoverflow.com/questions/49423330/could-not-find-method-api-for-arguments-directory-libs)