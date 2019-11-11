---
layout: post
tittle: Six Coins Weighing Problem
categories: [Math]

---

[This interesting math problem is from an interview of mine, if sany suspected infringement of knowledge property,  please contact me via email.]

## Problem Description

There are six different coins with the same appearance have different weights, weighing from 1, 2, 3, 4, 5, 6 grams, and 6 tags   which have been written 1, 2, 3, 4, 5, 6 respectively, to label the weight of each coin. A careless rich man just tag all the coins randomly as he like. A balance is given, your task is , try balance twice to judge whether all the tags are labeled the weights of coins correctly or not.



```
{_$$_}________{_$__}
        /\

         ?
         
 $   $   $   $   $   $
[2] [3] [1] [4] [6] [5]
```



```haskell

Do not look the answers quickly

Think for a while

Before you check the answer


























```





## Solution

First balance, Unique 3 coins vs 1 coin：

[1+2+3]  vs [ 6]

if the result is equal, then go to second balance, otherwise the tags are not correctly labeled. 

Here 1, 2, 3 can change from one other, and 4,5 can change from each other.

Second balance:

 [6+1]  vs  [3+5]

With all the actual weights possible combinations, left side weight sum range from 7 to 9,  right side weight sum range from 5 to 8.

```latex
left_sum \in [7, 9]
right_sum \in [5, 8]
```

if true:  

​	[6+1]  < [3+5]

else:

​	 [6+1] >= [3+5]
