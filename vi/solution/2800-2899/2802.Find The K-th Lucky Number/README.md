---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Math
    - String
---

<!-- problem:start -->

# [2802. Find The K-th Lucky Number 🔒](https://leetcode.com/problems/find-the-k-th-lucky-number)

[中文文档](/solution/2800-2899/2802.Find%20The%20K-th%20Lucky%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Chúng ta biết <code>4</code> và <code>7</code> là các chữ số <strong>may mắn</strong>. Một số được gọi là <strong>may mắn</strong> nếu nó <strong>chỉ</strong> chứa các chữ số may mắn.</p>

<p>Bạn được cho một số nguyên <code>k</code>, hãy trả về <em>số may mắn thứ </em><code>k<sup>th</sup></code><em>&nbsp;dưới dạng một <strong>chuỗi</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> k = 4
<strong>Output:</strong> &quot;47&quot;
<strong>Giải thích:</strong> Số may mắn đầu tiên là 4, số thứ hai là 7, số thứ ba là 44 và số thứ tư là 47.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> k = 10
<strong>Output:</strong> &quot;477&quot;
<strong>Giải thích:</strong> Các số may mắn được sắp xếp theo thứ tự tăng dần là:
4, 7, 44, 47, 74, 77, 444, 447, 474, 477. Vì vậy, số may mắn thứ 10<sup>th</sup> là 477.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> k = 1000
<strong>Output:</strong> &quot;777747447&quot;
<strong>Giải thích:</strong> Có thể chứng minh rằng số may mắn thứ 1000<sup>th</sup> là 777747447.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Việc sinh các số may mắn theo thứ tự cho đến số thứ $k$ không đáp ứng được quy mô đầu vào. Có đúng $2^n$ số may mắn có $n$ chữ số, tương ứng với việc ánh xạ các bit nhị phân thành $4$ và $7$. Trừ số lượng các độ dài ngắn hơn để xác định $n$, sau đó quyết định từng bit từ trái sang phải: ghi $4$ nếu $k$ nằm trong nửa đầu có $2^{n-1}$ phần tử, ngược lại ghi $7$ và trừ đi nửa đó.

<!-- thinking:end -->

Theo mô tả bài toán, một số may mắn chỉ chứa các chữ số $4$ và $7$, nên số lượng số may mắn có $n$ chữ số là $2^n$.

Ta khởi tạo $n=1$, sau đó lặp để kiểm tra xem $k$ có lớn hơn $2^n$ hay không. Nếu có, ta trừ $2^n$ khỏi $k$ và tăng $n$ lên, cho đến khi $k$ nhỏ hơn hoặc bằng $2^n$. Khi đó, ta chỉ cần tìm số may mắn thứ $k$ trong các số may mắn có $n$ chữ số.

Nếu $k$ nhỏ hơn hoặc bằng $2^{n-1}$, chữ số đầu tiên của số may mắn thứ $k$ là $4$, ngược lại chữ số đầu tiên là $7$. Sau đó, ta trừ $2^{n-1}$ khỏi $k$ và tiếp tục xác định chữ số thứ hai, cho đến khi xác định được tất cả các chữ số của số may mắn có $n$ chữ số.

Độ phức tạp thời gian là $O(\log k)$, và độ phức tạp không gian là $O(\log k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kthLuckyNumber(self, k: int) -> str:
        n = 1
        while k > 1 << n:
            k -= 1 << n
            n += 1
        ans = []
        while n:
            n -= 1
            if k <= 1 << n:
                ans.append("4")
            else:
                ans.append("7")
                k -= 1 << n
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String kthLuckyNumber(int k) {
        int n = 1;
        while (k > 1 << n) {
            k -= 1 << n;
            ++n;
        }
        StringBuilder ans = new StringBuilder();
        while (n-- > 0) {
            if (k <= 1 << n) {
                ans.append('4');
            } else {
                ans.append('7');
                k -= 1 << n;
            }
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string kthLuckyNumber(int k) {
        int n = 1;
        while (k > 1 << n) {
            k -= 1 << n;
            ++n;
        }
        string ans;
        while (n--) {
            if (k <= 1 << n) {
                ans.push_back('4');
            } else {
                ans.push_back('7');
                k -= 1 << n;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func kthLuckyNumber(k int) string {
	n := 1
	for k > 1<<n {
		k -= 1 << n
		n++
	}
	ans := []byte{}
	for n > 0 {
		n--
		if k <= 1<<n {
			ans = append(ans, '4')
		} else {
			ans = append(ans, '7')
			k -= 1 << n
		}
	}
	return string(ans)
}
```

#### TypeScript

```ts
function kthLuckyNumber(k: number): string {
    let n = 1;
    while (k > 1 << n) {
        k -= 1 << n;
        ++n;
    }
    const ans: string[] = [];
    while (n-- > 0) {
        if (k <= 1 << n) {
            ans.push('4');
        } else {
            ans.push('7');
            k -= 1 << n;
        }
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
