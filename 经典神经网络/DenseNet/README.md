# DenseNet
paper: <br>
https://arxiv.org/abs/1608.06993
# 简介
DenseNet是在ResNet发表后深受其影响，同时又更为优秀的一种网络结构，由康威大学清华大学、facebook的三位作者共同提出，论文发表于2017，获得了CVPR 2017的最佳论文奖。其核心即dense block稠密块继承和发扬了ResNet中shortcut这一设计使得layer之间可以“稠密”互联，同时，正如其名，不只是层于层间的连接，而且是稠密连接(也借鉴了Inception模块的思想)，DenseNet里对于dense block中的每一层，都以前面的所有层作为输入(而其自身也用作后续所有层的输入)，层和层之间正是通过ResNet中的shortcut方式进行连接。这种稠密连接的优点：它们减轻了梯度消失的问题，增强了特征传播，鼓励特征重用，并大大减少了参数数量. <br>
![DenseNet](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%BB%8F%E5%85%B8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/DenseNet/img/DenseNet.png) <br>
图中将部分前后相邻的运算抽象为模块A和模块B.与ResNet的主要区别在于,DenseNet里模块B的输出不是像ResNet那样和模块A的输出相加,而是在通道维上连结.这样模块A的输出可以直接传入模块B后面的层。在这个设计里,模块A直接跟模块B后面的所有层连接在了一起.这也是它被称为“稠密连接”的原因. DenseNet的主要构建模块是稠密块(dense block)和过渡层(transition layer).前者定义了输入和输出是如何连结的,后者则用来控制通道数,使之不过大. <br>
# 稠密块
![稠密块](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%BB%8F%E5%85%B8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/DenseNet/img/denseblock.png) <br>
![稠密块动图](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%BB%8F%E5%85%B8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/DenseNet/img/denseblock1.gif) <br>
上图为论文中给出的5层稠密块.<br>

