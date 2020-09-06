## SSP: Semantic Space Projection for Knowledge Graph Embedding with Text Descriptions 论文读书笔记

### 1. Motivation

​	Knowledge graph embedding 将知识图中的实体和关系表示为低维的连续向量，从而使知识图谱可以适用于机器学习模型。 尽管Knowledge graph embedding已经有大量相关，但大多数方法仅仅集中在事实三元组（fact triples）上，而实体和关系的补充文本描述（Textual descriptions）尚未得到充分利用。为此，本文提出了SSP[1] 模型，它结合了知识图三元组和以及实体的文本描述共同学习，能够同时捕获图结构和语义信息。Textual descriptions 的示例如下图所示。

![1563362500894](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563362500894.png)

##### 融入文本描述的优势有如下两点：

+ 其一：可以发掘实体间的语义相关性。原文的论述为：“the semantic relevance between entities is capable to recognize the true triples, which are difficult to be inferred only with fact triples.” 我的理解为：在模型的学习过程中能够更好的拟合，比如说三元组(Anna Roosevelt, Parents, Franklin Roosevelt), 仅从图结构中并不能学习到一些该fact的特征，但是融合descriptions 的语义信息就能够发掘出语义相关性，因为在实体Anna Roosevelt的description中存在一些像“Roosevelt”和“Daughter of the President”的关键词。

+ 其二：精确的语义表述能够提升两个三元组的可区分性。比如，对于一个查询”the profession of Daniel
  Sturgeon”，有两个候选答案：“politician”和 “lawyer”。仅从图结构信息很难区分应该返回哪个答案，但是由于Daniel Sturgeon的description中含有大量像“Democratic Party”、 “State Legislature”、甚至是“Politician”的关键词。因此在此模型中头节点Daniel Sturgeon与尾节点politician在关系profession对应的 的超平面上距离更接近。

  ![1563365089404](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563365089404.png)

##### 本文与前面工作相比的优势：

+ 前面的工作DKRL[2]和“Jointly”[3] 都是weak-correlation modeling。原文的描述是：“Both DKRL and “Jointly” apply first-order constraints which are weak in capturing the correlation of texts and triples.  It’s note worthy that triple embedding is always the main procedure and textual descriptions must interact with triples for better embedding.”  前两篇工作都是将entity vector和text vector 通过某种计算方式直接结合为最终的embedding。而本文则是将更侧重于更强的交互形式，如图2所示，通过将triple的embedding映射到一个语义超平面上，因此embedding的拓扑结构与语义是更贴合的。
+ 更重要的一点是该项工作的计算复杂度贴近于TransE

### 2. Methodology

#### 1). Loss Function

![1563427073379](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563427073379.png)

此模型的loss函数分为两部分，前半部分与Trans系列的loss大体相似，不同点在于distance的计算，此处$ f_r $函数为score而不是distance，因此是（正例的score-负例score）小于一定阈值时，该值越低loss越大。第二部分为topic-specific，用于训练更新每个entity的semantic vector（此vector会用于$ f_r $的计算中，用于将三元组投射到一个semantic 超平面）。semantic vector由预训练的topic模型NMF来初始化。本文在实验部分比较了更新semantic vector和固定这些vector对于模型的影响，分别记为**Std.**和**Joint.** 当μ = 0时，即为**Std.**

![1563428017390](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563428017390.png)

该loss为hinge loss，通常被用于最大间隔算法(maximum-margin)。loss$[x]_+$ 表示max{0,x}，$γ$是margin超参数。

![1563428916032](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563428916032.png)

$E$为实体集合，$D_e$ 为实体$e$的description的word集合。$C_{e,w}$ 为$w$ 在$e$的description中出现的次数，$s_e$为实体$e$的semantic vector， w为词$w$的话题分布（topic distribution）。在优化的过程中也是使用随机梯度下降。

#### 2).  Score Function

本文的Score function定义如下：

![1563431114679](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563431114679.png)

其中 $ e = h+r-t $ ， $ || x||_2 $ 为向量x的l2范数。

![1563431732780](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563431732780.png)

这里的定义前半部分我可以理解，计算的是 h+r-t投射到semantic超平面上的距离，取负 转换为score，后半部分为什么是+而不是-，让我很困惑。

上面的 $s = S(s_h, s_t)$用于构造语义超平面，定义如下：

![1563431964188](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563431964188.png)

其中$s_h, s_t$ 为头实体和尾实体对应的semantic vector。

### 3. Experiments

#### 1). Dataset

+ FB15K and FB20K，和DKRL[2]工作的数据集一致
+ WN18， The textual information of WN18 is the definitions that we extract from the Wordnet.

#### 2). Task

##### Knowledge Graph Completion

+ 评测方案： 通过计算候选三元组的$f_r(h,t)$ score值，得到一个ranking列表。通过计算原三元组的mean rank 和HITS@10（rank值不大于10的比例）来进行评测。

+ 评测结果

  ![1563432799731](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563432799731.png)

  ![1563432817139](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563432817139.png)

##### Entity Classification

+ 评测方案： 使用embedding vector和semantic vector的拼接向量$(s_e,e)$作为分类的输入特征。
+ 评测结果：

![1563433029882](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\1563433029882.png)



### Reference

[1]  [Xiao, Han, et al. "SSP: semantic space projection for knowledge graph embedding with text descriptions." *Thirty-First AAAI Conference on Artificial Intelligence*. 2017.](https://www.aaai.org/ocs/index.php/AAAI/AAAI17/paper/view/14306/14084)

[2] [Xie, Ruobing, et al. "Representation learning of knowledge graphs with entity descriptions." *Thirtieth AAAI Conference on Artificial Intelligence*. 2016.](https://www.aaai.org/ocs/index.php/AAAI/AAAI16/paper/view/12216/12004)

[3]  [Zhong, Huaping, et al. "Aligning knowledge and text embeddings by entity descriptions." *Proceedings of the 2015 Conference on Empirical Methods in Natural Language Processing*. 2015.](https://www.aclweb.org/anthology/D15-1031)









