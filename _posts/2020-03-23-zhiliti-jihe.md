---
layout:     post
title:      面试中的智力题集合
subtitle:   面试中的智力题集合
date:       2020-03-23
author:     王鹏程
header-img: img/post-bg-ios10.jpg
catalog: true
tags:
    - 后台开发
    - 智力题
    - 面试
---





# 参考

- [六道腾讯、百度、美团常爱问的面试智力题和答案](https://blog.csdn.net/abcdefg90876/article/details/102752131?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

## 1. 赛马, 25匹马, 5个赛道, 知道每场第一的具体成绩, 至少跑多少场可以找到最快的三匹

- [面试题：赛马问题](https://blog.csdn.net/jiutianhe/article/details/40744023)
- [腾讯QQ面试题：赛马问题](https://blog.csdn.net/zjhzyzc/article/details/4743585?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)
- [一个很有意思的赛马问题](https://blog.csdn.net/Amy_mm/article/details/88733460?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)

类似***\*K路归并排序\****, 分为***\*5组\****进行比赛, 选出5个第一名, 然后进行比赛, 确定一个, 并把第一名取走, 再从第一名的队列中, 选择第二名, 放入其中, 进行比赛.依次类推, 选择***\*最优的3匹马\****.共***\*需要8次比较\****, 5次初选, 3次选择最优.



![](https://img-blog.csdnimg.cn/20190322095144928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FteV9tbQ==,size_16,color_FFFFFF,t_70)

最少10场（剩9匹的时候，选第一组的后3，第二组的前3，第三组的前2，如果第三组的第一这轮名次大于等于3，那就已经分出前四了），最多11场。