# EM-algorithm-coin-toss-sample
an EM (Expectation-Maximization) Algorithm sample coin toss 机器学习EM算法例子 抛硬币实验的程序实现

# 实验内容
两个硬币，一个硬币正面朝上概率是0.8，另一个是0.5。每次抽取一个硬币抛100次，进行100组实验。得到的数据有100组实验分成每组的实验结果，但不包含这一组实验用的硬币是哪一个的信息。怎么计算两个硬币正面朝上的概率呢？神奇的EM算法就可以做到。本程序就实现了EM算法。

# 原理
显然，有些组实验结果中正面朝上次数多，大约80次左右，有些组实验结果只有50次左右。这就有玄机了，隐隐约约感觉这里面的信息和最终结论有着千丝万缕的联系。假如能猜出这一组数据属于哪个硬币，然后用这一组结论作为硬币正面朝上的概率，那么……更神奇的是，这个步骤多次迭代重复还能增加一点点精确度

# 用到的数学公式
那些各种期望啊、对数极大似然估计啊、条件概率条件有多个值啊，看得是云里雾里……这个实验就没用那么多高深的概率论公式，就一个公式：贝叶斯公式。毕竟全靠加减乘除还是不够，贝叶斯公式没学过概率论多想想也是可以相通的。

# 参考文献呢
1. 最早提出抛硬币这个生动形象例子的文章https://www.nature.com/articles/nbt1406.pdf
2. 对于抛硬币例子解释了具体计算过程而且避免了使用极大对数似然估计、有多个先验概率等高深难懂概率论知识的文章http://www.stokastik.in/em/
3. 使用了似然率的概念，不过讲解详细的文章https://math.unm.edu/~james/week9-STAT574.pdf
4. 对于数学尤其是概率论知识要求高，充满复杂难懂公式，不过更普遍化的文章（逐个解释了教材中公式中各种变量字母的意义，可以作为教材上公式的补充资料）https://www.cs.cmu.edu/~tom/10701_sp11/recitations/EM%20Algorithm.pdf

# 版权 License
*.py file：WTFPL
