# Jenkins

```shell
docker pull jenkins
docker run -d -p 49001:8080 -v $PWD/jenkins:/var/jenkins_home:z -t jenkins --name ci-1
```



查看版本 `docker run jenkins --version`

