激活函数
-------
https://zhuanlan.zhihu.com/p/73214810
#### 为什么会用到激活函数？
首先数据的分布绝大多数是非线性的，而一般神经网络的计算是线性的，引入激活函数，是在神经网络中引入非线性，强化网络的学习能力。所以激活函数的最大特点就是非线性。
#### 为什么会存在多种激活函数？
Sigmoid和tanh的特点是将输出限制在(0,1)和(-1,1)之间，说明Sigmoid和tanh适合做概率值的处理，例如LSTM中的各种门；而ReLU就不行，因为ReLU无最大值限制，可能会出现很大值。同样，根据ReLU的特征，Relu适合用于深层网络的训练，而Sigmoid和tanh则不行，因为它们会出现梯度消失。
#### 使用relu激活函数时，是否还存在梯度消失的问题？
梯度衰减因子包括激活函数导数，此外，还有多个权重连乘也会影响。梯度消失只是表面说法，按照这样理解，底层使用非常大的学习率，或者人工添加梯度噪音，原则上也能回避，有不少论文这样试了，然而目前来看，有用，但没太大的用处。
#### 1.sigmoid
sigmoid函数也称为Logistic函数，因为Sigmoid函数可以从Logistic回归（LR）中推理得到，也是LR模型指定的激活函数。<br>
sigmod函数的取值范围在（0, 1）之间，可以将网络的输出映射在这一范围，方便分析。<br>
函数表达式以及导数表达式：<br>
![sigmoid](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/sigmoid.png)<br>
函数以及导数曲线：<br>
![sigmoid](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/sigmoid2.png)
优点：平滑曲线且易于求导。<br>
缺点：<br>
1.激活函数计算量大（在正向传播和反向传播中都包含幂运算和除法)<br>
2.反向传播求误差梯度时，求导涉及除法<br>
3.Sigmoid导数取值范围是[0, 0.25]，由于神经网络反向传播时的“链式反应”，很容易就会出现梯度消失的情况。例如对于一个10层的网络， 根据0.25^10 = 0.000000954，第10层的误差相对第一层卷积的参数W1的梯度将是一个非常小的值，这就是所谓的“梯度消失”<br>
4.Sigmoid的输出不是0均值（即zero-centered,这会导致后一层的神经元将得到上一层输出的非0均值的信号作为输入，随着网络的加深，会改变数据的原始分布。

#### 2.tanh
tanh(Hyperbolic Tangent)为双曲正切函数,tanh和 sigmoid 相似，都属于饱和激活函数，区别在于输出值范围由(0,1)变为了(-1,1)，可以把tanh函数看做是sigmoid向下平移和拉伸后的结果,从其函数表达式即可看出。<br>
函数表达式：<br>
![tanh](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/tanh.png)
函数及其导数曲线：<br>
![tanh1](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/tanh1.png)
![tanh2](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/tanh2.png)
相比Sigmoid函数:<br>
1.tanh的输出范围时(-1, 1)，解决了Sigmoid函数的不是zero-centered输出问题 <br>
2.幂运算的问题仍然存在 <br>
3.tanh导数范围在(0, 1)之间，相比sigmoid的(0, 0.25)，梯度消失（gradient vanishing）问题会得到缓解，但仍然还会存在。<br>

#### 3.ReLu
Relu(Rectified Linear Unit)——修正线性单元函数：该函数形式比较简单，表达式为：relu=max(0, x)<br>
函数及其导数曲线：<br>
![relu](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/relu.png)
![relu](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E6%BF%80%E6%B4%BB%E5%87%BD%E6%95%B0%E4%B8%8E%E6%8D%9F%E5%A4%B1%E5%87%BD%E6%95%B0/img/relu1.png) <br>
从上图可知，ReLU的有效导数是常数1，解决了深层网络中出现的梯度消失问题，也就使得深层网络可训练。同时ReLU又是非线性函数，所谓非线性，就是一阶导数不为常数；对ReLU求导，在输入值分别为正和为负的情况下，导数是不同的，即ReLU的导数不是常数，所以ReLU是非线性的（只是不同于Sigmoid和tanh，relu的非线性不是光滑的.<br>
ReLU在x>0下，导数为常数1的特点：<br>
导数为常数1的好处就是在“链式反应”中不会出现梯度消失，但梯度下降的强度就完全取决于权值的乘积，这样就可能会出现梯度爆炸问题。解决这类问题：一是控制权值，让它们在（0，1）范围内；二是做梯度裁剪，控制梯度下降强度，如ReLU(x)=min(6, max(0,x)).<br>
ReLU在x<0下，输出置为0的特点：<br>
描述该特征前，需要明确深度学习的目标：深度学习是根据大批量样本数据，从错综复杂的数据关系中，找到关键信息（关键特征).换句话说，就是把密集矩阵转化为稀疏矩阵，保留数据的关键信息，去除噪音，这样的模型就有了鲁棒性.ReLU将x<0的输出置为0，就是一个去噪音，稀疏矩阵的过程.而且在训练过程中，这种稀疏性是动态调节的，网络会自动调整稀疏比例，保证矩阵有最优的有效特征.<br>\
但是ReLU 强制将x<0部分的输出置为0（置为0就是屏蔽该特征),可能会导致模型无法学习到有效特征，所以如果学习率设置的太大，就可能会导致网络的大部分神经元处于'dead'状态，所以使用ReLU的网络，学习率不能设置太大.<br>
ReLU作为激活函数的特点：<br>
1.相比Sigmoid和tanh，ReLU摒弃了复杂的计算，提高了运算速度 <br>
2.解决了梯度消失问题，收敛速度快于Sigmoid和tanh函数，但要防范ReLU的梯度爆炸 <br>
3.容易得到更好的模型，但也要防止训练中出现模型'Dead'情况 <br>


