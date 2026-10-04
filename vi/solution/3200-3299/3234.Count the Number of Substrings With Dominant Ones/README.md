---
comments: true
difficulty: Medium
rating: 2556
source: Weekly Contest 408 Q3
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [3234. Count the Number of Substrings With Dominant Ones](https://leetcode.com/problems/count-the-number-of-substrings-with-dominant-ones)

[中文文档](/solution/3200-3299/3234.Count%20the%20Number%20of%20Substrings%20With%20Dominant%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Đã cho một chuỗi nhị phân <code>s</code>.</p>

<p>Trả về số lượng <span data-keyword="substring-nonempty">chuỗi con</span> có số lượng số 1 <strong>chiếm ưu thế</strong>.</p>

<p>Một chuỗi có số lượng số 1 <strong>chiếm ưu thế</strong> nếu số lượng số 1 trong chuỗi <strong>lớn hơn hoặc bằng</strong> <strong>bình phương</strong> của số lượng số 0 trong chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;00011&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con có số lượng số 1 chiếm ưu thế được thể hiện trong bảng dưới đây.</p>
</div>

<table>
	<thead>
		<tr>
			<th>i</th>
			<th>j</th>
			<th>s[i..j]</th>
			<th>Số lượng số 0</th>
			<th>Số lượng số 1</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>3</td>
			<td>3</td>
			<td>1</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td>4</td>
			<td>4</td>
			<td>1</td>
			<td>0</td>
			<td>1</td>
		</tr>
		<tr>
			<td>2</td>
			<td>3</td>
			<td>01</td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr>
			<td>3</td>
			<td>4</td>
			<td>11</td>
			<td>0</td>
			<td>2</td>
		</tr>
		<tr>
			<td>2</td>
			<td>4</td>
			<td>011</td>
			<td>1</td>
			<td>2</td>
		</tr>
	</tbody>
</table>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;101101&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các chuỗi con có số lượng số 1 <strong>không chiếm ưu thế</strong> được thể hiện trong bảng dưới đây.</p>

<p>Vì tổng cộng có 21 chuỗi con và 5 trong số đó có số lượng số 1 không chiếm ưu thế, nên có 16 chuỗi con có số lượng số 1 chiếm ưu thế.</p>
</div>

<table>
	<thead>
		<tr>
			<th>i</th>
			<th>j</th>
			<th>s[i..j]</th>
			<th>Số lượng số 0</th>
			<th>Số lượng số 1</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>1</td>
			<td>0</td>
			<td>1</td>
			<td>0</td>
		</tr>
		<tr>
			<td>4</td>
			<td>4</td>
			<td>0</td>
			<td>1</td>
			<td>0</td>
		</tr>
		<tr>
			<td>1</td>
			<td>4</td>
			<td>0110</td>
			<td>2</td>
			<td>2</td>
		</tr>
		<tr>
			<td>0</td>
			<td>4</td>
			<td>10110</td>
			<td>2</td>
			<td>3</td>
		</tr>
		<tr>
			<td>1</td>
			<td>5</td>
			<td>01101</td>
			<td>2</td>
			<td>3</td>
		</tr>
	</tbody>
</table>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 4 * 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các ký tự <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Một chuỗi con cần $c_1\ge c_0^2$. Vì $n\le 4\times 10^4$, việc kiểm tra mọi cặp đầu mút có độ phức tạp bậc hai. Bất đẳng thức này buộc $c_0\le\sqrt{n}$, nên sau khi cố định đầu trái, ta chỉ cần lần qua một vài số 0.
>
> $\textit{nxt}[i]$ là vị trí của số 0 đầu tiên tại hoặc sau $i$. Với mỗi $i$, ta lần theo các số 0, cập nhật $c_0$ và khi $c_1$ đủ lớn thì cộng số lượng đầu phải hợp lệ. Mỗi vị trí bắt đầu được lần qua $O(\sqrt{n})$ lần.

<!-- thinking:end -->

Theo mô tả bài toán, một chuỗi có số lượng số 1 chiếm ưu thế thỏa mãn $\textit{cnt}_1 \geq \textit{cnt}_0^2$, nghĩa là giá trị lớn nhất của $\textit{cnt}_0$ không vượt quá $\sqrt{n}$, trong đó $n$ là độ dài chuỗi. Vì vậy, ta có thể liệt kê giá trị của $\textit{cnt}_0$, sau đó tính số lượng chuỗi con thỏa mãn điều kiện.

Trước hết, ta tiền xử lý vị trí của số $0$ đầu tiên bắt đầu từ mỗi vị trí trong chuỗi và lưu vào mảng $\textit{nxt}$, trong đó $\textit{nxt}[i]$ biểu thị vị trí của số $0$ đầu tiên bắt đầu từ vị trí $i$, hoặc bằng $n$ nếu không tồn tại.

Tiếp theo, ta duyệt từng vị trí $i$ trong chuỗi làm vị trí bắt đầu của chuỗi con, khởi tạo $\textit{cnt}_0$ bằng $0$ hoặc $1$ (tùy thuộc vị trí hiện tại có phải là $0$ hay không). Sau đó, ta dùng một con trỏ $j$ bắt đầu từ vị trí $i$, lần lượt nhảy đến vị trí của số $0$ tiếp theo và cập nhật giá trị của $\textit{cnt}_0$.

Với chuỗi con bắt đầu tại vị trí $i$ và chứa $\textit{cnt}_0$ số 0, nó có thể chứa nhiều nhất $\textit{nxt}[j + 1] - i - \textit{cnt}_0$ số 1. Nếu giá trị này lớn hơn hoặc bằng $\textit{cnt}_0^2$, thì tồn tại một chuỗi con thỏa mãn điều kiện. Khi đó, ta cần xác định có bao nhiêu vị trí mà đầu phải $\textit{nxt}[j + 1] - 1$ có thể dịch sang trái mà vẫn thỏa mãn điều kiện. Cụ thể, đầu phải có tổng cộng $\min(\textit{nxt}[j + 1] - j, \textit{cnt}_1 - \textit{cnt}_0^2 + 1)$ lựa chọn khả dĩ. Ta cộng dồn số lượng này vào đáp án. Sau đó, ta di chuyển con trỏ $j$ đến vị trí của số $0$ tiếp theo và tiếp tục liệt kê giá trị tiếp theo của $\textit{cnt}_0$ cho đến khi $\textit{cnt}_0^2$ vượt quá độ dài chuỗi.

Độ phức tạp thời gian là $O(n \times \sqrt{n})$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfSubstrings(self, s: str) -> int:
        n = len(s)
        nxt = [n] * (n + 1)
        for i in range(n - 1, -1, -1):
            nxt[i] = nxt[i + 1]
            if s[i] == "0":
                nxt[i] = i
        ans = 0
        for i in range(n):
            cnt0 = int(s[i] == "0")
            j = i
            while j < n and cnt0 * cnt0 <= n:
                cnt1 = (nxt[j + 1] - i) - cnt0
                if cnt1 >= cnt0 * cnt0:
                    ans += min(nxt[j + 1] - j, cnt1 - cnt0 * cnt0 + 1)
                j = nxt[j + 1]
                cnt0 += 1
        return ans
```

#### Java

```java
class Solution {
    public int numberOfSubstrings(String s) {
        int n = s.length();
        int[] nxt = new int[n + 1];
        nxt[n] = n;
        for (int i = n - 1; i >= 0; --i) {
            nxt[i] = nxt[i + 1];
            if (s.charAt(i) == '0') {
                nxt[i] = i;
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt0 = s.charAt(i) == '0' ? 1 : 0;
            int j = i;
            while (j < n && 1L * cnt0 * cnt0 <= n) {
                int cnt1 = nxt[j + 1] - i - cnt0;
                if (cnt1 >= cnt0 * cnt0) {
                    ans += Math.min(nxt[j + 1] - j, cnt1 - cnt0 * cnt0 + 1);
                }
                j = nxt[j + 1];
                ++cnt0;
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
    int numberOfSubstrings(string s) {
        int n = s.size();
        vector<int> nxt(n + 1);
        nxt[n] = n;
        for (int i = n - 1; i >= 0; --i) {
            nxt[i] = nxt[i + 1];
            if (s[i] == '0') {
                nxt[i] = i;
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int cnt0 = s[i] == '0' ? 1 : 0;
            int j = i;
            while (j < n && 1LL * cnt0 * cnt0 <= n) {
                int cnt1 = nxt[j + 1] - i - cnt0;
                if (cnt1 >= cnt0 * cnt0) {
                    ans += min(nxt[j + 1] - j, cnt1 - cnt0 * cnt0 + 1);
                }
                j = nxt[j + 1];
                ++cnt0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfSubstrings(s string) int {
	n := len(s)
	nxt := make([]int, n+1)
	nxt[n] = n
	for i := n - 1; i >= 0; i-- {
		nxt[i] = nxt[i+1]
		if s[i] == '0' {
			nxt[i] = i
		}
	}
	ans := 0
	for i := 0; i < n; i++ {
		cnt0 := 0
		if s[i] == '0' {
			cnt0 = 1
		}
		j := i
		for j < n && int64(cnt0*cnt0) <= int64(n) {
			cnt1 := nxt[j+1] - i - cnt0
			if cnt1 >= cnt0*cnt0 {
				ans += min(nxt[j+1]-j, cnt1-cnt0*cnt0+1)
			}
			j = nxt[j+1]
			cnt0++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function numberOfSubstrings(s: string): number {
    const n = s.length;
    const nxt: number[] = Array(n + 1).fill(0);
    nxt[n] = n;
    for (let i = n - 1; i >= 0; --i) {
        nxt[i] = nxt[i + 1];
        if (s[i] === '0') {
            nxt[i] = i;
        }
    }
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let cnt0 = s[i] === '0' ? 1 : 0;
        let j = i;
        while (j < n && cnt0 * cnt0 <= n) {
            const cnt1 = nxt[j + 1] - i - cnt0;
            if (cnt1 >= cnt0 * cnt0) {
                ans += Math.min(nxt[j + 1] - j, cnt1 - cnt0 * cnt0 + 1);
            }
            j = nxt[j + 1];
            ++cnt0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_substrings(s: String) -> i32 {
        let n = s.len();
        let mut nxt = vec![n; n + 1];

        for i in (0..n).rev() {
            nxt[i] = nxt[i + 1];
            if &s[i..i + 1] == "0" {
                nxt[i] = i;
            }
        }

        let mut ans = 0;
        for i in 0..n {
            let mut cnt0 = if &s[i..i + 1] == "0" { 1 } else { 0 };
            let mut j = i;
            while j < n && (cnt0 * cnt0) as i64 <= n as i64 {
                let cnt1 = nxt[j + 1] - i - cnt0;
                if cnt1 >= (cnt0 * cnt0) {
                    ans += std::cmp::min(nxt[j + 1] - j, cnt1 - cnt0 * cnt0 + 1);
                }
                j = nxt[j + 1];
                cnt0 += 1;
            }
        }

        ans as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
