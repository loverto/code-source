# git 登录账号密码输错，提示授权失败

fatal: Authentication failed for

设置重新登陆

git config --system --unset credential.helper

# Svn 迁移 git 操作实战记录

## 正则表达式生成svn系统中用到的用户名

svn log --xml --quiet | grep author | sort -u | \
  perl -pe 's/.*>(.*?)<.*/$1 = /'

## users.txt的内容如下  

cuiw = cuiw<cuiw@xinfangshengcom>
tangy = tangy<tangy@xinfangsheng.com>
wanghongwei = wanghongwei<wanghongwei@xinfangsheng.com>
yinlf = yinlf<yinlf@xinfangsheng.com>
zhangh = zhangh<zhangh@xinfangsheng.com>

## 用yinlf这个用户登录svn，用users.txt 映射svn与git的用户映射关系

## 下载git 时注意指定的目录和名称映射

git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/popay --branches=branchs --tags=tag --trunk=trunks/popay --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --no-minimize-url
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/communal --branches=branchs --tags=tag --trunk=trunks/communal  --ignore-paths='(\\.idea|logPath_IS_UNDEFINED|.*\\.iml|target)' --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --no-minimize-url
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/communal_base --branches=initial --tags=tag --trunk=trunk/communal_base  --ignore-paths='(\\.idea|logPath_IS_UNDEFINED|.*\\.iml|target)' --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --no-minimize-url

git svn clone svn://192.168.0.51/dev/preorder --tags=tag --branches=trunk --ignore-paths='(\\.idea|logPath_IS_UNDEFINED|.*\\.iml|target)'  --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --no-minimize-url

git svn clone svn://192.168.0.51/dev/openplatform --tags=tags --branches=branches --trunk=trunk/openplatform    --ignore-paths='(\\.idea|logPath_IS_UNDEFINED|.*\\.iml|target)'  --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --no-minimize-url


git svn clone svn://192.168.0.51/dev/gateway --branches=trunk/2.0.9.*/open-api  --username=yinlf --authors-file=users.txt --no-metadata --prefix "" --ignore-paths='(\\.idea|logPath_IS_UNDEFINED|.*\\.iml|target)'  --no-minimize-url 
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/popay --branches=branchs --tags=tag --trunk=trunks/popay --username=yinlf --authors-file=users.txt --no-metadata --prefix ""
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/popay --branches=branchs --tags=tag --trunk=trunks/popay --username=yinlf --authors-file=users.txt --no-metadata --prefix ""
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/popay --branches=branchs --tags=tag --trunk=trunks/popay --username=yinlf --authors-file=users.txt --no-metadata --prefix ""
git svn clone svn://192.168.0.51/XINFANGSHENG/trunk/popay --branches=branchs --tags=tag --trunk=trunks/popay --username=yinlf --authors-file=users.txt --no-metadata --prefix ""

## svn 标签转换为git 格式的标签

for t in $(git for-each-ref --format='%(refname:short)' refs/remotes/tags); do git tag tag-${t/tags\//} $t && git branch -D -r $t; done

## svn 分支转换为git 分支

for b in $(git for-each-ref --format='%(refname:short)' refs/remotes); do git branch $b refs/remotes/$b && git branch -D -r $b; done

## 移除无效的svn分支

for p in $(git for-each-ref --format='%(refname:short)' | grep @); do git branch -D $p; done

## 推送所有分支

git push -u origin --all


## 推送所有的tag

git push -u origin --tags


## docker svn2git 操作