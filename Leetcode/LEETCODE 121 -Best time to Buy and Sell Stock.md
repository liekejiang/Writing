# LEETCODE 121 :Best time to Buy and Sell Stock

标签（空格分隔）： LEETCODE ALGORITHM

---
[Leetcode 题目地址](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)

题目分析
--------
  我们要使用算法计算出如何买卖股票收益最大化，输入是一个list或称一维数组，表示股票在这几天内的价格，而不是价格的变化，这点要注意。
  接下来是要注意的要点，首先我们不能在买入一只股票之前将其卖出，因此我们必须先固定买入日再决定卖出日。接下来我讲介绍3种python的解决方案，分别使用分治和一个最简单的方法。最后分享一个我使用的不合适方法，~~盲目使用动态规划结果并不完全符合题目要求~~。

Divide and Conquer 方法
------------
```python
def MaxProfit(prices):
    if prices == []:
        return 0
    if len(prices) == 1:
        return prices[0]
    
    changing = [0 for col in range(len(prices)-1)]
    for i in range(1,len(prices)):
        changing[i-1] = prices[i] - prices[i-1]
    
    low = 0
    high = len(changing) - 1
    low,high,SUM = find_maximum_subarray(changing,low,high)
    
    if SUM < 0:
        return 0
    else:
        return SUM
    
def find_maximum_subarray(changing,low,high):
    if high == low:
        return(low, high, changing[low])
    else:
        mid = math.floor((low+high)/2)
        (left_low, left_high, left_sum) = find_maximum_subarray(changing,low,mid)
        (right_low,right_high,right_sum) = find_maximum_subarray(changing,mid+1,high)
        (cross_low,cross_high,cross_sum) = find_max_crossing_subarray(changing,low,mid,high)
    
    if left_sum >=right_sum and left_sum >= cross_sum:
        return(left_low, left_high, left_sum)
    elif right_sum >= left_sum and right_sum >= cross_sum:
        return(right_low,right_high,right_sum)
    else:
        return(cross_low,cross_high, cross_sum)
    
def find_max_crossing_subarray(prices, low, mid, high):
    left_sum = 0
    all_sum = 0
    max_left = 0
    for i in range(mid,low-1,-1):
        all_sum += prices[i]
        if all_sum > left_sum:
            left_sum = all_sum
            max_left = i

    right_sum = 0
    all_sum = 0
    max_right = 0
    for j in range(mid+1,high+1):
        all_sum += prices[j]
        if all_sum> right_sum:
            right_sum = all_sum
            max_right = j
    return(max_left, max_right,left_sum+right_sum)
    
```
本方法通关了leetcode的时间限制检测，但是排在90%的地方，即使是$O(nlgn)$的时间复杂度，对该题来说也是不合格的。本方法参考了算法导论第四章分治策略的最大子数组问题的解决方法，我们递归地求解目标解，每层递归需要进行分解，计算，合并三个步骤。

MaxProfit（)作为主函数，首先要求向量prices的差分，得到股票每日相较于前一天价值的变化向量，股票最大收益就是从变化向量里截取一段子向量，使得该子向量和比其他任何子向量之和都大。随后调用find_maximum_subarray（）。主要思想是以变化向量的中点为界，认为最大子数组由自中点向前与向后的两组子向量组成，即最大子数组必定是以下三种情况的一种：在向量的前半部分，在向量的后半部分，横跨中点。因此可以写出递归式，判断当前输入向量的前半部分最大子数组之和，后半部分之和与横跨中点的最大子数组之和。前后半部分的最大子数组之和继续调用find_maximum_subarray（），横跨中点的最大子数组之和由find_max_crossing_subarray（）求得，所需时间为$O(len(input vecor))$。 

find_maximum_subarray（）会持续递归直到输入向量单位为1，此时直接返回该单个元素值。有了第一个递归返回值，递归栈里的函数便可以回溯计算，直到给出最终的最大子数组之和。

然而还有比这个答案快的多的方法。

