# ConcurrentHashMap1.7
## 底层原理
分段锁+cas保证并发安全




# ConcurrentHashMap1.8


## put流程
- 先判断key和value是否为null，如果为null报空指针
- 对key进行hash
- 遍历table，如果table为空
- 否则，从这个数组的位置开始遍历，如果这个位置为null，那么直接插入
```java
 Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
```
- MOVEND常量为-1，判断当前数组位置是否正在扩容，如果正在扩容，帮助进行扩容。然后再插入。
- fh是根节点的哈希值，如果大于等于0说明是链表，如果小于0说明是红黑树
```java
 else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
```
- 对链表/红黑树中的根节点加了synchronized锁，那么在同一时间内，只能有一个线程对这条链表(红黑树)进行操作。拿到锁之后再判断一下根节点是否发生了变化，发生变化的话就重新进入循环。如果没有发生变化，那么直接判断是否有重复的值，如果没有，那么直接插入。尾插法， 然后判断是否需要扩容
- 如果这个数组位置不为null，进行遍历，进行synchronized上锁，如果是链表，遍历链表，如果是树结构，进行树的遍历。


- 如果 f 是红黑树 ，那么就是已经树化了，数据的插入就是红黑树的插入。要注意的是，这里是红黑树的话 f 是一个TreeBin 对象而不直接是红黑树的根节点，因为在红黑树的插入操作时有可能红黑树的根节点发生变化。如果对红黑树的根节点进行加锁，put之后根节点发生了变化，其他线程获得这个就可以获得新节点并加锁，这样就会出错。如果是TreeBin 对象的话，锁住的就是这个对象，根节点发生变化不会应该加锁。
- 在ConcurrentHashMap中不是直接存储TreeNode来实现的，而是用TreeBin来包装TreeNode来实现的。也就是说在实际的ConcurrentHashMap桶中，存放的是TreeBin对象，而不是TreeNode对象。之所以TreeNode继承自Node是为了附带next指针，而这个next指针可以在TreeBin中寻找下一个TreeNode，这里也是与HashMap之间比较大的区别。


- 如果找到了，那么替换该旧值，也可以不替换，否则就是没找到，要进行插入。
- 插入使用尾插法
- 链表如果插入成功后，会判断是否需要进行树化操作。
- 最后对数组的元素进行+1

### put方法
1. 先传入一个k和v的键值对，不可为空（HashMap是可以为空的），如果为空就直接报错。
2. 接着去判断table是否为空，如果为空就进入初始化阶段。
3. 如果判断数组中某个指定的桶是空的，那就直接把键值对插入到这个桶中作为头节点，而且这个操作不用加锁。
4. 如果这个要插入的桶中的hash值为-1，也就是MOVED状态（也就是这个节点是forwordingNode），那就是说明有线程正在进行扩容操作，那么当前线程就进入协助扩容阶段。
5. 需要把数据插入到链表或者树中，如果这个节点是一个链表节点，那么就遍历这个链表，如果发现有相同的key值就更新value值，如果遍历完了都没有发现相同的key值，就需要在链表的尾部插入该数据。插入结束之后判断该链表节点个数是否大于8，如果大于就需要把链表转化为红黑树存储。
6. 如果这个节点是一个红黑树节点，那就需要按照树的插入规则进行插入。
7. put结束之后，需要给map已存储的数量+1，在addCount方法中判断是否需要扩容


### 初始化
初始化时，会将sizeCtl置为-1，表示数组正在初始化或者扩容，如果有线程进来，调用yield()方法，让出CPU的一次执行事件
```java
 private final Node<K,V>[] initTable() {
        Node<K,V>[] tab; int sc;
        while ((tab = table) == null || tab.length == 0) {
            if ((sc = sizeCtl) < 0)
                让出CPU
                Thread.yield(); // lost initialization race; just spin
            else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
                try {
                    if ((tab = table) == null || tab.length == 0) {
                        int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                        @SuppressWarnings("unchecked")
                        Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                        table = tab = nt;
                        sc = n - (n >>> 2);
                    }
                } finally {
                    sizeCtl = sc;
                }
                break;
            }
        }
        return tab;
    }
```
### addCount()
> 为什么ConcurrentHashMap要用这种形式来处理呢？

