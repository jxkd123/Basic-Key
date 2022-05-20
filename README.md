# Basic-Key

BP神经网络拟合非线性函数

https://www.jianshu.com/p/7e2f32b7aca0


本章涉及知识点：

1、多层神经网络的数学模型

2、前向传播算法

3、经典激活函数

4、经典损失函数

5、神经网络的优化过程

6、反向传播算法的推导

7、梯度下降算法优化

8、衰减学习率的设计

9、案例引出—拟合非线性曲线

10、python编程实现拟合非线性曲线

11、结果分析

一、多层神经网络的数学模型

通常一个多层神经网络由L层神经元组成，分为输入层、输出层和隐藏层

输入层：神经网络的第一层，表示一列或多列矩阵的输入

输出层：神经网络的最后一层，表示网络最终的输出结果，通常是一列矩阵

隐藏层：神经网络的中间层，有L-2层，表示每一层神经元通过前向传播算法计算的结果矩阵

多层神经网络的数学模型
上述数学模型理论上可以学习模拟出任何的映射函数，下面我们定义出神经网络中的所有变量含义

X表示：神经网络的输入矩阵：

输入矩阵
y表示：神经网络的最终输出矩阵：

输出矩阵
表示：第l层隐藏层的第i个神经元的输出，设第l层的有k个神经元，则第l层的输出为：

第l层的输出
表示：第l-1层的第j个神经元和第l层的第i个神经元的连接权重，即两个神经元之间的突触

表示：第l层第i个神经元的偏置项

表示：第l层第i个神经元的输入

二、前向传播算法

前向传播算法指的是依次向前计算相邻隐藏层之间的连接输出，直到模型的最终输出值。其特点是：

作用于相邻层的两个神经元之间的连接计算，且前一层神经元的输出是后一层神经元的输入

设对于第l层的第i个神经元，有l-1层的n个神经元与该神经元有突触相连，则

第l层的第i个神经元的输入为：

第l层的第i个神经元的输入
第l层的第i个神经元的输出为：

第l层的第i个神经元的输出
其中f(.)表示神经元的激活函数

三、经典激活函数

由于线性函数具有叠加性，使得无论计算多少层隐藏层，最后的输出结果都是线性变化的，即整个模型都是线性的。但是实际生活中大多数问题都是非线性问题，而线性模型处理这些非线性问题的效果是非常不理想的，为此我们需要对每个神经元的输出做非线性变化，使得整个模型非线性

激活函数的目的：去除每个神经元输出的线性化，使得整个神经网络模型呈非线性化

神经网络中常用的几种非线性激活函数为

（1）sigmod函数

sigmod函数
其导数为

sigmod导数
（2）双曲正切函数

双曲正切函数
其导数为

双曲正切函数导数
（3）ReLU函数

ReLU函数
四、经典损失函数

损失函数的意义：神经网络学习到的模型效果，以及优化的目标是通过损失函数来定义量化的

监督式学习有两大类问题：分类和回归

分类问题：根据事先的样本和分好的类目，学习一个分类函数，使得它可以将新样本分类到正确的类目中

回归问题：根据事先的样本和对应的数值，学习一个回归函数，使得它可以用新的输入预测出具体的数值

无论对于学习到的分类函数，还是回归函数，其本质都是模型的前向传播算法

（1）对于分类问题：

我们需要事先定义一个场景（规模），准备好若干样例样本和定义好的所有类目，然后训练神经网络模型学习这个样本的分类逻辑规则，最后对于输入的任何一个新样本，模型都可以输出其所属的类目

需要注意的是，在分类问题的最终输出结果里，由于我们将“一个样例属于某一个类别”视为一个概率事件，则该样例属于所有类目的各自概率就构成了该样例的概率分布，而神经网络的输出却不一定是概率分布，为此我们可以用softmax将结果转化为概率分布：

softmax输出概率分布
最后我们使用交叉熵函数，来描述模型输出概率分布p和真实概率分布q之间的距离，即量化其分类损失

交叉熵函数
（2）对于回归问题：

我们需要事先准备一组输入输出的数据样本集，然后训练神经网络模型学习这个样本的输入输出映射法则，最后对于任意一个实数R，模型都可以预测出一个具体的数值

