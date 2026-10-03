---
comments: true
difficulty: Medium
rating: 1680
source: Weekly Contest 241 Q3
tags:
    - Design
    - Array
    - Hash Table
---

<!-- problem:start -->

# [1865. Finding Pairs With a Certain Sum](https://leetcode.com/problems/finding-pairs-with-a-certain-sum)

[中文文档](/solution/1800-1899/1865.Finding%20Pairs%20With%20a%20Certain%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>. Hãy triển khai một cấu trúc dữ liệu hỗ trợ hai loại query:</p>

<ol>
	<li><strong>Add</strong> một số nguyên dương vào phần tử tại một chỉ số cho trước trong mảng <code>nums2</code>.</li>
	<li><strong>Count</strong> số cặp <code>(i, j)</code> sao cho <code>nums1[i] + nums2[j]</code> bằng một giá trị cho trước (<code>0 &lt;= i &lt; nums1.length</code> và <code>0 &lt;= j &lt; nums2.length</code>).</li>
</ol>

<p>Hãy triển khai class <code>FindSumPairs</code>:</p>

<ul>
	<li><code>FindSumPairs(int[] nums1, int[] nums2)</code> Khởi tạo đối tượng <code>FindSumPairs</code> với hai mảng số nguyên <code>nums1</code> và <code>nums2</code>.</li>
	<li><code>void add(int index, int val)</code> Cộng <code>val</code> vào <code>nums2[index]</code>, tức là thực hiện <code>nums2[index] += val</code>.</li>
	<li><code>int count(int tot)</code> Trả về số cặp <code>(i, j)</code> sao cho <code>nums1[i] + nums2[j] == tot</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;FindSumPairs&quot;, &quot;count&quot;, &quot;add&quot;, &quot;count&quot;, &quot;count&quot;, &quot;add&quot;, &quot;add&quot;, &quot;count&quot;]
[[[1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]], [7], [3, 2], [8], [4], [0, 1], [1, 1], [7]]
<strong>Đầu ra</strong>
[null, 8, null, 2, 1, null, null, 11]

<strong>Giải thích</strong>
FindSumPairs findSumPairs = new FindSumPairs([1, 1, 2, 2, 2, 3], [1, 4, 5, 2, 5, 4]);
findSumPairs.count(7);  // trả về 8; các cặp (2,2), (3,2), (4,2), (2,4), (3,4), (4,4) tạo thành 2 + 5 và các cặp (5,1), (5,5) tạo thành 3 + 4
findSumPairs.add(3, 2); // lúc này nums2 = [1,4,5,<strong><u>4</u></strong><code>,5,4</code>]
findSumPairs.count(8);  // trả về 2; các cặp (5,2), (5,4) tạo thành 3 + 5
findSumPairs.count(4);  // trả về 1; cặp (5,0) tạo thành 3 + 1
findSumPairs.add(0, 1); // lúc này nums2 = [<strong><u><code>2</code></u></strong>,4,5,4<code>,5,4</code>]
findSumPairs.add(1, 1); // lúc này nums2 = [<code>2</code>,<strong><u>5</u></strong>,5,4<code>,5,4</code>]
findSumPairs.count(7);  // trả về 11; các cặp (2,1), (2,2), (2,4), (3,1), (3,2), (3,4), (4,1), (4,2), (4,4) tạo thành 2 + 5 và các cặp (5,3), (5,5) tạo thành 3 + 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums1[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= nums2[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= index &lt; nums2.length</code></li>
	<li><code>1 &lt;= val &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= tot &lt;= 10<sup>9</sup></code></li>
	<li>Có nhiều nhất <code>1000</code> lần gọi đến <code>add</code> và <code>count</code> <strong>mỗi loại</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần cập nhật một phần tử của $nums2$ và đếm số cặp có tổng bằng $tot$. $nums2$ rất dài, nên không thể quét lồng nhau trong mỗi query.
>
> $nums1$ có độ dài tối đa $10^3$, nên ta liệt kê mảng này. Frequency map của $nums2$ cho phép $\textit{count}$ cộng $cnt[tot-x]$ với mỗi $x\in nums1$, còn $\textit{add}$ giảm tần suất của giá trị cũ và tăng tần suất của giá trị mới.

<!-- thinking:end -->

Ta nhận thấy độ dài mảng $\textit{nums1}$ không vượt quá ${10}^3$, trong khi độ dài mảng $\textit{nums2}$ lên đến ${10}^5$. Vì vậy, nếu liệt kê trực tiếp mọi cặp chỉ số $(i, j)$ và kiểm tra xem $\textit{nums1}[i] + \textit{nums2}[j]$ có bằng giá trị $\textit{tot}$ đã cho hay không, chương trình sẽ vượt quá giới hạn thời gian.

