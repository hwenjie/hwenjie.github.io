## DockerFile介绍

dockerfile是用来构建镜像的文件

构建：  
1、编写dockerfile文件  
2、docker build 构建镜像文件  
3、docker run 运行镜像  
4、docker push 发布镜像

![avatar](./images/WX20200809-014156@2x.png ':size=400')


## DockerFile的指令


```shell
FROM            # 基础镜像，指定构建的镜像基于哪个镜像
MAINTAINER      # 镜像作者/维护者 信息
RUN             # 构建镜像时运行的命令
ADD             # 添加内容（复制文件到镜像）
WORKDIR         # 镜像工作目录、默认当前目录
VOLUME          # 挂载的目录
EXPOSE          # 暴露的端口
RUN             # 运行
CMD             # 指定容器启动时运行的命令，只有最后一个有效，会被替代
ENTRYOINT       # 指定容器启动时运行的命令，以追加的方式
ONBUILD         # 被继承时触发
COPY            # 类似ADD 将文件拷贝到镜像中
ENV             # 构建时设置环境变量
```
![avatar](./images/WX20200809-014401@2x.png ':size=600')

## 实例


