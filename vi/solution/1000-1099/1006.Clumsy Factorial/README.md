---
comments: true
difficulty: Medium
rating: 1407
source: Weekly Contest 127 Q2
tags:
    - Stack
    - Math
    - Simulation
---

<!-- problem:start -->

# [1006. Clumsy Factorial](https://leetcode.com/problems/clumsy-factorial)

[中文文档](/solution/1000-1099/1006.Clumsy%20Factorial/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Giai thừa</strong> của số nguyên dương <code>n</code> là tích của tất cả số nguyên dương nhỏ hơn hoặc bằng <code>n</code>.</p>

<ul>
	<li>Ví dụ, <code>factorial(10) = 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1</code>.</li>
</ul>

<p>Ta tạo <strong>giai thừa vụng về</strong> bằng cách dùng các số nguyên theo thứ tự giảm dần, thay phép nhân bằng chuỗi phép toán cố định theo thứ tự: nhân <code>&#39;*&#39;</code>, chia <code>&#39;/&#39;</code>, cộng <code>&#39;+&#39;</code> và trừ <code>&#39;-&#39;</code>.</p>

<ul>
	<li>Ví dụ, <code>clumsy(10) = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1</code>.</li>
</ul>

<p>Tuy nhiên, các phép toán vẫn tuân theo thứ tự ưu tiên thông thường. Ta thực hiện tất cả phép nhân và chia trước các phép cộng, trừ; các phép nhân và chia được xử lý từ trái sang phải.</p>

<p>Ngoài ra, phép chia được dùng là phép chia lấy phần nguyên, nên <code>10 * 9 / 8 = 90 / 8 = 11</code>.</p>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>giai thừa vụng về của </em><code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> n = 4
<strong>Output:</strong> 7
<strong>Giải thích:</strong> 7 = 4 * 3 / 2 + 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> n = 10
<strong>Output:</strong> 12
<strong>Giải thích:</strong> 12 = 10 * 9 / 8 + 7 - 6 * 5 / 4 + 3 - 2 * 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Stack + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thực hiện lần lượt các phép toán $\times,\div,+, -$ với các số từ $n$ trở xuống đúng theo định nghĩa, nhưng nhân và chia có độ ưu tiên cao hơn cộng và trừ nên không thể cộng dồn từ trái sang phải. Vì $N\le 10^4$, ta có thể duyệt tuyến tính; vấn đề là xử lý đúng thứ tự ưu tiên.
>
> Phép nhân và chia cần kết hợp ngay với toán hạng trước đó. Các phép cộng và trừ chỉ tạo ra số hạng có dấu, nên có thể để đến cuối mới cộng.
>
> Dùng stack lưu các số hạng đang chờ, còn $k\bmod 4$ luân phiên qua bốn phép toán: $\times$ và $\div$ thay phần tử trên đỉnh, $+$ và $-$ push $x$ hoặc $-x$; đáp án là tổng các phần tử trong stack.

<!-- thinking:end -->

Có thể xem quá trình tính giai thừa vụng về là mô phỏng bằng stack.

Ta định nghĩa stack `stk`, ban đầu push $n$ vào stack, và biến $k$ biểu diễn phép toán hiện tại, khởi tạo $k = 0$.

Sau đó, bắt đầu từ $n-1$, ta duyệt từng $x$ và xử lý $x$ tùy theo giá trị hiện tại của $k$:

- Khi $k = 0$, tương ứng với phép nhân: pop phần tử trên đỉnh stack, nhân với $x$ rồi push kết quả trở lại stack;
- Khi $k = 1$, tương ứng với phép chia: pop phần tử trên đỉnh stack, chia cho $x$, lấy phần nguyên rồi push kết quả trở lại stack;
- Khi $k = 2$, tương ứng với phép cộng: push trực tiếp $x$ vào stack;
- Khi $k = 3$, tương ứng với phép trừ: push $-x$ vào stack.

Tiếp theo, cập nhật $k = (k + 1) \mod 4$.

Cuối cùng, tổng các phần tử trong stack là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số nguyên $N$ được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def clumsy(self, n: int) -> int:
        k = 0
        stk = [n]
        for x in range(n - 1, 0, -1):
            if k == 0:
                stk.append(stk.pop() * x)
            elif k == 1:
                stk.append(int(stk.pop() / x))
            elif k == 2:
                stk.append(x)
            else:
                stk.append(-x)
            k = (k + 1) % 4
        return sum(stk)
```

#### Java

```java
class Solution {
    public int clumsy(int n) {
        Deque<Integer> stk = new ArrayDeque<>();
        stk.push(n);
        int k = 0;
        for (int x = n - 1; x > 0; --x) {
            if (k == 0) {
                stk.push(stk.pop() * x);
            } else if (k == 1) {
                stk.push(stk.pop() / x);
            } else if (k == 2) {
                stk.push(x);
            } else {
                stk.push(-x);
            }
            k = (k + 1) % 4;
        }
        return stk.stream().mapToInt(Integer::intValue).sum();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int clumsy(int n) {
        stack<int> stk;
        stk.push(n);
        int k = 0;
        for (int x = n - 1; x; --x) {
            if (k == 0) {
                stk.top() *= x;
            } else if (k == 1) {
                stk.top() /= x;
            } else if (k == 2) {
                stk.push(x);
            } else {
                stk.push(-x);
            }
            k = (k + 1) % 4;
        }
        int ans = 0;
        while (!stk.empty()) {
            ans += stk.top();
            stk.pop();
        }
        return ans;
    }
};
```

#### Go

```go
func clumsy(n int) (ans int) {
	stk := []int{n}
	k := 0
	for x := n - 1; x > 0; x-- {
		switch k {
		case 0:
			stk[len(stk)-1] *= x
		case 1:
			stk[len(stk)-1] /= x
		case 2:
			stk = append(stk, x)
		case 3:
			stk = append(stk, -x)
		}
		k = (k + 1) % 4
	}
	for _, x := range stk {
		ans += x
	}
	return
}
```

#### TypeScript

```ts
function clumsy(n: number): number {
    const stk: number[] = [n];
    let k = 0;
    for (let x = n - 1; x; --x) {
        if (k === 0) {
            stk.push(stk.pop()! * x);
        } else if (k === 1) {
            stk.push((stk.pop()! / x) | 0);
        } else if (k === 2) {
            stk.push(x);
        } else {
            stk.push(-x);
        }
        k = (k + 1) % 4;
    }
    return stk.reduce((acc, cur) => acc + cur, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
