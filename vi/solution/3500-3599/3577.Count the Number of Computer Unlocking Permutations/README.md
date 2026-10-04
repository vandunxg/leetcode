---
comments: true
difficulty: Medium
rating: 1749
source: Weekly Contest 453 Q2
tags:
    - Brainteaser
    - Array
    - Math
    - Combinatorics
---

<!-- problem:start -->

# [3577. Count the Number of Computer Unlocking Permutations](https://leetcode.com/problems/count-the-number-of-computer-unlocking-permutations)

[中文文档](/solution/3500-3599/3577.Count%20the%20Number%20of%20Computer%20Unlocking%20Permutations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>complexity</code> có độ dài <code>n</code>.</p>

<p>Có <code>n</code> máy tính <strong>bị khóa</strong> trong một căn phòng, được đánh nhãn từ 0 đến <code>n - 1</code>, mỗi máy có một mật khẩu <strong>duy nhất</strong>. Mật khẩu của máy tính <code>i</code> có độ phức tạp là <code>complexity[i]</code>.</p>

<p>Mật khẩu của máy tính có nhãn 0 đã được <strong>giải mã</strong> và đóng vai trò là nút gốc. Tất cả các máy tính khác phải được mở khóa bằng mật khẩu của máy tính này hoặc một máy tính đã được mở khóa trước đó, theo các quy tắc sau:</p>

<ul>
	<li>Bạn có thể giải mã mật khẩu của máy tính <code>i</code> bằng mật khẩu của máy tính <code>j</code>, trong đó <code>j</code> là một số nguyên <strong>nhỏ hơn</strong> <code>i</code> bất kỳ có độ phức tạp thấp hơn. (tức là <code>j &lt; i</code> và <code>complexity[j] &lt; complexity[i]</code>)</li>
	<li>Để giải mã mật khẩu của máy tính <code>i</code>, bạn phải mở khóa trước một máy tính <code>j</code> sao cho <code>j &lt; i</code> và <code>complexity[j] &lt; complexity[i]</code>.</li>
</ul>

<p>Hãy tìm số <span data-keyword="permutation-array">hoán vị</span> của <code>[0, 1, 2, ..., (n - 1)]</code> biểu diễn một thứ tự hợp lệ để mở khóa các máy tính, bắt đầu với máy tính 0 là máy tính duy nhất được mở khóa ban đầu.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>chia dư</strong> cho 10<sup>9</sup> + 7.</p>

<p><strong>Lưu ý</strong> rằng mật khẩu của máy tính <strong>có nhãn</strong> 0 được giải mã, <em>không phải</em> máy tính ở vị trí đầu tiên trong hoán vị.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">complexity = [1,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các hoán vị hợp lệ là:</p>

<ul>
	<li>[0, 1, 2]
	<ul>
		<li>Mở khóa máy tính 0 trước tiên bằng mật khẩu gốc.</li>
		<li>Mở khóa máy tính 1 bằng mật khẩu của máy tính 0 vì <code>complexity[0] &lt; complexity[1]</code>.</li>
		<li>Mở khóa máy tính 2 bằng mật khẩu của máy tính 1 vì <code>complexity[1] &lt; complexity[2]</code>.</li>
	</ul>
	</li>
	<li>[0, 2, 1]
	<ul>
		<li>Mở khóa máy tính 0 trước tiên bằng mật khẩu gốc.</li>
		<li>Mở khóa máy tính 2 bằng mật khẩu của máy tính 0 vì <code>complexity[0] &lt; complexity[2]</code>.</li>
		<li>Mở khóa máy tính 1 bằng mật khẩu của máy tính 0 vì <code>complexity[0] &lt; complexity[1]</code>.</li>
	</ul>
	</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">complexity = [3,3,3,4,4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có hoán vị nào có thể mở khóa tất cả các máy tính.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= complexity.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= complexity[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Máy tính $0$ được mở khóa ngay từ đầu; mọi máy khác chỉ có thể được mở bằng một máy đã mở khóa có độ phức tạp thấp hơn nghiêm ngặt. Nếu có một $i>0$ sao cho $\textit{complexity}[i] \le \textit{complexity}[0]$, máy đó sẽ không bao giờ mở được.
>
> Ngược lại, $0$ có thể mở khóa tất cả các máy còn lại, và thứ tự của các máy còn lại là một hoán vị bất kỳ của $\{1,\ldots,n-1\}$, tức là $(n-1)!$. Ta nhân dần trong khi duyệt.

<!-- thinking:end -->

Vì mật khẩu của máy tính số $0$ đã được mở khóa, với mọi máy tính khác $i$, nếu $\text{complexity}[i] \leq \text{complexity}[0]$, thì không thể mở khóa máy tính $i$, nên ta trả về $0$. Ngược lại, mọi hoán vị đều hợp lệ và có đúng $(n - 1)!$ hoán vị.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\text{complexity}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPermutations(self, complexity: List[int]) -> int:
        mod = 10**9 + 7
        ans = 1
        for i in range(1, len(complexity)):
            if complexity[i] <= complexity[0]:
                return 0
            ans = ans * i % mod
        return ans
```

#### Java

```java
class Solution {
    public int countPermutations(int[] complexity) {
        final int mod = (int) 1e9 + 7;
        long ans = 1;
        for (int i = 1; i < complexity.length; ++i) {
            if (complexity[i] <= complexity[0]) {
                return 0;
            }
            ans = ans * i % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPermutations(vector<int>& complexity) {
        const int mod = 1e9 + 7;
        long long ans = 1;
        for (int i = 1; i < complexity.size(); ++i) {
            if (complexity[i] <= complexity[0]) {
                return 0;
            }
            ans = ans * i % mod;
        }
        return ans;
    }
};
```

#### Go

```go
func countPermutations(complexity []int) int {
	mod := int64(1e9 + 7)
	ans := int64(1)
	for i := 1; i < len(complexity); i++ {
		if complexity[i] <= complexity[0] {
			return 0
		}
		ans = ans * int64(i) % mod
	}
	return int(ans)
}
```

#### TypeScript

```ts
function countPermutations(complexity: number[]): number {
    const mod = 1e9 + 7;
    let ans = 1;
    for (let i = 1; i < complexity.length; i++) {
        if (complexity[i] <= complexity[0]) {
            return 0;
        }
        ans = (ans * i) % mod;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_permutations(complexity: Vec<i32>) -> i32 {
        const MOD: i64 = 1_000_000_007;
        let mut ans = 1i64;
        for i in 1..complexity.len() {
            if complexity[i] <= complexity[0] {
                return 0;
            }
            ans = ans * i as i64 % MOD;
        }
        ans as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int CountPermutations(int[] complexity) {
        const int mod = (int) 1e9 + 7;
        long ans = 1;
        for (int i = 1; i < complexity.Length; ++i) {
            if (complexity[i] <= complexity[0]) {
                return 0;
            }
            ans = ans * i % mod;
        }
        return (int) ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
