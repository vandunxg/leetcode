---
comments: true
difficulty: Easy
rating: 1450
source: Biweekly Contest 94 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [2511. Maximum Enemy Forts That Can Be Captured](https://leetcode.com/problems/maximum-enemy-forts-that-can-be-captured)

[中文文档](/solution/2500-2599/2511.Maximum%20Enemy%20Forts%20That%20Can%20Be%20Captured/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>forts</code> <strong>được đánh chỉ số từ 0</strong>, có độ dài <code>n</code>, biểu diễn vị trí của một số pháo đài. <code>forts[i]</code> có thể là <code>-1</code>, <code>0</code> hoặc <code>1</code>, trong đó:</p>

<ul>
	<li><code>-1</code> biểu thị rằng <strong>không có pháo đài</strong> tại vị trí <code>i<sup>th</sup></code>.</li>
	<li><code>0</code> biểu thị rằng có một pháo đài <strong>địch</strong> tại vị trí <code>i<sup>th</sup></code>.</li>
	<li><code>1</code> biểu thị rằng pháo đài tại vị trí <code>i<sup>th</sup></code> nằm dưới quyền chỉ huy của bạn.</li>
</ul>

<p>Bây giờ, bạn quyết định di chuyển quân đội từ một pháo đài của mình tại vị trí <code>i</code> đến một vị trí trống <code>j</code> sao cho:</p>

<ul>
	<li><code>0 &lt;= i, j &lt;= n - 1</code></li>
	<li>Quân đội <strong>chỉ</strong> đi qua các pháo đài địch. Cụ thể, với mọi <code>k</code> thỏa mãn <code>min(i,j) &lt; k &lt; max(i,j)</code>, ta có <code>forts[k] == 0.</code></li>
</ul>

<p>Trong khi di chuyển, tất cả pháo đài địch trên đường đi đều bị <strong>chiếm</strong>.</p>

<p>Trả về <em>số lượng <strong>lớn nhất</strong> pháo đài địch có thể chiếm được</em>. Nếu <strong>không thể</strong> di chuyển quân đội hoặc bạn không có pháo đài nào dưới quyền chỉ huy, trả về <code>0</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> forts = [1,0,0,-1,0,0,0,0,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
- Di chuyển quân đội từ vị trí 0 đến vị trí 3 sẽ chiếm 2 pháo đài địch, tại vị trí 1 và 2.
- Di chuyển quân đội từ vị trí 8 đến vị trí 3 sẽ chiếm 4 pháo đài địch.
Vì 4 là số lượng pháo đài địch lớn nhất có thể chiếm, ta trả về 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> forts = [0,0,1,-1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì không thể chiếm pháo đài địch nào, ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= forts.length &lt;= 1000</code></li>
	<li><code>-1 &lt;= forts[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một lần di chuyển bắt đầu tại pháo đài của ta, đi qua một đoạn các ô trống và dừng tại pháo đài địch; số pháo đài chiếm được là số lượng số 0 ở giữa. Với $n\le 1000$, ta có thể liệt kê các cặp đầu mút, nhưng không cần quét lại về bên phải cho từng $i$.
>
> Đặt $i$ tại một ô khác 0 và để $j$ bỏ qua các số 0 tiếp theo đến số khác 0 đầu tiên. Nếu hai dấu của chúng đối nhau, $j-i-1$ số 0 có thể cập nhật đáp án; sau đó đặt $i$ thành $j$ để duyệt mảng đúng một lần.

<!-- thinking:end -->

Ta dùng một con trỏ $i$ để duyệt mảng $forts$, và một con trỏ $j$ bắt đầu duyệt từ vị trí tiếp theo của $i$ cho đến khi gặp vị trí khác 0 đầu tiên, tức là $forts[j] \neq 0$. Nếu $forts[i] + forts[j] = 0$, ta có thể di chuyển quân đội giữa $i$ và $j$, chiếm $j - i - 1$ pháo đài địch. Ta dùng biến $ans$ để ghi nhận số pháo đài địch lớn nhất có thể chiếm.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng `forts`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def captureForts(self, forts: List[int]) -> int:
        n = len(forts)
        i = ans = 0
        while i < n:
            j = i + 1
            if forts[i]:
                while j < n and forts[j] == 0:
                    j += 1
                if j < n and forts[i] + forts[j] == 0:
                    ans = max(ans, j - i - 1)
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int captureForts(int[] forts) {
        int n = forts.length;
        int ans = 0, i = 0;
        while (i < n) {
            int j = i + 1;
            if (forts[i] != 0) {
                while (j < n && forts[j] == 0) {
                    ++j;
                }
                if (j < n && forts[i] + forts[j] == 0) {
                    ans = Math.max(ans, j - i - 1);
                }
            }
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int n = forts.size();
        int ans = 0, i = 0;
        while (i < n) {
            int j = i + 1;
            if (forts[i] != 0) {
                while (j < n && forts[j] == 0) {
                    ++j;
                }
                if (j < n && forts[i] + forts[j] == 0) {
                    ans = max(ans, j - i - 1);
                }
            }
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func captureForts(forts []int) (ans int) {
	n := len(forts)
	i := 0
	for i < n {
		j := i + 1
		if forts[i] != 0 {
			for j < n && forts[j] == 0 {
				j++
			}
			if j < n && forts[i]+forts[j] == 0 {
				ans = max(ans, j-i-1)
			}
		}
		i = j
	}
	return
}
```

#### TypeScript

```ts
function captureForts(forts: number[]): number {
    const n = forts.length;
    let ans = 0;
    let i = 0;
    while (i < n) {
        let j = i + 1;
        if (forts[i] !== 0) {
            while (j < n && forts[j] === 0) {
                j++;
            }
            if (j < n && forts[i] + forts[j] === 0) {
                ans = Math.max(ans, j - i - 1);
            }
        }
        i = j;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn capture_forts(forts: Vec<i32>) -> i32 {
        let n = forts.len();
        let mut ans = 0;
        let mut i = 0;
        while i < n {
            let mut j = i + 1;
            if forts[i] != 0 {
                while j < n && forts[j] == 0 {
                    j += 1;
                }
                if j < n && forts[i] + forts[j] == 0 {
                    ans = ans.max(j - i - 1);
                }
            }
            i = j;
        }
        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
