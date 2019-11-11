---
layout: post
tittle: Six Coins Weighing Problem
categories: [Math]
---

## Problem Description

There are six different coins with the same appearance have different weights, weighing from 1, 2, 3, 4, 5, 6 grams, and 6 tags   which have been written 1, 2, 3, 4, 5, 6 respectively, to label the weight of each coin. A careless rich man just tag all the coins randomly as he like. A balance is given, your task is , try balance twice to judge whether all the tags are labeled the weights of coins correctly or not.

```
{_$$_}________{_$__}
        /\

         ?
         
 $   $   $   $   $   $
[2] [3] [1] [4] [6] [5]
```







## Solution

Unique 3 vs 1：
1+2+3 = 6

Here 1, 2, 3 can change from one other, and 4,5 can change from each other.
6+1 vs 3+5
[7, 9]   vs [5, 8] 
if true 6+1 < 3+5
else 6+1 >= 3+5