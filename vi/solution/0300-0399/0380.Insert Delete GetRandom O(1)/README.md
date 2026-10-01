---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - Math
    - Randomized
---

<!-- problem:start -->

# [380. Insert Delete GetRandom O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1)

[中文文档](/solution/0300-0399/0380.Insert%20Delete%20GetRandom%20O%281%29/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy cài đặt class <code>RandomizedSet</code>:</p>

<ul>
	<li><code>RandomizedSet()</code> Khởi tạo đối tượng <code>RandomizedSet</code>.</li>
	<li><code>bool insert(int val)</code> Thêm phần tử <code>val</code> vào set nếu chưa có. Trả về <code>true</code> nếu phần tử chưa tồn tại, ngược lại trả về <code>false</code>.</li>
	<li><code>bool remove(int val)</code> Xóa phần tử <code>val</code> khỏi set nếu phần tử đó tồn tại. Trả về <code>true</code> nếu phần tử có trong set, ngược lại trả về <code>false</code>.</li>
	<li><code>int getRandom()</code> Trả về một phần tử ngẫu nhiên trong set hiện tại (đảm bảo có ít nhất một phần tử khi gọi phương thức này). Mỗi phần tử phải có <b>xác suất được trả về như nhau</b>.</li>
</ul>

<p>Hãy cài đặt các hàm của class sao cho mỗi hàm có độ phức tạp thời gian&nbsp;<code>O(1)</code>&nbsp;<strong>trung bình</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;RandomizedSet&quot;, &quot;insert&quot;, &quot;remove&quot;, &quot;insert&quot;, &quot;getRandom&quot;, &quot;remove&quot;, &quot;insert&quot;, &quot;getRandom&quot;]
[[], [1], [2], [2], [], [1], [2], []]
<strong>Đầu ra</strong>
[null, true, false, true, 2, true, false, 2]

<strong>Giải thích</strong>
RandomizedSet randomizedSet = new RandomizedSet();
randomizedSet.insert(1); // Inserts 1 to the set. Returns true as 1 was inserted successfully.
randomizedSet.remove(2); // Returns false as 2 does not exist in the set.
randomizedSet.insert(2); // Inserts 2 to the set, returns true. Set now contains [1,2].
randomizedSet.getRandom(); // getRandom() should return either 1 or 2 randomly.
randomizedSet.remove(1); // Removes 1 from the set, returns true. Set now contains [2].
randomizedSet.insert(2); // 2 was already in the set, so return false.
randomizedSet.getRandom(); // Since 2 is the only number in the set, getRandom() will always return 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>-2<sup>31</sup> &lt;= val &lt;= 2<sup>31</sup> - 1</code></li>
	<li>Sẽ có tối đa <code>2 *&nbsp;</code><code>10<sup>5</sup></code> lần gọi các hàm <code>insert</code>, <code>remove</code> và <code>getRandom</code>.</li>
	<li>Cấu trúc dữ liệu sẽ có <strong>ít nhất một</strong> phần tử khi gọi <code>getRandom</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Danh sách động

<!-- thinking:start -->

> **Tư duy**
>
> Cần một set hỗ trợ thêm, xóa và chọn ngẫu nhiên đồng đều, tất cả trong $O(1)$. Hash map riêng không có index; còn xóa khỏi array riêng thì mất $O(n)$.
>
> Array lưu các giá trị, map lưu index của chúng. Khi thêm, append phần tử; khi xóa, đổi chỗ với phần tử cuối, pop phần tử cuối rồi cập nhật index. Chọn ngẫu nhiên bằng `choice` trên array.

<!-- thinking:end -->

Ta dùng danh sách động $q$ để lưu các phần tử trong set, và hash table $d$ để lưu index của mỗi phần tử trong $q$.

Khi thêm một phần tử, nếu phần tử đã có trong hash table $d$ thì trả về `false`; nếu chưa, thêm phần tử vào cuối danh sách động $q$, đồng thời thêm phần tử và index của nó trong $q$ vào hash table $d$, rồi trả về `true`.

Khi xóa một phần tử, nếu phần tử không có trong hash table $d$ thì trả về `false`; nếu có, lấy index của phần tử trong danh sách $q$ từ hash table, rồi đổi chỗ phần tử cuối $q[-1]$ với $q[i]$ và cập nhật index của $q[-1]$ trong hash table thành $i$. Sau đó xóa phần tử cuối khỏi $q$, đồng thời xóa phần tử khỏi hash table, rồi trả về `true`.

Để lấy phần tử ngẫu nhiên, ta chọn ngẫu nhiên một phần tử trong danh sách động $q$ rồi trả về.

Độ phức tạp thời gian là $O(1)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số phần tử trong set.

<!-- tabs:start -->

#### Python3

```python
class RandomizedSet:
    def __init__(self):
        self.d = {}
        self.q = []

    def insert(self, val: int) -> bool:
        if val in self.d:
            return False
        self.d[val] = len(self.q)
        self.q.append(val)
        return True

    def remove(self, val: int) -> bool:
        if val not in self.d:
            return False
        i = self.d[val]
        self.d[self.q[-1]] = i
        self.q[i] = self.q[-1]
        self.q.pop()
        self.d.pop(val)
        return True

    def getRandom(self) -> int:
        return choice(self.q)


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

#### Java

```java
class RandomizedSet {
    private Map<Integer, Integer> d = new HashMap<>();
    private List<Integer> q = new ArrayList<>();
    private Random rnd = new Random();

