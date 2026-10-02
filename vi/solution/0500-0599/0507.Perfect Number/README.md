---
comments: true
difficulty: Easy
tags:
    - Math
---

<!-- problem:start -->

# [507. Perfect Number](https://leetcode.com/problems/perfect-number)

[中文文档](/solution/0500-0599/0507.Perfect%20Number/README.md)

## Mô tả

<!-- description:start -->

<p><a href="https://en.wikipedia.org/wiki/Perfect_number" target="_blank"><strong>Số hoàn hảo</strong></a> là <strong>số nguyên dương</strong> bằng tổng các <strong>ước dương</strong> của nó, không tính chính nó. <strong>Ước</strong> của số nguyên <code>x</code> là số nguyên mà <code>x</code> chia hết cho nó.</p>

<p>Cho số nguyên <code>n</code>, trả về <code>true</code><em> nếu </em><code>n</code><em> là số hoàn hảo; nếu không, trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 28
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> 28 = 1 + 2 + 4 + 7 + 14
1, 2, 4, 7 và 14 đều là ước của 28.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 7
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Số hoàn hảo bằng tổng các ước thực sự của nó. Duyệt từ $1$ đến $num-1$ quá chậm khi $num \le 10^8$.
>
> Các ước xuất hiện theo cặp: nếu $i$ chia hết $num$ thì $num/i$ cũng là ước, nên vòng lặp chỉ cần chạy đến $\sqrt{num}$. Số $1$ có tổng ước thực sự bằng $0$ nên không phải số hoàn hảo. So sánh tổng đã cộng với $num$.

<!-- thinking:end -->

Đầu tiên, kiểm tra $\textit{num}$ có bằng 1 không. Nếu có, đây không phải số hoàn hảo nên trả về $\text{false}$.

Tiếp theo, duyệt các ước dương của $\textit{num}$ bắt đầu từ 2. Nếu $\textit{num}$ chia hết cho ước dương $i$, cộng $i$ vào tổng $\textit{s}$. Nếu thương của $\textit{num}$ chia cho $i$ khác $i$, cộng cả thương đó vào $\textit{s}$.

Cuối cùng, kiểm tra $\textit{s}$ có bằng $\textit{num}$ hay không.

Độ phức tạp thời gian là $O(\sqrt{n})$, trong đó $n$ là giá trị của $\textit{num}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkPerfectNumber(self, num: int) -> bool:
        if num == 1:
            return False
        s, i = 1, 2
        while i <= num // i:
            if num % i == 0:
                s += i
                if i != num // i:
                    s += num // i
            i += 1
        return s == num
```

#### Java

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        if (num == 1) {
            return false;
        }
        int s = 1;
        for (int i = 2; i <= num / i; ++i) {
            if (num % i == 0) {
                s += i;
                if (i != num / i) {
                    s += num / i;
                }
            }
        }
        return s == num;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkPerfectNumber(int num) {
        if (num == 1) {
            return false;
        }
        int s = 1;
        for (int i = 2; i <= num / i; ++i) {
            if (num % i == 0) {
                s += i;
                if (i != num / i) {
                    s += num / i;
                }
            }
        }
        return s == num;
    }
};
```

#### Go

```go
func checkPerfectNumber(num int) bool {
	if num == 1 {
		return false
	}
	s := 1
	for i := 2; i <= num/i; i++ {
		if num%i == 0 {
			s += i
			if j := num / i; i != j {
				s += j
			}
		}
	}
	return s == num
}
```

#### TypeScript

```ts
function checkPerfectNumber(num: number): boolean {
    if (num <= 1) {
        return false;
    }
    let s = 1;
    for (let i = 2; i <= num / i; ++i) {
        if (num % i === 0) {
            s += i;
            if (i * i !== num) {
                s += num / i;
            }
        }
    }
    return s === num;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
