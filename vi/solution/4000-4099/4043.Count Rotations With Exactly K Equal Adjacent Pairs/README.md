---
comments: true
difficulty: Easy
rating: 1209
source: Weekly Contest 518 Q1
---

<!-- problem:start -->

# [4043. Count Rotations With Exactly K Equal Adjacent Pairs](https://leetcode.com/problems/count-rotations-with-exactly-k-equal-adjacent-pairs)

[Tài liệu tiếng Trung](/solution/4000-4099/4043.Count%20Rotations%20With%20Exactly%20K%20Equal%20Adjacent%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một chuỗi <code>s</code> có độ dài <code>n</code> và một số nguyên <code>k</code>.</p>

<p>Một <strong>phép xoay vòng</strong> của <code>s</code> được tạo ra bằng cách chọn một <span data-keyword="string-prefix">tiền tố</span> của <code>s</code> có độ dài từ 0 đến <code>n - 1</code> (bao gồm cả hai đầu), rồi chuyển tiền tố đó ra cuối chuỗi trong khi vẫn giữ nguyên thứ tự của tất cả các ký tự.</p>

<p>Với <strong>mỗi phép xoay vòng</strong> của <code>s</code>, gọi <strong>điểm số</strong> là số lượng chỉ số <code>i</code> sao cho <code>0 &lt;= i &lt; n - 1</code> và các ký tự ở vị trí <code>i</code> và <code>i + 1</code> bằng nhau.</p>

<p>Trả về số lượng phép xoay vòng của <code>s</code> có điểm số bằng <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aab&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép xoay vòng của <code>s</code> là:</p>

<ul>
	<li><code>&quot;aab&quot;</code>: Các ký tự ở vị trí 0 và 1 bằng nhau, nên <code>score = 1</code>.</li>
	<li><code>&quot;aba&quot;</code>: Không có hai ký tự liền kề nào bằng nhau, nên <code>score = 0</code>.</li>
	<li><code>&quot;baa&quot;</code>: Các ký tự ở vị trí 1 và 2 bằng nhau, nên <code>score = 1</code>.</li>
</ul>

<p>Vì <code>score</code> bằng <code>k</code> trong 2 phép xoay vòng của <code>s</code>, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abca&quot;, k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phép xoay vòng của <code>s</code> là:</p>

<ul>
	<li><code>&quot;abca&quot;</code>: Không có hai ký tự liền kề nào bằng nhau, nên <code>score = 0</code>.</li>
	<li><code>&quot;bcaa&quot;</code>: Các ký tự ở vị trí 2 và 3 bằng nhau, nên <code>score = 1</code>.</li>
	<li><code>&quot;caab&quot;</code>: Các ký tự ở vị trí 1 và 2 bằng nhau, nên <code>score = 1</code>.</li>
	<li><code>&quot;aabc&quot;</code>: Các ký tự ở vị trí 0 và 1 bằng nhau, nên <code>score = 1</code>.</li>
</ul>

<p>Vì <code>score</code> chỉ bằng <code>k</code> trong 1 phép xoay vòng của <code>s</code>, đáp án là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == s.length &lt;= 100</code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>0 &lt;= k &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một phép dịch trái theo vòng chỉ thay đổi hai cặp ký tự liền kề ở hai đầu; các cặp bằng nhau ở giữa vẫn giữ nguyên. Nếu dựng lại chuỗi cho từng phép xoay thì độ phức tạp sẽ là bậc hai.
>
> Trước hết tính điểm số của chuỗi ban đầu, sau đó trong $O(1)$ trừ đi cặp đầu bị mất và cộng thêm cặp cuối mới, rồi đếm số lần điểm số bằng $k$.
>
> Dùng chỉ số modulo $n$ để thực hiện tất cả $n$ phép xoay trên chuỗi ban đầu.

<!-- thinking:end -->

Gọi $n$ là độ dài của chuỗi. Trước tiên tính điểm số của chuỗi ban đầu $s$, tức là số lượng chỉ số $i$ sao cho $s[i] = s[i + 1]$ ($0 \leq i < n - 1$). Nếu $\textit{score} = k$, tăng đáp án thêm $1$.

Sau đó bắt đầu từ chuỗi ban đầu và dịch vòng sang trái một ký tự, tổng cộng $n - 1$ lần. Ở lần dịch thứ $t$ ($t = 0, 1, \ldots, n - 2$), ký tự được chuyển ra cuối là $s[t]$, và điểm số chỉ thay đổi ở hai vị trí:

- cặp ký tự liền kề ở đầu biến mất, cụ thể là $s[t]$ và $s[t + 1]$;
- một cặp ký tự liền kề mới xuất hiện ở cuối, cụ thể là $s[t - 1]$ và $s[t]$.

Tất cả các chỉ số được lấy modulo $n$. Do đó, có thể cập nhật $\textit{score}$ trong $O(1)$ thời gian và đếm các phép xoay vòng có điểm số bằng $k$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countRotations(self, s: str, k: int) -> int:
        n = len(s)
        score = sum(a == b for a, b in pairwise(s))
        ans = int(score == k)

        for i in range(n, n * 2 - 1):
            score += int(s[i % n] == s[(i - 1) % n])
            score -= int(s[i % n] == s[(i + 1) % n])
            ans += int(score == k)

        return ans
```

#### Java

```java
class Solution {
    public int countRotations(String s, int k) {
        int n = s.length();
        int score = 0;

        for (int i = 0; i < n - 1; i++) {
            score += s.charAt(i) == s.charAt(i + 1) ? 1 : 0;
        }

        int ans = score == k ? 1 : 0;

        for (int i = n; i < n * 2 - 1; i++) {
            score += s.charAt(i % n) == s.charAt((i - 1) % n) ? 1 : 0;
            score -= s.charAt(i % n) == s.charAt((i + 1) % n) ? 1 : 0;
            ans += score == k ? 1 : 0;
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countRotations(string s, int k) {
        int n = s.size();
        int score = 0;

        for (int i = 0; i < n - 1; i++) {
            score += s[i] == s[i + 1];
        }

        int ans = score == k;

        for (int i = n; i < n * 2 - 1; i++) {
            score += s[i % n] == s[(i - 1) % n];
            score -= s[i % n] == s[(i + 1) % n];
            ans += score == k;
        }

        return ans;
    }
};
```

#### Go

```go
func countRotations(s string, k int) int {
	n := len(s)
	score := 0

	for i := 0; i < n-1; i++ {
		if s[i] == s[i+1] {
			score++
		}
	}

	ans := 0
	if score == k {
		ans++
	}

	for i := n; i < n*2-1; i++ {
		if s[i%n] == s[(i-1)%n] {
			score++
		}
		if s[i%n] == s[(i+1)%n] {
			score--
		}
		if score == k {
			ans++
		}
	}

	return ans
}
```

#### TypeScript

```ts
function countRotations(s: string, k: number): number {
    const n = s.length;
    let score = 0;

    for (let i = 0; i < n - 1; i++) {
        score += s[i] === s[i + 1] ? 1 : 0;
    }

    let ans = score === k ? 1 : 0;

    for (let i = n; i < n * 2 - 1; i++) {
        score += s[i % n] === s[(i - 1) % n] ? 1 : 0;
        score -= s[i % n] === s[(i + 1) % n] ? 1 : 0;
        ans += score === k ? 1 : 0;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
