# VoxelNet
paper: https://arxiv.org/abs/1711.06396 <br>

# 架构
![VoxelNet](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B/%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E7%82%B9%E4%BA%91/VoxelNet/img/VoxelNet.png) <br>


# 解析
大致思路是：首先将空间划分为多个方格(voxel)，每个方格内会有一些三维点。每个方格可以求出里面所有点的平均的位置(mx,my,mz)，每个三维点结合这些平均信息就可以得到一个7维的特征(x,y,z,r,x-mx,y-my,z-mz)。将这7维特征作为输入的通道数，通过3d卷积把垂直于地面的轴上的数据逐步压缩到1维，并把同一个voxel的数据累加，这样就把一个w*h*l*7的数据映射成w*h*128的数据了。后面再连接一个标准rpn就可以求出物体的位置了。<br>
其亮点在于：通过三维卷积把垂直于地面的轴逐渐消掉，而不是通过手工设计的方法将其消掉。<br>
# Voxel feature encoding layer
![VFE](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B/%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E7%82%B9%E4%BA%91/VoxelNet/img/VFE.png) <br>
#  Region proposal network architecture
![RPN](https://github.com/MA-JIE/pytorch-deep-learning/blob/master/%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B/%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE%E7%82%B9%E4%BA%91/VoxelNet/img/RPN.png) <br>
