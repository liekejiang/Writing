# LEETCODE 746: Min Cost Climbing Stairs

标签（空格分隔）： ALGORITHM LEETCODE 动态规划


---
[Leetcode 题目地址](https://leetcode.com/problems/min-cost-climbing-stairs/description/)

题目分析
--------
  题目给我们一个一维数组，代表一个梯子登上每格对应的花费（理解成长度？），假设我们一次可以登上一或者二格梯子，只需要花费第一格梯子的代价。我们可以从梯子的第一格或者第二格出发，到达倒数第二格或者最后一格即认为到达顶端。
  接下来介绍两种动态规划方法，第一种是我自己想出来的不成熟的动态规划方法，第二种是官方答案。

不成熟的动态规划方法
------------
```python
class Solution:
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        L = len(cost)
        TYPE = [0 for row in range(L)]
        Value = [0 for row in range(L)]

        if L == 0 or L == []:
            return 0
        elif L == 1:
            return cost[0]
        elif L == 2:
            return min(cost)
        elif L == 3:
            return min([cost[1],cost[0]+cost[2]])

        #case L = 3
        if cost[1] < cost[0] + cost[2]:
            TYPE[2] = 2
            Value[2] = cost[1]
        else:
            TYPE[2] = 1
            Value[2] = cost[0] + cost[2]
        
        #case L = 4
        if cost[3]+cost[1] <= cost[1]+cost[2] and cost[3]+cost[1] <= cost[2]+cost[0]:
            TYPE[3] = 1
            Value[3] = cost[3]+cost[1]
        elif cost[1]+cost[2] <= cost[0]+cost[2] and cost[1]+cost[2] <= cost[1]+cost[3]:
            TYPE[3] = 2
            Value[3] = cost[1]+cost[2]
        else:
            TYPE[3] = 2
            Value[3] = cost[0]+cost[2]
        
        for i in range(4,L):
            if TYPE[i-2] == 2 and TYPE[i-1] == 1:
                TYPE[i] = 2
                Value[i] = Value[i-1]
            elif TYPE[i-2] == 2 and TYPE[i-1] == 2:
                if cost[i] > cost[i-1]:
                    if Value[i-1] + cost[i-1] > Value[i-2] + cost[i-1]:
                        TYPE[i] = 2
                        Value[i] = Value[i-2] + cost[i-1]
                    else:
                        TYPE[i] = 2
                        Value[i] = Value[i-1] + cost[i-1]                        
                else:
                    if Value[i-1] + cost[i] > Value[i-2] + cost[i-1]:
                        TYPE[i] = 2
                        Value[i] = Value[i-2] + cost[i-1]
                    else:
                        TYPE[i] = 1
                        Value[i] = Value[i-1] + cost[i]  
            elif TYPE[i-2] == 1 and TYPE[i-1] == 2:
                if cost[i] > cost[i-1]:
                    TYPE[i] = 2
                    Value[i] = Value[i-1] + cost[i-1]
                else:
                    TYPE[i] = 1
                    Value[i] = Value[i-1] + cost[i]

        return Value[-1]
```

此方法的主要思想是，从第一格到第i格的花费是基于从第一格到第i-1以及第i-2格花费的。即我们可以在前两格的最优解基础上构建当前最优解。然而官方答案也是如此，而且更加简洁快速。我的方法需要记录当前最优解是在最后一格退出还是倒数第二格退出，以便后面格方便构建最优解。
最优解的构建过程如下：先判断前两个最优解是如果构建的，一共三种情况：①第$(i-1)$与$(i-2)$的解都是在倒数第二格退出，②第$(i-1)$与$(i-2)$的解分别在倒数第一格和倒数第二格退出，③第$(i-1)$与$(i-2)$的解分别在倒数第二格和倒数第一格退出。 还有一种情况在$i \geq 5$时不存在， 因此三格和四格的情况需要手动确认作为后续计算的基础。

①第一种情况十分复杂，有好几种可能，没有固定的规律，需要遍历所有解。
②第二种情况只需要关注第$(i-1)$格的解，因为$(i-2)$格的解被包括在内。而第$(i)$格的解可以直接继承第$(i-1)$格的解，以倒数第二格退出。
③第三种情况第$(i-1)$与$(i-2)$的解是一致的，我们需要判断倒数第一和第二格的代价，然后取较小的那个加上第$(i-1)的解。

具体的公式推倒省略。

官方动态规划方法
------------
```python
class Solution:
    def minCostClimbingStairs(self, cost):
        """
        :type cost: List[int]
        :rtype: int
        """
        L = len(cost)
        C1 = 0
        C2 = 0
        
        for i in range(L):
            temp = C1
            C1 = min(C1,C2)+cost[i]
            C2 = temp
            
            
        return min(C1,C2)
```
使用官方提示写出来的自底向上的动态规划算法，简练而快速。C1和C2可以看作是在梯子开头加上两节花费为0的格子，使得不用处理长度为1和2的特殊情况。
最优子结构为：$当前解 = min(前一个格子的最优解,min(前一个格子，前两个格子)+当前格子花费)$
官方的解法固定了起始位置，因此只要判断在当前格子退出的最优解以及在倒数第二格格子退出（也即前一个最优解）中哪个小即可。前一个格子退出的最优解在轮到当前格子时已经解出，在算法中由C2指代，因此我们只需要关注如何计算当前格子退出的最优解。
当前格子退出的最优解计算如下，设当前是第$i$格，那么我们已知第$i-1$格与$i-2$格的最优解，选择其中小的那个加上第$i$格的花费即可。

总结
------------
虽然上述两种方法都是one-pass，但是细节上差距甚远，这点也直接体现在运行速度上，前者只比50%的算法快而后者击败了86%的算法。第一个算法的子结构划分不够简单，子结构为第$i$格的最优解，子结构的高集成度反而导致了组合子结构的困难，而第二个算法使用了更低级的计算结果，反而在组合的时候更加简单。