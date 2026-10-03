---
comments: true
difficulty: Medium
rating: 1371
source: Weekly Contest 289 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2244. Minimum Rounds to Complete All Tasks](https://leetcode.com/problems/minimum-rounds-to-complete-all-tasks)

[中文文档](/solution/2200-2299/2244.Minimum%20Rounds%20to%20Complete%20All%20Tasks/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>tasks</code> được đánh chỉ số từ <strong>0</strong>, trong đó <code>tasks[i]</code> biểu thị độ khó của một task. Trong mỗi round, bạn có thể hoàn thành 2 hoặc 3 task có <strong>cùng độ khó</strong>.</p>

<p>Hãy trả về <em>số round <strong>ít nhất</strong> cần thiết để hoàn thành tất cả task, hoặc </em><code>-1</code><em> nếu không thể hoàn thành tất cả task.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [2,2,3,3,2,4,4,4,4,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Một cách hoàn thành tất cả task là:
- Ở round đầu tiên, hoàn thành 3 task có độ khó 2.
- Ở round thứ hai, hoàn thành 2 task có độ khó 3.
- Ở round thứ ba, hoàn thành 3 task có độ khó 4.
- Ở round thứ tư, hoàn thành 2 task có độ khó 4.
Có thể chứng minh rằng không thể hoàn thành tất cả task trong ít hơn 4 round, nên đáp án là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> tasks = [2,3,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Chỉ có 1 task có độ khó 2, nhưng trong mỗi round, bạn chỉ có thể hoàn thành 2 hoặc 3 task có cùng độ khó. Vì vậy, không thể hoàn thành tất cả task và đáp án là -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= tasks.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= tasks[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>

<p><strong>Lưu ý:</strong> Bài này giống với <a href="https://leetcode.com/problems/minimum-number-of-operations-to-make-array-empty/description/" target="_blank">2870: Minimum Number of Operations to Make Array Empty.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi round hoàn thành $2$ hoặc $3$ task có cùng độ khó. Các độ khó độc lập với nhau, nên ta đếm số lần xuất hiện của từng độ khó. Nếu số lượng là $1$ thì không thể hoàn thành.
>
> Với $v \ge 2$, ưu tiên các round gồm $3$ task: có $\lfloor v/3 \rfloor$ round đầy đủ, cộng thêm một round nếu còn dư (phần dư bằng $1$ được xử lý bằng hai round gồm $2$ task).

<!-- thinking:end -->

Ta dùng một bảng băm để đếm số task của mỗi độ khó. Sau đó, ta duyệt qua bảng băm. Với mỗi độ khó, nếu số task là $1$ thì không thể hoàn thành tất cả task, vì vậy ta trả về $-1$. Nếu không, ta tính số round cần thiết để hoàn thành các task có độ khó này rồi cộng vào đáp án.

Cuối cùng, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `tasks`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumRounds(self, tasks: List[int]) -> int:
        cnt = Counter(tasks)
        ans = 0
        for v in cnt.values():
            if v == 1:
                return -1
            ans += v // 3 + (v % 3 != 0)
        return ans
```

#### Java

```java
class Solution {
    public int minimumRounds(int[] tasks) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int t : tasks) {
            cnt.merge(t, 1, Integer::sum);
        }
        int ans = 0;
        for (int v : cnt.values()) {
            if (v == 1) {
                return -1;
            }
            ans += v / 3 + (v % 3 == 0 ? 0 : 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumRounds(vector<int>& tasks) {
        unordered_map<int, int> cnt;
        for (auto& t : tasks) {
            ++cnt[t];
        }
        int ans = 0;
        for (auto& [_, v] : cnt) {
            if (v == 1) {
                return -1;
            }
            ans += v / 3 + (v % 3 != 0);
        }
        return ans;
    }
};
```

#### Go

```go
func minimumRounds(tasks []int) int {
	cnt := map[int]int{}
	for _, t := range tasks {
		cnt[t]++
	}
	ans := 0
	for _, v := range cnt {
		if v == 1 {
			return -1
		}
		ans += v / 3
		if v%3 != 0 {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumRounds(tasks: number[]): number {
    const cnt = new Map();
    for (const t of tasks) {
        cnt.set(t, (cnt.get(t) || 0) + 1);
    }
    let ans = 0;
    for (const v of cnt.values()) {
        if (v == 1) {
            return -1;
        }
        ans += Math.floor(v / 3) + (v % 3 === 0 ? 0 : 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn minimum_rounds(tasks: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        for &t in tasks.iter() {
            let count = cnt.entry(t).or_insert(0);
            *count += 1;
        }
        let mut ans = 0;
        for &v in cnt.values() {
            if v == 1 {
                return -1;
            }
            ans += v / 3 + (if v % 3 == 0 { 0 } else { 1 });
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
