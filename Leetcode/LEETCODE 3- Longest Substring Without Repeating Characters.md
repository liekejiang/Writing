# LEETCODE 3 : Longest Substring Without Repeating Characters

题目要求找出给定的一个字符串中没有重复字符的最长子字符串，例如`abcabcbb`中的最长字串是`abc`长度为3。

我首先尝试使用之前股票价格题目的方法使用单次遍历寻找目标子字符串，但是在处理遇到重复字符时重新开始计数的问题时碰到了大麻烦，一度以为单次遍历无法解决问题。这个问题是这样的，以`dvadacbd`为例，当我遇到第二个`d`时，若想正确计算出最长无重复子字符串，则必须从开头的`v`开始计数，但是这样就必须额外使用一些数据变量存放某种可以让我们便于回望的变量。就这样我开始尝试使用动态规划。

### 动态规划
动态规划的两大必要属性是`最优子结构`和`子问题重叠`。首先我们关注子问题重叠，我们可以轻易地发现，假设长度为n的字符串中的最优解LSWRC已经确定，在长度为n+1的字符串中：
* 之前的LSWRC如果与新加入的一个字符组成新解那么`子问题重叠`成立；
* 如果新加上的字符与其他任意字符组合长度依然不超过LSWRC，`子问题重叠`成立；

需要注意的是，LSWRC可能不止一个，所以上述结论依然成立。接下来是重点`最优子结构`，我使用两个额外数据结构，一个与输入字符串等长的数组用来存放从前往后计算在当前位置为止的子字符串中的LSWRC，另一个长度为255（ASCII码等长）的字符串用于存放每种字符之前的出现位置，若在该字符串中从未出现，则设为-1。在之后的改进中发现第二个数组太蠢，占用空间和速度都不理想，转为使用hashtable存放重复字符的出现位置，并且随时更新。使用hashtable（在python为`dict()`)后运行速度有较大改进。

通过对重复字符串的举例分析，我推倒出当前位置字符的LSWRC的长度和之前字符位置处的LSWRC长度的关系, 设`LSWRC()`返回给定位置处的最长无重复子字符串的长度，`repeat()`返回该位置处字符最近一次出现时的位置：
* LSWRC(n) = min(1+LSWRC(n-1), n-LSWRC(repeat(n)))

这么一说可能有点难懂，下面进行分析。首先是最简单的理想情况，当我们的搜寻过程按顺序进行到下一位字符时，LSWRC长度很可能只是上一位处的长度+1，这意味着当前搜寻处的字符并非之前出现过的重复字符；当出现的是重复字符时，有两种情况： 一是从上次重复字符处到当前字符位置这一段子字符串内没有出现过成对的相同字符，那么对于当前字符来说，LSWRC的长度就是上次该字符出现的位置到当前字符位置的差值。 如果在这一段子字符串中间出现了别的成对的重复字符，那么很明显当前字符位置取不到理想最大值，这时候只能根据前一位（n-1）字符位置处的LSWRC+1获得。

我的算法相比于官方给出的算法要稍慢一点，虽然都是O(n)的，但是我的算法n前面的系数会更大。但是这带来了一个好处，如果题目要给出取到最大值的所有子字符串情况，我的动态规划算法可以轻松查表求出。


### 官方答案分析
首先给出的是暴力解法，思想是遍历所有可能的子字符串并且检查每个子字符串中是否有重复字符出现，这太NAIVE了。在此基础上给出了Sliding Window的解法：用两个变量存放当前搜寻子字符串的首尾位置，然后使用一个哈希表存放当前子字符串内出现过的所有单字符。在程序中不断扩展子字符串的尾位置变量，若下一位字符不出现在哈希表种，则将其纳入当前子字符串；若下一位出现在当前子字符串中，则从前往后移动首位置变量并从哈希表中删除相对应的字符直到下一位字符对应的字符从哈希表中删除。

这个算法在实现的时候并未遇到什么麻烦，一次通过。但是在实现其的改进算法的时候遇到了算法的细节方面的问题。改进算法优化了基础Sliding Window的时间复杂度, 从2n优化到1n。哈希表不再表示字符的有无而是记录字符之前的出现位置，然后每次检测到重复字符就将首位置变量移动到之前重复字符的后一位，如此循环。这就带来了以下问题：
* 若有重复字符出现在0位置，判定是否出现重复字符的`if`条件就不能使用带有返回`value`的方法，在`python`中不能使用`dict.get()`而是使用`in`和`dict.key()`。
* 首位置变量有可能出现位置倒退的情况，如果后来的字符先遇到其对应的重复字符。这时候可以使用`max()`函数。

下面给出各个方法对应的代码：
```python
# 动态规划
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        import numpy as np
        
        if len(s) == 0:
            return 0
        
        l = {ord(s[0]):0}
        output = np.zeros(len(s))+1

        for i in range(1,len(s)):
            if l.get(ord(s[i])) == None:
                output[i] = output[i-1]+1
                l[ord(s[i])] = i
            else:
                if i - l[ord(s[i])] > output[i-1] + 1:
                    output[i] = output[i-1] + 1
                else:
                    output[i] = i - l[ord(s[i])]
                
                l[ord(s[i])] = i
        
        max_output = 1
        for i in output:
            if i > max_output:
                max_output = i

        return int(max_output)
        
# Sliding Window
class Solution:
    def lengthOfLongestSubstring(self, s: 'str') -> 'int':
        substring = dict()
        str_start = 0
        str_end = 0
        max_length = 0
        while(str_end < len(s) and str_start < len(s)):
            if  substring.get(s[str_end]):
                substring.pop(s[str_start])
                str_start += 1
            else:
                substring[s[str_end]] = 1
                str_end += 1
                max_length = max(max_length, str_end - str_start)        
        return max_length
        
##  Optimized Sliding Window
class Solution:
    def lengthOfLongestSubstring(self, s: 'str') -> 'int':
        substring = dict()
        str_start = 0
        str_end = 0
        max_length = 0
        while(str_end < len(s) and str_start < len(s)):
            
            if  s[str_end] in substring.keys():
                str_start = max(substring[s[str_end]] + 1, str_start)

            substring[s[str_end]] = str_end
            str_end += 1
            max_length = max(max_length, str_end - str_start)     
            print(str_start)
                
        return max_length
    
a = Solution()
```


