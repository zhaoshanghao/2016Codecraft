# 华为codecraft算法大赛源码 #

大赛官网：**[华为codecraft算法大赛](http://codecraft.huawei.com/home/detail)**  
赛题思路：**[华为codecraft算法大赛---寻路](http://blog.csdn.net/sb19931201/article/details/52496925)**

## 代码说明 ##

- **codecraft.rar**是按官方要求提交的代码以及生成的二进制程序。
- Linux下编译脚本在**HUAWEI Code Craft 2016 初赛赛题包.zip**中。
- 为了方便调试，Windows环境下C代码在**hellow.cpp**中。

## 比赛情况 ##
比赛前中期还保持不错的名次，一直维持在二十名左右以为稳稳三十二强了，结果后期突破不大，最后几天呼呼的被超过，真是太天真了。后来回想确实方法思路比较死板，一口咬定DFS+剪枝，没有去尝试新的算法如A*，模拟退火算法等，还有大赛群里分享说的动态规划、数模等。

## 赛题介绍 ##
1 问题定义  
给定一个带权重的有向图G=(V,E)，V为顶点集，E为有向边集，每一条有向边均有一个权重。对于给定的顶点s、t，以及V的子集V'，寻找从s到t的不成环有向路径P，使得P经过V'中所有的顶点(对经过V'中节点的顺序不做要求)。
若不存在这样的有向路径P，则输出无解，程序运行时间越短，则视为结果越优；若存在这样的有向路径P，则输出所得到的路径，路径的权重越小，则视为结果越优，在输出路径权重一样的前提下，程序运行时间越短，则视为结果越优。  
说明：  
1）图中所有权重均为[1，20]内的整数；  
2）任一有向边的起点不等于终点；  
3）连接顶点A至顶点B的有向边可能超过一条，其权重可能一样，也可能不一样；  
4）该有向图的顶点不会超过600个，每个顶点出度(以该点为起点的有向边的数量)不超过8；  
5）V'中元素个数不超过50；  
6）从s到t的不成环有向路径P是指，P为由一系列有向边组成的从s至t的有向连通路径，且不允许重复经过任一节点；  
7）路径的权重是指所有组成该路径的所有有向边的权重之和。

2 输入与输出  
输入文件格式  
以两个.csv 文件(csv 是以逗号为分隔符的文本文件)给出输入数据，一个为图的数据(G)，一个为需要计算的路径信息(s,t,V')。文件每行以换行符(ASCII'\n'即0x0a)为结尾。

1）图的数据中，每一行包含如下的信息：  
LinkID,SourceID,DestinationID,Cost  
其中，LinkID 为该有向边的索引，SourceID 为该有向边的起始顶点的索引，DestinationID为该有向边的终止顶点的索引，Cost 为该有向边的权重。顶点与有向边的索引均从0 开始 编号(不一定连续，但用例保证索引不重复)。  
2）路径信息中，只有一行如下数据：  
SourceID,DestinationID,IncludingSet  
其中，SourceID 为该路径的起点，DestinationID 为该路径的终点，IncludingSet 表示必须经过的顶点集合V'，其中不同的顶点索引之间用'|'分割。  
输出文件格式  
输出文件同样为一个.csv 文件。  
1）如果该测试用例存在满足要求的有向路径P，则按P 经过的有向边顺序，依次输出有向边的索引，索引之间用'|'分割；
2）如果该测试用例不存在满足要求的有向路径P，则输出两个字符NA；
3）只允许输出最多一条有向路径。

3.简单用例说明
![](http://i.imgur.com/inL8uIW.png)

## 比赛思路及总结 ##
看完题目我当时第一反应就是最短路径算法，但是直接套用过来是肯定不行的，因为最短路径算法(dijkstra)是一层一层往外扩，然后利用贪心算法的思想得到每一层级的最优路径。二本赛题加了必过点的限制，因此需要对算法进行改造，为了能得到赛题结果，在一步步改造的过程中发现已经抛弃了贪心算法思想，最后渐渐的升级深度优先遍历(DFS)的基本思想，但是记录每一步遍历的顺序，方便回溯(在找到可行解的基础上还要考虑最优解)。随着比赛的进行，赛题数据复杂度的增加，回溯版DFS不能在有效的时间内得到最优解，甚至不能得到一个可行解。因此我又在该基础上增加了"剪枝"的思想,就是剪断一些出度较大的节点的路径，这样可以降低复杂度，并且能得到不错的结果，“剪枝”的尺度与位置其实是随机(缺乏科学性)的，由于是比赛，所以可以通过评分程序去修改算法拟合赛题数据。也是因为这种小聪明导致我这次比赛前中期效果不错，后期却非常乏力。这是应该吸取的教训，还是应该从算法本身的有效性入手，那样才会有真正的提高。

