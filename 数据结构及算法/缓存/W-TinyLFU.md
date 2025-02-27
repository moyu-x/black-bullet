在 TinyLFU 前面放了一个基于 LRU 的 Window Cache，从而可以使得前面提到的突发性稀疏流量会缓存在 Window Cache 里，只有在 Window Cache 里被淘汰的缓存才会过继给后面的 TinyLFU。至于最后的 Main Cache，虽然 W-TinyLFU 使用了分段式 LRU 来实现，但我们也可以根据实际情况修改使其符合我们需要的场景。

![[assets/e0f4bdcf1c5296649388e66f93e1a6cb_MD5.jpeg]]


例子：caffeine