Ta có thể chỉ liệt kê mảng ngắn hơn $\textit{nums1}$ không? Câu trả lời là có. Ta dùng một hash table $\textit{cnt}$ để đếm số lần xuất hiện của mỗi phần tử trong mảng $\textit{nums2}$, sau đó duyệt từng phần tử $x$ trong mảng $\textit{nums1}$ và tính tổng $\textit{cnt}[\textit{tot} - x]$.

Khi gọi phương thức $\text{add}$, trước hết ta giảm giá trị tương ứng với $\textit{nums2}[index]$ trong $\textit{cnt}$ đi $1$, sau đó cộng $\textit{val}$ vào $\textit{nums2}[index]$, cuối cùng tăng giá trị tương ứng với $\textit{nums2}[index]$ trong $\textit{cnt}$ lên $1$.

Khi gọi phương thức $\text{count}$, ta chỉ cần duyệt mảng $\textit{nums1}$ và tính tổng $\textit{cnt}[\textit{tot} - x]$ với mỗi phần tử $x$.

Độ phức tạp thời gian là $O(n \times q)$, độ phức tạp không gian là $O(m)$. Ở đây, $n$ và $m$ lần lượt là độ dài của các mảng $\textit{nums1}$ và $\textit{nums2}$, còn $q$ là số lần gọi phương thức $\text{count}$.

<!-- tabs:start -->

#### Python3

```python
class FindSumPairs:

    def __init__(self, nums1: List[int], nums2: List[int]):
        self.cnt = Counter(nums2)
        self.nums1 = nums1
        self.nums2 = nums2

    def add(self, index: int, val: int) -> None:
        self.cnt[self.nums2[index]] -= 1
        self.nums2[index] += val
        self.cnt[self.nums2[index]] += 1

    def count(self, tot: int) -> int:
        return sum(self.cnt[tot - x] for x in self.nums1)


# Your FindSumPairs object will be instantiated and called as such:
# obj = FindSumPairs(nums1, nums2)
# obj.add(index,val)
# param_2 = obj.count(tot)
```

#### Java

```java
class FindSumPairs {
    private int[] nums1;
    private int[] nums2;
    private Map<Integer, Integer> cnt = new HashMap<>();

    public FindSumPairs(int[] nums1, int[] nums2) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        for (int x : nums2) {
            cnt.merge(x, 1, Integer::sum);
        }
    }

    public void add(int index, int val) {
        cnt.merge(nums2[index], -1, Integer::sum);
        nums2[index] += val;
        cnt.merge(nums2[index], 1, Integer::sum);
    }

    public int count(int tot) {
        int ans = 0;
        for (int x : nums1) {
            ans += cnt.getOrDefault(tot - x, 0);
        }
        return ans;
    }
}

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * FindSumPairs obj = new FindSumPairs(nums1, nums2);
 * obj.add(index,val);
 * int param_2 = obj.count(tot);
 */
```

#### C++

```cpp
class FindSumPairs {
public:
    FindSumPairs(vector<int>& nums1, vector<int>& nums2) {
        this->nums1 = nums1;
        this->nums2 = nums2;
        for (int x : nums2) {
            ++cnt[x];
        }
    }

    void add(int index, int val) {
        --cnt[nums2[index]];
        nums2[index] += val;
        ++cnt[nums2[index]];
    }

    int count(int tot) {
        int ans = 0;
        for (int x : nums1) {
            ans += cnt[tot - x];
        }
        return ans;
    }

private:
    vector<int> nums1;
    vector<int> nums2;
    unordered_map<int, int> cnt;
};

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * FindSumPairs* obj = new FindSumPairs(nums1, nums2);
 * obj->add(index,val);
 * int param_2 = obj->count(tot);
 */
```

#### Go

```go
type FindSumPairs struct {
	nums1 []int
	nums2 []int
	cnt   map[int]int
}

func Constructor(nums1 []int, nums2 []int) FindSumPairs {
	cnt := map[int]int{}
	for _, x := range nums2 {
		cnt[x]++
	}
	return FindSumPairs{nums1, nums2, cnt}
}

func (this *FindSumPairs) Add(index int, val int) {
	this.cnt[this.nums2[index]]--
	this.nums2[index] += val
	this.cnt[this.nums2[index]]++
}

func (this *FindSumPairs) Count(tot int) (ans int) {
	for _, x := range this.nums1 {
		ans += this.cnt[tot-x]
	}
	return
}

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * obj := Constructor(nums1, nums2);
 * obj.Add(index,val);
 * param_2 := obj.Count(tot);
 */
```

