问题分析的方法
===
最近在工作中遇到一个用php开源netconf客户端(lamoni/netconf)连接netopeer2执行命令失败的问题。<br/>
测试代码就是开源代码给的示例代码，然而并没有用。
通过3天时间的分析，解决了问题。下面对解决问题的方法/原则做一些总结。

1.开始之前，需要整理思路：分析问题的方法有哪些，使用的先后顺序
---
在分析之前需要理下思路，有哪些方法可以用来分析，有没有先后顺序。<br/>
这时候建议按摩菲定理来把最有可能的方法排在前面。<br/>
切忌想到一个方法就开始分析，失败后又马上换另一个刚想到的方法。没有条理，效率很低。<br/>
之前的同事花了9天的时间来尝试解决这个问题，而且还没成功，可能有这样的原因。<br/>

2.重视错误提示
---
命令失败的时候有一个错误提示：xml命名空间错误。<br/>
然而netopeer2的log不太好，不知道这个错误报在哪个命令上。最初一直以为是最后的<get-config>命令有问题。<br/>
可是把该命令改来改去，仍然不能成功。后来通过修改netopeer2源代码，强行打印出了错误的命令。<br/>
结果发现是前面ssh连接建立后\<hello\>命令的命名空间有问题，并最终解决了问题。<br/>
在大多数情况下，如果有明确的错误提示，按错误提示进行分析，应该能够最快解决问题。
  
3.静下心，深入下去
---
一开始尝试不同的方法，而没有查看netopeer2的源代码，浪费了不少时间。<br/>
所以需要深入分析的时候，还是应该踏踏实实的深入下去。

4.概念和技术细节需要学习
---
在测试过程中，对命令的分隔符很疑惑。一会是]]>]]>，一会是#<数字>和##。<br/>
在这个上面测试的时间花得比较多，然而都比较浪费。<br/>
最后通过读netconf标准才知道,前者是1.0的标准，后者是1.1的标准。<br/>
客户端可以和服务器通过能力命令协商使用。<br/>
如果先把这个概念搞清楚，应该也不会浪费那么多时间了。

5.进行回顾
---
问题解决后，进行回顾。对分析方法进行总结，以提高下一次解决问题的效率。

总结
----
开始分析之前，整理分析的方法并排先后顺序。<br/>
如有明确错误提示，进行深入的分析。<br/>
需要深入和学习的时候，踏踏实实的吧。
