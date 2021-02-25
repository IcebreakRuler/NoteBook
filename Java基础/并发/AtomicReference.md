# 原子引用类型
## atomicReference
## atomicStampedReference
解决ABA问题

主线程仅能判断共享变量的值与最初值A是否相同，不能感知到这种从A改为B又改回A的情况，如果主线程希望：只要有其他线程动过了共享变量，那么自己的cas就算失败了，这时，仅比较值是不够的，需要再加一个版本号。


## atomicMarkableReference

但是有时候，并不关心引用变量更改了几次，只是单纯的关心是否更改过，所以就有了atomicMarkableReference。







