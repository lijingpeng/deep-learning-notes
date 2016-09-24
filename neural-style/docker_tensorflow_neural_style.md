
## 在Docker中运行Tensorflow版本的neural style


## Docker镜像构建


Dockerfile如下：
```bash
FROM tensorflow/tensorflow:latest

RUN echo deb http://mirrors.aliyun.com/ubuntu trusty universe >> /etc/apt/sources.list
RUN echo deb http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb-src http://mirrors.aliyun.com/ubuntu/ trusty main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main restricted universe multiverse >> /etc/apt/sources.list
RUN echo deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main restricted universe multiverse >> /etc/apt/sources.list
RUN apt-get update

# clone code
RUN apt-get install -y --no-install-recommends git
RUN git clone https://github.com/anishathalye/neural-style.git

# install pillow and its dependences
RUN apt-get install -y libffi-dev libssl-dev libtiff5-dev libjpeg8-dev zlib1g-dev \
    libfreetype6-dev liblcms2-dev libwebp-dev tcl8.6-dev tk8.6-dev python-tk
RUN pip install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/ -U pip
RUN pip install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/ -U Pillow
# RUN pip install --trusted-host pypi.douban.com -i http://pypi.douban.com/simple/ -U pyopenssl ndg-httpsclient pyasn1

# Too slow, use docker volume instead
# RUN apt-get install -y wget
# RUN wget http://www.vlfeat.org/matconvnet/models/beta16/imagenet-vgg-verydeep-19.mat
# RUN mv imagenet-vgg-verydeep-19.mat neural-style

CMD ["/run_jupyter.sh"]
```

复制这段代码，创建名为Dockerfile的文件，然后执行：
```bash
docker build -t docker_neural_style .
```

注意：
1. 本镜像的构建基于Tensorflow官方，请放心使用
2. 依赖已经训练好的网络：imagenet-vgg-verydeep-19.mat，这个文件有500M多，下载站点在国外，不建议在Docker构建过程中直接下载，可以使用下载工具比如迅雷下载到本地，然后把文件映射到容器中就可以了。

## 下载已经训练好的深度网络


```
wget http://www.vlfeat.org/matconvnet/models/beta16/imagenet-vgg-verydeep-19.mat
```

假设该文件保存在 /Users/you/ 目录下

## 在Docker中执行

```bash
docker run -it -p 8888:8888 -v /Users/you:/notebooks/neural-style-mat docker_neural_style /bin/bash
```
注意：这条命令将/Users/you/映射到容器中的/notebooks/neural-style-mat并启动容器。

```bash
python neural_style.py --content examples/1-content.jpg --styles examples/1-style.jpg --output examples/myoutput.jpg --network ../neural-style-mat/imagenet-vgg-verydeep-19.mat
```

执行neural_style脚本。
需要注意的是Tensorflow不支持L-BFGS, 并且由Tensorflow的实现比Torch慢三倍左右。在笔者的MacBook Pro上，纯CPU跑梵高风格画作迭代1000轮要耗时6个小时左右。鉴于此，有条件的直接上GPU吧。
