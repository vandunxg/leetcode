---
comments: true
difficulty: Easy
rating: 1332
source: Weekly Contest 146 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [1128. Number of Equivalent Domino Pairs](https://leetcode.com/problems/number-of-equivalent-domino-pairs)

[中文文档](/solution/1100-1199/1128.Number%20of%20Equivalent%20Domino%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách <code>dominoes</code>. <code>dominoes[i] = [a, b]</code> <strong>tương đương với</strong> <code>dominoes[j] = [c, d]</code> khi và chỉ khi (<code>a == c</code> và <code>b == d</code>) hoặc (<code>a == d</code> và <code>b == c</code>); nghĩa là có thể xoay một quân domino để nó giống quân còn lại.</p>

<p>Trả về <em>số cặp </em><code>(i, j)</code><em> thỏa mãn </em><code>0 &lt;= i &lt; j &lt; dominoes.length</code><em> và </em><code>dominoes[i]</code><em> <strong>tương đương với</strong> </em><code>dominoes[j]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> dominoes = [[1,2],[2,1],[3,4],[5,6]]
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> dominoes = [[1,2],[1,2],[1,1],[1,2],[2,2]]
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= dominoes.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>dominoes[i].length == 2</code></li>
	<li><code>1 &lt;= dominoes[i][j] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các cặp xem $[a,b]$ và $[b,a]$ là như nhau. Nếu với mỗi quân mới đều duyệt tất cả quân trước đó thì độ phức tạp là bậc hai. Mã hóa $\min(a,b)$ ở hàng chục và $\max(a,b)$ ở hàng đơn vị để tạo key trong khoảng $0..99$. Cộng số lần key này đã xuất hiện vào đáp án rồi tăng bộ đếm, nhờ đó mỗi quân chỉ được ghép với các quân tương đương đã gặp trước nó.

<!-- thinking:end -->

Ta có thể ghép hai số của mỗi quân domino theo thứ tự tăng dần để tạo thành một số có hai chữ số; khi đó các quân tương đương sẽ có cùng số biểu diễn. Ví dụ, cả `[1, 2]` và `[2, 1]` đều được ghép thành `12`, còn `[3, 4]` và `[4, 3]` đều thành `34`.

Sau đó, duyệt tất cả quân domino và dùng mảng $cnt$ có độ dài $100$ để ghi số lần xuất hiện của mỗi số có hai chữ số. Với mỗi quân, gọi số được ghép là $x$; cộng $cnt[x]$ vào đáp án rồi tăng $cnt[x]$ lên $1$. Tiếp tục với quân kế tiếp để đếm tổng số cặp domino tương đương.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(C)$, trong đó $n$ là số quân domino, còn $C$ là số lượng tối đa các số có hai chữ số có thể ghép từ quân domino, bằng $100$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numEquivDominoPairs(self, dominoes: List[List[int]]) -> int:
        cnt = Counter()
        ans = 0
        for a, b in dominoes:
            x = a * 10 + b if a < b else b * 10 + a
            ans += cnt[x]
            cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int numEquivDominoPairs(int[][] dominoes) {
        int[] cnt = new int[100];
        int ans = 0;
        for (var e : dominoes) {
            int x = e[0] < e[1] ? e[0] * 10 + e[1] : e[1] * 10 + e[0];
            ans += cnt[x]++;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numEquivDominoPairs(vector<vector<int>>& dominoes) {
        int cnt[100]{};
        int ans = 0;
        for (auto& e : dominoes) {
            int x = e[0] < e[1] ? e[0] * 10 + e[1] : e[1] * 10 + e[0];
            ans += cnt[x]++;
        }
        return ans;
    }
};
```

#### Go

```go
func numEquivDominoPairs(dominoes [][]int) (ans int) {
	cnt := [100]int{}
	for _, e := range dominoes {
		x := e[0]*10 + e[1]
		if e[0] > e[1] {
			x = e[1]*10 + e[0]
		}
		ans += cnt[x]
		cnt[x]++
	}
	return
}
```

#### TypeScript

```ts
function numEquivDominoPairs(dominoes: number[][]): number {
    const cnt: number[] = new Array(100).fill(0);
    let ans = 0;

    for (const [a, b] of dominoes) {
        const key = a < b ? a * 10 + b : b * 10 + a;
        ans += cnt[key];
        cnt[key]++;
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn num_equiv_domino_pairs(dominoes: Vec<Vec<i32>>) -> i32 {
        let mut cnt = [0i32; 100];
        let mut ans = 0;

        for d in dominoes {
            let a = d[0] as usize;
            let b = d[1] as usize;
            let key = if a < b { a * 10 + b } else { b * 10 + a };
            ans += cnt[key];
            cnt[key] += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
