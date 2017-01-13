# 集体智慧编程 笔记

这本书主要是大概浏览一下相关领域的基本应用，方便以后遇到类似问题至少知道怎么开始

### 列表推导式 list comprehension

一种方便简洁的语法形式

    [表达式 for 变量 in 列表]
    or
    [表达式 for 变量 in 列表 if 条件]

    l=[1,2,3,4,5,6,7,8,9]
    print [v*10 for v in l if v>4]
    timesten=dict([(v,v*10) for v in l])

## 导言

+ 利用开放的 API 搜集数据 + 机器学习算法和统计方法 = 借助集体智慧的相关方法
+ 所有机器算法都有过度归纳的可能性

## 提供推荐 Making Recommendations

+ 如何根据群体偏好来为人们提供推荐
+ 两条相似度评判体系：欧几里得距离和皮尔逊相关度
+ 皮尔逊方法修正了“夸大分值(grade inflation)”的情况。如果某人总是倾向于给出比另一个人更高的分值，而二者的分值之差又始终保持一致，则他们依然可能会存在很好的相关性。
    * 首先找出两位评论者都曾评价过的物品，然后计算两者的评分总和与平方和，并求得评分的乘积之和，最后利用这些计算出皮尔逊相关系数
+ 如何找到商品之间的近似关系
+ 与前面类似，不过人与物品的关系对调
+ 以上都是基于用户的协作型过滤(user-based collabonative filtering)

另一种是基于物品的协作型过滤(item-based collaborative filtering)，总体思路是为每件物品预先计算好最为相近的其他物品。然后，当我们想为某位用户提供推荐时，就可以查看他曾经评过分的物品，并从中选出排位靠前者，再构造出一个加权列表，其中包含了与这些选中物品最为相近的其他物品。尽管第一步要求我们检查所有的数据，但是物品间的比较不会像用户间的比较那么频繁变化。

