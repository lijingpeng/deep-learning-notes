## 使用Docker加速器

因为众所周知的原因，国内在进行Docker pull的时候经常出现time out，幸运的是我们可以使用国内的加速镜像来进行加速。DaoCloud和阿里云都提供镜像加速服务，首先要注册DaoCloud和阿里云的账户，以阿里云为例，注册成功后会给出一个专属加速地址，依次找到：
```
控制台—>容器服务—>镜像与模板—>镜像—>镜像仓库控制台—>加速器
```
在https://cr.console.aliyun.com/accelerator 中可以看到地址，地址的格式一般为
```
您的专属加速器地址 https://唯一识别码.mirror.aliyuncs.com
```

## 配置Docker加速器

### Ubuntu设置
下面的设置来自于阿里云官方：

如果您的系统是 Ubuntu 12.04 14.04，Docker 1.9 以上
```
echo "DOCKER_OPTS=\"\$DOCKER_OPTS --registry-mirror=https://bocga3be.mirror.aliyuncs.com\"" | sudo tee -a /etc/default/docker
sudo service docker restart
```
如果您的系统是 Ubuntu 15.04 16.04，Docker 1.9 以上
```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo tee /etc/systemd/system/docker.service.d/mirror.conf <<-'EOF'
[Service]
ExecStart=
ExecStart=/usr/bin/docker daemon -H fd:// --registry-mirror=https://bocga3be.mirror.aliyuncs.com
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### Mac
现在有Docker for Mac客户端:

点击状态栏中的Docker图标->Preferences->Advanced—>Registry mirrors

把加速地址添加到镜像地址中，点击Apply & Restart
