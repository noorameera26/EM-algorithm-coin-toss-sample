EM算法调研与实例

学号

姓名

定义

最大期望算法（下简称EM算法）在机器学习中被用于寻找，依赖于不可观察的隐性变量的概率模型中的参数的最大似然估计。

算法

首先，先做出假设的$$\Theta^{0}$$，教材中$$\Theta^{0}$$包括之后出现的$$\Theta^{1}$$、$$\Theta^{2}$$等等容易被误解为隐变量（由于分类器分类数据总要分配到多种情况，如果有一个隐变量，那么每一类都对应自己的一个相应隐变量的值，所以$$\Theta$$不一定只是个数字而是个向量甚至矩阵）的指数形式。经过阅读资料，$$\Theta^{t}$$应表示EM算法进行第t次后$$\Theta$$的值。

然后循环开始。

循环的第一步是**E步骤**。E是Expectation。这一步骤是根据输入的$$\Theta$$，推测出隐变量的概率分布。输出的概率被称为“membership
probabilities”，英文翻译更容易理解，我个人的理解是在当前隐变量条件下，未分类的各组数据属于各组（最终的结果是将数据分类）的概率。

循环的第二步是**M步骤**。M是Maximization。这一步是根据之前得到的期望得到参数最大化似然。我个人的理解是在E步骤得到的每组数据属于每一类得概率情况下，调整$$\Theta$$使得$$\Theta$$能把未知数据分类成E步骤结果的情形。

之后就是不断循环E-M步骤使得$$\Theta$$收敛。得到的$$\Theta$$就是需要的结果。

公式

**E步骤：**

$$
Q\left( \Theta \middle| \Theta^{t} \right) = E_{Z|X,\Theta^{t}}logL(\Theta;x,Z)
$$

其中logL表示对数似然估计 E表示期望

$$\Theta;x,Z$$表示意思和$$\Theta|x,Z$$相同，都是x、Z条件下$$\Theta$$的概率

根据上下文可理解E下标的$$Z|X,\Theta^{t}$$表示当已知数据集X和$$\theta^{(t)}$$这种情况下Z的分布是什么。得到的$$Q\left(
\Theta \middle| \Theta^{t} \right)$$是对数在这种情况下的对数似然。

**M步骤：**

$$
\Theta^{t + 1} = argmax\ Q\left( \Theta \middle| \Theta^{t} \right)
$$

结合E步骤，argmax函数是求当$$Q\left( \Theta \middle| \Theta^{t}
\right)$$最大时的$$\theta$$值。含义就是$$\theta$$如何取值才能让$$Z|X,\Theta^{t}$$概率最大，即修改$$\Theta$$尽可能贴合数据。

实例

根据参考资料，有一个实验：

假设有两个不同的、不均匀的硬币。根据常识，对于这两枚硬币分别多次抛起，最终分别能得到几乎固定的正面朝上的概率。假如现在有多次实验数据（每次实验用的硬币是随机抽取），例如：

| 硬币编号 | 实验数据   | 正面朝上概率 |
|----------|------------|--------------|
| 1        | 正正正正反 | 0. 8         |
| 1        | 正正反正正 | 0. 8         |
| 2        | 正正反反正 | 0. 4         |
| 2        | 正反反反正 | 0. 6         |

显然可以得出硬币1正面朝上概率0. 8，硬币2正面朝上概率0. 5的结论。

如果实验中去掉上表第一列，即不知道每次实验抛的是哪枚硬币，似乎无法计算两枚硬币正面朝上的概率。

但经过思考，如果每组实验中实验次数较多，那么正面朝上概率几乎就是该硬币正面朝上的概率，而实验组数也较多，可以排除极个别情况下一组实验内硬币正面朝上概率罕见地大幅度偏离硬币正面朝上的概率。所以假如能把实验数据根据相似度分组，例如分成两组，那么这两组对应的正面朝上概率可以说就是两枚硬币的正面朝上概率（当然具体哪个数字对应哪个硬币没法知道）。

可以用以下方法求解这两枚硬币分别的正面朝上概率：

1.  假设硬币1正面朝上概率$$\theta_{1} = 0.6$$，硬币2正面朝上概率$$\theta_{2} =
    0.3$$。

2.  E步骤：每一组实验数据都可以计算出这组数据属于两个硬币的概率。为了完成这个计算，必须理解并使用以下贝叶斯公式：

![](media/64b754654440e6d7573bd73d52d58e9e.png)