    public RandomizedSet() {
    }

    public boolean insert(int val) {
        if (d.containsKey(val)) {
            return false;
        }
        d.put(val, q.size());
        q.add(val);
        return true;
    }

    public boolean remove(int val) {
        if (!d.containsKey(val)) {
            return false;
        }
        int i = d.get(val);
        d.put(q.get(q.size() - 1), i);
        q.set(i, q.get(q.size() - 1));
        q.remove(q.size() - 1);
        d.remove(val);
        return true;
    }

    public int getRandom() {
        return q.get(rnd.nextInt(q.size()));
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * boolean param_1 = obj.insert(val);
 * boolean param_2 = obj.remove(val);
 * int param_3 = obj.getRandom();
 */
```

#### C++

```cpp
class RandomizedSet {
public:
    RandomizedSet() {
    }

    bool insert(int val) {
        if (d.count(val)) {
            return false;
        }
        d[val] = q.size();
        q.push_back(val);
        return true;
    }

    bool remove(int val) {
        if (!d.count(val)) {
            return false;
        }
        int i = d[val];
        d[q.back()] = i;
        q[i] = q.back();
        q.pop_back();
        d.erase(val);
        return true;
    }

    int getRandom() {
        return q[rand() % q.size()];
    }

private:
    unordered_map<int, int> d;
    vector<int> q;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

#### Go

```go
type RandomizedSet struct {
	d map[int]int
	q []int
}

func Constructor() RandomizedSet {
	return RandomizedSet{map[int]int{}, []int{}}
}

func (this *RandomizedSet) Insert(val int) bool {
	if _, ok := this.d[val]; ok {
		return false
	}
	this.d[val] = len(this.q)
	this.q = append(this.q, val)
	return true
}

func (this *RandomizedSet) Remove(val int) bool {
	if _, ok := this.d[val]; !ok {
		return false
	}
	i := this.d[val]
	this.d[this.q[len(this.q)-1]] = i
	this.q[i] = this.q[len(this.q)-1]
	this.q = this.q[:len(this.q)-1]
	delete(this.d, val)
	return true
}

func (this *RandomizedSet) GetRandom() int {
	return this.q[rand.Intn(len(this.q))]
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * obj := Constructor();
 * param_1 := obj.Insert(val);
 * param_2 := obj.Remove(val);
 * param_3 := obj.GetRandom();
 */
```

#### TypeScript

```ts
class RandomizedSet {
    private d: Map<number, number> = new Map();
    private q: number[] = [];

    constructor() {}

    insert(val: number): boolean {
        if (this.d.has(val)) {
            return false;
        }
        this.d.set(val, this.q.length);
        this.q.push(val);
        return true;
    }

    remove(val: number): boolean {
        if (!this.d.has(val)) {
            return false;
        }
        const i = this.d.get(val)!;
        this.d.set(this.q[this.q.length - 1], i);
        this.q[i] = this.q[this.q.length - 1];
        this.q.pop();
        this.d.delete(val);
        return true;
    }

    getRandom(): number {
        return this.q[Math.floor(Math.random() * this.q.length)];
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * var obj = new RandomizedSet()
 * var param_1 = obj.insert(val)
 * var param_2 = obj.remove(val)
 * var param_3 = obj.getRandom()
 */
```

#### Rust

```rust
use rand::Rng;
use std::collections::HashSet;

struct RandomizedSet {
    list: HashSet<i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl RandomizedSet {
    fn new() -> Self {
        Self {
            list: HashSet::new(),
        }
    }

    fn insert(&mut self, val: i32) -> bool {
        self.list.insert(val)
    }

    fn remove(&mut self, val: i32) -> bool {
        self.list.remove(&val)
    }

    fn get_random(&self) -> i32 {
        let i = rand::thread_rng().gen_range(0, self.list.len());
        *self.list.iter().collect::<Vec<&i32>>()[i]
    }
}
```

#### C#

```cs
public class RandomizedSet {
    private Dictionary<int, int> d = new Dictionary<int, int>();
    private List<int> q = new List<int>();

    public RandomizedSet() {

    }

    public bool Insert(int val) {
        if (d.ContainsKey(val)) {
            return false;
        }
        d.Add(val, q.Count);
        q.Add(val);
        return true;
    }

    public bool Remove(int val) {
        if (!d.ContainsKey(val)) {
            return false;
        }
        int i = d[val];
        d[q[q.Count - 1]] = i;
        q[i] = q[q.Count - 1];
        q.RemoveAt(q.Count - 1);
        d.Remove(val);
        return true;
    }

    public int GetRandom() {
        return q[new Random().Next(0, q.Count)];
    }
}

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet obj = new RandomizedSet();
 * bool param_1 = obj.Insert(val);
 * bool param_2 = obj.Remove(val);
 * int param_3 = obj.GetRandom();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
