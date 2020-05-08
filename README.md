# How2pwn
## Author: Wenhuo

### - github仅作为仓库使用，个人新博客已迁移至：https://fandazh.cn 。老域名已关闭网站，CTF系列可能不会再更新了，现在主要更新how2CVE。
&nbsp;&nbsp;&nbsp;&nbsp;我个人做过的CTF的bin题收集，有逆向和pwn两个方向。最近主要研究pwn，初学pwn的人建议移步先看看我写的[malloc原理解析](https://github.com/fangdada/ctf/tree/master/how2pwn/MALLOC)，我博客里也有而且博客里至少比github好看2333，下面的题目重复的话说明都涉及了，而且题目放上去的不按难度顺序，所以自行看看难度做吧，题目目录下有个**文件夹how2pwn**，这里面都是讲解要做出这道题目的**前置知识**以及相应的**demo**实现，也就是推荐新手先看的（不是全都有，只有最近复现的题目有，嘻嘻），这样学起来应该比现在清晰一些。那么导航就放在下面了：

## ret2dlresolve

> 目前栈题比起以前少很多了，ret2dlresolve基本可以说是栈题的巅峰了，这一技巧虽然刚开始理解起来难一些，但是一次掌握之后再做就很简单了；

</br>

- [XDCTF2015 pwn200](https://github.com/fangdada/ctf/tree/master/XDCTF2015/pwn200)
- [0CTF2018 babystack](https://github.com/fangdada/ctf/tree/master/0CTF2018/babystack)
- ...

</br>

## fastbin

> 同一组大小的fastbin被free后由fd指针指向前一个freed的chunk，伪造fastbin要注意绕过size check，e.g. 0x70大小的堆块，伪造的size要在0x70-0x7f的区间里。

</br>

- [LCTF2016 pwn200](https://github.com/fangdada/ctf/tree/master/how2pwn/house_of_spirit/lctf2016_pwn200)
- [QCTF2018 NoLeak](https://github.com/fangdada/ctf/tree/master/QCTF2018/NoLeak)
- [RCTF2018 RNote3](https://github.com/fangdada/ctf/tree/master/RCTF2018/RNote3)
- [RCTF2018 babyheap](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyheap)
- [RCTF2018 stringer](https://github.com/fangdada/ctf/tree/master/RCTF2018/stringer)
- [tiesan2018 littlenote](https://github.com/fangdada/ctf/tree/master/tiesan2018/littlenote)
- [tiesan2018 bookstore](https://github.com/fangdada/ctf/tree/master/tiesan2018/bookstore)
- ...

</br>

## largebin

> largebin attach相比smallbin多拥有fd_nextsize和bk_nextsize，并且largebin可以用malloc_consolidate吞并fastbin块，利用malloc_consolidate经常可以在只能申请fastbin的情况下生成smallbin来leak地址libc_base。

</br>

- [LCTF2017 2ez4u](https://github.com/fangdada/ctf/tree/master/LCTF2017/largebin_2ez4u)
- [HCTF2018 heapstorm](https://github.com/fangdada/ctf/tree/master/HCTF2018/heapstorm)
- [0CTF2018 heapstorm2](https://github.com/fangdada/ctf/tree/master/0CTF2018/heapstorm2)
- ...

</br>


## tcache

> 最近tcache机制的pwn题越来越多，因此必须得明白tcache机制与往常glibc2.23等版本的不同处，tcache的安全检查特别少，因此这类题的难点通常就在如何leak出libc基址以及如何创建重叠堆块。

</br>

- [QCTF2018 babyheap](https://github.com/fangdada/ctf/tree/master/QCTF2018/babyheap)
- [HITCON2018 children_tcache](https://github.com/fangdada/ctf/tree/master/HITCON2018/child_tcache)
- [LCTF2018 easy_heap](https://github.com/fangdada/ctf/tree/master/LCTF2018/easyheap)
- ...

</br>

## unsafe unlink

> 对刚学堆利用的bin手来说这通常是第一课，目前只放这一题，后续还有largebin下的unlink实现

</br>

- [demo](https://github.com/fangdada/ctf/tree/master/how2pwn/unsafe_unlink)
- [0CTF2018 heapstorm2](https://github.com/fangdada/ctf/tree/master/0CTF2018/heapstorm2)
- [SCTF2018 bufoverflow\_a](https://github.com/fangdada/ctf/tree/master/SCTF2018/bufoverflow_a)
- ...

</br>


## _IO_FILE

> 首先请移步至对[\_IO_FILE](https://fanda.cloud/archives/tag/_io_file)的详细分析文章，PWN中攻击\_IO\_FILE的题通常都是综合unsortedbin attack修改\_IO\_list\_all，利用其\_chain指向可控地址进而篡改vtable来劫持流程的，或者攻击\_IO_FILE结构的成员以扩大输出范围达到内存泄漏的目的。

</br>

- [HITCON2016 house\_of\_orange](https://github.com/fangdada/ctf/tree/master/how2pwn/house_of_orange/hitcon2016)
- [SCTF2018 sbbs](https://github.com/fangdada/ctf/tree/master/SCTF2018/sbbs)
- [SCTF2018 bufoverflow\_a](https://github.com/fangdada/ctf/tree/master/SCTF2018/bufoverflow_a)
- ...

</br>

# how2kernel
[点这里跳转](https://github.com/fangdada/kernelPWN)

***

# How2reverse

&nbsp;&nbsp;&nbsp;&nbsp;<font size=2>终于终于，我终于更新how2reverse了23333，鸽了好久，前一阵子太忙，然后又学习Linux kernel和angr框架，后来出了莫名其妙的问题后卡住了，没事做就来更新下这个。好了废话说完了，来看看how2reverse主要讲什么：</font></br>

## 动态调试

> 我始终认为动态调试是一个逆向分析师必须要熟练掌握的技巧，无论是CTF题还是真实对抗环境，都要能利用动态调试见招拆招找到自己想要的。建议工具：IDA远程调试，GDB+插件（例如peda，pwndbg皆可）。

</br>

- [0CTF2016 momo](https://github.com/fangdada/ctf/tree/master/0CTF2016/momo)
- [QCTF2018 babyre](https://github.com/fangdada/ctf/tree/master/QCTF2018/babyre)
- [鹏城杯2018初赛 badblock](https://github.com/fangdada/ctf/tree/master/%E9%B9%8F%E5%9F%8E%E6%9D%AF2018/badblock)
- [RCTF2018 simple_re](https://github.com/fangdada/ctf/tree/master/RCTF2018/simple_re)
- [RCTF2018 magic](https://github.com/fangdada/ctf/tree/master/RCTF2018/magic)
- [RCTF2018 babyvm](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyvm)
- ...

</br>

## Z3-Solver

> 利用符号变量约束器求解唯一解在CTF中也是一种解法，到如今这成为了CTFer必须掌握的一个技巧。与爆破类似，必须知道程序的逻辑，例如加密算法，之后利用python的z3模块编写脚本约束求解。

</br>

- [SUCTF2018 simpleformat](https://github.com/fangdada/ctf/tree/master/SUCTF2018/simpleformat)
- [RCTF2018 babyre2](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyre2)
- [0CTF2017 engineTest](https://github.com/fangdada/ctf/tree/master/0CTF2017/engineTest)
- ...

</br>


## 静态分析

> 只需要静态分析的题常常花样特别多，除去偶尔的逆向pyc和mips或是直接一个log文件，爆破题和加密题常是静态分析题，我个人喜欢配合GDB验证自己还原的算法是否出错。

</br>

- [QCTF2018 asong](https://github.com/fangdada/ctf/tree/master/QCTF2018/asong)
- [RCTF2018 babyre](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyre)
- [QCTF2018 babymips](https://github.com/fangdada/ctf/tree/master/QCTF2018/babymips)
- [0CTF2017 py](https://github.com/fangdada/ctf/tree/master/0CTF2017/py)
- [0CTF2016 trace](https://github.com/fangdada/ctf/tree/master/0CTF2016/trace)
- [SCTF2018 babymips](https://github.com/fangdada/ctf/tree/master/SCTF2018/babymips)
- [SUCTF2018 Enigma](https://github.com/fangdada/ctf/tree/master/SUCTF/Engima)
- [starCTF2018 milktea](https://github.com/fangdada/ctf/tree/master/starctf/milktea)
- [CISCN2018 reverse_03](https://github.com/fangdada/ctf/tree/master/CISCN2018/reverse_03)
- [SUCTF2018 simpleformat](https://github.com/fangdada/ctf/tree/master/SUCTF2018/simpleformat)
- ...

</br>

## 爆破

> 爆破常用与一些加密题，例如爆破hash算法。这一类题通常需要还原算法，若爆破时间过长应注意一下是否是自己的脚本问题或是解题思路。

</br>

- [RCTF2018 babyre](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyre)
- [RCTF2018 babyvm](https://github.com/fangdada/ctf/tree/master/RCTF2018/babyvm)
- [RCTF2018 simple_re](https://github.com/fangdada/ctf/tree/master/RCTF2018/simple_re)
- [RCTF2018 magic](https://github.com/fangdada/ctf/tree/master/RCTF2018/magic)
- [SUCTF2018 Enigma](https://github.com/fangdada/ctf/tree/master/SUCTF/Engima)
- [CISCN2018 reverse_03](https://github.com/fangdada/ctf/tree/master/CISCN2018/reverse_03)
- ...

</br>

## 脱壳

>壳分为压缩壳与加密壳，在Windows下加密壳比Linux下的加密壳多，但CTF中大多都是压缩壳，无论是使用脱壳机还是手动脱壳都十分简单，在这里我都用手动的方式脱壳。

</br>

- [DDCTF2019 reverse2](https://github.com/fangdada/ctf/tree/master/how2reverse/ddctf_reverse2)

</br>

## 待续

> …...

</br>

- ...

</br>



---

# How2CVE

&emsp;&emsp;<font size=2>趁着有空闲时间，在这里做一下CVE的复现，路线就从栈到堆最后到内核吧，如果有POC的文件格式是公开的话可能还会尝试用fuzzer再复现一遍。好接下来就一步一步慢慢来：</font></br>

## 栈

> 栈溢出漏洞主要在信息安全早期横行，但在现在已经很少能看到新鲜的栈溢出漏洞了，但这不妨碍栈溢出成为学习漏洞利用的第一课。

</br>

- [CVE-2010-2883](https://github.com/fangdada/how2CVE/tree/master/CVE-2010-2883)
- [CVE-2010-3333](https://github.com/fangdada/how2CVE/tree/master/CVE-2010-3333)
- [CVE-2012-0158](https://github.com/fangdada/how2CVE/tree/master/CVE-2012-0158)
- [CVE-2014-3791](https://github.com/fangdada/how2CVE/tree/master/CVE-2014-3791)

- ...

</br>



## 堆

</br>

- ...

</br>



## 内核

</br>

- ...

</br>

