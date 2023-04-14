#spring-docker-package-maven-plugin

为了优化docker构建，可以更好的利用缓冲层。在Maven package 打包阶段，把jar 中的文件提出到不同的文件夹中，并生成对应的DockerFile。
DockerFile 中基础镜像默认使用jdk17。 可以通过baseImage 参数指定自定义镜像。

最终编译结果在 target/docker 中。 一般情况只需要进入该文件夹中执行 docker build 命令。

提出jar命令：

```bash
java -Djarmode=layertools -jar JAR_NAME extract --destination DIR
```

DockerFile：

```dockerfile
FROM openjdk:17-jdk-alpine
RUN addgroup --system app && adduser --system --no-create-home --ingroup app app
USER app
WORKDIR /app
COPY run.sh ./
COPY dependencies/ ./
COPY spring-boot-loader/ ./
COPY snapshot-dependencies/ ./
COPY application/ ./
ENTRYPOINT ["sh","./run.sh"]
```