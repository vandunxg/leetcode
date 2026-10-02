---
comments: true
difficulty: Easy
tags:
    - Graph
    - Array
    - Hash Table
---

<!-- problem:start -->

# [997. Find the Town Judge](https://leetcode.com/problems/find-the-town-judge)

[中文文档](/solution/0900-0999/0997.Find%20the%20Town%20Judge/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một thị trấn có <code>n</code> người, được đánh số từ <code>1</code> đến <code>n</code>. Có tin đồn rằng một trong số họ bí mật là thẩm phán của thị trấn.</p>

<p>Nếu thẩm phán của thị trấn tồn tại, người đó thỏa mãn các điều kiện sau:</p>

<ol>
	<li>Thẩm phán không tin tưởng ai.</li>
	<li>Mọi người khác đều tin tưởng thẩm phán.</li>
	<li>Chỉ có duy nhất một người thỏa mãn cả điều kiện <strong>1</strong> và <strong>2</strong>.</li>
</ol>

<p>Cho mảng <code>trust</code>, trong đó <code>trust[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị người mang số <code>a<sub>i</sub></code> tin tưởng người mang số <code>b<sub>i</sub></code>. Nếu một quan hệ tin tưởng không xuất hiện trong mảng <code>trust</code>, thì quan hệ đó không tồn tại.</p>

<p>Nếu thẩm phán tồn tại và có thể xác định được, hãy trả về số của người đó; nếu không, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, trust = [[1,2]]
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, trust = [[1,3],[2,3]]
<strong>Đầu ra:</strong> 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, trust = [[1,3],[2,3],[3,1]]
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= trust.length &lt;= 10<sup>4</sup></code></li>
	<li><code>trust[i].length == 2</code></li>
	<li>Tất cả các cặp trong <code>trust</code> đều <strong>khác nhau</strong>.</li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Thẩm phán không tin tưởng ai và được $n-1$ người còn lại tin tưởng. Vì vậy, đó là node duy nhất có bậc ra bằng $0$ và bậc vào bằng $n-1$. Dùng hai mảng đếm số người mỗi người tin tưởng và số người tin tưởng họ; duyệt $trust$ một lần rồi kiểm tra từng số người.

<!-- thinking:end -->

Tạo hai mảng $cnt1$ và $cnt2$ có độ dài $n + 1$, lần lượt lưu số người mà mỗi người tin tưởng và số người tin tưởng từng người.

Tiếp theo, duyệt mảng $trust$. Với mỗi cặp $[a_i, b_i]$, tăng $cnt1[a_i]$ và $cnt2[b_i]$ thêm $1$.

Cuối cùng, xét từng người $i$ trong đoạn $[1,..n]$. Nếu $cnt1[i] = 0$ và $cnt2[i] = n - 1$, thì $i$ là thẩm phán của thị trấn, nên trả về $i$. Nếu duyệt hết mà không tìm thấy người nào như vậy, trả về $-1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $trust$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        cnt1 = [0] * (n + 1)
        cnt2 = [0] * (n + 1)
        for a, b in trust:
            cnt1[a] += 1
            cnt2[b] += 1
        for i in range(1, n + 1):
            if cnt1[i] == 0 and cnt2[i] == n - 1:
                return i
        return -1
```

#### Java

```java
class Solution {
    public int findJudge(int n, int[][] trust) {
        int[] cnt1 = new int[n + 1];
        int[] cnt2 = new int[n + 1];
        for (var t : trust) {
            int a = t[0], b = t[1];
            ++cnt1[a];
            ++cnt2[b];
        }
        for (int i = 1; i <= n; ++i) {
            if (cnt1[i] == 0 && cnt2[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<int> cnt1(n + 1);
        vector<int> cnt2(n + 1);
        for (auto& t : trust) {
            int a = t[0], b = t[1];
            ++cnt1[a];
            ++cnt2[b];
        }
        for (int i = 1; i <= n; ++i) {
            if (cnt1[i] == 0 && cnt2[i] == n - 1) {
                return i;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func findJudge(n int, trust [][]int) int {
	cnt1 := make([]int, n+1)
	cnt2 := make([]int, n+1)
	for _, t := range trust {
		a, b := t[0], t[1]
		cnt1[a]++
		cnt2[b]++
	}
	for i := 1; i <= n; i++ {
		if cnt1[i] == 0 && cnt2[i] == n-1 {
			return i
		}
	}
	return -1
}
```

#### TypeScript

```ts
function findJudge(n: number, trust: number[][]): number {
    const cnt1: number[] = new Array(n + 1).fill(0);
    const cnt2: number[] = new Array(n + 1).fill(0);
    for (const [a, b] of trust) {
        ++cnt1[a];
        ++cnt2[b];
    }
    for (let i = 1; i <= n; ++i) {
        if (cnt1[i] === 0 && cnt2[i] === n - 1) {
            return i;
        }
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_judge(n: i32, trust: Vec<Vec<i32>>) -> i32 {
        let mut cnt1 = vec![0; (n + 1) as usize];
        let mut cnt2 = vec![0; (n + 1) as usize];

        for t in trust.iter() {
            let a = t[0] as usize;
            let b = t[1] as usize;
            cnt1[a] += 1;
            cnt2[b] += 1;
        }

        for i in 1..=n as usize {
            if cnt1[i] == 0 && cnt2[i] == (n as usize) - 1 {
                return i as i32;
            }
        }

        -1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
