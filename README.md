 pytorch-deep-learning
 ========
 优化算法
 -------
### 梯度下降与随机梯度下降
#### 一维梯度下降
一维梯度下降可以让我们更加直观地去理解梯度下降的概念以及方式.优化算法的目标就是通过迭代不断更新变量值,使得目标函数f(x)的值不断减小,达到我们想要的效果.<br>
![一维梯度下降](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/images/%E4%B8%80%E7%BB%B4.png)
![](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/images/1.png)

#### 多维梯度下降
在神经网络中,我们处理的优化问题一般是多维的,需要同时去优化多维的变量,首先是去表示出目标函数对各个变量的偏导数. <br>
![](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/images/2.png)
在某点x处,我们可以表示出以该点为圆心,半径为"1"的各个方向,向外散发的方向导数,然后我们期望得到使目标函数的值不断减小的同时,其减小的速度也是最快的,即寻找方向导数的最小值,这样是下降最快的方向.
![](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/images/3.png)
