---
comments: true
difficulty: Medium
rating: 1702
source: Biweekly Contest 67 Q2
tags:
    - Array
    - Dynamic Programming
    - Prefix Sum
---

<!-- problem:start -->

# [2100. Find Good Days to Rob the Bank](https://leetcode.com/problems/find-good-days-to-rob-the-bank)

[中文文档](/solution/2100-2199/2100.Find%20Good%20Days%20to%20Rob%20the%20Bank/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn và một băng nhóm trộm đang lên kế hoạch cướp ngân hàng. Cho một mảng số nguyên <code>security</code> <strong>được đánh số từ 0</strong>, trong đó <code>security[i]</code> là số bảo vệ làm nhiệm vụ vào ngày thứ <code>i<sup>th</sup></code>. Các ngày được đánh số bắt đầu từ <code>0</code>. Bạn cũng được cho một số nguyên <code>time</code>.</p>

<p>Ngày thứ <code>i<sup>th</sup></code> là một ngày thích hợp để cướp ngân hàng nếu:</p>

<ul>
	<li>Có ít nhất <code>time</code> ngày ở trước và sau ngày thứ <code>i<sup>th</sup></code>,</li>
	<li>Số bảo vệ tại ngân hàng trong <code>time</code> ngày <strong>trước</strong> ngày <code>i</code> là <strong>không tăng</strong>, và</li>
	<li>Số bảo vệ tại ngân hàng trong <code>time</code> ngày <strong>sau</strong> ngày <code>i</code> là <strong>không giảm</strong>.</li>
</ul>

<p>Cụ thể hơn, điều này có nghĩa là ngày <code>i</code> là một ngày thích hợp để cướp ngân hàng khi và chỉ khi <code>security[i - time] &gt;= security[i - time + 1] &gt;= ... &gt;= security[i] &lt;= ... &lt;= security[i + time - 1] &lt;= security[i + time]</code>.</p>

<p>Trả về <em>danh sách <strong>tất cả</strong> các ngày <strong>(đánh số từ 0) </strong>thích hợp để cướp ngân hàng</em>.<em> Thứ tự các ngày được trả về <strong> </strong><strong>không</strong> quan trọng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> security = [5,3,3,3,5,6,2], time = 2
<strong>Đầu ra:</strong> [2,3]
<strong>Giải thích:</strong>
Vào ngày 2, ta có security[0] &gt;= security[1] &gt;= security[2] &lt;= security[3] &lt;= security[4].
Vào ngày 3, ta có security[1] &gt;= security[2] &gt;= security[3] &lt;= security[4] &lt;= security[5].
Không có ngày nào khác thỏa mãn điều kiện này, nên ngày 2 và ngày 3 là hai ngày thích hợp duy nhất để cướp ngân hàng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> security = [1,1,1,1,1], time = 0
<strong>Đầu ra:</strong> [0,1,2,3,4]
<strong>Giải thích:</strong>
Vì time bằng 0, mọi ngày đều là ngày thích hợp để cướp ngân hàng, nên trả về tất cả các ngày.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> security = [1,2,3,4,5,6], time = 2
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong>
Không có ngày nào có 2 ngày trước đó với số bảo vệ không tăng.
Do đó, không có ngày nào thích hợp để cướp ngân hàng, nên trả về một danh sách rỗng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= security.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= security[i], time &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Việc kiểm tra từng ngày ứng viên $i$ bằng cách duyệt $\textit{time}$ ngày sang trái và sang phải tốn $O(\textit{time})$ cho mỗi chỉ số. Khi $n$ và $\textit{time}$ đều có thể lên tới $10^5$, cách này không đáp ứng được.
>
> Các đoạn không tăng / không giảm cần thiết có thể được tích lũy dọc theo mảng: nếu $\textit{security}[i]\le \textit{security}[i-1]$, độ dài đoạn không tăng về bên trái tại $i$ bằng độ dài tại $i-1$ cộng một; nếu không thì đặt lại. Độ dài đoạn không giảm về bên phải cũng tương tự. Vì vậy, cả hai phía có thể được tính bằng hai lượt duyệt tuyến tính.
>
> Do đó, ta duy trì các mảng $\textit{left}$ và $\textit{right}$, sau đó thu thập các chỉ số thỏa mãn $\min(\textit{left}[i],\textit{right}[i])\ge \textit{time}$. Nếu $n\le 2\cdot\textit{time}$, không ngày nào có thể có $\textit{time}$ hàng xóm ở cả hai phía, nên đáp án là rỗng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def goodDaysToRobBank(self, security: List[int], time: int) -> List[int]:
        n = len(security)
        if n <= time * 2:
            return []
        left, right = [0] * n, [0] * n
        for i in range(1, n):
            if security[i] <= security[i - 1]:
                left[i] = left[i - 1] + 1
        for i in range(n - 2, -1, -1):
            if security[i] <= security[i + 1]:
                right[i] = right[i + 1] + 1
        return [i for i in range(n) if time <= min(left[i], right[i])]
```

#### Java

```java
class Solution {
    public List<Integer> goodDaysToRobBank(int[] security, int time) {
        int n = security.length;
        if (n <= time * 2) {
            return Collections.emptyList();
        }
        int[] left = new int[n];
        int[] right = new int[n];
        for (int i = 1; i < n; ++i) {
            if (security[i] <= security[i - 1]) {
                left[i] = left[i - 1] + 1;
            }
        }
        for (int i = n - 2; i >= 0; --i) {
            if (security[i] <= security[i + 1]) {
                right[i] = right[i + 1] + 1;
            }
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = time; i < n - time; ++i) {
            if (time <= Math.min(left[i], right[i])) {
                ans.add(i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> goodDaysToRobBank(vector<int>& security, int time) {
        int n = security.size();
        if (n <= time * 2) return {};
        vector<int> left(n);
        vector<int> right(n);
        for (int i = 1; i < n; ++i)
            if (security[i] <= security[i - 1])
                left[i] = left[i - 1] + 1;
        for (int i = n - 2; i >= 0; --i)
            if (security[i] <= security[i + 1])
                right[i] = right[i + 1] + 1;
        vector<int> ans;
        for (int i = time; i < n - time; ++i)
            if (time <= min(left[i], right[i]))
                ans.push_back(i);
        return ans;
    }
};
```

#### Go

```go
func goodDaysToRobBank(security []int, time int) []int {
	n := len(security)
	if n <= time*2 {
		return []int{}
	}
	left := make([]int, n)
	right := make([]int, n)
	for i := 1; i < n; i++ {
		if security[i] <= security[i-1] {
			left[i] = left[i-1] + 1
		}
	}
	for i := n - 2; i >= 0; i-- {
		if security[i] <= security[i+1] {
			right[i] = right[i+1] + 1
		}
	}
	var ans []int
	for i := time; i < n-time; i++ {
		if time <= left[i] && time <= right[i] {
			ans = append(ans, i)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function goodDaysToRobBank(security: number[], time: number): number[] {
    const n = security.length;
    if (n <= time * 2) {
        return [];
    }
    const l = new Array(n).fill(0);
    const r = new Array(n).fill(0);
    for (let i = 1; i < n; i++) {
        if (security[i] <= security[i - 1]) {
            l[i] = l[i - 1] + 1;
        }
        if (security[n - i - 1] <= security[n - i]) {
            r[n - i - 1] = r[n - i] + 1;
        }
    }
    const res = [];
    for (let i = time; i < n - time; i++) {
        if (time <= Math.min(l[i], r[i])) {
            res.push(i);
        }
    }
    return res;
}
```

#### Rust

```rust
use std::cmp::Ordering;

impl Solution {
    pub fn good_days_to_rob_bank(security: Vec<i32>, time: i32) -> Vec<i32> {
        let time = time as usize;
        let n = security.len();
        if time * 2 >= n {
            return vec![];
        }
        let mut g = vec![0; n];
        for i in 1..n {
            g[i] = match security[i].cmp(&security[i - 1]) {
                Ordering::Less => -1,
                Ordering::Greater => 1,
                Ordering::Equal => 0,
            };
        }
        let (mut a, mut b) = (vec![0; n + 1], vec![0; n + 1]);
        for i in 1..=n {
            a[i] = a[i - 1] + (if g[i - 1] == 1 { 1 } else { 0 });
            b[i] = b[i - 1] + (if g[i - 1] == -1 { 1 } else { 0 });
        }
        let mut res = vec![];
        for i in time..n - time {
            if a[i + 1] - a[i + 1 - time] == 0 && b[i + 1 + time] - b[i + 1] == 0 {
                res.push(i as i32);
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
