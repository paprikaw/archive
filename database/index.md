index join:

tuple number * selection cost(index one) vs
scan cost of outer tuples * scan cost of inner tuple(not index one)

selection cost << scan cost of inner tuple
tuple number >> scan cost of outer tuples

所以真正的矛盾在于在没有index的loop中，cost只有从磁盘读取到内存的cost（内存cost忽略不计）

但是在有index的loop中，我们需要加上一个selection的cost。而且外层tuple数量非常大的时候，就需要做非常多次selection。

比如：

对于两张都只占用一个page的表来说：

外层：1000tuple
内层：10tuple

不用selection的话，只需要一次磁盘读取
使用index的话，需要1000次selection

如果selection的cost比磁盘读取小的多的话，那么使用index才是有可能有advantage的。
如果没有的话，则可能会出现使用index更慢的情况。

综上所述，在使用index join的时候，需要考虑：

1. selection 的效率
2. 外层tuple的数量
3. 外层tuple全部从磁盘读取到内存所花费的时间
3. 内层tuple全部从磁盘读取到内存所花费的时间
