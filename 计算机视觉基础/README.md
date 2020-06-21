计算机视觉
=========
#### 图像增广(iamge augmentation)

图像增广(image augmentation)技术通过对训练图像做一系列随机改变,来产生相似但又不同的训练样本,从而扩大训练数据集的规模。图像增广的另一种解释是,随机改变训练样本可以降低模型对某些属性的依赖,从而提高模型的泛化能力。例如,我们可以对图像进行不同方式的裁剪,使感兴趣的物体出现在不同位置,从而减轻模型对物体出现位置的依赖性。我们也可以调整亮度、色彩等因素来降低模型对色彩的敏感度。<br>
比如：翻转和裁剪, 变换图像颜色，或叠加多个图像增广方法<br>

#### 微调
微调是迁移学习中的一种常用技术。当我们需要构建自己的数据集去达到我们的需求时，有可能会出现数据集规模较小，导致模型出现过拟合的问题。我们可以在现有公开数据集训练的基础上，采用他们的参数作为初始参数，进而训练,可以提高模型的泛化能力。<br>
微调由以下四步构成：<br>
(1)在源数据集(如ImageNet数据集)上预训练一个神经网络模型,即源模型。<br>
(2)创建一个新的神经网络模型,即目标模型。它复制了源模型上除了输出层外的所有模型设计及其参数。我们假设这些模型参数包含了源数据集上学习到的知识,且这些知识同样适用于目标数据集。我们还假设源模型的输出层跟源数据集的标签紧密相关,因此在目标模型中不予采用。<br>
(3)为目标模型添加一个输出大小为目标数据集类别个数的输出层,并随机初始化该层的模型参数。<br>
(4)在目标数据集(如椅子数据集)上训练目标模型。我们将从头训练输出层,而其余层的参数都是基于源模型的参数微调得到的。<br>
![微调](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/tune.png)
#### 目标检测与边界框
在目标检测中，我们常使用边界框来描述目标位置:<br>
[左上x, 左上y, 右下x, 右下y]<br>

#### 锚框(anchor box)
目标检测算法的目的在于更准确的预测目标的真实边界框(grounf-truth bbox),不同的模型使用的区域采样方法可能不同,锚框是其中的一种思路：<br>
它以每个像素为中心生成多个大小和宽高比(aspect ratio)不同的边界框. <br>
锚框的两个属性：大小 s ∈ (0, 1], 宽高比为 r > 0, 锚框的宽和高将分别为 ws * sqrt(r)和 hs / sqrt(r).若随意将大小与宽高比组合，计算量会偏大，我们通常只对包含s1或r1的大小与宽高比的组合感兴趣,即:<br>
(s1 , r1 ), (s1 , r2 ), . . . , (s1 , rm ), (s2 , r1 ), (s3 , r1 ), . . . , (sn , r1) <br>
如下图就是以某个像素为中心，得出的锚框：<br>
![anchorbox](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/anchorbox.png) <br>
#### 交并比(IoU)
当去衡量锚框与真实框的相似度时，我们引入了交并比的概念：对于两个集合A和B，交集 / 并集<br>
![交并比](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/IoU.png)
#### 标注训练集的锚框
在目标检测的训练集中，对于每个图像，已经标注了真实的边界框以及里面所含目标的类别。在生成锚框之后,我们主要依据与锚框相似的真实边界框的位置和类别信息为锚框标注。<br>
![dog&&cat](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/dog_cat.png)<br>
如上图所示，黑色框为真实边界框,记为label1, label2. 0,1,2,3,4,5为锚框，通过左上角和右下角的坐标构造而成。代码如下所示，其坐标值分别除以了图像的宽和高<br>
![dog&&cat](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code1.png)<br>
然后，我们为锚框分配与其相似的真实边界框。主要思路为先根据真实边界框，选出与之最为相似的锚框，剩下的锚框再与真实边界框匹配，若IoU大于阈值，则为其分配真实边界框<br>
![dog&&cat](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/anchor_label.png)<br>
我们通过下方代码来为锚框标注类别和偏移量。<br>
![code2](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code2.png)<br>
返回的结果中含有三项：<br>
* 锚框标注的类别：<br>
![code3](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code3.png)<br>
* 掩码(mask)变量形状为锚框个数的四倍:<br>
![code4](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code4.png)<br>
* 每个锚框的四个偏移量，负类锚框的偏移量为“0”:<br>
![code5](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code5.png)<br>
对于“偏移量”以及“负类锚框”的定义：
* 偏移量: <br>
设锚框A及其被分配的真实边界框B的中心坐标分别为(xa , ya )和(xb , yb ),A和B的宽分别为wa 和 wb ,高分别为ha 和hb,一个常用的技巧是将A的偏移量标注为:<br>
![偏移量](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/offset.png)<br>
其中常数的默认值为μx = μy = μw = μh = 0, σx = σy = 0.1, σw = σh = 0.2。<br>
* 负类锚框: <br>
如果一个锚框没有被分配真实边界框,我们只需将该锚框的类别设为背景。类别为背景的锚框通常被称为负类锚框,其余则被称为正类锚框。<br>

#### 输出预测边界框
在进行预测的时候，模型并非直接输出预测边界框，而是先为图像生成多个锚框，并为这些锚框一一预测类别和偏移量，之后，我们根据锚框以及预测偏移量得到预测边界框。但是，当锚框比较多时，在同一个目标上可能会输出较多相似的预测边界框，为了只保留其置信度最高的边界框，我们一般采用非极大值抑制(non-maximum suppression NMS)的方法。<br>
![预测边界框](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/prediction.png)<br>
我们以上图为例讲解如何最终输出预测边界框:<br>
* 我们构造四个锚框，为了方便期间，假设锚框的偏移量均为“0”，意思就是锚框就是我们待筛选的预测边界框。通过对每个锚框预测不同类别的概率取最大值，即置信度，就得到了每个锚框的预测类别<br>
![code6](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code6.png)<br>
如上图所示，对于每个锚框，我们通过比较得出了对应的预测类别以及预测概率，对于“dog”来说，有三个预测框，我们该选择哪一个呢？<br>
首先，我们将预测边界框按照置信度排序，得到一个列表L:[0.9:dog, 0.9:cat, 0.8:dog, 0.7:dog],从L中选取置信度最高的作为基准，这里选择“0.9:dog”,计算其余边界框与其的交并比，若大于阈值(一般为0.5)，则从L中移除，标记为“-1”，这样L中保留了置信度最高的预测边界框并移除了与其相似的其他预测边界框:<br>
L = [0.9:dog(0), 0.9:cat(1), 0.8:dog(-1), 0.7:dog(-1)]<br>
然后对置信度第二高的预测边界框进行同样的操作....<br>
![code7](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code7.png)<br>
输出中，第一个元素代表预测类别(0:dog, 1:cat)，'-1'代表被移除,第二个元素是置信度，后四个元素为预测边界框左上角x,y以及右下角x,y坐标，值域在0~1之间。最终可视化结果如下:<br>
![code8](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/img/code8.png)<br>





