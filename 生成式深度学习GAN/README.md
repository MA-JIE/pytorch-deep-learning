生成式深度学习
=========
# 一.变分自编码器(Variational Autoencoders) <p>
基于贝叶斯推理,其目标是潜在的建模,从模型中采样新的数据.<p>

论文参考: <p>

https://arxiv.org/pdf/1606.05908.pdf

# 二.Generative Adversarial Nets: GAN <p>
基于博弈论,目的是找到达到纳什均衡的判别器网络和生成器网络 <p>

生成器网络: 以一个潜在空间的随机向量作为输入,并将其解码为一张合成图像.就像伪造达芬奇画作的伪造者一样. <p>

判别器网络: 以一张图像(真实的或合成的)作为输入,并预测该图像来自训练集还是来自生成器网络.就像鉴别赝品的鉴赏者一样. <p>

![整体架构](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%94%9F%E6%88%90%E5%BC%8F%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0GAN/images/GAN.png)

  GAN的优化过程不是求损失函数的最小值,而是保持生成与判别两股力量的动态平衡. <p>
