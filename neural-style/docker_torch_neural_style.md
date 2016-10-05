## 在Docker中运行torch版的neural style

[TensorFlow neural-style](https://github.com/anishathalye/neural-style), TensorFlow版本的实现比Torch版本的实现要慢很多，因此本文介绍如何运行torch版本的neural style。为了避免搭建环境时候解决各种烦人的依赖问题，这里依然使用Docker环境，Dockerfile来自[这里](https://github.com/lijingpeng/dl-docker). 环境搭建可以参考[这里](https://github.com/lijingpeng/deep-learning-notes/blob/master/environment/all_in_one_docker.md)

torch版本的neural style来自于[jcjohnson-neural-style](https://github.com/jcjohnson/neural-style)，支持CPU和GPU，依赖torch7和loadcaffe，Docker环境中已经安装好了这些依赖。

## 下载训练好的VGG网络