问题还是处在并发上，ConcurrentHashMap是并发集合，如果用一个成员变量来统计元素个数的话，为了保证并发情况下共享变量的的安全，势必会需要通过加锁或者自旋来实现，如果竞争比较激烈的情况下，size的设置上会出现比较大的冲突反而影响了性能，所以在ConcurrentHashMap采用了分片的方法来记录大小。

有一个名为CounterCell的数组，CounterCell是一个简单的内部静态类，每个CounterCell都是一个**用于记录元素个数的的单元**。像一般的集合记录集合大小，直接定义一个size的成员变量即可，当出现改变的时候只要更新这个变量就行。

**第一种情况**：
- 如果counterCells为null，并且当前线程没有拿到baseCount，会进入初始化
    - CounterCell a   long  v  int m   uncontended true
    - 如果当前格子没有被其他线程初始化，此时进行初始化，生成2个格子，根据生成的h，随机将值(x=1)放入某一个格子中，将cellsBusy置为0，并且init为true，然后break退出循环。

**第二种情况**：
- 从外面与其他线程竞争失败了
- 如果counterCells不为null，此时根本不会去争抢baseCount
    - wasUncontended为true
    - 重新计算hash
    - 再次竞争，成功break，没有成功继续往下执行
    - collide=true
    - 再一次计算hash
    - 再次竞争，如果还是失败了，那么直接对格子进行扩容，扩容为原来的2倍。
    - 扩容完之后，再次进入循环，对格子中的value进行+1
    
**第三种情况：**
如果此时格子没有倍初始化，且有两个线程进行竞争，那么此时只会有一个线程能够成功初始化格子，此时另外一个线程会重新对baseCount进行++，如果竞争失败了，那么这个线程重新for循环，此时格子已经被初始化了。

**第四种情况：**
当有多个线程竞争，如果有一个线程正在扩容格子，并且扩容完成了，那么此时另一个线程不能再扩容，重新计算hash，重新竞争。

**第五种情况：**
当uncontended为true时，那么只有2种情况，第一种格子数组还没有被初始化，另一种当前格子数组的位置为null。来看第二种，第二种种，比如有两个线程，都要去给格子中的这个位置放入值，此时只能有一个线程能够成功。另一个线程会失败，此时另一个线程再经过2轮循环后，会对格子数组进行扩容。



## rezise()
### 什么时候会扩容？
1. 如果新增节点之后，所在链表的元素个数达到了阈值 8，则会调用treeifyBin方法把链表转换成红黑树，不过在结构转换之前，会对数组长度进行判断，实现如下：如果数组长度n小于阈值MIN_TREEIFY_CAPACITY，默认是64，则会调用tryPresize方法把数组长度扩大到原来的两倍，并触发transfer方法，重新调整节点的位置。

2. 调用put方法新增节点时，在结尾会调用addCount方法记录元素个数，并检查是否需要进行扩容，当数组元素个数达到阈值时，会触发transfer方法，重新调整节点的位置。 

3. putAll 批量插入或者插入节点后发现存在链表长度达到 8 个或以上，但数组长度为 64 以下时会触发扩容 。

4. 扩容状态下其他线程对集合进行插入、修改、删除、合并、compute 等操作时遇到 ForwardingNode 节点会触发扩容 。（帮助扩容）

扩容的时候，会有一个步长，默认为16，意思就是每个线程在当前数组中扩容16个元素位置
扩容是在addCount()方法中，里面会对addCount()计数后，通过sumCount方法赋值给s，然后进行判断sizeCtl。

1. 如果此时nextTab为空，那么会初始化一个2倍长度的新数组

2. ForwardingNode，当数组当前元素扩容完后，会在当前元素放置一个ForwardingNode  MOVED

3. 每个线程根据步长进行相应的扩容，当扩容完毕后。会去判断相应的步长，如果此时数组所有位置上都在扩容，那么这个线程就直接返回。 如果没有，会接着对数组上位置进行扩容。

4. 扩容流程跟hashmap1.8类似


当有其他线程put时，如果当前位置此时正在扩容，那么就会帮助数组进行扩容。

    