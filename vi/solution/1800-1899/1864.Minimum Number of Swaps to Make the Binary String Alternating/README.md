---
comments: true
difficulty: Medium
rating: 1600
source: Weekly Contest 241 Q2
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1864. Minimum Number of Swaps to Make the Binary String Alternating](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-binary-string-alternating)

[中文文档](/solution/1800-1899/1864.Minimum%20Number%20of%20Swaps%20to%20Make%20the%20Binary%20String%20Alternating/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi nhị phân <code>s</code>, hãy trả về <em>số lần đổi chỗ ký tự <strong>ít nhất</strong> để chuỗi trở nên <strong>xen kẽ</strong>, hoặc </em><code>-1</code><em> nếu không thể.</em></p>

<p>Một chuỗi được gọi là <strong>xen kẽ</strong> nếu không có hai ký tự kề nhau nào giống nhau. Ví dụ, các chuỗi <code>&quot;010&quot;</code> và <code>&quot;1010&quot;</code> là chuỗi xen kẽ, còn chuỗi <code>&quot;0100&quot;</code> thì không.</p>

<p>Có thể đổi chỗ bất kỳ hai ký tự nào, kể cả khi chúng <strong>không kề nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;111000&quot;
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Đổi chỗ các vị trí 1 và 4: &quot;1<u>1</u>10<u>0</u>0&quot; -&gt; &quot;1<u>0</u>10<u>1</u>0&quot;
Chuỗi lúc này là chuỗi xen kẽ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;010&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Chuỗi đã xen kẽ, không cần đổi chỗ.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> s = &quot;1110&quot;
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s[i]</code> là <code>&#39;0&#39;</code> hoặc <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể đổi chỗ các bit để đạt mẫu $0101\ldots$ hoặc $1010\ldots$. Hai mẫu đích này yêu cầu số lượng $0/1$ khác nhau, nên nếu độ lệch lớn hơn một thì không thể thực hiện.
>
> So sánh $n_0$ và $n_1$: nếu chúng lệch nhau quá $1$, trả về $-1$; nếu bằng nhau, thử cả hai bit bắt đầu; nếu không, bắt đầu bằng bit chiếm đa số. Mỗi lần đổi chỗ sửa được hai vị trí sai, nên đáp án bằng một nửa số vị trí sai.

<!-- thinking:end -->

Trước hết, ta đếm số ký tự $0$ và $1$ trong chuỗi $\textit{s}$, lần lượt ký hiệu là $n_0$ và $n_1$.

Nếu hiệu tuyệt đối giữa $n_0$ và $n_1$ lớn hơn $1$, không thể tạo thành chuỗi xen kẽ, nên ta trả về $-1$.

Nếu $n_0$ và $n_1$ bằng nhau, ta có thể tính số lần đổi chỗ cần thiết để biến chuỗi thành chuỗi xen kẽ bắt đầu bằng $0$ và bắt đầu bằng $1$, rồi lấy giá trị nhỏ hơn.

Nếu $n_0$ và $n_1$ không bằng nhau, ta chỉ cần tính số lần đổi chỗ cần thiết để biến chuỗi thành chuỗi xen kẽ bắt đầu bằng ký tự xuất hiện nhiều hơn.

Bài toán được rút gọn thành việc tính số lần đổi chỗ cần thiết để biến chuỗi $\textit{s}$ thành chuỗi xen kẽ bắt đầu bằng ký tự $c$.

Ta định nghĩa hàm $\text{calc}(c)$, biểu diễn số lần đổi chỗ cần thiết để biến chuỗi $\textit{s}$ thành chuỗi xen kẽ bắt đầu bằng ký tự $c$. Ta duyệt chuỗi $\textit{s}$; tại mỗi vị trí $i$, nếu parity của $i$ khác $c$, ta cần đổi chỗ ký tự ở vị trí này và tăng bộ đếm lên $1$. Vì mỗi lần đổi chỗ khiến hai vị trí được sửa, số lần đổi chỗ cuối cùng bằng một nửa bộ đếm.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi $\textit{s}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, s: str) -> int:
        def calc(c: int) -> int:
            return sum((c ^ i & 1) != x for i, x in enumerate(map(int, s))) // 2

        n0 = s.count("0")
        n1 = len(s) - n0
        if abs(n0 - n1) > 1:
            return -1
        if n0 == n1:
            return min(calc(0), calc(1))
        return calc(0 if n0 > n1 else 1)
```

#### Java

```java
class Solution {
    private char[] s;

    public int minSwaps(String s) {
        this.s = s.toCharArray();
        int n1 = 0;
        for (char c : this.s) {
            n1 += (c - '0');
        }
        int n0 = this.s.length - n1;
        if (Math.abs(n0 - n1) > 1) {
            return -1;
        }
        if (n0 == n1) {
            return Math.min(calc(0), calc(1));
        }
        return calc(n0 > n1 ? 0 : 1);
    }

    private int calc(int c) {
        int cnt = 0;
        for (int i = 0; i < s.length; ++i) {
            int x = s[i] - '0';
            if ((i & 1 ^ c) != x) {
                ++cnt;
            }
        }
        return cnt / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwaps(string s) {
        int n0 = ranges::count(s, '0');
        int n1 = s.size() - n0;
        if (abs(n0 - n1) > 1) {
            return -1;
        }
        auto calc = [&](int c) -> int {
            int cnt = 0;
            for (int i = 0; i < s.size(); ++i) {
                int x = s[i] - '0';
                if ((i & 1 ^ c) != x) {
                    ++cnt;
                }
            }
            return cnt / 2;
        };
        if (n0 == n1) {
            return min(calc(0), calc(1));
        }
        return calc(n0 > n1 ? 0 : 1);
    }
};
```

#### Go

```go
func minSwaps(s string) int {
	n0 := strings.Count(s, "0")
	n1 := len(s) - n0
	if abs(n0-n1) > 1 {
		return -1
	}
	calc := func(c int) int {
		cnt := 0
		for i, ch := range s {
			x := int(ch - '0')
			if i&1^c != x {
				cnt++
			}
		}
		return cnt / 2
	}
	if n0 == n1 {
		return min(calc(0), calc(1))
	}
	if n0 > n1 {
		return calc(0)
	}
	return calc(1)
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function minSwaps(s: string): number {
    const n0 = (s.match(/0/g) || []).length;
    const n1 = s.length - n0;
    if (Math.abs(n0 - n1) > 1) {
        return -1;
    }
    const calc = (c: number): number => {
        let cnt = 0;
        for (let i = 0; i < s.length; i++) {
            const x = +s[i];
            if (((i & 1) ^ c) !== x) {
                cnt++;
            }
        }
        return Math.floor(cnt / 2);
    };
    if (n0 === n1) {
        return Math.min(calc(0), calc(1));
    }
    return calc(n0 > n1 ? 0 : 1);
}
```

#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var minSwaps = function (s) {
    const n0 = (s.match(/0/g) || []).length;
    const n1 = s.length - n0;
    if (Math.abs(n0 - n1) > 1) {
        return -1;
    }
    const calc = c => {
        let cnt = 0;
        for (let i = 0; i < s.length; i++) {
            const x = +s[i];
            if (((i & 1) ^ c) !== x) {
                cnt++;
            }
        }
        return Math.floor(cnt / 2);
    };
    if (n0 === n1) {
        return Math.min(calc(0), calc(1));
    }
    return calc(n0 > n1 ? 0 : 1);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
