---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [822. Card Flipping Game](https://leetcode.com/problems/card-flipping-game)

[中文文档](/solution/0800-0899/0822.Card%20Flipping%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>fronts</code> và <code>backs</code> có độ dài <code>n</code>, trong đó lá bài thứ <code>i<sup>th</sup></code> có số nguyên dương <code>fronts[i]</code> ở mặt trước và <code>backs[i]</code> ở mặt sau. Ban đầu, mỗi lá bài được đặt trên bàn sao cho số ở mặt trước hướng lên và số ở mặt sau hướng xuống. Bạn có thể lật tùy ý số lá bài (có thể không lật lá nào).</p>

<p>Sau khi lật bài, một số nguyên được xem là <strong>tốt</strong> nếu nó nằm úp trên một lá bài và <strong>không</strong> nằm ngửa trên bất kỳ lá bài nào.</p>

<p>Hãy trả về <em>số nguyên tốt nhỏ nhất có thể đạt được sau khi lật bài</em>. Nếu không có số nguyên tốt nào, trả về <code>0</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fronts = [1,2,4,4,7], backs = [1,3,4,1,3]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Nếu lật lá bài thứ hai, các số ngửa lên là [1,3,4,4,7] và các số úp xuống là [1,2,4,1,3].
2 là số nguyên tốt nhỏ nhất vì nó xuất hiện ở mặt úp nhưng không xuất hiện ở mặt ngửa.
Có thể chứng minh 2 là số nguyên tốt nhỏ nhất có thể đạt được khi lật một số lá bài.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> fronts = [1], backs = [1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong>
Dù lật bài thế nào cũng không có số nguyên tốt nào, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == fronts.length == backs.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= fronts[i], backs[i] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Một số xuất hiện ở cả hai mặt của cùng một lá bài không thể là số nguyên tốt nếu nằm ở mặt ngửa. Vì $n\le 1000$, trước tiên hãy thu thập mọi giá trị giống nhau ở mặt trước và mặt sau.
>
> Đáp án là giá trị nhỏ nhất trong các giá trị còn lại, hoặc $0$ nếu không có giá trị nào.

<!-- thinking:end -->

Ta nhận thấy tại vị trí $i$, nếu $\textit{fronts}[i]$ bằng $\textit{backs}[i]$ thì giá trị đó chắc chắn không thỏa mãn điều kiện.

Vì vậy, trước tiên ta tìm tất cả phần tử có cùng giá trị ở cả mặt trước lẫn mặt sau, rồi lưu chúng vào hash set $s$.

Tiếp theo, ta duyệt tất cả phần tử trong cả hai mảng mặt trước và mặt sau. Với mỗi phần tử $x$ **không** nằm trong hash set $s$, ta cập nhật giá trị nhỏ nhất của đáp án.

Cuối cùng, nếu tìm được phần tử thỏa mãn điều kiện, ta trả về giá trị nhỏ nhất; nếu không thì trả về $0$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của các mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def flipgame(self, fronts: List[int], backs: List[int]) -> int:
        s = {a for a, b in zip(fronts, backs) if a == b}
        return min((x for x in chain(fronts, backs) if x not in s), default=0)
```

#### Java

```java
class Solution {
    public int flipgame(int[] fronts, int[] backs) {
        Set<Integer> s = new HashSet<>();
        int n = fronts.length;
        for (int i = 0; i < n; ++i) {
            if (fronts[i] == backs[i]) {
                s.add(fronts[i]);
            }
        }
        int ans = 9999;
        for (int v : fronts) {
            if (!s.contains(v)) {
                ans = Math.min(ans, v);
            }
        }
        for (int v : backs) {
            if (!s.contains(v)) {
                ans = Math.min(ans, v);
            }
        }
        return ans % 9999;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int flipgame(vector<int>& fronts, vector<int>& backs) {
        unordered_set<int> s;
        int n = fronts.size();
        for (int i = 0; i < n; ++i) {
            if (fronts[i] == backs[i]) {
                s.insert(fronts[i]);
            }
        }
        int ans = 9999;
        for (int& v : fronts) {
            if (!s.count(v)) {
                ans = min(ans, v);
            }
        }
        for (int& v : backs) {
            if (!s.count(v)) {
                ans = min(ans, v);
            }
        }
        return ans % 9999;
    }
};
```

#### Go

```go
func flipgame(fronts []int, backs []int) int {
	s := map[int]struct{}{}
	for i, a := range fronts {
		if a == backs[i] {
			s[a] = struct{}{}
		}
	}
	ans := 9999
	for _, v := range fronts {
		if _, ok := s[v]; !ok {
			ans = min(ans, v)
		}
	}
	for _, v := range backs {
		if _, ok := s[v]; !ok {
			ans = min(ans, v)
		}
	}
	return ans % 9999
}
```

#### TypeScript

```ts
function flipgame(fronts: number[], backs: number[]): number {
    const s: Set<number> = new Set();
    const n = fronts.length;
    for (let i = 0; i < n; ++i) {
        if (fronts[i] === backs[i]) {
            s.add(fronts[i]);
        }
    }
    let ans = 9999;
    for (const v of fronts) {
        if (!s.has(v)) {
            ans = Math.min(ans, v);
        }
    }
    for (const v of backs) {
        if (!s.has(v)) {
            ans = Math.min(ans, v);
        }
    }
    return ans % 9999;
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn flipgame(fronts: Vec<i32>, backs: Vec<i32>) -> i32 {
        let n = fronts.len();
        let mut s: HashSet<i32> = HashSet::new();

        for i in 0..n {
            if fronts[i] == backs[i] {
                s.insert(fronts[i]);
            }
        }

        let mut ans = 9999;
        for &v in fronts.iter() {
            if !s.contains(&v) {
                ans = ans.min(v);
            }
        }
        for &v in backs.iter() {
            if !s.contains(&v) {
                ans = ans.min(v);
            }
        }

        if ans == 9999 {
            0
        } else {
            ans
        }
    }
}
```

#### C#

```cs
public class Solution {
    public int Flipgame(int[] fronts, int[] backs) {
        var s = new HashSet<int>();
        int n = fronts.Length;
        for (int i = 0; i < n; ++i) {
            if (fronts[i] == backs[i]) {
                s.Add(fronts[i]);
            }
        }
        int ans = 9999;
        for (int i = 0; i < n; ++i) {
            if (!s.Contains(fronts[i])) {
                ans = Math.Min(ans, fronts[i]);
            }
            if (!s.Contains(backs[i])) {
                ans = Math.Min(ans, backs[i]);
            }
        }
        return ans % 9999;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
