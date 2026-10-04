---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2847. Smallest Number With Given Digit Product 🔒](https://leetcode.com/problems/smallest-number-with-given-digit-product)

[中文文档](/solution/2800-2899/2847.Smallest%20Number%20With%20Given%20Digit%20Product/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <strong>dương</strong> <code>n</code>, hãy trả về <em>một chuỗi biểu diễn số nguyên <strong>dương nhỏ nhất</strong> sao cho tích các chữ số của nó bằng</em> <code>n</code><em>, hoặc </em><code>&quot;-1&quot;</code><em> nếu không tồn tại số như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 105
<strong>Đầu ra:</strong> &quot;357&quot;
<strong>Giải thích:</strong> 3 * 5 * 7 = 105. Có thể chứng minh 357 là số nhỏ nhất có tích các chữ số bằng 105. Vì vậy, đáp án là &quot;357&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7
<strong>Đầu ra:</strong> &quot;7&quot;
<strong>Giải thích:</strong> Vì 7 chỉ có một chữ số nên tích các chữ số của nó là 7. Ta sẽ chứng minh 7 là số nhỏ nhất có tích các chữ số bằng 7. Vì tích của các số từ 1 đến 6 lần lượt là từ 1 đến 6, nên đáp án là &quot;7&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 44
<strong>Đầu ra:</strong> &quot;-1&quot;
<strong>Giải thích:</strong> Có thể chứng minh không tồn tại số nào có tích các chữ số bằng 44. Vì vậy, đáp án là &quot;-1&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>18</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích thừa số nguyên tố + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số phải có tích bằng $n$ và tạo thành số nhỏ nhất. Nếu có một thừa số nguyên tố lớn hơn $9$ thì không thể thực hiện. Phân tích từ $9$ xuống $2$ để ưu tiên dùng các chữ số lớn, sau đó xuất chúng theo thứ tự tăng dần; trường hợp $n=1$ cho kết quả là chữ số $1$.

<!-- thinking:end -->

Ta xét việc phân tích số $n$ thành các thừa số nguyên tố. Nếu $n$ có thừa số nguyên tố lớn hơn $9$, không thể tìm được số thỏa mãn điều kiện, vì các thừa số nguyên tố lớn hơn $9$ không thể tạo ra bằng cách nhân các số từ $1$ đến $9$. Ví dụ, không thể tạo ra $11$ bằng cách nhân các số từ $1$ đến $9$. Do đó, ta chỉ cần kiểm tra $n$ có thừa số nguyên tố lớn hơn $9$ hay không. Nếu có, trả về $-1$ ngay.

Ngược lại, nếu các thừa số nguyên tố gồm $7$ và $5$, thì trước tiên có thể phân tách số $n$ thành một số các chữ số $7$ và $5$. Hai chữ số $3$ có thể gộp thành một chữ số $9$, ba chữ số $2$ có thể gộp thành một chữ số $8$, còn một chữ số $2$ và một chữ số $3$ có thể gộp thành một chữ số $6$. Vì vậy, ta chỉ cần phân tách số thành các chữ số từ $2$ đến $9$. Ta có thể sử dụng phương pháp tham lam, ưu tiên phân tách thành $9$, sau đó đến $8$, và tiếp tục như vậy.

Độ phức tạp thời gian là $O(\log n)$, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestNumber(self, n: int) -> str:
        cnt = [0] * 10
        for i in range(9, 1, -1):
            while n % i == 0:
                n //= i
                cnt[i] += 1
        if n > 1:
            return "-1"
        ans = "".join(str(i) * cnt[i] for i in range(2, 10))
        return ans if ans else "1"
```

#### Java

```java
class Solution {
    public String smallestNumber(long n) {
        int[] cnt = new int[10];
        for (int i = 9; i > 1; --i) {
            while (n % i == 0) {
                ++cnt[i];
                n /= i;
            }
        }
        if (n > 1) {
            return "-1";
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 2; i < 10; ++i) {
            while (cnt[i] > 0) {
                sb.append(i);
                --cnt[i];
            }
        }
        String ans = sb.toString();
        return ans.isEmpty() ? "1" : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string smallestNumber(long long n) {
        int cnt[10]{};
        for (int i = 9; i > 1; --i) {
            while (n % i == 0) {
                n /= i;
                ++cnt[i];
            }
        }
        if (n > 1) {
            return "-1";
        }
        string ans;
        for (int i = 2; i < 10; ++i) {
            ans += string(cnt[i], '0' + i);
        }
        return ans == "" ? "1" : ans;
    }
};
```

#### Go

```go
func smallestNumber(n int64) string {
	cnt := [10]int{}
	for i := 9; i > 1; i-- {
		for n%int64(i) == 0 {
			cnt[i]++
			n /= int64(i)
		}
	}
	if n != 1 {
		return "-1"
	}
	sb := &strings.Builder{}
	for i := 2; i < 10; i++ {
		for j := 0; j < cnt[i]; j++ {
			sb.WriteByte(byte(i) + '0')
		}
	}
	ans := sb.String()
	if len(ans) > 0 {
		return ans
	}
	return "1"
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