>   该公式对应本问题的形式就是

$$
P\left( A_{1} \middle| B \right) = \frac{P\left( B \middle| A_{1} \right)P(A_{1})}{P\left( B \middle| A_{1} \right)P\left( A_{1} \right) + P\left( B \middle| A_{2} \right)P(A_{2})}
$$

$$
P\left( A_{2} \middle| B \right) = \frac{P\left( B \middle| A_{2} \right)P(A_{2})}{P\left( B \middle| A_{1} \right)P\left( A_{1} \right) + P\left( B \middle| A_{2} \right)P(A_{2})}
$$

>   $$P\left( A_{1} \middle| B
>   \right)$$的意思是如果一组实验结果最终确实发生了那么硬币1被选中这个事件发生的概率。由于就选一枚硬币而且对于这一组实验实验结果当然发生了，所以其实意思就是这组实验结果是硬币1实验结果的概率。

>   $$P(A_{1})$$的意思是硬币1被选中的概率，此处就是0.5（因为硬币随机抽取）

>   $$P\left( B \middle| A_{1}
>   \right)$$的意思是如果选择硬币1，那么得到此组实验数据的概率，例如对于“正正正正反”，就是$$\theta_{1}^{4}
>   \cdot \left( 1 - \theta_{1} \right)^{1}$$

1.  M步骤：根据实验数据调整两枚硬币正面朝上的概率。

>   例如对于“正正正正反”，使用第二步计算出$$P\left( A_{1} \middle| B \right) =
>   0.901$$，$$P\left( A_{1} \middle| B \right) =
>   0.099$$。这个结果说明了这组数据来自硬币1的概率是0.901，来自硬币2的概率是0.099。那么此次得到的4个正中硬币1的“贡献是”是$$4
>   \times 0.901$$, 4个正中硬币2的“贡献”是$$4 \times 0.099$$……依次类推得到下表

| 实验数据 | 来自硬币1的“贡献” | 来自硬币2的“贡献” |
|----------|-------------------|-------------------|
| 正4次    | 4 \\times 0.901   | 4 \\times 0.099   |
| 反1次    | 1 \\times 0.901   | 1 \\times 0.099   |

>   计算所有组中各硬币对正方面的“贡献”之和，得到

$$
\theta_{1}^{(2)} = \frac{10.2}{10.2 + 4.5} = 0.69
$$

>   硬币2朝上的概率

$$
\theta_{2}^{(2)} = \frac{2.46}{2.46 + 2.76} = 0.52
$$

以上3运行完$$\Theta$$值就更新了，然后循环2、3步骤，直到$$\Theta$$收敛就得到了正确的答案。

把以上步骤写成程序（代码：<https://github.com/1970633640/EM-algorithm-coin-toss-sample/blob/master/main.py>），实验次数和组数适当增大，得到结果硬币1正面朝上概率$$\theta_{1}
= 0.79$$，硬币2正面朝上概率$$\theta_{2} =
0.50$$。精确度要求0.001时循环5次就可以得到结果。

可能的问题

1 猜测的初值是否可以相同？

>   不行，如果猜测的初值相同，运算效果相同，那么无论怎么循环$$\theta$$永远都相同，所以算法无法发挥作用

2 最后得到的结果能否区分顺序？

>   不行。例如两个硬币朝上概率是0.8和0.5，如果初值换成0.3和0.6，得到的结论就反过来了。

3 如果两枚硬币被抽取做实验的概率不同，还能得出正确的结论吗？

>   经过实验测试，可以。

参考资料

1 What is the expectation maximization algorithm?, Chuong B Do & Serafim
Batzoglou, <https://www.nature.com/articles/nbt1406.pdf>
注：最早提出抛硬币这个生动形象例子的文章

2 Expectation Maximization with an Example, Stokastik,
<http://www.stokastik.in/em/>
注：对于抛硬币例子解释了具体计算过程而且避免了使用极大对数似然估计、有多个先验概率等高深难懂概率论知识的文章

3 EM algorithm, James, <https://math.unm.edu/~james/week9-STAT574.pdf>
注：使用了似然率的概念，讲解详细的文章

4 EM Algorithm, Xi Chen,
<https://www.cs.cmu.edu/~tom/10701_sp11/recitations/EM%20Algorithm.pdf>
注：对于数学尤其是概率论知识要求高，充满复杂难懂公式，不过更普遍化的文章（逐个解释了教材中公式中各种变量字母的意义，可以作为教材上公式的补充资料）