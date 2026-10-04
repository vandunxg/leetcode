---
comments: true
difficulty: Medium
rating: 1509
source: Weekly Contest 344 Q2
tags:
    - Design
    - Hash Table
---

<!-- problem:start -->

# [2671. Frequency Tracker](https://leetcode.com/problems/frequency-tracker)

[中文文档](/solution/2600-2699/2671.Frequency%20Tracker/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một cấu trúc dữ liệu theo dõi các giá trị trong đó và trả lời một số truy vấn liên quan đến tần suất xuất hiện của chúng.</p>

<p>Hãy triển khai lớp <code>FrequencyTracker</code>.</p>

<ul>
	<li><code>FrequencyTracker()</code>: Khởi tạo đối tượng <code>FrequencyTracker</code> với một mảng ban đầu rỗng.</li>
	<li><code>void add(int number)</code>: Thêm <code>number</code> vào cấu trúc dữ liệu.</li>
	<li><code>void deleteOne(int number)</code>: Xóa <strong>một</strong> lần xuất hiện của <code>number</code> khỏi cấu trúc dữ liệu. Cấu trúc dữ liệu có thể <strong>không chứa</strong> <code>number</code>; trong trường hợp đó, không có gì bị xóa.</li>
	<li><code>bool hasFrequency(int frequency)</code>: Trả về <code>true</code> nếu có một số trong cấu trúc dữ liệu xuất hiện đúng <code>frequency</code> lần, ngược lại trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;FrequencyTracker&quot;, &quot;add&quot;, &quot;add&quot;, &quot;hasFrequency&quot;]
[[], [3], [3], [2]]
<strong>Đầu ra</strong>
[null, null, null, true]

<strong>Giải thích</strong>
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(3); // The data structure now contains [3]
frequencyTracker.add(3); // The data structure now contains [3, 3]
frequencyTracker.hasFrequency(2); // Returns true, because 3 occurs twice

</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;FrequencyTracker&quot;, &quot;add&quot;, &quot;deleteOne&quot;, &quot;hasFrequency&quot;]
[[], [1], [1], [1]]
<strong>Đầu ra</strong>
[null, null, null, false]

<strong>Giải thích</strong>
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(1); // The data structure now contains [1]
frequencyTracker.deleteOne(1); // The data structure becomes empty []
frequencyTracker.hasFrequency(1); // Returns false, because the data structure is empty

</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;FrequencyTracker&quot;, &quot;hasFrequency&quot;, &quot;add&quot;, &quot;hasFrequency&quot;]
[[], [2], [3], [1]]
<strong>Đầu ra</strong>
[null, false, null, true]

<strong>Giải thích</strong>
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.hasFrequency(2); // Returns false, because the data structure is empty
frequencyTracker.add(3); // The data structure now contains [3]
frequencyTracker.hasFrequency(1); // Returns true, because 3 occurs once

</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= number &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= frequency &lt;= 10<sup>5</sup></code></li>
	<li>Có nhiều nhất <code>2 *&nbsp;10<sup>5</sup></code>&nbsp;lần gọi đến <code>add</code>, <code>deleteOne</code> và <code>hasFrequency</code>&nbsp;<strong>tổng cộng</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> `hasFrequency` hỏi liệu có giá trị nào có số lần xuất hiện cho trước hay không; việc duyệt qua mọi bộ đếm sẽ không đáp ứng được khi $q \le 10^5$.
>
> $cnt$ lưu tần suất của từng số, còn $freq$ lưu số lượng các số có cùng tần suất đó. Mỗi lần cập nhật sẽ giảm bucket cũ rồi tăng bucket mới; truy vấn là $freq[f]>0$.

<!-- thinking:end -->

Ta định nghĩa hai hash table: $cnt$ dùng để ghi lại số lần xuất hiện của mỗi số, còn $freq$ dùng để ghi lại số lượng các số có mỗi tần suất.

Với thao tác `add`, trước tiên ta giảm giá trị tương ứng với $cnt[number]$ trong hash table $freq$, sau đó tăng $cnt[number]$, rồi tăng giá trị tương ứng với $cnt[number]$ trong $freq$.

Với thao tác `deleteOne`, trước tiên ta kiểm tra xem $cnt[number]$ có lớn hơn không. Nếu có, ta giảm giá trị tương ứng với $cnt[number]$ trong hash table $freq$, sau đó giảm $cnt[number]$, rồi tăng giá trị tương ứng với $cnt[number]$ trong $freq$.

Với thao tác `hasFrequency`, ta chỉ cần trả về việc $freq[frequency]$ có lớn hơn không.

Về độ phức tạp thời gian, vì sử dụng hash table nên mỗi thao tác có độ phức tạp thời gian là $O(1)$. Độ phức tạp không gian là $O(n)$, trong đó $n$ là số lượng các số phân biệt.

<!-- tabs:start -->

#### Python3

```python
class FrequencyTracker:
    def __init__(self):
        self.cnt = defaultdict(int)
        self.freq = defaultdict(int)

    def add(self, number: int) -> None:
        self.freq[self.cnt[number]] -= 1
        self.cnt[number] += 1
        self.freq[self.cnt[number]] += 1

    def deleteOne(self, number: int) -> None:
        if self.cnt[number]:
            self.freq[self.cnt[number]] -= 1
            self.cnt[number] -= 1
            self.freq[self.cnt[number]] += 1

    def hasFrequency(self, frequency: int) -> bool:
        return self.freq[frequency] > 0


# Your FrequencyTracker object will be instantiated and called as such:
# obj = FrequencyTracker()
# obj.add(number)
# obj.deleteOne(number)
# param_3 = obj.hasFrequency(frequency)
```