One-Pass 方法
------------
```python
def maxProfit(prices):

    if prices == []:
        return 0
    if len(prices) == 1:
        return 0
    
    L = len(prices)
    peak = prices[0]
    valley = prices[0]
    profit = 0
    
    for i in range(1,L):
        peak = prices[i]
        print(peak-valley)
        
        if peak - valley > profit:
            profit = peak - valley
            
        if prices[i] < valley:
            valley = prices[i]
            
    return profit
```
这是官方给出的$O(n)$的算法，在遍历给定的价格向量时需要记录尖峰（最大值）与低谷（最小值），其最大差值即为我们想要的最大收益。在计算差值的时候需要注意时间顺序，由于低谷的出现位置不晚先于尖峰，因此在更新过低谷值之后必须更新尖峰才能计算差值。

为了简便，我们把遍历的每个值都设为尖峰然后计算尖峰与当前低谷的差，若新差值大于当前差值，更新收益。 我们选择在单次遍历的最后检测是否更新低谷值，这样便不会干扰差值的计算。如果在单次循环的末尾更新了低谷值，在下次循环的开头会更新尖峰值，因此并不会有时序上的问题。


动态规划方法
------------
```python
    prices = [7,1,5,3,6,4]
    n = len(prices)
    index = [[0 for row in range(n)] for col in range(n)]
    Profit = [[0 for row in range(n)] for col in range(n)]
    for i in range(n):
        P = 0
        index[i][i] = i
        for j in range(i+1,n):

            if P < ((prices[j] - prices[index[i][j-1]]) + (prices[index[i][j-1]])-prices[i]): 
                ##difference + last value
                P = ((prices[j] - prices[index[i][j-1]]) + (prices[index[i][j-1]])-prices[i])
                Profit[i][j] = P
                index[i][j] = j
            else:
                Profit[i][j] = P
                index[i][j] = index[i][j-1]
            print(i,j,'and Profit = ',P)
        
    print('Best profit is',P)
```
首先给出结论，该方法计算时间为$O（n^2)$,如果单纯为了满足题目要求，和暴力计算没有差别，并不适合该题的求解。如果题目为给定一段时间内的股票价格，要你求出所有组合情况的最佳收益值，那么使用此方法最快。

这里使用基于填表的自底向上的动态规划方法。在代码中维护两个矩阵数组index和Profit。index用于记录买入卖出日的关系，row代表买入日，col代表卖出日。Profit记录对应的买入卖出日的收益。3，4行是在不使用numpy包的情况下全零初始化一个6*6矩阵的方法。
在接下来的外层for循环中，首先清零临时收益变量P，再对矩阵index的第i行第i列赋值i。i从0开始，代表for循环的当前次数，实际意义为买入日。进入内层循环，内层循环代表卖出日，需要计算收益，收益的计算方式为：$收益 = (卖出日价格 - 前一日价格)+(前一日价格 - 买入日价格) = 当日收益增值 + 以一日最佳收益$

如果P的值小于当前收益，P会被更新，Profit里会记录当前买入卖出日的最大化收益，index会记录当前买入卖出日范围内的收益最大卖出日。如果P的值大于当前收益，代表当前买入卖出日并非最优，Profit记录当前P值，即在当前买入卖出日范围内的最大化收益值（卖出日并非当前日），index则会记录前一日的买入卖出日范围内的最大化收益卖出日。如果前一日取得最大化收益，那么该次记录正确，如果前一日并没有取得最大化收益，那么前一日记录的是大前提的最大化收益卖出日，以此类推，从当前最大化收益日一直延续下去。

使用算法导论上的结构来分析该动态规划问题，为了求解规模为n的原问题，我们先求解形式完全一致但是规模更小的子问题。即循环从第一天买入第一天卖出开始的原因，并且记录获取当前最佳收益买入卖出天的序号。接下来我们固定买入日，改变卖出日，即可在前一天的基础上判断收益是否增加。如果收益增加，当前买入卖出日组合对应的矩阵值便会更新，否则使用前一天的最佳日，在index矩阵里记录前一日的买入卖出日组合。

之所以使用这个方法与暴力求解相差无几，是因为一共只有$O(n^2)$种情况，与动态规划分析子问题需要的情况一致，如果改为钢板切割问题，则会有$O(2^n)$种情况。






