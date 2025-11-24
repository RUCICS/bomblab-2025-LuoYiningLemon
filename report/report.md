# bomblab 报告

姓名：罗翌宁

学号：2024201573

| 总分 | phase_1 | phase_2 | phase_3 | phase_4 | phase_5 | phase_6 | secret_phase |
| --------- | ------------- | ------------- | ------------- | ----------------- |-----------|-----------|-----------|
| 3        | 1            | 1            | 1            | 0 |0  |0  |0  |


scoreboard 截图：

![image](./imgs/image.png)

<!-- TODO: 用一个scoreboard的截图，本地图片，放到 imgs 文件夹下，不要用这个 github，pandoc 解析可能有问题 -->

## 解题报告

<!-- 对你拆掉的每个phase进行分析，并写出你得出答案的历程 -->

<!-- 如果能用伪代码还原题目源代码最佳（不属于先前提到的大段代码），语言描述自己的分析也可，每道题目的图片不建议超过两张 -->

### phase_1

```c
A fading Light awaits two in a closed world of forgotten Conflict.
```

讲解题目思路

通过读汇编我们不难发现，
```c
0000000000001435 <phase_1>:
  1435: sub    $0x8,%rsp
  1439: lea    0x1d40(%rip),%rsi    # 3180  
  1440: call   strings_not_equal
  1445: test   %eax,%eax
  1447: jne    144e   
  1449: add    $0x8,%rsp
  144d: ret         
  144e: call   explode_bomb
  1453: jmp    1449
```

这里进行的是将0x1d40＋%rip传入%rsi，并传入strings_not_equal与我们输入的字符串进行对比，所以我们只需要找到0x1d40＋%rip处存储的字符串就可以了

运行时rip会指向 1440: call   strings_not_equal，在Layout里面可以看到这一行的地址是0x0x55555555440

所以最后传入strings_not_equal的地址就是0x55555555440+0x1d40=0x555555557180

所以我们直接在运行时用x/s0x555555557180这个地址存储的字符串就可以找到答案

### phase_2

```c
// 附上题目答案
```

讲解题目思路

首先来看一下这个题目要求的输入：    
```c    
    1464:	48 89 44 24 28       	mov    %rax,0x28(%rsp)
    1469:	31 c0                	xor    %eax,%eax
    146b:	48 89 e2             	mov    %rsp,%rdx
    146e:	48 8d 4c 24 04       	lea    0x4(%rsp),%rcx
    1473:	4c 8d 4c 24 0c       	lea    0xc(%rsp),%r9
    1478:	4c 8d 44 24 08       	lea    0x8(%rsp),%r8
    147d:	48 8d 35 6d 21 00 00 	lea    0x216d(%rip),%rsi        # 35f1 <array.0+0x391>
    1484:	e8 c7 fc ff ff       	call   1150 <__isoc99_sscanf@plt>
    1489:	83 f8 04             	cmp    $0x4,%eax
```

这里调用了__isoc99_sscanf@plt，上面的上面的4行说明我们要把函数的参数分别输入到%rsp，0x4(%rsp)，0x8(%rsp)，0xc(%rsp)，而下面的判断sscanf函数返回值是否等于4也说明了我们理论上应该需要输入4个整数


0x55555555a140
0x55555555a120
### ......

## 反馈/收获/感悟/总结

<!-- 这一节，你可以简单描述你在这个 lab 上花费的时间/你认为的难度/你认为不合理的地方/你认为有趣的地方 -->

<!-- 或者是收获/感悟/总结 -->

<!-- 200 字以内，可以不写 -->

## 参考的重要资料

<!-- 有哪些文章/论文/PPT/课本对你的实现有重要启发或者帮助，或者是你直接引用了某个方法 -->

<!-- 请附上文章标题和可访问的网页路径 -->