#### TypeScript

```ts
class FindSumPairs {
    private nums1: number[];
    private nums2: number[];
    private cnt: Map<number, number>;

    constructor(nums1: number[], nums2: number[]) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        this.cnt = new Map();
        for (const x of nums2) {
            this.cnt.set(x, (this.cnt.get(x) || 0) + 1);
        }
    }

    add(index: number, val: number): void {
        const old = this.nums2[index];
        this.cnt.set(old, this.cnt.get(old)! - 1);
        this.nums2[index] += val;
        const now = this.nums2[index];
        this.cnt.set(now, (this.cnt.get(now) || 0) + 1);
    }

    count(tot: number): number {
        return this.nums1.reduce((acc, x) => acc + (this.cnt.get(tot - x) || 0), 0);
    }
}

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * var obj = new FindSumPairs(nums1, nums2)
 * obj.add(index,val)
 * var param_2 = obj.count(tot)
 */
```

#### Rust

```rust
use std::collections::HashMap;

struct FindSumPairs {
    nums1: Vec<i32>,
    nums2: Vec<i32>,
    cnt: HashMap<i32, i32>,
}

impl FindSumPairs {
    fn new(nums1: Vec<i32>, nums2: Vec<i32>) -> Self {
        let mut cnt = HashMap::new();
        for &x in &nums2 {
            *cnt.entry(x).or_insert(0) += 1;
        }
        Self { nums1, nums2, cnt }
    }

    fn add(&mut self, index: i32, val: i32) {
        let i = index as usize;
        let old_val = self.nums2[i];
        *self.cnt.entry(old_val).or_insert(0) -= 1;
        if self.cnt[&old_val] == 0 {
            self.cnt.remove(&old_val);
        }

        self.nums2[i] += val;
        let new_val = self.nums2[i];
        *self.cnt.entry(new_val).or_insert(0) += 1;
    }

    fn count(&self, tot: i32) -> i32 {
        let mut ans = 0;
        for &x in &self.nums1 {
            let target = tot - x;
            if let Some(&c) = self.cnt.get(&target) {
                ans += c;
            }
        }
        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 */
var FindSumPairs = function (nums1, nums2) {
    this.nums1 = nums1;
    this.nums2 = nums2;
    this.cnt = new Map();
    for (const x of nums2) {
        this.cnt.set(x, (this.cnt.get(x) || 0) + 1);
    }
};

/**
 * @param {number} index
 * @param {number} val
 * @return {void}
 */
FindSumPairs.prototype.add = function (index, val) {
    const old = this.nums2[index];
    this.cnt.set(old, this.cnt.get(old) - 1);
    this.nums2[index] += val;
    const now = this.nums2[index];
    this.cnt.set(now, (this.cnt.get(now) || 0) + 1);
};

/**
 * @param {number} tot
 * @return {number}
 */
FindSumPairs.prototype.count = function (tot) {
    return this.nums1.reduce((acc, x) => acc + (this.cnt.get(tot - x) || 0), 0);
};

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * var obj = new FindSumPairs(nums1, nums2)
 * obj.add(index,val)
 * var param_2 = obj.count(tot)
 */
```

#### C#

```cs
public class FindSumPairs {
    private int[] nums1;
    private int[] nums2;
    private Dictionary<int, int> cnt = new Dictionary<int, int>();

    public FindSumPairs(int[] nums1, int[] nums2) {
        this.nums1 = nums1;
        this.nums2 = nums2;
        foreach (int x in nums2) {
            if (cnt.ContainsKey(x)) {
                cnt[x]++;
            } else {
                cnt[x] = 1;
            }
        }
    }

    public void Add(int index, int val) {
        int oldVal = nums2[index];
        if (cnt.TryGetValue(oldVal, out int oldCount)) {
            if (oldCount == 1) {
                cnt.Remove(oldVal);
            } else {
                cnt[oldVal] = oldCount - 1;
            }
        }
        nums2[index] += val;
        int newVal = nums2[index];
        if (cnt.TryGetValue(newVal, out int newCount)) {
            cnt[newVal] = newCount + 1;
        } else {
            cnt[newVal] = 1;
        }
    }

    public int Count(int tot) {
        int ans = 0;
        foreach (int x in nums1) {
            int target = tot - x;
            if (cnt.TryGetValue(target, out int count)) {
                ans += count;
            }
        }
        return ans;
    }
}

/**
 * Your FindSumPairs object will be instantiated and called as such:
 * FindSumPairs obj = new FindSumPairs(nums1, nums2);
 * obj.Add(index,val);
 * int param_2 = obj.Count(tot);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
