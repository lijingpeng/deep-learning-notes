## All in one docker

如果你不想单独安装每个深度学习组件，并且厌倦于安装过程中的各种依赖冲突等问题，那么推荐你使用Docker来搭建深度学习工作环境。下面是一个可以参考的 [All in one docker](https://github.com/lijingpeng/dl-docker) 环境。几乎包含了所有的流行的深度学习框架，并且分别有CPU版本和GPU版本，与虚拟机不同的是，Docker几乎没有性能损失，因此你可以放心的使用它。需要注意的是，GPU版本的Docker只能在Linux系统上运行。

## 包含的框架及系统依赖

- Ubuntu 14.04
- CUDA 7.5 (GPU version only)
- cuDNN v4 (GPU version only)
- Tensorflow
- Caffe
- Theano
- Keras
- Lasagne
- Torch (includes nn, cutorch, cunn and cuDNN bindings)
- iPython/Jupyter Notebook (including iTorch kernel)
- Numpy, SciPy, Pandas, Scikit Learn, Matplotlib
- A few common libraries used for deep learning

## build

CPU version
```
docker pull floydhub/dl-docker:cpu
```

## RUN

CPU Version
```
docker run -it -p 8888:8888 -p 6006:6006 -v /sharedfolder:/root/sharedfolder floydhub/dl-docker:cpu bash
```

GPU Version
```
nvidia-docker run -it -p 8888:8888 -p 6006:6006 -v /sharedfolder:/root/sharedfolder floydhub/dl-docker:gpu bash
```