我们可以用MSE（均方误差）来描述模型输出结果和真实结果的距离，即量化其回归损失

MSE函数
五、神经网络的优化过程

神经网络的优化步骤可以分为下面两个阶段：

（1）通过前向传播算法，通过矩阵乘法计算出输出值，并将真实值和输出值对比得到两者之间的差距

（2）通过反向传播算法，计算损失函数对模型中每个参数的梯度，通过梯度下降算法来更新每一个参数

至此我们可以看到反向传播算法的艺术为：

反向计算出损失函数对每一层隐藏层的突触的梯度，并更新优化其上一层隐藏层的突触。使得神经网络每一次学习到的模型都变得更加聪明

六、反向传播算法的推导

我们先整理出前向传播算法中第l隐藏层的第i个神经元的输入和输出为

第l层第i个神经元的输入
第l层第i个神经元的输出
结合上面两个式子，可以得到第l层第i个神经元的输入和第l-1层第i的神经元的输出的函数关系为

第l层第i个神经元的输入
为了推导方便，我们定义第i个神经元的损失函数为

第i个神经元的损失函数
其中dk(i)表示第i个神经元的真实输出值，yk(i)表示神经网络模型计算出的输出值

下面推导第i个神经元的误差对第l-1（）层第j个神经元对第l-2层第i个神经元的突触权重的梯度为：

第l-1层第j个神经元的梯度计算1
上式最后3个偏导数的意义为

：表示第l层的第k个神经元的输入对第l-1层的第j个神经元的输出的偏导数

：表示第l-1层的第j个神经元的输出对第l-1层的第j个神经元的输入的偏导数

：表示第l-1层的第j个神经元的输入对第l-1层第j个神经元和第l-2层第i个神经元的权重的偏导数

分别计算出这3个偏导数为

第一个偏导数
第二个偏导数
第三个偏导数
带入这三个偏导数，得

第l-1层第j个神经元的梯度计算2
同理可以推导出第l-1层第j个神经元的偏置项梯度为：

第l-1层第j个神经元的偏置项梯度
为了方便整理输出结果，我们定义

每一层隐藏层的误差系数
可以整理出第l-1层的第j个神经元的权重梯度和偏置项梯度为：

                                                                     

                                                                     

由数学归纳法，可以得到第l层的第j个神经元的权重梯度和偏置项梯度为:

                                                                    

                                                                    

七、梯度下降算法优化

得到每一个隐藏层的连接权重和偏置项的梯度后，我们利用学习率和梯度下降算法更新每一层的连接权重和偏置项

梯度下降算法
八、衰减学习率的设计

在配合梯度下降优化的过程中，如果学习率设置过大，则容易导致模型过拟合；如果设置过小，会使得模型优化的速度变得很缓慢。为此我们加入衰减因子和学习次数来计算模型每次学习的学习率

衰减学习率
这样做的优势是：在初始学习阶段，学习率会较大；随着学习次数的增加，学习率会依次降低

九、案例引出—拟合非线性曲线

案例：求一个神经网络模型，可以拟合下面曲线

案例拟合非线性曲线
案例曲线
显然这是一个回归问题，我们构建出案例的神经网络模型如下：

输入层规模：100x1，第一个隐藏层规模：1x24，第二个隐藏层规模：24x3，输出层规模：3x1

则模型按照前向传播算法和矩阵乘法规则，计算过程如下

案例模型的计算过程
最终我们得到了100x1的输出规模，即完成回归问题中输入和输出的一一映射关系

十、python编程实现拟合非线性曲线

前向传播算法：

前向传播算法
反向传播算法：

反向传播算法
训练BP神经网络：

训练BP神经网络
十一、结果分析

模型运行结果如下

模型结果
从结果中可以看到，学习到模型是一条光滑的曲线来拟合源数据点，且模型的误差在下降得越来越缓慢

至此，我们对比《最小二乘法—多项式拟合非线性函数》，可以总结出深度学习和传统机器学习的一些特点

（1）机器学习是一种实现人工智能的方法，它基于不同的具体数学算法

（2）深度学习是一种实现机器学习的技术，它基于包含多个隐藏层的神经网络

作者：PrivateEye_zzy
链接：https://www.jianshu.com/p/7e2f32b7aca0
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
