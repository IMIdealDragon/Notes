# 手撕LRU

LRU：最近最少使用，其实就是将最久未被使用的踢出去。那样的我们只要每次将使用过了就更新它。

基本需求：插入，删除，查找O(1)，且具有顺序性。

查找删除插入都快的只能是哈希表，顺序性的可以是链表。

哈希表弥补链表的查找不足，链表弥补哈希的无序性。

为什么要用双向链表而不用单向链表呢？

具体实现：

get方法：

从缓存中取出一个key，如果key未存在，返回错误，如果key已存在，则放到链表头，再返回该元素对应的value

put方法：

向缓存中写入，如果key未存在，则直接插入链表头，如果key已存在，则更新它，并放到链表头。


```cpp
class LRUCache {
private:
    int cap;
    // 双链表：装着 (key, value) 元组
    list<pair<int, int>> cache;
    // 哈希表：key 映射到 (key, value) 在 cache 中的位置
    unordered_map<int, list<pair<int, int>>::iterator> map;
public:
    LRUCache(int capacity) {
        this->cap = capacity; 
    }
    
    int get(int key) {
        auto it = map.find(key);
        // 访问的 key 不存在
        if (it == map.end()) return -1;
        // key 存在，把 (k, v) 换到队头
        pair<int, int> kv = *map[key];
        cache.erase(map[key]);
        cache.push_front(kv);
        // 更新 (key, value) 在 cache 中的位置
        map[key] = cache.begin();
        return kv.second; // value
    }
    
    void put(int key, int value) {

        /* 要先判断 key 是否已经存在 */ 
        auto it = map.find(key);
        if (it == map.end()) {
            /* key 不存在，判断 cache 是否已满 */ 
            if (cache.size() == cap) {
                // cache 已满，删除尾部的键值对腾位置
                // cache 和 map 中的数据都要删除
                auto lastPair = cache.back();
                int lastKey = lastPair.first;
                map.erase(lastKey);
                cache.pop_back();
            }
            // cache 没满，可以直接添加
            cache.push_front(make_pair(key, value));
            map[key] = cache.begin();
        } else {
            /* key 存在，更改 value 并换到队头 */
            cache.erase(map[key]);
            cache.push_front(make_pair(key, value));
            map[key] = cache.begin();
        }
    }
};
```

https://zhuanlan.zhihu.com/p/34133067