#### Java

```java
class FrequencyTracker {
    private Map<Integer, Integer> cnt = new HashMap<>();
    private Map<Integer, Integer> freq = new HashMap<>();

    public FrequencyTracker() {
    }

    public void add(int number) {
        freq.merge(cnt.getOrDefault(number, 0), -1, Integer::sum);
        cnt.merge(number, 1, Integer::sum);
        freq.merge(cnt.get(number), 1, Integer::sum);
    }

    public void deleteOne(int number) {
        if (cnt.getOrDefault(number, 0) > 0) {
            freq.merge(cnt.get(number), -1, Integer::sum);
            cnt.merge(number, -1, Integer::sum);
            freq.merge(cnt.get(number), 1, Integer::sum);
        }
    }

    public boolean hasFrequency(int frequency) {
        return freq.getOrDefault(frequency, 0) > 0;
    }
}

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * FrequencyTracker obj = new FrequencyTracker();
 * obj.add(number);
 * obj.deleteOne(number);
 * boolean param_3 = obj.hasFrequency(frequency);
 */
```

#### C++

```cpp
class FrequencyTracker {
public:
    FrequencyTracker() {
    }

    void add(int number) {
        freq[cnt[number]]--;
        cnt[number]++;
        freq[cnt[number]]++;
    }

    void deleteOne(int number) {
        if (cnt[number]) {
            freq[cnt[number]]--;
            cnt[number]--;
            freq[cnt[number]]++;
        }
    }

    bool hasFrequency(int frequency) {
        return freq[frequency] > 0;
    }

private:
    unordered_map<int, int> cnt;
    unordered_map<int, int> freq;
};

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * FrequencyTracker* obj = new FrequencyTracker();
 * obj->add(number);
 * obj->deleteOne(number);
 * bool param_3 = obj->hasFrequency(frequency);
 */
```

#### Go

```go
type FrequencyTracker struct {
	cnt  map[int]int
	freq map[int]int
}

func Constructor() FrequencyTracker {
	return FrequencyTracker{map[int]int{}, map[int]int{}}
}

func (this *FrequencyTracker) Add(number int) {
	this.freq[this.cnt[number]]--
	this.cnt[number]++
	this.freq[this.cnt[number]]++
}

func (this *FrequencyTracker) DeleteOne(number int) {
	if this.cnt[number] > 0 {
		this.freq[this.cnt[number]]--
		this.cnt[number]--
		this.freq[this.cnt[number]]++
	}
}

func (this *FrequencyTracker) HasFrequency(frequency int) bool {
	return this.freq[frequency] > 0
}

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(number);
 * obj.DeleteOne(number);
 * param_3 := obj.HasFrequency(frequency);
 */
```

#### TypeScript

```ts
class FrequencyTracker {
    private cnt: Map<number, number>;
    private freq: Map<number, number>;

    constructor() {
        this.cnt = new Map();
        this.freq = new Map();
    }

    add(number: number): void {
        const f = this.cnt.get(number) || 0;
        this.freq.set(f, (this.freq.get(f) || 0) - 1);
        this.cnt.set(number, f + 1);
        this.freq.set(f + 1, (this.freq.get(f + 1) || 0) + 1);
    }

    deleteOne(number: number): void {
        const f = this.cnt.get(number) || 0;
        if (f > 0) {
            this.freq.set(f, (this.freq.get(f) || 0) - 1);
            this.cnt.set(number, f - 1);
            this.freq.set(f - 1, (this.freq.get(f - 1) || 0) + 1);
        }
    }

    hasFrequency(frequency: number): boolean {
        return (this.freq.get(frequency) || 0) > 0;
    }
}

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * var obj = new FrequencyTracker()
 * obj.add(number)
 * obj.deleteOne(number)
 * var param_3 = obj.hasFrequency(frequency)
 */
```

#### Rust

```rust
use std::collections::HashMap;

struct FrequencyTracker {
    cnt: HashMap<i32, i32>,
    freq: HashMap<i32, i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl FrequencyTracker {
    fn new() -> Self {
        FrequencyTracker {
            cnt: HashMap::new(),
            freq: HashMap::new(),
        }
    }

    fn add(&mut self, number: i32) {
        let count = self.cnt.entry(number).or_insert(0);
        let prev_freq = self.freq.entry(*count).or_insert(0);
        *prev_freq -= 1;
        *count += 1;
        let new_freq = self.freq.entry(*count).or_insert(0);
        *new_freq += 1;
    }

    fn delete_one(&mut self, number: i32) {
        if let Some(count) = self.cnt.get_mut(&number) {
            if *count > 0 {
                if let Some(prev_freq) = self.freq.get_mut(count) {
                    *prev_freq -= 1;
                }
                *count -= 1;
                if let Some(new_freq) = self.freq.get_mut(count) {
                    *new_freq += 1;
                }
            }
        }
    }

    fn has_frequency(&self, frequency: i32) -> bool {
        self.freq.get(&frequency).map_or(false, |&freq| freq > 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