[MovieLens 数据集](http://grouplens.org/datasets/movielens/)

### 基于用户进行过滤还是基于物品进行过滤

在针对大数据集生成推荐列表时，基于物品进行过滤的方式明显要比基于用户的过滤更快，不过有维护物品相似度表的额外开销。而对于稀疏数据集，基于物品的过滤方法通常要优于基于用户的过滤方法，而对于密集数据集而言，两者的效率几乎是一样的。

尽管如此，基于用户的过滤方法更加易于实现，而且无需额外步骤，因此更加适用于规模较小的变化非常频繁的内存数据集。

## 发现群组 Discovering Groups

数据聚类(data clustering) 用以寻找紧密相关的事、人或观点，并将其可视化的方法。

### 监督学习和无监督学习

+ 利用样本输入和期望输入来学习如何预测的技术被称为监督学习法(supervised learning methods)，例如神经网络、决策树、向量支持机和贝叶斯过滤。
+ 聚类是无监督学习(unsupervised learning)的一个例子。还有非负矩阵因式分解(non-negative matrix factorization)和自组织映射(self-organizing maps)

### 单词向量 Word Vectors

为聚类算法准备数据的常见做法是定义一组公共的数值型属性，然后利用这些属性对数据项进行比较。例如可以根据内容对博客用户进行分类

### 分级聚类

通过连续不断地将最为相似的群组两两合并，来构造出一个群组的层级结构。其中的每个群组都是从单一元素开始的。通常，待分级聚类完成之后，我们可以利用树状图(dendrogram)来展现所得到的结果。

但是这种聚类的计算量惊人，很多时候 Kmeans 是一个更好的方法。

+ 先对笔记进行分词，然后聚类找到风格，又或者是自动提取出关键字
+ Beautiful Soup 用来处理网页
+ 多维缩放技术(multidimensional scaling) 缩放的过程中一些信息可能会丢失掉，但缩放后的结果会更加有助于我们理解算法的原理

## 搜索与排名 Searching and Ranking

全文搜索算法是最重要的集体智慧算法之一。

### 搜索引擎的组成

首要步骤是找到一种搜集文档的方法，然后建立索引，最后一步是通过查询返回一个经过排序的文档列表。

### PageRank 算法

为每个网页都赋予了一个指示网页重要程度的评价值。网页的重要性是依据指向该网页的所有其他网页的重要性，以及这些网页中所包含的链接数求得的。

理论上，PageRank 计算的是某个人在任意次链接点击之后到达某一网页的可能性。如果某个网页拥有来自其他热门网页的外部回指链接越多，人们无意间到达该网页的可能性也就越大。当然，如果用户始终不停地点击，那么他们终将到达每一个网页，但是大多数人在浏览一段时间之后就会停止点击。为了反映这一情况，PageRank 还使用了一个值为 0.85 的阻尼因子，用以指示用户持续点击每个网页中链接的概率为 85%。

计算时为所有的 PageRank 都设置一个任意的初始值，经过反复迭代，会越来越接近真实值。

### 从点击行为中学习

构造一个人工神经网络，以一组节点(神经元)构成，并且彼此相连，称为多层感知机(multilayer perceptron, MLP) 网络。此类网络由多层神经元构造而成，其中第一层神经元接受输入，最后一层给予输出。神经网络可以有多个中间层，称为隐藏层，其职责是对输入进行组合

## 优化 Optimization

随机优化(stochastic optimization)的技术来解决协作类问题，擅长处理受多种变量影响，存在许多可能解的问题，以及结果因这些变量的组合而产生很大变化的问题。

成本函数是用优化算法解决问题的关键，它通常是最难确定的。任何优化算法的目标，就是要寻找一组能够使成本函数的返回结果达到最小化的输入。对于好坏程度并没有特定的衡量尺度，唯一的要求就是函数返回的值越大，表示该方案越差。

爬山法以一个随机解开始，然后在其临近的解集中寻找更好的题解(拥有更低的成本)，最后的解是一个局部范围内的最小值，但却不一定是全局最优解。解决这一难题的一种方法是随机重复爬山法(random-restart hill climbing)

退火是指将合金加热后再慢慢冷却的过程。大量的原子因为受到激发而向周围跳跃，然后又逐渐稳定到一个低能阶的状态，所以这些原子能够找到一个低能阶的配置(configuration)。退火算法以一个问题的随机解开始，用一个变量来表示温度，这一温度开始非常高，而后逐渐变低。每一次迭代期间，算法会随机选中题解中的某个数字，然后朝某个方向变化。退火过程开始阶段会接受表现比较差的解。随着退火过程的不断进行，算法越来越不可能接受较差的解，直到最后，它将只会接受更优的解。

介绍模拟退火前，先介绍爬山算法。爬山算法是一种简单的贪心搜索算法，该算法每次从当前解的临近解空间中选择一个最优解作为当前解，直到达到一个局部最优解。爬山算法实现很简单，其主要缺点是会陷入局部最优解，而不一定能搜索到全局最优解。如下图所示：假设C点为当前解，爬山算法搜索到A点这个局部最优解就会停止搜索，因为在A点无论向那个方向小幅度移动都不能得到更优的解。

![pci1](./_resource/pci1.jpg)

爬山法是完完全全的贪心法，每次都鼠目寸光的选择一个当前最优解，因此只能搜索到局部的最优值。模拟退火其实也是一种贪心算法，但是它的搜索过程引入了随机因素。模拟退火算法以一定的概率来接受一个比当前解要差的解，因此有可能会跳出这个局部的最优解，达到全局的最优解。以图1为例，模拟退火算法在搜索到局部最优解A后，会以一定的概率接受到E的移动。也许经过几次这样的不是局部最优的移动后会到达D点，于是就跳出了局部最大值A。

模拟退火算法描述：

+ 若J( Y(i+1) )>= J( Y(i) )  (即移动后得到更优解)，则总是接受该移动
+ 若J( Y(i+1) )< J( Y(i) )  (即移动后的解比当前解要差)，则以一定的概率接受移动，而且这个概率随着时间推移逐渐降低（逐渐降低才能趋向稳定）
+ 这里的“一定的概率”的计算参考了金属冶炼的退火过程，这也是模拟退火算法名称的由来。

爬山算法：兔子朝着比现在高的地方跳去。它找到了不远处的最高山峰。但是这座山不一定是珠穆朗玛峰。这就是爬山算法，它不能保证局部最优值就是全局最优值。

模拟退火：兔子喝醉了。它随机地跳了很长时间。这期间，它可能走向高处，也可能踏入平地。但是，它渐渐清醒了并朝最高方向跳去。这就是模拟退火。

遗传算法 Genetic Algorithm

先随机生成一组解，称之为种群(population)，在优化过程中的每一步，算法会计算整个种群的成本函数，从而得到一个有关题解的有序列表。在对题解进行排序之后，一个新的种群，也就是下一代，被创建出来了。首先，我们将当前种群中位于最顶端的题解加入其所在的新种群中(精英选拔法elitism)。新种群中的余下部分是由修改最优解后形成的全新解所组成。

有两种修改题解的方法，一种是变异(mutation)，通常的做法是对一个既有解进行微小的简单的随机的改变。另一种方法称之为交叉(crossover)或配对(breeding)。这种方法是选取最优解中的两个解，然后将它们按照某种方式进行结合。

作为遗传算法生物背景的介绍，下面内容了解即可：
+ 种群(Population)：生物的进化以群体的形式进行，这样的一个群体称为种群。
+ 个体：组成种群的单个生物。
+ 基因 ( Gene ) ：一个遗传因子。 
+ 染色体 ( Chromosome ) ：包含一组的基因。
+ 生存竞争，适者生存：对环境适应度高的、牛B的个体参与繁殖的机会比较多，后代就会越来越多。适应度低的个体参与繁殖的机会比较少，后代就会越来越少。
+ 遗传与变异：新个体会遗传父母双方各一部分的基因，同时有一定的概率发生基因变异。

简单说来就是：繁殖过程，会发生基因交叉( Crossover ) ，基因突变 ( Mutation ) ，适应度( Fitness )低的个体会被逐步淘汰，而适应度高的个体会越来越多。那么经过N代的自然选择后，保存下来的个体都是适应度很高的，其中很可能包含史上产生的适应度最高的那个个体。

## 文档过滤 Document Filtering

根据内容来对文档进行分类。将单词作为特征时，其假设是：某些单词相对而言更有可能会出现于垃圾信息中。

在训练的初期阶段，极少出现的单词会变得异常敏感。为了解决上述问题，在我们手头掌握的有关当前特征的信息极为有限时，我们还需要根据一个假设的概率来作出判断。

朴素贝叶斯分类器：假设要被组合的各个概率是彼此独立的。Pr(A|B) = Pr(A|B) x Pr(A) / Pr(B)。有些时候承认不知道答案，要好过判断答案就是概率值稍大一些的分类，所以为每个分类设定一个最小的阈值。

费舍尔方法：朴素贝叶斯方法的一种替代方案，可以给出非常精确的结果，尤其是和垃圾信息过滤。与朴素贝叶斯过滤器利用特征概率来计算整篇文档的概率不同，费舍尔方法为文档中的每个特征都求得了分类的概率，又将这些概率组合起来，并判断其是否有可能构成一个随机集合。

!! 可以结合下面的决策树来进行文档类型的分类，不能确定的就还放回 Inbox 中

## 决策树建模

相比于其他方法，决策树是一种更为简单的机器学习方法，它是对被观测数据进行分类的一种相当直观的方法。使用一种叫做 CART(Classification and Regression Trees, 分类回归树)的算法。为了构造决策树，算法首先创建一个根节点。然后通过评估表中的所有观测变量，从中选出最合适的变量对数据进行拆分。

基尼不纯度(Gini Impurity)将来自集合中的某种结果随机应用与集合中的某一数据的预期误差率。

熵(Entropy): 集合的无序程度

上面两个的主要区别在于，熵达到峰值的过程要相对慢一些。因此，熵对于混乱集合的惩罚往往要更重一些

## 构建价格模型

### k-最近邻算法

对于葡萄酒定价问题而言，找到几瓶情况最为相近的酒，并假设其价格大体相同。算法通过寻找与当前关注的商品情况相似的一组商品，对这些商品的价格求平均值，进而作出价格预测。k 的取值过大或者过小都会降低准确性。

需要定义相似度，同样可以用不同的距离公式来决定。实现起来相对简单，但是计算量很大，不过优点在于，每次有新数据加入，都无须重新进行训练。

不同的近邻可以有不同的权重，越接近的应该权重越大。具体的权重分配方法有以下几种供参考：

+ 反函数：例如倒数
+ 减法函数：用一个常量相减
+ 高斯函数：执行速度较慢

交叉验证：将数据拆分成训练集与测试集的一系列技术的统称。将训练集传入算法，随着正确答案的得出，就得到了一组用以进行预测的数据集。随后，我们要求算法对测试集中的每一项数据都作出预测。其所给出的答案，将与正确答案进行对比，所发会计算出一个整体分值，以评估其所做预测的准确程度。

不足之处：算法需要计算针对每个点的距离，预测过程的计算量很大。

## 高阶分类：核方法与 SVM

决策边界是这样一条线：位于这条线一侧的每一个点会被赋予某个分类，而位于另一次的每个点会被赋予另一个分类。

线性分类器的问题在只能找到一条直线。为了改进，对于任何用到了点积运算的算法(包括线性分类器)可以采用一种叫做核技法(The Kernel Trick)的技术。

核技法的思路是用一个新的函数来取代原来的点积函数，当借助某个映射函数将数据第一次变换到更高维度的坐标空间时，新函数将会返回高维度坐标空间内的点积结果，但是现实中我们只会采用少数几种变换方法，其中一种叫做径向基函数(radial-basis function)

径向基函数与点积类似，它接受两个向量作为输入参数，并返回一个标量值。与点积不同的是，径向基函数是非线性的，因而它能够将数据映射到更为复杂的空间中。

### 支持向量机 (Support-Vector Machines)

支持向量机是广为人知的一组方法的统称，其思路是，尝试寻找一条尽可能远离所有分类的线，这条线被称为最大间隔超平面(maximum-margin hyperplane)。我们将位于这条分界线附近的坐标点称作支持向量。寻找支持向量，并利用支持向量来寻找分界线的算法便是支持向量机。在高维数据集上有不错的表现

libSVM package

## 寻找独立特征

!! 这个可以用在笔记中，自动发现重要特征

在数据集并未明确标识结果的前提下，从中提取出重要的潜在特征来。和聚类一样，这些方法的目的不是为了预测，而是要尝试对数据进行特征识别，并且告诉我们值得关注的重要信息。

非负矩阵因式分解(NMF)，本书涉及的最为复杂的技术之一。权重矩阵与特征矩阵。利用NumPy

## 智能进化 Evolving Intelligence

遗传编程(genetic programming)的技术，通常的工作方式是：以一大堆程序(种群)开始，随后，这些程序将会在一个由用户定义的任务中展开竞争。接下来，算法可以采取两种不同的方式对表现最好的程序实施复制和修改(变异/配对)。在每一个复制和修改的阶段，算法都会借助于一个适当的函数对程序的质量作出评估。因为种群大小保持不变，所以优胜劣汰。直到终止条件满足才会结束，不同的问题有不同的终止条件。

## 算法总结

+ 贝叶斯分类器：只要我们能将其转换成一组特征列表，所谓特征，就是指一个给定项中存在或缺少的某种东西。
    * 优势在于接受大数据量训练和查询时也可以保持高速，并且对分类器实际学习状况的解释还是相对简单的。
    * 最大缺陷是无法处理基于特征组合所产生的变化结果。
+ 决策树分类器：利用决策树进行分类非常简单，但对决策树进行训练则需要更多技巧。
    * 优点在于解释一个受训模型是非常容易的，能够很容易地处理变量之间的相互影响。
    * 缺点是不支持增量式训练，有新的数据就需要重新开始。
+ 神经网络：层与层之间通过突触(synapse)彼此相连
    * 优点是可以处理复杂的非线性函数，并且能发现不同输入间的依赖关系；允许增量式训练
    * 缺点在于是一种黑盒方法，并且在选择训练数据的比率以及与问题相适应的网络规模方面，并没有明确的规则可以遵循，往往是依据大量的实验。训练比率过高可能过拟，过低可能学习程度不足。
+ 支持向量机(SVM)：针对每个数据集的最佳核变换函数及其相应的参数都是不一样的，需要大量数据进行训练。也是一种黑盒技术，由于存在向高维空间的变换，SVM 的分类过程甚至更加难于理解。也许能得到很好的答案，但是我们永远都不知道为什么。
+ k-最近邻算法：能够利用复杂函数进行预测，又保持简单易懂，是一种在线技术，可以随时加入数据。主要缺点在于为了完成预测所有的训练数据都缺一不可，空间时间问题，算法效率较低。寻找合理的缩放因子是一项乏味的工作。
+ 聚类：找相似，多维缩放
+ 非负矩阵因式分解：非监督算法
+ 优化：模拟退火，遗传算法，成本函数

## 数学公式

+ 欧几里得距离
+ 皮尔逊相关系数
+ 加权平均
+ Tanimoto 系数
+ 条件概率
+ 基尼不纯度
+ 熵
+ 方差
+ 高斯函数
+ 点积