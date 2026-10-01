---
comments: true
difficulty: Hard
tags:
    - Design
    - Array
    - Hash Table
    - Math
    - Randomized
---

<!-- problem:start -->

# [381. Insert Delete GetRandom O(1) - Duplicates allowed](https://leetcode.com/problems/insert-delete-getrandom-o1-duplicates-allowed)

[中文文档](/solution/0300-0399/0381.Insert%20Delete%20GetRandom%20O%281%29%20-%20Duplicates%20allowed/README.md)

## Mô tả

<!-- description:start -->

<p><code>RandomizedCollection</code> là cấu trúc dữ liệu lưu một tập hợp số có thể chứa phần tử trùng lặp (multiset). Cấu trúc cần hỗ trợ thêm, xóa phần tử cụ thể và trả về một phần tử ngẫu nhiên.</p>

<p>Hãy cài đặt class <code>RandomizedCollection</code>:</p>

<ul>
	<li><code>RandomizedCollection()</code> Khởi tạo đối tượng <code>RandomizedCollection</code> rỗng.</li>
	<li><code>bool insert(int val)</code> Thêm phần tử <code>val</code> vào multiset, kể cả khi phần tử đó đã tồn tại. Trả về <code>true</code> nếu trước đó chưa có phần tử này, ngược lại trả về <code>false</code>.</li>
	<li><code>bool remove(int val)</code> Xóa một phần tử <code>val</code> khỏi multiset nếu phần tử đó tồn tại. Trả về <code>true</code> nếu có phần tử cần xóa, ngược lại trả về <code>false</code>. Nếu <code>val</code> xuất hiện nhiều lần, chỉ xóa một lần.</li>
	<li><code>int getRandom()</code> Trả về một phần tử ngẫu nhiên từ multiset hiện tại. Xác suất trả về một giá trị <strong>tỉ lệ thuận</strong> với số lần giá trị đó xuất hiện trong multiset.</li>
</ul>

<p>Bạn cần cài đặt các hàm của class sao cho mỗi hàm có độ phức tạp thời gian <code>O(1)</code> <strong>trung bình</strong>.</p>

<p><strong>Lưu ý:</strong> Bộ test được tạo sao cho <code>getRandom</code> chỉ được gọi khi <code>RandomizedCollection</code> có <strong>ít nhất một</strong> phần tử.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RandomizedCollection&quot;, &quot;insert&quot;, &quot;insert&quot;, &quot;insert&quot;, &quot;getRandom&quot;, &quot;remove&quot;, &quot;getRandom&quot;]
[[], [1], [1], [2], [], [1], []]
<strong>Đầu ra</strong>
[null, true, false, true, 2, true, 1]

<strong>Giải thích</strong>
RandomizedCollection randomizedCollection = new RandomizedCollection();
randomizedCollection.insert(1);   // return true since the collection does not contain 1.
                                  // Inserts 1 into the collection.
randomizedCollection.insert(1);   // return false since the collection contains 1.
                                  // Inserts another 1 into the collection. Collection now contains [1,1].
randomizedCollection.insert(2);   // return true since the collection does not contain 2.
                                  // Inserts 2 into the collection. Collection now contains [1,1,2].
randomizedCollection.getRandom(); // getRandom should:
                                  // - return 1 with probability 2/3, or
                                  // - return 2 with probability 1/3.
randomizedCollection.remove(1);   // return true since the collection contains 1.
                                  // Removes 1 from the collection. Collection now contains [1,2].
randomizedCollection.getRandom(); // getRandom should return 1 or 2, both equally likely.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= val &lt;= 2<sup>31</sup> - 1</code></li>
	<li>Tổng cộng có nhiều nhất <code>2 * 10<sup>5</sup></code> lời gọi đến <code>insert</code>, <code>remove</code> và <code>getRandom</code>.</li>
	<li>Cấu trúc dữ liệu sẽ có <strong>ít nhất một</strong> phần tử khi gọi <code>getRandom</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Khác với bài 380, bài này cho phép phần tử trùng lặp. Một giá trị có thể nằm ở nhiều chỉ số, vì vậy map lưu một set các vị trí.
>
> Khi thêm, ta đưa chỉ số mới vào set tương ứng. Khi xóa, ta lấy một chỉ số bất kỳ, hoán đổi phần tử ở đó với phần tử cuối mảng, rồi cập nhật set vị trí của cả hai giá trị. Lệnh random vẫn chọn từ mảng, nên giá trị xuất hiện nhiều lần tự nhiên có xác suất được chọn cao hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class RandomizedCollection:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.m = {}
        self.l = []

    def insert(self, val: int) -> bool:
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        """
        idx_set = self.m.get(val, set())
        idx_set.add(len(self.l))
        self.m[val] = idx_set
        self.l.append(val)
        return len(idx_set) == 1

    def remove(self, val: int) -> bool:
        """
        Removes a value from the collection. Returns true if the collection contained the specified element.
        """
        if val not in self.m:
            return False
        idx_set = self.m[val]
        idx = list(idx_set)[0]
        last_idx = len(self.l) - 1
        self.l[idx] = self.l[last_idx]
        idx_set.remove(idx)

        last_idx_set = self.m[self.l[last_idx]]
        if last_idx in last_idx_set:
            last_idx_set.remove(last_idx)
        if idx < last_idx:
            last_idx_set.add(idx)
        if not idx_set:
            self.m.pop(val)
        self.l.pop()
        return True

    def getRandom(self) -> int:
        """
        Get a random element from the collection.
        """
        return -1 if len(self.l) == 0 else random.choice(self.l)


# Your RandomizedCollection object will be instantiated and called as such:
# obj = RandomizedCollection()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

#### Java

```java
class RandomizedCollection {
    private Map<Integer, Set<Integer>> m;
    private List<Integer> l;
    private Random rnd;

    /** Initialize your data structure here. */
    public RandomizedCollection() {
        m = new HashMap<>();
        l = new ArrayList<>();
        rnd = new Random();
    }

    /**
     * Inserts a value to the collection. Returns true if the collection did not already contain
     * the specified element.
     */
    public boolean insert(int val) {
        m.computeIfAbsent(val, k -> new HashSet<>()).add(l.size());
        l.add(val);
        return m.get(val).size() == 1;
    }

    /**
     * Removes a value from the collection. Returns true if the collection contained the specified
     * element.
     */
    public boolean remove(int val) {
        if (!m.containsKey(val)) {
            return false;
        }
        Set<Integer> idxSet = m.get(val);
        int idx = idxSet.iterator().next();
        int lastIdx = l.size() - 1;
        l.set(idx, l.get(lastIdx));
        idxSet.remove(idx);

        Set<Integer> lastIdxSet = m.get(l.get(lastIdx));
        lastIdxSet.remove(lastIdx);
        if (idx < lastIdx) {
            lastIdxSet.add(idx);
        }
        if (idxSet.isEmpty()) {
            m.remove(val);
        }
        l.remove(lastIdx);
        return true;
    }

    /** Get a random element from the collection. */
    public int getRandom() {
        int size = l.size();
        return size == 0 ? -1 : l.get(rnd.nextInt(size));
    }
}

/**
 * Your RandomizedCollection object will be instantiated and called as such:
 * RandomizedCollection obj = new RandomizedCollection();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
