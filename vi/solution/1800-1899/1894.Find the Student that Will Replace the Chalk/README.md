---
comments: true
difficulty: Medium
rating: 1355
source: Biweekly Contest 54 Q2
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [1894. Find the Student that Will Replace the Chalk](https://leetcode.com/problems/find-the-student-that-will-replace-the-chalk)

[中文文档](/solution/1800-1899/1894.Find%20the%20Student%20that%20Will%20Replace%20the%20Chalk/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> học sinh trong một lớp, được đánh số từ <code>0</code> đến <code>n - 1</code>. Giáo viên sẽ giao bài cho từng học sinh, bắt đầu từ học sinh số <code>0</code>, rồi đến học sinh số <code>1</code> và tiếp tục như vậy cho đến học sinh số <code>n - 1</code>. Sau đó, giáo viên sẽ lặp lại quy trình, lại bắt đầu từ học sinh số <code>0</code>.</p>

<p>Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>chalk</code> và một số nguyên <code>k</code>. Ban đầu có <code>k</code> viên phấn. Khi học sinh số <code>i</code> được giao một bài, học sinh đó sẽ dùng <code>chalk[i]</code> viên phấn để giải bài. Tuy nhiên, nếu số viên phấn hiện tại <strong>nhỏ hơn</strong> <code>chalk[i]</code>, học sinh số <code>i</code> sẽ được yêu cầu <strong>thay</strong> phấn.</p>

<p>Hãy trả về <em><strong>chỉ số</strong> của học sinh sẽ <strong>thay</strong> phấn</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> chalk = [5,1,5], k = 22
<strong>Đầu ra:</strong> 0
<strong>Giải thích: </strong>Các học sinh lần lượt giải bài như sau:
- Học sinh số 0 dùng 5 viên phấn, nên k = 17.
- Học sinh số 1 dùng 1 viên phấn, nên k = 16.
- Học sinh số 2 dùng 5 viên phấn, nên k = 11.
- Học sinh số 0 dùng 5 viên phấn, nên k = 6.
- Học sinh số 1 dùng 1 viên phấn, nên k = 5.
- Học sinh số 2 dùng 5 viên phấn, nên k = 0.
Học sinh số 0 không còn đủ phấn, nên phải thay phấn.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> chalk = [3,4,1,2], k = 25
<strong>Đầu ra:</strong> 1
<strong>Giải thích: </strong>Các học sinh lần lượt giải bài như sau:
- Học sinh số 0 dùng 3 viên phấn, nên k = 22.
- Học sinh số 1 dùng 4 viên phấn, nên k = 18.
- Học sinh số 2 dùng 1 viên phấn, nên k = 17.
- Học sinh số 3 dùng 2 viên phấn, nên k = 15.
- Học sinh số 0 dùng 3 viên phấn, nên k = 12.
- Học sinh số 1 dùng 4 viên phấn, nên k = 8.
- Học sinh số 2 dùng 1 viên phấn, nên k = 7.
- Học sinh số 3 dùng 2 viên phấn, nên k = 5.
- Học sinh số 0 dùng 3 viên phấn, nên k = 2.
Học sinh số 1 không còn đủ phấn, nên phải thay phấn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>chalk.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= chalk[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng và phép modulo + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Học sinh dùng phấn theo chu kỳ và $k$ có thể rất lớn, trong khi tổng phấn của một vòng chỉ ở mức vừa phải. Mô phỏng từng vòng sẽ quá chậm.
>
> Lấy $k$ modulo tổng phấn của một vòng, sau đó duyệt từ chỉ số $0$; học sinh đầu tiên cần nhiều phấn hơn phần còn lại chính là đáp án.

<!-- thinking:end -->

Vì học sinh giải bài theo từng vòng, ta có thể cộng lượng phấn cần dùng của tất cả học sinh để được tổng $s$. Sau đó, ta lấy phần dư của $k$ khi chia cho $s$, từ đó biết số phấn còn lại sau vòng cuối cùng.

Tiếp theo, ta chỉ cần mô phỏng vòng cuối. Ban đầu, số phấn còn lại là $k$ và học sinh số $0$ bắt đầu giải bài. Khi số phấn còn lại nhỏ hơn lượng phấn học sinh hiện tại cần dùng, học sinh đó cần bổ sung phấn, nên ta trả về ngay chỉ số $i$ của học sinh hiện tại. Ngược lại, ta trừ lượng phấn học sinh hiện tại cần dùng khỏi số phấn còn lại và tăng chỉ số $i$ lên một để mô phỏng học sinh tiếp theo.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là số học sinh. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def chalkReplacer(self, chalk: List[int], k: int) -> int:
        s = sum(chalk)
        k %= s
        for i, x in enumerate(chalk):
            if k < x:
                return i
            k -= x
```

#### Java

```java
class Solution {
    public int chalkReplacer(int[] chalk, int k) {
        long s = 0;
        for (int x : chalk) {
            s += x;
        }
        k %= s;
        for (int i = 0;; ++i) {
            if (k < chalk[i]) {
                return i;
            }
            k -= chalk[i];
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int chalkReplacer(vector<int>& chalk, int k) {
        long long s = accumulate(chalk.begin(), chalk.end(), 0LL);
        k %= s;
        for (int i = 0;; ++i) {
            if (k < chalk[i]) {
                return i;
            }
            k -= chalk[i];
        }
    }
};
```

#### Go

```go
func chalkReplacer(chalk []int, k int) int {
	s := 0
	for _, x := range chalk {
		s += x
	}
	k %= s
	for i := 0; ; i++ {
		if k < chalk[i] {
			return i
		}
		k -= chalk[i]
	}
}
```

#### TypeScript

```ts
function chalkReplacer(chalk: number[], k: number): number {
    const s = chalk.reduce((acc, cur) => acc + cur, 0);
    k %= s;
    for (let i = 0; ; ++i) {
        if (k < chalk[i]) {
            return i;
        }
        k -= chalk[i];
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn chalk_replacer(chalk: Vec<i32>, k: i32) -> i32 {
        let mut s: i64 = chalk.iter().map(|&x| x as i64).sum();
        let mut k = (k as i64) % s;
        for (i, &x) in chalk.iter().enumerate() {
            if k < (x as i64) {
                return i as i32;
            }
            k -= x as i64;
        }
        0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
