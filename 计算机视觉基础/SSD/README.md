single shot multibox detection(SSD)
==================================
单发多框检测
----------------
该目标检测模型基于边界框，锚框，多尺度检测等概念构建而成.<br>
[锚框&&多尺度检测](../README.md)<br>
设计图如下,主要由一个基础网络块和若干多尺度特征块串联而成:<br>
![SSD](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89%E5%9F%BA%E7%A1%80/SSD/img/SSD.png)<br>
基础网络块用来从原始图像中抽取特征，一般会选择常用的深度卷积网络.
# 类别预测层

