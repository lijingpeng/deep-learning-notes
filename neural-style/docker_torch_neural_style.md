## 在Docker中运行torch版的neural style

[TensorFlow neural-style](https://github.com/anishathalye/neural-style), TensorFlow版本的实现比Torch版本的实现要慢很多，因此本文介绍如何运行torch版本的neural style。为了避免搭建环境时候解决各种烦人的依赖问题，这里依然使用Docker环境，Dockerfile来自[这里](https://github.com/lijingpeng/dl-docker). 环境搭建可以参考[这里](https://github.com/lijingpeng/deep-learning-notes/blob/master/environment/all_in_one_docker.md)

torch版本的neural style来自于[jcjohnson-neural-style](https://github.com/jcjohnson/neural-style)，支持CPU和GPU，依赖torch7和loadcaffe，Docker环境中已经安装好了这些依赖。

## 下载训练好的VGG网络
首先clone代码：
```
https://github.com/jcjohnson/neural-style
```

neural style需要训练好的VGG网络结果，这里需要提前下载好：
```
sh models/download_models.sh
```
需要加载以下两个文件：
VGG_ILSVRC_19_layers.caffemodel
VGG_ILSVRC_19_layers_deploy.prototxt
caffemodel文件比较大，建议使用下载工具下载到本地。

## 在Docker中运行

#### 第一步：运行Docker
由于VGG训练结果文件、图片文件等都存放到了本地电脑上，因此我们在启动docker时需要把这些文件映射到Docker中
```
docker run -it -p 8888:8888 -p 6006:6006 -v /Users/frank:/root/sharedfolder floydhub/dl-docker-load:cpu
```
我这里直接把用户主文件夹映射进去了，实际可以根据自己文件的存放位置来调整。

#### 第二步：进入neural style代码的目录
假设clone下来的代码存放到了/Users/frank/Downloads/neural-style下，则：
```
cd ~/sharedfolder/Downloads/neural-style
```

#### 第三步：执行

```bash
th neural_style.lua -style_image examples/inputs/starry_night.jpg -content_image ~/sharedfolder/Downloads/content.png -output_image ~/sharedfolder/Downloads/nn_out.png -model_file ~/sharedfolder/Downloads/VGG_ILSVRC_19_layers.caffemodel -proto_file ~/sharedfolder/Downloads/VGG_ILSVRC_19_layers_deploy.prototxt -gpu -1 -optimizer adam -num_iterations 800 -print_iter 1
```


- -style_image 表示风格图片文件位置
- -content_image 表示内容图片的位置，也就是你想要改变风格的文件
- -output_image 表示输出文件位置
- -model_file 表示下载好的caffemodel文件
- -proto_file caffemodel模型的配置文件
- -gpu -1 -1表示不采用GPU，使用CPU版本
- -optimizer adam 优化方式选择adam，速度较快，但是结果一般没有L-BFGS好
- -num_iterations 迭代次数
- -print_iter 1 每一轮迭代都要在控制台上显示一次结果


更多参数设置请参考：[neural-style](https://github.com/jcjohnson/neural-style)

接下来就是漫长的等待了，如果使用CPU的话这个等待时间将会非常长...
