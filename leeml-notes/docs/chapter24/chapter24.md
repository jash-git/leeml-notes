


![](res/chapter24-1.png)



我把dimension reduction分为两种，一种做的事情叫做“化繁为简”，它可以分为两种：一种是cluster，一种是dimension reduction。所谓的“化繁为简”的意思：现在有很多种不同的input，比如说：你现在找一个function，它可以input看起来像树的东西，output都是抽象的树，把本来比较复杂的input变成比较简单的output。那在做unsupervised learning的时候，你只会有function的其中一边。比如说：我们要找一个function要把所有的树都变成抽象的树，但是你所拥有的train data就只有一大堆的image(各种不同的image)，你不知道它的output应该是要长什么样子。

那另外一个unsupervised learning可以做Generation,也就是无中生有，我们要找一个function，你随机给这个function一个input(输入一个数字1，然后output一棵树；输入数字2，然后output另外一棵树)。在这个task里面你要找一个可以画图的function，你只有这个function的output，但是你没有这个function的input。你这只有一大堆的image，但是你不知道要输入什么样的code才会得到这些image。这张投影片我们先focus在dimension reduction这件事上，而且我们只focus在linear dimension reduction上。

## 聚类



![](res/chapter24-2.png)

那我们现在来说clustering，什么是clustering呢？clustering就是说：假设有一大堆的image，然后你就把它们分成一类一类的。之后你就可以说：这边所有的image都属于cluster1，这边都属于cluster2，这边都属于cluster3。有些不同的image用同一个cluster来表示，就可以做到“化繁为简”这件事。那这边的问题是：要到底有几个cluster，这个没有好的方法，就跟neural network要有几层一样。但是你不能太多，这张有9张image，就有9个cluster ，这样做跟没做是一样的。把全部的image放在一个cluster里，这跟没做是一样的。要怎样选择适当的cluster，这个你要empirical的来决定它      

### K-means                                                                   

那在cluster方法里面，最常用的叫做k-means。我们有一大堆的data，他们都是unlabel($x^1,...x^N$)，一个$x$就代表一张image ，我们要把它做成K个cluster。怎样做呢？我们要先找这些cluster的center。(假设这边$x$的用vector来表示的话，这边的center也都是一样长度的vector)有K个cluster就需要有K个center。那初始的center咋样来呢，你可以从你的train data里面随机找K个$x$出来，就是你的k个center。

接下里，你要对所有在train data 中的$x$，都做以下的事情：你决定说，现在的每一个$x$属于1到K中的哪一个cluster。现在假设$x$跟第i个class center最接近的话，那$x$就属于$c^i$，那我们用一个binary value(上标n，下标i)$b^n_i$来代表说第n个$x$有没有属于第i个class，如果第n个x属于第i个class的话，它的value就是1，反之就是0.接下来，你要update你的cluster，方法也是很直觉的(假设你要update第i个cluster center，你就把所有属于第i个cluster的x通通拿出来做平均，你就得到第i个cluster的center，然后你要反复的做)

只所以在initialize cluster的时候，你会想到直接从你的database里面去挑K个$x$做center，有一个很重要的原因是假设你是纯粹随机的（不是从database里面挑的），你很有可能在第一次assign这个$x$的时候，就没有任何一个x和某一个cluster很像，或者某个cluster没有一个$x$,

然后你updata的时候纯粹就是==无用功==，所以最好从database里面选取K个$x$作为cluster center。

### 层次聚类

![](res/chapter24-3.png)



那cluster有另外一种方法叫做Hierarchical Agglomerative Clusteing(HAC),那这个方法是先建一个tree。假设你现在有5个example，你想要把它做cluster，那你先做一个tree structure，咋样来建这个tree structure呢？你把这5个example两两去算它的相似度，然后挑最相似的那个pair出来。假设最相似的那个pair是第一个example和第二个example merge起来再平均，得到一个新的vector，这个vector代表第一个和第二个example。现在只剩下四笔data了，然后两两再算相似度，发现说最后两笔是最像的，再把他们merge average起来。得到另外一笔data。现在只剩下三笔data了，然后两两算他们的similarity，发现黄色这个和中间这个最像，然后再把他们平均起来，最后发现只剩红色跟绿色，在把它们平均起来，得到这个tree 的root。你就根据这五笔data他们之间的相似度，就建立出一个tree structure，这只是建立一个tree structure，这个tree structure告诉我们说：哪些example是比较像的。比较早分枝代表比较不像的。

接下来你要做clustering，你要决定在这个tree structure上面切一刀(切在图上蓝色的线)，你如果切这个地方的时候，那你就把你的五笔data变成是三个cluster。如果你这一刀切在红色的那部分，就变成了二个cluster，如果你这一刀切在绿色这一部分，就变成了四个cluster。这个就是HAC的做法

HAC跟刚才K-means最大的差别就是：你如果决定你的cluster的数目，在k-means里面你要决定K value是多少，有时候你不知道有多少cluster不容易想，你可以换成HAC，好处就是你现在不决定有多少cluster，而是决定你要切在这个 tree structure的哪里。



## Distributed Representation

![](res/chapter24-4.png)



光只做cluster是非常卡的，在做cluster的时候，我们就是"以偏概全"这个就好像说：“念能力”有分成六大类，每一个人都要被assign成这其中的一类。要咋样决定它是属于哪一类呢？拿一杯水看看它有什么反应，就assign成哪一类。比如说：水满出来了就是强化系，所以小傑是强化系。这样子把每一个人都assign某一个系是不够的，太过粗糙的。像bisiji就有说：小傑其实是接近放出系的强化系能力的念能力者。如果你只是说它是强化系的，这是loss掉很多information的，你应该这样表示小傑，应该说：强化系是0.7，放出系是0.25，强化系是跟变化系是比较接近的，所以有强化系就有一部分的变化系的能力，其他系的能力为0。所以你应该要用一个vector来表示你的$x$，那这个vector每一个dimension就代表了某一种特值，那这件事就叫做：distributed representation。

如果原来你的$x$是一个非常high dimension的东西，比如说image，你现在用它的特值来描述，它就会从比较高维的空间变成比较低维的空间。那这件事情就被叫做：dimension reduction。这是一样的事情，只是不同的称呼而已。

## 降维

![](res/chapter24-5.png)

### 降维样例

那从另外一个角度来看：为什么dimension reduction可能是有用的。举例来说：假设你的data分布是这样的(在3D里面像螺旋的样子)，但是用3D空间来描述这些data其实是很浪费的，其实你从资源就可以说：你把这个类似地毯卷起来的东西把它摊开就变成这样(右边的图)。所以你只需要在2D的空间就可以描述这个3D的information，你根本不需要把这个问题放到这个3D来解，这是把问题复杂化，其实你可以在2D就可以做这个task

![](res/chapter24-6.png)



我们来举一个具体的例子：比如说我们考虑MNIST，在MNIST里面，每一个input digit都是用28*28dimension来描述它。但是多数28 *28dimension的vector你把它转成一个image看起来不像一个数字。你random sample一个28 *28vector转成一个image，可以是这样的(数字旁边的图)，它看起来根本就不不像数字。所以在这28 *28维空间里面 。digit这个vector其实是很少的，所以你要描述一个digit，你根本就不需要用到28 *28维，你要描述一个digit，你要的dimension远比28 *28维少。

我们举一个很极端的例子：比如说这里有一堆3，这堆3如果你是从pixel来看待的话，你要用28*28维来描述一个image，然后实际上这些3只需要一个维度就可以来表示了。为什么呢？因为这些3就只是说：把原来的3放在这就是中间这张image，右转10度就是这张，右转2度变它，左转10、20度。所以你唯一要记录的只有今天这张image它是左转多少度右转多少度，你就可以知道说它在维的空间里面应该长什么样子。你只需要抓重这个重点(角度的变化)，你就可以知道28维空间中的变化，所以你只需要一维就可以描述这些image

![](res/chapter24-7.png)



那怎么做dimension reduction呢?在做dimension reduction的时候，我们要做的事情就是找一个function，这个function的input是一个vector x，output是另外一个vector z。但是因为是dimension reduction，所以你output这个vector z这个dimension要比input这个x还要小，这样才是在做dimension reduction。

### 特征选择

在做dimension reduction里面最简单是feature selection，这个方法是：你把你的data分布拿出来看一下，本来在二维的平面上，但是你发现都集中在$x_2$dimension这里，这个$x_1$dimension没什么用，把它拿掉就只有$x_2$dimension，你就等于做到dimension reduction这件事了。这个方法不见得有用，因为有很多时候，你的case是：你任何一个dimension都不能拿掉。

### 主成分分析

另外一个常见的方法叫做Principe component abalysis(PCA),PCA做的事情就是：这个function是一个很简单的linear function，这个input x跟这个output z之间的关系就是一个linear transform，你把这个x乘上一个matrix w，你就得到它的output z。现在要做的事情就是:根据一大堆的x(我们现在不知道z长什么样子)我们要把w找出来



![](res/chapter24-8.png)



接下来我们来介绍一下PCA，我们刚才讲过PCA要做的事情就是找这个W(z=Wx)，这个W咋样找呢？假设我们现在考虑一个比较简单的case，我们考虑一个one dimension的case。我们现在假设只要把我们的data project到一维的空间上，也就是z是一个scale，w其实就是一个row。那我们就用$w^1$来表示W的第一个row，把x跟$w^1$做内积就得到了$z_1$。接下来我们要问的问题是：我们要找的这个$W^1$，应该要长什么样子。我们先假设$w^1$的长度是1(这个假设是有必要的，等下你会更清楚我们为什么要这个假设)如果的长度是1的话，那$w^1$跟x做内积得到的$z_1$意味着：w跟x是高维空间中的一个点，$w^1$是高维空间中的vector，$z_1$就是x在$w^1$上的投影。所以我们现在做的事情就是把一堆x通过$w^1$变成$z_1$(得到一堆$z_1$，每个x都变成$z_1$)那现在的问题就是这个$w^1$应该长什么样子呢，怎么选取这个$w^1$呢？

举例来说，这个x的分布(图中蓝色的点，每一个点代表宝可梦)，这个分布的横坐标是攻击力，纵坐标是防御力。那今天我要把二维投影到一维，我应该要选什么样的$w^1$呢？我可以选$w^1$指向这个方向(红色的箭头)，也可以选$w^1$指向那个方向(橙色的箭头)，我选不同的方向得到的结果会是不一样的。那你总得给我们一个目标，我们才能知道要选咋样的$w^1$。我们的目标是这样的：我们希望选一个$w^1$，它经过projection以后得到这些$z_1$的分布是越大越好，也就是我们不希望说通过这个projection以后所有的点通通挤在一起，把本来的data point跟data point之间的奇异度拿掉了。我们是希望说：经过projection以后，不同的data point他们之间的区别我们仍然是可以看的出来，所以找一个projection方向它可以让projection后的various是越大越好。如果我们看这个例子的话，你就会觉得说：如果是选这个方向的话(红色箭头)，经过projection以后，可能会分布在这个range(large variance)；如果选这个方向的话(橙色箭头)，那么你的点可以是这个range(small variance)。所以你要选择$w^1$的时候，你可能会选择$w^1$的方向是large variance这个方向。从这个图上，你可以看出$w^1$其实是代表宝可梦的强度，宝可梦可能有一个factor代表它的强度，这个隐藏的factor同时影响了它的防御力跟攻击力，所以防御力跟攻击力是会同时上升的。

#### 数学推导

那我们要用equation来表示的话，你就会说：我们现在要去maximize的对象是$z_1$的variance，$z_1$的variance就是summation over所有的$z_1$，然后$z_1$减去$(\bar z_1)^2$。

![](res/chapter24-9.png)



假设你知道咋样来做(等一下来讲怎么做)，你找到一个$w^1$，你就可以让$z_1$最大，然后就结束了。你现在不只是想要投影到一维，你想要投影到更多维(二维)。现在你想要投影到二维的平面的时候，这时候你就把x跟另外一个$w^2$做内积,$w^1,w^2$就是W的第一个row和第二个row。那我们咋样来找$w^2$呢？跟刚才找$z_1$一样，首先假设$w^2$的长度为1，$z_2$的分布也是越大越好，但是你只是要让$z_2$的variance越大越好，这样你找出不就 是$w^1$吗，但是你$w^1$刚才已经找过了，这样的话你就等于什么事都没有做。所以你要再加一个condition，刚才已经找到$w^1$了，这次要$w^2$跟$w^1$是垂直的($w1 * w^2=0$)。你先把$w^1$，再找$w^2$,等等，这就看你要projection到几维了。(projection要几维是你自己要决定的)。把$w^1,w^2,...$排起来成W就结束了。

这个W是一个orthogonal matrix，这时候，你看它的row($w^1,w^2$)是orthogonal，$w^1,w^2$的长度都是1，所以它是一个orthogonal matrix。

![](res/chapter24-10.png)

接下里的问题就是怎样来找$w^1,w^2$(咋样来解这个问题)，这个解法是蛮容易的。经典的方法:$z_1$等于$w^1x$,$z_1$的平均值summation over$z_1$（公式里面少除n）,也就是summation over$w^1x$,$w^1$跟data point无关，可以提出来变为先summation over x在$w^1 \sum x$得到$w^1$跟$\bar{x}$。接下来我们要maximize的对象是$z_1$的variance($\sum_{z_1} (z_1-\bar{z_1})^2$)公式整理为$\sum( w^1(x-\bar{x}))^2$。可以把这个式子做一个转化：$w^1$是一个vector，$x- \bar{x}$是一个vector。假设$w^1$是a，$x-\bar{x}$是b，可以写成a的transform乘 b的平方，可以写成a的transpose 乘以b再乘以a的transpose乘以b，可以写成a的transpose乘以b再乘以a的transpose乘以b的transpose(a的transpose乘以b是一个scale）：找一个$w^1$，它可以maximizeing$(w^1)^TSw^1$（$S=Cov(x)$）。但这个是要有constraint，如果没有constriant的话，这个问题会有无聊的solution，把每个值都变无穷大，这样就结束了，所以这个问题是要有constraint。这个问题constraint是：$w^2$的L-Norm等于1

![image](res/chapter24-11.png)



有了这些以后呢，我们要解这个问题。S是covariance matrix，又是半正定。也就是说所有的eigenvalues都是non-negative(比较困惑的话，可以去看李老师的现代课)。这个问题的solution就是：$w^1$是covariance matrix的eigenvector。它不只是一个eigenvector，它是对应到最大的eigenvalue $\lambda $那一个eigenvector。这个就是结论

中间的过程是：首先我要用lagrange multiplier(bitshop appendix)，式子是：$g(w^1)=(w^1)^TSw^1-\alpha ((w^1)^Tw^1-1)$，接下来你把这个g对所有的w做偏微分(w是一个vector，有很多的element)，令这个式子通通等于0(偏微分)，整理一下你会得到这个式子：$Sw^1-\alpha w^1=0$。这个式子告诉我们说：solution是满足这个式子，如果写成$Sw^1=\alpha w^1$的话，$w^1$就是S的一个eigenvector。但是S的eigenvector有很多，而且你还可以找到eigenvector的长度等于1。所以你接下来要做的事情是，看哪一个eigenvector带到这个式子里面($(w^1)^TSw^1$)可以maximizing$(w^1)^TSw^1$。整理一下变为$\alpha (w^1)^Tw^1$，得到结果为$\alpha$，谁可以让这个$\alpha$最大呢？答案是：$w^1$是对应到largest eigenvalue 的eigenvector时最大，这个$\alpha$是最大的eigenvalue $\lambda _1$.



![image](res/chapter24-12.png)



那我要找$w^2$的话，我们要解是这样的equation：$(w^2)^TSw^2$  $(w^2)^Tw^2=1  $ $(w^2)^Ts^1=0$。我们要maximizing根据$w^2$投影以后的variance。结论是：$w^2$也是covariance matrix S的一个eigenvector，然后它对应到$2^{nd}$ largest eigenvector $\lambda _2$。那我们现在来解它：你先写一个function g，function里包括了你要maximzing的对象，还包括了两个constraint，分别乘以$\alpha,\beta $。接下来你对所有的参数做偏微分($w^2$所有的element)。做完以后你得到这个式子($Sw^2-\alpha w^2-\beta w^1=0$)，然后坐左同时乘以$w^1$的transpose(乘以$w^1$的transpose以后，会出现$(w^1)^Tw^1$会等于1,$(w^1)^TW^2$等于0)，整理一下等于$((w^1)^TSw^2)^T$(scale)，在整理一下得到$(w^{2})^TSw^1$。我们已经知道$w^1$是S的eigenvector，而且它对应到最大的eigenvalue $\lambda_1 $($Sw^1=\lambda w^1$)。从这边我们得到$\beta $等于0，所以剩下的$Sw^2- \alpha w^2=0$，然后得出$Sw^2=\alpha w^2$。$w^2$是一个eigenvector，但是它是哪一个eigenvector呢？它是第二大eigenvector。

 

![image](res/chapter24-13.png)



z =Wx，这里神奇的地方就是：z的covariance是diagonal matrix，也就是说如果我们今天做PCA，你原来的data distribution可能是左边这张图，做完PCA以后，你会做decorrelation，你会让你不同的dimension间的covariance是0.也就是说你算z这个vector covariance matrix的话，会发现它是(Disgonal matrix)，这样做是有好处的。假设你PCA得到的feature(z)，这个新的feature是要其他的model用的，你的model假设说是一个generative model，你用gaussion来描述某一个class的distribution，而你在做gaussion假设的时候，你假设说input data它的covariance是diagonal，你假设不同的dimension之间没有decorrelation，这样可以减少你的参数量。

你把原来的input data做PCA以后，再丢给其他的model，其它的model就可以假设现在的input data它的dimension之间没有decorrelation。所以它就可以用简单的model处理你的input data，这样就可以避免overfitting的情形。

这件事情怎么说明呢？z的covariance是$z-\bar{z}$乘以$(z-\bar{z})^T=WSW^T$,s=Cov(x)，把S乘进$w^1,...w^k$变成$[Sw^1,Sw^2,...Sw^k]$,$w^1$是S的eigenvector，$\lambda$是eigenvalue，所以$[Sw^1,Sw^2,...Sw^k]$,$w^1$变成$[\lambda w^1,\lambda w^2,...\lambda w^k,]$，然后把W乘进去，然后就变成了$[\lambda W w^1,\lambda Ww^2,...\lambda Ww^k,]$。($w^1$是W的第一个row)W乘以$w^1$等于$e_1$（$w_1,...,w_n$是相互正交）,$e_1$就是vector第一维是1，其它都是0，这个东西就是Diagonal matrix。

PCA,第一个找出的$w^1$是covariance matrix对应到最大eigenvalue的eigenvector，然后找出的$w^2$就是对应到第二大的eigenvector,以此类推。有一个证明告诉你说：这么做的话，每次投影的时候都可以让variance最大。

#### 主成分分析的另一个角度

![image](res/chapter24-14.png)



我们从更清楚的角度来看PCA，你就会知道PCA到底在做什么。假设我们考虑的是手写的数字，我们知道这些数字其实是一些basic component所组成的。这些basic component可能就代表笔画。举例来说：人类所手写的数字就是这些basic component所组成的，有斜的直线，横的直线，有比较长的直线，然后还有小圈，大圈等等，这些basic component加起来就可以得到一个数字。这些basic component写做$u^1,u^2,...u^5$，这些basic component其实就是一个一个的vector。假设我们现在考虑的是mnist，mnist一张image28*28piexl，也就是28 *28维的vector。这些component其实就是28 *28维的vector，把这些vector加起来以后，你所得到的vector就代表了一个diagit。如果把它写成formulation的话就是：$x\approx c_1u^1+c_2u^2+....c_ku^k+\bar{x}$，x代表一张image里面的pixel，x等于$c_1u^1$component乘以$c_2u^2$这个component，一直加到$c_ku^k$component，再加上$\bar{x}$,$\bar{x}$是所有image的平均。所以每一张image就是有一堆component的linear conformation再加上它平均所组成的。

举例来说：7是这三个component加起来的结果，假设7就是x的话，$c_1=1,c_2=0,c_3=1,c_4=0,c_5=1$，你可以用$c_1,c_2,...c_k$来表示一张image。假设component比pixel的数目少的话，那么这个描述是比较有效的。7是1倍的$u^1$,1倍$u^2$,1倍的$u^5$所组成的，所以7是一个vector，它的第一维，第三维，第5维是1.

![image](res/chapter24-15.png)



现在把$\bar{x}$移到左边，x减掉所以image的平均等于一堆component linear conformation，这些linear comformation写作$\hat{x}$，那现在假设我们不知道这些component 是什么，不知道$u^1$到$u^k$的vector长什么样子。那我们咋样找K vector出来呢？我们要做的事情就是：我们要去找K vector使得$\hat{x}$跟$x-\bar{x}$越接近越好，他们的差用reconstruction error来描述。接下来我们要做事情就是：找K 个vector可以minimize这个reconstruction error。

在PCA中我们想说：我们要找一个matrix W，x乘以matrix W就得到了z，把W的每一个row写出来就是$[w_1,w_2,...,w_k]$，x乘上一个$[(w_1)^T,(w_2)^T,...(w_k)^T]$,以此类推。那么说：是$[w_1,w_2,...,w_k]$是covariance matrix的eigenvector，事实上你要解这个式子$L=mi_n(u^1,...u^k)\sum \left \| (x-\bar{x}) -(\sum_{k=1}^{k}c_ku^k)\right \|$，找出$u^1,...u^k$。由PCA找出的这个解$w^1,...w^k$就是可以让L最小化



![image](res/chapter24-16.png)



我们有一大堆的x，现在假设有一个$x_1$，这个$x_1$减去$\bar{x}$等于$u^1$乘以component weight，c上标1下标1(下标1代表说：它是$u^1$的weight,上标1代表说：$x^1$的$u^1$component weight)，$x^1-\bar{x} \approx c^1_1u^1+c^1_2u^2+...$.

$x-\bar{x}$是一个vector(把这个vector拿出来),$u^1,u^2...$是一排vector(把它排起来，排起来就是一个matrix)，columns的数目是K个，把这些component weight排成一排变成一个vector，vector乘以matrix变成另一个vector。我们不只是有一笔data，$x^2-\bar{x}$是另外一个黄色的vector，这个vector($c_1^2,c_2^2,....$)乘以matrix $u^2$等于另一个黄色的vector，依次类推。

那我们把所有的data用这个式子来表示的话，这样就会得到一个matrix，这个matrix的cloumns等于data的数目(你有1万笔data，cloumns=10000)。现在$\left\{\begin{matrix}
... &... \\ 
u^1 & u^2 \\ 
 ...& ...
\end{matrix}\right.$乘以这个matrix $\left\{\begin{matrix}
c_1^1 &c_1^2 \\ 
c_2^1 & c_2^2 \\ 
 ...& ...
\end{matrix}\right.$跟这个matrix越接近越好，所以你要minimize左边两个matrix跟右边这个matrix之间的差距是会被minimize的，也就是说：用SVD提供给我们的matrix拆解方法，拆成这是三个matrix相乘后，跟左边的matrix是最接近的。

#### 奇异值分解

![image](res/chapter24-17.png)



那要咋样来解这个问题呢？加入你有学过大一现代化，你就应该知道该咋样来解(可以参考李宏毅老师现代的课程)。每一个matrix X，你可以用SVD把它拆成一个matrix U(m *k)乘上一个matrix $\sum$(k *k)乘上matrix V(k *n)。这个matrix U就是matrix$\left\{\begin{matrix}
... &... \\ 
u^1 & u^2 \\ 
 ...& ...
\end{matrix}\right.$，这个$\sum$V就是$\left\{\begin{matrix}
c_1^1 &c_1^2 \\ 
c_2^1 & c_2^2 \\ 
 ...& ...
\end{matrix}\right.$。

如果我们今天用SVD将X拆成这三个matrix相乘，那右边三个matrix相乘的结果是最接近左边这个matrix的，那解出来的结果是什么样呢？其中U这个matrix的 K columns其实就是一组orthonormal vector，这组orthonormal vector是$XX^T$的eigenvector，U总共有K个orthonormal vector，这K 个orthonormal vector对应到$XX^T$最大的k个eigenvalue的eigenvector。

你会发现，这个$XX^T$就是covariance matrix，PCA之前找出的W就是covariance matrix的eigenvector。而我们这边说做SVD，解出来U的每个column就是covariance matrix的eigenvector，所以这个U得出的解就是PCA得到的解。所以我们说：PCA做的事情就是：你找出来的那些W其实就是component。

#### 主成分分析和神经网络

![image](res/chapter24-18.png)



我们现在已经知道从PCA找出的$w^1,...w^K$就是k个component$u^1,...u^K$。我们现在有一个根据component linear combination的结果叫做$\hat{x}$`(`$\sum_{k=1}^{K}c_kw^k$)。我们希望$\hat{x}$跟$x-\bar{x}$的差值越小越好，你要minimize reconstruction error。那我们现在已经根据SVD找出这个W了，那这个$c_k$的值是到底多少呢？这个$c_k$是每一个image都有自己的$c_k$，这个$c_k$就每个image各自找就好。如果现在要用$c_1,...c^k$对$w^k$做linear combination。这个$c_k=(x-\bar{x})w^k$，找一组$c_k$可以minimize$ \hat{x},(x-\bar{x})$

linear combinarion做的事情你可以想成用neural network来表示它，假设我们的$x-\bar{x}$就是一个vector(这边写做三维的vector)，假设现在只有2个component(k=2)，那我们先算出$c_1,c_2$，$c_1=(x-\bar{x})$($x-\bar{x}$）的每一个component乘上$w^1$的每一个component，接下来你就得到了$c_1$。($x-\bar{x}$是neural network的input，$c_1$是一个neural(linear neural )，$w^1_1,w^1_2,w^1_3$是weight，)。$c_1$
乘以$w^1$($c_1$乘上$w_1$的第一维($w_1^1$)得到一个value，乘上$w_1$的第二维($w_2^1$)得到一个value，乘上$w_1$的第三维($w_3^1$)得到一个value)。接下来再算一下$c_2$，$c_2$乘以$w^2$的结果和之前的加起来就是最后的output，$\hat{x_1},\hat{x_2},\hat{x_3}$就是$\hat{x}$。

接下来就是minimize error，我们要$\hat{x}$跟$x-\bar{x}$越接近越好(也就是这个output跟$x-\hat{x}$越接近越好)，那你可以发现说PCA可以表示成一个neural network，这个neural network只有一个hidden layer，这个hidden layer是linear activation function。那现在我们train 这个neural network,是要input一个东西得到output，这个input跟output越接近越好。这个东西就叫做Autoencode

这边就有一个问题，假设我们现在这个weight，不是用PCA的方法去找出$w^1,w^2,...w^k$。而是兜一个neural network，我们要minimize error，然后用Gradient Descent去train得到的weight，那就觉得你得到的结果会跟PCA得到的结果一样吗？这其实是会一样的(neural network没有办法保证是垂直的,你会得到另外一组解)。所以在linear的情况下，或许你就想要用PCA来找这个$W$比较快的，你用neural network是比较麻烦的。但是用neural network的好处是：可以deep。

#### 缺点

![image](res/chapter24-19.png)



PCA有一些很明显的弱点，一个是：它是unsupervised，今天加入给它一大堆点没有label。假设你要把它project到一维上，PCA可以找一个可以让data variance最大的那个dimension，在这个case中，就把它project到这一维上(红色箭头))。有一种可能是，这两组data point它们分别代表了两个class，如果你用PCA来做dimension reduction的话，你就会使得蓝色跟橙色的class被meger在一起，这样就无法分别了。

这时候你要怎么办呢？这时候你要引入label data，LDA是考虑label data考虑降维的方法，不过它是supervised，所以这个不是我们要讲的对象。

另外一个PCA的弱点是Linear，我们刚开始举得例子会说。我们刚开始举的例子说：这边有一堆点的分布是像S型的，我们期待说：做dimension reduction后可以把这个S型曲面可以把它拉直，这件事情对PCA来说是做不到的。如果这个S型曲面做PCA的话，它是这样子的(如图)，就像打扁一样，而不是把它拉开，拉开这件事情PCA是办不到的。等下我们会讲non-linear

#### 应用实例

![image](res/chapter24-20.png)



接下来，我们把PCA用在一些实际的问题上。比如说：用它来分析宝可梦的data，宝可梦总共有800种宝可梦，每个宝可梦可以用6个features来表示，分别是：生命值，攻击力，防御力，特殊攻击力，特殊防御力，速度。所以每一个宝可梦就是一个6维的vector，我们现在用PCA来分析它。

在PCA中有个常问的问题：我需要有多少个component，要把它project到一维，二维还是三维...。要多少个component就好像是neural network要几个layer，每个layer要有几个neural一样，所以这是你要自己决定的。

那一个常见的方法是这样的：我们去计算每一个principle components的$\lambda $(每一个principle component 就是一个eigenvector，一个eigenvector对应到一个eigenvalue $\lambda $)。这个eigenvalue代表principle component去做dimension reduction的时候，在principle component的那个dimension上它的variance有多大(variance就是$\lambda$)。


今天这个宝可梦的data总共有6维，所以covariance matrix是有6维。你可以找出6个eigenvector，找出6个eigenvalue。现在我们来计算一下每个eigenvalue的ratio(每个eigenvalue除以6个eigenvalue的总和)，得到的结果如图。

可以从这个结果看出来说：第五个和第六个principle component的作用是比较小的，你用这两个dimension来做projection的时候project出来的variance是很小的，代表说：现在宝可梦的特性在第五个和第六个principle component上是没有太多的information。所以我们今天要分析宝可梦data的话，感觉只需要前面四个principle component就好了。

![image](res/chapter24-21.png)





我们实际来分析一下，你做PCA以后得到四个principle component就是这个样子，每一个principle component就是一个vector，每一个宝可梦是用6位的vector来描述。我们来看每一个principle component做的事情是什么：如果我们看第一个principle component，第一个principle component每一个dimension都是正的，这个东西其实就代表了宝可梦的强度(如果你要产生一只宝可梦的时候，每一个宝可梦都是由这四个vector做linear conformation ，每一个宝 可梦可以想成是，这六个vector做linear conformation的结果，在选第一个principle component的时候，你给它的weight比较大，那这个宝可梦的六维都是强的，所以这第一个principle component就代表了这一只宝可梦的强度)。第二个principle component它在防御力的地方是正值，在速度的地方是负值，也就是说这个防御力跟速度是成反比的，你给第二个principle component一个weight的时候，你会增加那只宝可梦的防御力但是会减低它的速度。

我们把第一个和第二个principle component 画出来的话，你会发现是这个样子(图上有800个点，每一个点对应到一只宝可梦)，但这样我么很难知道每一只宝可梦是什么。

![image](res/chapter24-22.png)



如果我们看第三个和第四个component的话，会发现第三个principle component的特殊防御力是正的，攻击力和生命值是负的，说明是用生命值和攻击力换取特殊防御力的宝可梦。最后一个是：它的HP是正的，攻击力和防御力是负的，也就是说：它是用攻击力和防御力来换取生命力的宝可梦。

![image](res/chapter24-23.png)



我们拿它来做手写数字辨识的话，我们可以把每一张数字都拆成component乘以weight，加上另外一个component乘以weight，每一张component是一张image(28* 28的vector)。我们现在来画前30component的话(PCA)，你得到的结果是这样子的(如图所示)，你用这些component做linear conformation，你就得到所有的digit(0-9)，所以这些component就叫做Eigen digits(这些component其实都是covariance matrix的eigenvector)

![image](res/chapter24-24.png)



如果我们做人脸辨识的话，得到的结果是这样子的。找它们前30的pixel component，叫做Eigen-face。你把这些脸做linear conformation以后就可以得到所有的脸。但是这边跟我们预期的有些是不一样的，这是不是有bug啊。因为我们PCA找出来的是component，我们把很多component 做linear combine以后它会变成face。但是现在我们找出来的不是component，我们找出来的每一个图都几乎是完整的脸。

![image](res/chapter24-25.png)



为什么会这样呢？其实你仔细想PCA的特性，你会发现说，会得到这个结果是可能的。因为在PCA里面，weight它可以是任何值(可以是正的，也可以是负的)，所以当我们用pixel component组成一张image的时候，你可以把这个component相加，也可以相减。所以这会导致你找出的component不见得是一个图的basic的东西。

假设我要画一个9，那我可以先画一个8，把下面的圈圈减掉，然后把一杠加上去(你可以先画一个复杂的图，再把多于的减掉)。这些component其实不见得是笔画的东西，若你要得到类似笔画的东西，你要用Non-negative matrix factotization(NMF)。在Non-negative matrix factotization里面，我们刚才说：PCA它可以看做是对matrix X做SVD，SVD是matrix factorization的技术，它分解出来的两个matrix的值可以是正的，也可以是负的。那现在你要用NMF的话，我们会强迫所有的component weight都是正的。是正的好处就是，现在一张image必须由component叠加得到(你不能说：我先画一个很复杂的东西，再把一些东西去掉，得到一个digit)。如果你用PCA的话，你的dimension每一回都不见得是正的，会有一些负值，负值你是不知道要该怎么处理的。

用NMF的话值都是正的，那些component自然会形成一张image。

![image](res/chapter24-26.png)



在MNIST用NMF的话，你找出来的那些piexl component它就会清楚很多，你会发现你找出来的每个东西都是笔画了。

![image](res/chapter24-27.png)



如果你要看脸的话，你就会发现说：它长的是这个样子，它比较像是脸的一些部分。

### 矩阵分解

![image](res/chapter24-28.png)



假设我们现在做一个调查，调查每个人买公仔的数目。ABCDE代表五个人，A有5个春日，B有4个，C有1个，D有1个，E有0个。A有炮姐3个，B有3个，C有1个，D有1个，E有1个。A有姐寺0个，B有0个，C有0个，D有4个，E有5个。A有小唯1个，B有1个，C有5个，D有4个，E有4个。你会发现说：在这个matrix上面，每一个table里面的block并不是随机出现的。如果有两个公仔的人，那是因为每个人跟角色的背后是有一些特性的(有一些factor来操控这些特性)



![image](res/chapter24-29.png)



这些动漫仔会分为两种，有一种是萌傲娇的，有一种是萌天然呆的，每个人就是在萌傲娇和萌天然这个平面上一个点。那么A可能是偏萌傲娇的，每一个角色有傲娇属性也可能有呆萌属性，所以每个角色就是就是平面上的一个点，所以每个角色都可以用vector来描述它。如果某一个人萌的属性跟某一个角色它本身所具有的属性是match的话(它们背后的vector很像)，那这个人就会买这个公仔。

A,B,C它们的vector是左边这样子的，A是萌傲娇的，B也是萌傲娇的(没有A那么强)，C是萌天然呆的，每一个动漫人物角色后面也都有傲娇，天然呆这两种属性。如果它们背后属性是match的话，那这个人就会买这个公仔。世界操控的逻辑是这样子的，但是这些factors(萌傲娇还是萌天然呆)这些是没有办法直接被观察的。

![image](res/chapter24-30.png)



但是这些factors(萌傲娇还是萌天然呆)这些是没有办法直接被观察的，因为没有人去关心他心里是想什么，你也没有办法说：每一个动漫人物它背后的属性是什么，这个是没有办法直接观察到的。



![image](res/chapter24-31.png)



我们有的是动漫人物跟手上有的公仔的数目，我们要根据这个关系论出每一个人跟每一个动漫人物背后的factor(每个人背后都有一个二维的vector，代表他萌傲娇，萌天然呆的程度)，每个人物的背后也都有一个vector，代表他傲娇的属性和天然呆的属性。我们知道每个人手上有公仔的数目，把它合起来看做是一个matrix X。这个matrix X的row的数目是Otakud的数目，它的columns的数目是动漫角色的数目。

那我们现在做一个假设，这个matrix里面的element它都来自于两个vector的inner product。为什么A有5个春日的公仔呢？是因为$r^A$跟$r^1$的inner product很大(它的inner product是5，所以有5个春日的公仔)，如 果$r^A$跟$r^1$的inner product是4，那B就会有4个炮姐的公仔，以此类推。

如果这件事你用数学来表示的话，我们可以写成这样子。把$r^A$跟$r^B$排成一排，把$r^1$到$r^4$排起来。这个K是latent factor的数目，这个东西我们是没有办法知道的(我们把人分成萌傲娇萌天然呆，这样是不精确的分析方式。如果我们有更多data的话，我们应该更准确的知道有多少factor，要多少factor这些必须是试出来，这个就行principle component或者neural network的层数一样，你要事先决定好)我们就假设说现在latent factor的数目是K，你把它当成matrix row，就是一个M*K的matrix。如果你把$r^1$到$r^4$当做cloumns，就是K *N 的matrix。如果你这个M *K跟K *N的matrix乘起来就是M *N的matrix。那它的每一个dimension就是(最上角的dimension就是$r^A$乘以$r^1$，第一个row第二个column是$r^A$乘以$r^2$，以此类推)matrix X。那这样的话，我们做的事情就是找一组$r^A$到$r^E$，找一组$r^1$到$r^4$，把这两个matrix相乘以后，跟左边这个matrix X越接近越好(minimize error)。

这个就可以用刚才的SVD来解，你可能说SVD不是有三个解吗？这时候你看你是要把$\sum$并到左边还是右边，把这个matrix X拆成两个matrix，然后minimize error。

#### FM推荐系统中的应用

![image](res/chapter24-32.png)



但是有时候你会遇到这个问题，就是一些information是missing的(不知道这个information)。比如说：你不直道ABC手上有没有小叶撕的公仔，有可能在他所在的地区没有发行，那这个table只能写？

假设在table上有一些是问号的话，怎么办呢？你用刚才SVD的方法就会有点卡。那在这个matrix上有missing的value的话，我们还是可以做的，我们就用gradient descent的方法来做。我们来写一个loss function $L=\sum _{(i,j)}(r^i*r^j-n_{ij})^2$(你要i这个人乘以j这个角色的inner product跟它购买的数量$n_{ij}$是越接近越好)重点是说：我们在summation over这些element的时候，我可以避开这些missing data(我们在summation overi,j的时候，如果值没有，我就不算它，我们只算有定义值的部分)

![image](res/chapter24-33.png)



那我们就用gradient descent来实际做一下 ，我们假设latent factor的数目为2。那A到E都会对应到二维的vector，那每一个角色都会对应到一个vector(属性)。所以我们把它在两个维度里面比较大的维度挑出来的话，你就会发现说：A,B是萌同一种属性，C,D,E是萌同一种属性，1,2有同样的属性，3,4有同样的属性。你没有办法说每一个属性分别都代表着什么(不知道那个维度代表着天然呆或者傲娇)。

你需要先找出这些latent factor去分析它的结果，你就可以知道说(因为我们事先已经知道姐寺跟小唯是有天然呆属性)第一个维度代表天然呆的属性，第二个维度是傲娇的属性。有了这些data以后，你就可以预测你的missing value。

如果我们已经知道$r^3$,$r^A$，我们知道一个会购买公仔的数量其实是动漫角色背后的latent factor跟人背后的latent factor做inner product的结果。那我们把$r^3$跟$r^A$做inner product之后，你就可以预测说这个人会买多少公仔。

![image](res/chapter24-34.png)



刚才那个model可以做的更精致一点，我们刚才说：A背后的latent factor $r^A$跟1背后的latent factor$r^1$得到的结果就是table上面的数值。但是事实上可能还会有别的因素会操控它的数值，所以更精确的写法是：$r^A$跟$r^1$的inner product加上某一个跟A有关的scale$b_A$，再加上跟1有关的scale $b_1$其实才等于5。$b_A$代表说：A本身有多喜欢买工仔，$b_1$代表说：这个角色它本身会有多想让别人购买(这件事情跟属性是无关的)

所以改一下minimize的式子，改为$r^i$跟$r^j$的inner product加上$b_i$,$b_j$，然后你希望这个值跟$n_{ij}$越接近越好，用gradient descent来解。(你也可以在loss function后面加上regularization)

#### MF主题分析的应用

![image](res/chapter24-35.png)

、

matrix factorization有很多的应用，可以应用到topic analysis上面。如果把刚才讲的matrix factorization的技术用到topic analysis上面就叫做latent semantic analysis(LSA)。就是把刚才的动漫人物换成文章，把刚才的人换成词汇。table里面的值就是term frequency，把这个term frequency乘上一个weight代表说这个term本身有多重要。

怎样evaluation一个term重不重要呢？常用的方式是：inverse document frequency(计算每一个词汇在整个paper有多少比率的document涵盖这个词汇，假如说，每个词汇，每个document都有，那它的inverse document frequency就很小，代表着这个词汇的重要性是低的，假设某个词汇只有某一篇document有，那它的inverse document frequency就很大，代表这个词汇的重要性是高的。)


在这个task里面，如果你今天把这个matrix做分解的话，你就会找到每一个document背后那个latent factor，那这边的latent factor是什么呢？可能指的是topic(主题)，这个topic有多少是跟财经有关的，有多少是跟政治有关的。document1跟document2有比较多的“投资，股票”这样的词汇，那document1跟document2就有比较高的可能背后的latent factor是比较偏向“财经”的

topic analysis的方法多如牛毛，基本的精神是差不多的(有很多各种各样的变化)。常见的是probability latent semantic analysis(PLSA)和latent Dirchlet allocation(LDA)。这跟之前在machine learning讲的LDA是完全不一样的东西

### 未引入的其他相关方法

![image](res/chapter24-36.png)





这些是一些reference给大家参考，这边是跟PCA比较有关系的。

dimension reduction的方法多如牛毛，比如说MDS，MDS的特别是：它不需要把每一个data都表示成feature vector，它要知道feature vector跟feature vector之间的distance，知道这个distance，你就可以做dimension reduction。一般教科书举得例子会说：我现在一堆城市，你不知道咋样把城市描述成vector，但你知道城市跟城市之间的距离(每一笔data之间的距离)，那你就可以画在二维的平面上。其实MDS跟PCA是有一些关系的，如果你用某些特定的distance来衡量两个data point之间的距离的话，你做MDS就等于做PCA。其实PCA有个特性是：它保留了原来在高维空间中的距离(在高维空间的距离是远的，那么在低维空间中的距离也是远的，在高维空间的距离是近的，那么在低维空间中的距离也是近的)

PCA有几率的版本，叫做probability PCA。PCA有非线性的版本，叫做kernel PCA。CCA是说：你有两种不同的source，这时候你想要用CCA。假如说你要做语音辨识，两个source(一个是声音讯号，有那个人嘴巴的image(可以看到这个人的唇形，可以读他的唇语))把这两种不同的source都做dimension reduction，那这个就是CCA。



