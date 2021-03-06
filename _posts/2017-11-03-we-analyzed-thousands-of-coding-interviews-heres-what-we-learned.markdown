---
layout:     post
title:      "**翻译**-我们对数千个编程面试的分析结果"
subtitle:   "We analyzed thousands of coding interviews. Here’s what we learned."
date:       2017-11-03 18:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - 面试
    - 技术
    - 翻译

---

原文地址：[We analyzed thousands of coding interviews. Here’s what we learned](https://medium.freecodecamp.org/we-analyzed-thousands-of-coding-interviews-heres-what-we-learned-99384b1fda50)


# 我们对数千个编程面试的分析结果

---

![](https://cdn-images-1.medium.com/max/2000/1*nJCm0Uc5BOq12faK2KR4Dw.jpeg)

**注意：我写了这个帖子中的绝大部分内容，但传说中的 [Dave Holtz](https://twitter.com/daveholtz) 完成了数据处理的主要工作。我们可以在 [他的博客](http://daveholtz.net/) 中看到他的其它成果。**

如果你正在读这个帖子，很有可能，你正打算重新进入疯狂而可怕的技术面试的世界。

也许你是一个在读或刚毕业的学生，即将首次经历面试的流程。也许你是一个有经验的软件工程师，已经有几年没考虑过参加面试了。

无论哪种情况，面试流程的第一步，通常是读一堆在线面试指南（特别是由你感兴趣的公司写的那些），并和朋友们聊起他们面试（作为面试官和面试者）的经验。

更有可能，你在面试流程的“探索性”的第一步中学到的知识，将会告诉你如何准备、如何前往下一步。

这个典型的面试准备流程有一些问题：

- 绝大多数面试指南是从一个公司的角度写的。可能 A 公司很重视高效的代码，而 B 公司更重视的是高级的问题解决能力。除非你一心想去 A 公司，否则你大概不想太侧重于他们所关心的能力。
- 人们有时会撒谎，虽然可能不是故意的。他们可能会写“我们不关心具体的编程语言”，或是“哪怕答案不正确，解释你的思路也是值得的”。但是，实际上他们未必会这样做！我们并不是说科技公司是故意误导求职者的骗子。我们只是认为：有时候不明显的偏见会偷偷产生，人们甚至没有意识到它。
- 你从朋友或认识的人那里听到的传闻并不一定有事实根据。很多人认为短的面试意味着失败。同样，每个人都可以回忆起一个很长的面试，在这面试结束后他们以为：“我成功打动了面试官，我肯定可以进入下个阶段”。过去， [我们发现人们衡量自己面试表现的能力相当差](http://blog.interviewing.io/people-are-still-bad-at-gauging-their-own-interview-performance-heres-the-data/)。现在，我们打算直接观察诸如面试时长之类的指标，看看它们是否真的重要。

在我的公司 [interviewing.io](http://interviewing.io) 中，我们用独特的数据驱动方式去分析技术面试及其结果。我们有一个平台，可以让面试者匿名练习技术面试。如果事情顺利，他们可以解锁匿名参加真实面试的功能。他们可以在任何时间参加 Uber、Lyft 和 Twitch等顶级公司的面试。

有意思的是，练习面试和真实面试都发生在 interviewing.io 生态系统内。结果，我们可以收集到相当多的面试数据，用来分析并帮助我们更好地理解技术面试：它们传递的信号，什么有用，什么没有用，以及面试的哪些方面可能真的影响结果。

每一个面试，无论是真实的还是用于练习的，开始时都有面试官和面试者，他们在一个合作式的编程环境中，有语音、文字聊天，以及一块白板。他们可以直接开始讨论技术问题。

面试问题通常属于后端开发电话面试中常见的问题。

**在这些面试中，我们收集发生的一切。包括音频、面试者写的代码的数据和元数据，面试官和面试者对面试过程和对方的评价。**

如果你好奇，你可以在下方看到面试者和面试官的反馈表格的样子 —— 除了一个直接的是/否问题以外，我们还问了不同方面的面试表现（使用 1-4 分的评分表）。

我们还问了面试者一些额外的问题，这部分问题面试官是不知道的。其中一个问题是，面试者以前有没有见过刚才的面试问题。

![](https://cdn-images-1.medium.com/max/1600/1*WG4CovbdT88jxPEqXZBuiQ.png)

面试官反馈表格

![](https://cdn-images-1.medium.com/max/1600/1*FRfeOXn8visxr36sKprDNw.png)

面试者反馈表格

### 结果

在深入探究之前，需要注意：这些结论都是基于观察数据的，这意味着我们不能声称其中有很强的因果关系。但我们仍然可以分享观察到的奇特规律，并且解释我们发现了什么，以便让你做出自己的结论。

#### 以前见过面试问题

> **“我们正在讨论的是练习！”** —— 阿伦·艾弗森

从首要的东西开始。不算很聪明的人就能发现，最好的提升面试表现的方法之一是……练习面试。现在有大量的资源帮助你练习，包括我们自己的。做练习题的主要好处之一是：你被问到没见过的问题的概率会降低。如果你已经做过一两次的话，平衡一棵二叉树就显得不那么可怕了。

我们观察了3000个左右的面试，并把面试者见过与没有见过面试问题的结果相比较。你可以在下面的图中看到结果。

![](https://cdn-images-1.medium.com/max/1600/1*0ha_0_L7WbspbayJet6N1g.png)

**不出意料，见过题目的面试者通过的概率比没有见过的多16.6%。** 这个差异是统计显著的——所有的误差条都表示95%置信区间。

#### 用什么语言编程重要吗？

> **“不爱自己母语的人比野兽和臭鱼更加低等。”** — Jose Rizal

你可能认为，使用不同的语言会使面试得到更好的结果。比如，Python 的可读性会对面试有帮助。或者，有些语言处理数据结构的方式特别干净，会让常见的面试问题变得简单。我们想看看，使用不同的语言是否会对面试结果产生显著影响。
 
我们把自己平台上的面试按照语言分组，并过滤掉了面试数量小于 5 个的语言（这只删去了少量的几个面试）。然后，我们就可以看到面试结果随语言变化的函数。

分析结果在下表中显示。任何不重叠的置信区间都表示使用不同语言的面试者通过面试的统计学差异。

虽然我们没有把所有语言逐对比较，但是下面的数据显示，总的来说，**不同语言的面试通过率没有显著的差异。**（我们的平台上还有其它的语言，但语言越没名气，我们的数据点就越少。例如，所有用 [Brainfuck](https://en.wikipedia.org/wiki/Brainfuck) 的面试都很成功。开个玩笑。）
![](https://cdn-images-1.medium.com/max/2000/1*S1-Aj4ZEKgyuihnFftCD6w.png)

我们观察到的最常见的错误之一是：人们选择自己并不熟悉的语言，然后弄错了查看数组长度、遍历数组、创建哈希表之类的基本操作。但这只是我们定性的结论，没有统计数据支持。

当面试者故意选择一种时髦的语言，以试图打动面试官的时候，这种错误对他非常不利。相信我们，选择自己熟悉的语言，比时髦但不熟悉的语言更好。每一次都是这样。

#### 哪怕语言并不重要……使用该公司选用的语言是否有优势？

> **“救命，我已经变成本地人了。”** — Margaret Blaine

总的来说，语言和面试表现并没有特别紧密的关系。这很好。但是对方公司使用的语言可能会影响面试结果。一个使用 Ruby 的公司可能会说：“我们只雇佣 Ruby 程序员，如果你使用 Python 我们就不太可能雇你。”

而在另一方面，一个完全使用 Python 的公司会对使用 Python 的面试者更苛刻——他们完全了解这种语言，可能会因为面试者对 Python 的使用不完全地道而对他有意见。

下面的表和使用不同语言的面试成功率（也是用面试官愿意雇用面试者的概率来表示）的表很相似。但是，这个表是用面试语言是否在公司的技术栈内来分类的。

我们把这个分析限制在 C++, Java 和 Python ，因为这三种语言都有很多公司用和不用它们。**结果并不一致。对于 Python 和 C++而言，面试者使用的语言是否在公司的技术栈内，并不会对成功率产生显著的影响。但是，使用 Java 的面试者在使用 Java 的公司面试时，更有可能成功**(p=0.037)。

那么，为什么公司使用的语言是 Java 时，使用对方公司的语言会有帮助，而 Python 和 C++则没有呢？一个可能的解释是特定编程语言的社区（例如 Java）更看重程序员在该种语言上的工作经验。也有可能是因为使用 Java 的公司的面试官更有可能问出熟悉 Java 的人能回答得更好的问题。

![](https://cdn-images-1.medium.com/max/2000/1*scSrZGC6Zy9a_ij1S0kZsg.png)

#### 你使用什么语言，和别人眼中你的沟通能力有关吗？

> **“精巧地使用一门语言就像使用巫术一样。”** — Charles Baudelaire

虽然语言选择对总体表现的影响不那么大（使用 Java 语言的公司除外），我们很好奇，选择不同的语言是否在其他维度上影响面试结果。

例如，Python 之类非常易读的语言，可能导致面试者能够更好地交流。另外，C++ 之类底层的语言可能使面试者在技术能力上的评分更高。

另外，非常易读或者非常底层的语言，可能使得这两个分数相关（比如，也许有一个 C++ 候选人不能解释清楚自己在做什么，但是写的代码效率很高）。下面的表显示，面试者的技术能力和沟通能力的评分并没有可见的差异，对于各种语言都是如此。

![](https://cdn-images-1.medium.com/max/1600/1*Cin1yM1gw62D2Gl1fhdG-w.png)

**另外，无论如何，技术能力似乎和沟通能力紧密相关——不管什么语言，技术表现很好的面试者沟通能力不好，是很罕见的，反之亦然。**, 这很大程度上拆穿了工程师往往笨拙、语无伦次的谣言。

（我见过的最好的工程师都很擅长分解复杂的概念，并把它们向外行解释。为什么总有人认为优秀的程序员不擅社交？我完全想不通。）

#### 面试时长

> **“年轻的时候搞砸各种事情是没有关系的；你的恢复能力还很强。”** — Harold Prince

我们都经历过结束一场面试时，感觉自己表现很糟糕的情况。 通常，这种发挥不佳的感觉是出于我们自己发现，或者道听途说的经验法则。我们可能发现自己在想：“面试持续的时间不长？这很可能不是个好消息……”或者“我在面试中几乎什么都没有写！我肯定过不了。”使用自己的数据，我们试图研究这些衡量面试表现的经验法则是否有用。

首先，我们观察了面试的时间长度。面试时间短是否意味着面试者表现非常糟糕，面试官只能提早结束？或者，可能面试官不太有时间，或者他很快发现你是一个特别优秀的候选人？下图显示了成功与失败的候选人的面试时长（以分钟计）。

**从表格上我们很快可以看到：成功和失败的面试在时长上并没有差异——成功面试的平均时长是51.00分钟，而失败面试的平均长度是49.95分钟。差异是不显著的**。

（对于本帖子中的每一个比较，我们用 Fisher-Pitman 置换检验来比较平均值的差异。）

![](https://cdn-images-1.medium.com/max/1600/1*kUsYEVIdbSKNWH5Ea-ks_w.png)

#### 代码量

> **“简洁是智慧的灵魂。”** —— 威廉·莎士比亚

你可能经历过完全失败的面试。面试官问你一个你几乎不理解的问题，你问他“二分查找什么？”，并且在整个过程中几乎没有写任何代码。你可能希望纯粹通过聪明、魅力或者高级的问题解决能力通过这个面试。为了检验这种说法是否正确，我们观察了面试者所写代码的长度。下图展示了成功和失败的面试者所写代码的长度。从中很快可以发现，这两者还是很有差别的——失败的面试代码量更少。有两个现象可能导致这个问题。首先，不成功的面试者可能一开始写的代码就比较少。另外，他们可能更倾向于删除很多自己写出的失败的代码。

![](https://cdn-images-1.medium.com/max/2000/1*OyxyeBmyDfMdJaYyCDi6ng.png)

**成功的面试最终的代码平均有 2045 个字符，而不成功的平均只有 1760 个字符。** 这是很大的区别！这个发现是统计显著的，而且很可能不那么令人吃惊。

#### 代码模块化

> **“成熟程序员的标志是，愿意抛弃自己花时间写的代码，如果它没有意义的话。”** — Bram Cohen

除了看看你写了 *多少* 代码以外，我们也可以考虑一下代码的类型。传统的观点是好的程序员不用回收代码——他们写出模块化的代码并不断复用。我们希望知道在面试过程中，有哪些行为是受到鼓励的。我们看了用 Python 进行的面试[5](http://blog.interviewing.io/#guide-fn5)，并且数了最终的版本中代码定义了多少函数。我们想知道，成功的面试者是否定义了更多函数——更多的函数并不是模块化的定义，但根据我们的经验，这是一个标志模块化程度的很强的信号。同样，我们不可能断言其中存在很强的因果联系——也许有的面试官问的问题本身就会导致面试者写出更多或更少的函数。不管怎样，这是一个值得研究的趋势。

下面的图展示了面试官愿意和不愿意雇佣的面试者的 Python 函数数量的对比。很快可以发现，成功和失败的面试在这方面是*有*差别的。成功的面试者会写出更*多*的函数。

![](https://cdn-images-1.medium.com/max/2000/1*tJ71vF6YBjv-fq489afxSg.png)

**就平均水平而言，成功的 Python 面试者定义 3.29 个函数，而不成功的定义了 2.71 个。这个差异是统计显著的。结果是，面试官确实会对写出他们期望的代码的面试者有所奖励。**

#### 你的代码是否运行重要吗？

> **“要快速行动，不要害怕弄坏东西。如果你什么都没有弄坏，你就做得还不够快。”** — 马克·扎克伯格

> **“最强大的debug工具仍然是缜密的思维，以及准确安放的print语句。”*** — Brian Kernighan

对于技术面试，常见的观点是面试官并不真的在乎你的代码是否能够运行——他们关心的是解决问题的技能。我们收集了面试者写的代码是否运行，以及是否能够编译的数据，希望看看我们的数据中是否有这方面的证据。成功与不成功的面试的代码中，含有错误的概率是否有差异？另外，如果面试者犯了很多语法错误，他是否还可以被雇佣？

为了回答这些问题，我们查看了数据。我们把数据限制到超过 10 分钟，并且有超过5份代码被执行的面试。这过滤掉了面试官不希望面试者真的运行代码，或者由于某种原因提前终止的面试。接下来，我们测量了出现错误的代码的比例。[5](http://blog.interviewing.io/#guide-fn5) 当然，这种方式有它的局限性——比如，候选人可能写出能够编译，但是答案稍有不正确的代码。他们也可能得到正确的答案，却把它写到 stderr！虽然如此，这可以帮助我们感觉到它是否有差异。

下面的表给出了数据的一个概要。X 轴显示了所有执行次数中没有错误的代码的比例。所以，如果代码执行了 3 次，但只有 1 条错误信息，就算在 “30% - 40%” 一栏中。Y 轴显示所有面试中，成功和失败的面试处于该区域的比例。看一看下面的表就会发现，平均情况下成功的面试者会写出更多的没有错误的代码。但这种差异是否是显著的呢？

![](https://cdn-images-1.medium.com/max/2000/1*434O4qWrzxlU6YltbN6sIw.png)

就平均水平而言，成功的面试者的代码在 64% 的情况下运行成功（没有产生错误），而不成功的候选人的代码在 60% 的情况下可以成功运行，这个差异当然是显著的。 **同样，我们不能声称有任何因果联系。我们主要学到的是，成功的候选人通常写出的代码能够运行得更好，无论面试官在面试开始时告诉你什么。**

#### 在开始写代码之前是否应该等待一会，整理思路？

> **“不要忘记沉默的力量，不断出现的扰乱人思路的暂停，可能会使你的对手非常紧张。”** — Lance Morrow

我们也很好奇，成功的面试者在过程中是否会放慢节奏。面试问题通常是很复杂的！看到一个问题以后，后退一步去想一个完整的计划，可能比直接跳进去要好。为了验证这种观点是否正确，我们测量了候选人在面试中第一次运行代码的时间。下面是一个直方图，展示了成功和失败的面试者在开始面试之后，第一次运行代码的时间。很快地看一眼，你就可以发现，成功的候选人在开始运行代码之前，会等待得稍微久一点，虽然差别不是很大。

![](https://cdn-images-1.medium.com/max/2000/1*I0npBvlvkI3JVWr5Toi1_g.png)

更准确地说，**就平均水平而言，成功的候选人在整个面试过程的 27% 处第一次运行代码，而不成功的候选人在 23.9% 处首次运行，这个差异是显著的**。当然，这种现象还有其它的解释。例如，也许成功的候选人更擅长把时间花在拍面试官的马屁上。而且，像平常一样，我们也不能声称其中有因果关系——如果你在面试过程中花5分钟时间坐着，什么也不说，这将对你没有什么帮助。但是，这样做会对第一次运行代码的时间产生影响。

### 结论

总而言之，我们试图探究是什么使得面试官说：“你知道吗，我真的想雇佣这个人。”。这个帖子是我们的首次尝试。由于所有的数据都是观察数据，声称其中有因果联系是很困难的。

成功的面试者可能表现出特定的行为，但模仿这些行为并不保证你能成功。但是，这使我们有证据支持（或反对）你在网上看到的关于面试的许多建议。 

除此之外，还有很多事情可以做。这是第一个对我们的数据（在很多层面上，它蕴含着关于面试的宝贵信息）的定量分析。接下来，我们打算做一个更深层、定性的研究，并开始把不同的问题分类，看看哪一类问题含有重要的信号。我们也将分析用户的二级行为，这些行为不是把样本代码做一个回归分析，看看面试时长就可以测量的。

如果你想帮助我们，并想要听一些技术面试，[写信给我](mailto:aline@interviewing.io)！

### END

***

朱渊博，特普通一人，渊为深，博为广，一直努力做一个有深度有广度的人。多混迹于Github, Stackoverflow, ITeye, 知乎, Linkedin 等地带。蹉跎中练就了一身码农好本领。 酷爱编程，但绝不宅。爱生活，爱思考，性情温和但充满激情，做事有主见，信奉读万卷书行万里路。
<http://www.freerambo.com/about>




