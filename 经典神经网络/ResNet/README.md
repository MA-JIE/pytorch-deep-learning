# 残差网络(ResNet)
paper: <br>
https://arxiv.org/abs/1512.03385 <br>
# 什么是残差
残差在数理统计中是指实际观察值与估计值（拟合值）之间的差。如果回归模型正确的话，我们可以将残差看作误差的观测值。<br>
更准确地，假设我们想要找一个 𝑥，使得 𝑓(𝑥)=𝑏，给定一个 𝑥 的估计值 𝑥0，残差（residual）就是 𝑏−𝑓(𝑥0)，同时，误差就是 𝑥−𝑥0。 <br>
# 残差网络
对神经网络模型添加新的层,充分训练后的模型是否只可能更有效地降低训练误差?理论上,原模型解的空间只是新模型解的空间的子空间。也就是说,如果我们能将新添加的层训练成恒等映射f(x) = x,新模型和原模型将同样有效。由于新模型可能得出更优的解来拟合训练数据集,因此添加层似乎更容易降低训练误差。然而在实践中,添加过多的层后训练误差往往不降反升。即使利用批量归一化带来的数值稳定性使训练深层模型更加容易,该问题卷积神经网络仍然存在.<br>
# 残差块(residual block)
![res block](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%BB%8F%E5%85%B8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/ResNet/img/res_block.png) <br>
ResNet沿用了VGG全3 × 3卷积层的设计。残差块里首先有2个有相同输出通道数的3 × 3卷积层.每个卷积层后接一个批量归一化层和ReLU激活函数。然后我们将输入跳过这两个卷积运算后直接加在最后的ReLU激活函数前。这样的设计要求两个卷积层的输出与输入形状一样,从而可以相加.如果想改变通道数,就需要引入一个额外的1 × 1卷积层来将输入变换成需要的形状后再做相加运算. <br>
如上图所示,𝑥 表示输入，𝐹(𝑥) 表示残差块在第二层激活函数之前的输出，即 𝐹(𝑥)=𝑊2𝜎(𝑊1𝑥)，其中 𝑊1 和 𝑊2 表示第一层和第二层的权重，𝜎 表示 ReLU 激活函数.（这里省略了 bias.）最后残差块的输出是 𝜎(𝐹(𝑥)+𝑥). 当没有 shortcut connection（即上图右侧从 𝑥 到 ⨁ 的箭头）时，残差块就是一个普通的 2 层网络。残差块中的网络可以是全连接层，也可以是卷积层。设第二层网络在激活函数之前的输出为 𝐻(𝑥)。如果在该双层层网络中，最优的输出就是输入 𝑥，那么对于没有 shortcut connection 的网络，就需要将其优化成 𝐻(𝑥)=𝑥；对于有 shortcut connection 的网络，即残差块，如果最优输出是 𝑥，则只需要将 𝐹(𝑥)=𝐻(𝑥)−𝑥 优化为 0 即可. 后者的优化会比前者简单.这也是残差这一叫法的由来.<br>
# 网络模型
![resnet](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%BB%8F%E5%85%B8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/ResNet/img/ResNet.png) <br>
最右边为残差网络模型.<br>
残差网络可以不是卷积神经网络，用全连接层也可以。当然，残差网络在被提出的论文中是用来处理图像识别问题。<br>

# 为什么残差网络会work
The main reason the residual network works is that it's so easy for these extra layers to learn the identity function that you're kind of guaranteed that it doesn't hurt performance. And then lot of time you maybe get lucky and even helps performance, or at least is easier to go from a decent baseline of not hurting performance, and then creating the same can only improve the solution from there. <br>
我们给一个网络不论在中间还是末尾加上一个残差块，并给残差块中的 weights 加上L2正则化（weight decay），这样𝐹(𝑥)=0 是很容易的。这种情况下加上一个残差块和不加之前的效果会是一样，所以加上残差块不会使得效果变得差。如果残差块中的隐藏单元学到了一些有用信息，那么它可能比 identity mapping（即 𝐹(𝑥)=0）表现的更好. <br>

