## window 下使用jhipster的docker 镜像创建项目
docker run --name jhipster -v /e/workspace/java/baidu:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster
