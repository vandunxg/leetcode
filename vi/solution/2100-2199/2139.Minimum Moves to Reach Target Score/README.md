---
comments: true
difficulty: Medium
rating: 1417
source: Weekly Contest 276 Q2
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [2139. Minimum Moves to Reach Target Score](https://leetcode.com/problems/minimum-moves-to-reach-target-score)

[中文文档](/solution/2100-2199/2139.Minimum%20Moves%20to%20Reach%20Target%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi với các số nguyên. Ban đầu bạn có số nguyên <code>1</code> và muốn đạt đến số nguyên <code>target</code>.</p>

<p>Trong một lượt, bạn có thể:</p>

<ul>
	<li><strong>Tăng</strong> số nguyên hiện tại thêm một (tức là <code>x = x + 1</code>).</li>
	<li><strong>Nhân đôi</strong> số nguyên hiện tại (tức là <code>x = 2 * x</code>).</li>
</ul>

<p>Bạn có thể sử dụng phép <strong>tăng</strong> <strong>bất kỳ</strong> số lần nào, tuy nhiên chỉ được sử dụng phép <strong>nhân đôi</strong> <strong>nhiều nhất</strong> <code>maxDoubles</code> lần.</p>

<p>Cho hai số nguyên <code>target</code> và <code>maxDoubles</code>, hãy trả về <em>số lượt ít nhất cần dùng để đạt đến </em><code>target</code><em> khi bắt đầu từ </em><code>1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 5, maxDoubles = 0
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Liên tục tăng thêm 1 cho đến khi đạt target.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 19, maxDoubles = 2
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Ban đầu, x = 1
Tăng 3 lần nên x = 4
Nhân đôi một lần nên x = 8
Tăng một lần nên x = 9
Nhân đôi lần nữa nên x = 18
Tăng một lần nên x = 19
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> target = 10, maxDoubles = 4
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong><b> </b>Ban đầu, x = 1
Tăng một lần nên x = 2
Nhân đôi một lần nên x = 4
Tăng một lần nên x = 5
Nhân đôi lần nữa nên x = 10
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= maxDoubles &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Quay lui + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Ta bắt đầu từ $1$ và có thể tăng hoặc nhân đôi, với nhiều nhất $\textit{maxDoubles}$ lần nhân đôi. Nếu tìm theo chiều thuận, hai phép biến đổi sẽ xen kẽ nhau. Theo chiều ngược, nếu target là số chẵn và vẫn còn lượt nhân đôi thì ta nên chia đôi; nếu không thì trừ một, vì phép nhân đôi có giá trị nhất trên các giá trị chẵn lớn.
>
> $\textit{target}$ có thể rất lớn trong khi số lượt nhân đôi nhỏ, nên đường đi theo chiều ngược có độ phức tạp $O(\min(\log \textit{target},\textit{maxDoubles}))$. Khi không còn lượt nhân đôi, đáp án là $\textit{target}-1$.
>
> Đệ quy: chia đôi khi số hiện tại là số chẵn và vẫn còn lượt nhân đôi; nếu không thì trừ một.

<!-- thinking:end -->

Trước hết, hãy bắt đầu bằng cách quay lui từ trạng thái cuối. Với giả sử trạng thái cuối là $target$, trạng thái trước đó của $target$ có thể là $target - 1$ hoặc $target / 2$, tùy thuộc vào tính chẵn lẻ của $target$ và giá trị của $maxDoubles$.

Nếu $target=1$, không cần thực hiện phép biến đổi nào, nên ta có thể trả về trực tiếp $0$.

Nếu $maxDoubles=0$, ta chỉ có thể dùng phép tăng, nên cần $target-1$ phép biến đổi.

Nếu $target$ là số chẵn và $maxDoubles>0$, ta có thể dùng phép nhân đôi, cần $1$ phép biến đổi, sau đó giải đệ quy với $target/2$ và $maxDoubles-1$.

Nếu $target$ là số lẻ, ta chỉ có thể dùng phép tăng, cần $1$ phép biến đổi, sau đó giải đệ quy với $target-1$ và $maxDoubles$.

Độ phức tạp thời gian là $O(\min(\log target, maxDoubles))$, và độ phức tạp không gian là $O(\min(\log target, maxDoubles))$.

Ta cũng có thể chuyển quy trình trên sang dạng lặp để tránh phần bộ nhớ dùng cho đệ quy.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, target: int, maxDoubles: int) -> int:
        if target == 1:
            return 0
        if maxDoubles == 0:
            return target - 1
        if target % 2 == 0 and maxDoubles:
            return 1 + self.minMoves(target >> 1, maxDoubles - 1)
        return 1 + self.minMoves(target - 1, maxDoubles)
```

#### Java

```java
class Solution {
    public int minMoves(int target, int maxDoubles) {
        if (target == 1) {
            return 0;
        }
        if (maxDoubles == 0) {
            return target - 1;
        }
        if (target % 2 == 0 && maxDoubles > 0) {
            return 1 + minMoves(target >> 1, maxDoubles - 1);
        }
        return 1 + minMoves(target - 1, maxDoubles);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(int target, int maxDoubles) {
        if (target == 1) {
            return 0;
        }
        if (maxDoubles == 0) {
            return target - 1;
        }
        if (target % 2 == 0 && maxDoubles > 0) {
            return 1 + minMoves(target >> 1, maxDoubles - 1);
        }
        return 1 + minMoves(target - 1, maxDoubles);
    }
};
```

#### Go

```go
func minMoves(target int, maxDoubles int) int {
	if target == 1 {
		return 0
	}
	if maxDoubles == 0 {
		return target - 1
	}
	if target%2 == 0 && maxDoubles > 0 {
		return 1 + minMoves(target>>1, maxDoubles-1)
	}
	return 1 + minMoves(target-1, maxDoubles)
}
```

#### TypeScript

```ts
function minMoves(target: number, maxDoubles: number): number {
    if (target === 1) {
        return 0;
    }
    if (maxDoubles === 0) {
        return target - 1;
    }
    if (target % 2 === 0 && maxDoubles) {
        return 1 + minMoves(target >> 1, maxDoubles - 1);
    }
    return 1 + minMoves(target - 1, maxDoubles);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 sử dụng đệ quy có độ sâu bằng với số bước đi theo chiều ngược; một vòng lặp sẽ loại bỏ call stack.
>
> Trong khi vẫn còn lượt nhân đôi và $\textit{target}>1$, giảm một giá trị lẻ hoặc dịch phải một giá trị chẵn; sau đó cộng phần $\textit{target}-1$ còn lại.
>
> Chính sách này giống Lời giải 1, nhưng được viết dưới dạng vòng lặp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMoves(self, target: int, maxDoubles: int) -> int:
        ans = 0
        while maxDoubles and target > 1:
            ans += 1
            if target % 2 == 1:
                target -= 1
            else:
                maxDoubles -= 1
                target >>= 1
        ans += target - 1
        return ans
```

#### Java

```java
class Solution {
    public int minMoves(int target, int maxDoubles) {
        int ans = 0;
        while (maxDoubles > 0 && target > 1) {
            ++ans;
            if (target % 2 == 1) {
                --target;
            } else {
                --maxDoubles;
                target >>= 1;
            }
        }
        ans += target - 1;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMoves(int target, int maxDoubles) {
        int ans = 0;
        while (maxDoubles > 0 && target > 1) {
            ++ans;
            if (target % 2 == 1) {
                --target;
            } else {
                --maxDoubles;
                target >>= 1;
            }
        }
        ans += target - 1;
        return ans;
    }
};
```

#### Go

```go
func minMoves(target int, maxDoubles int) (ans int) {
	for maxDoubles > 0 && target > 1 {
		ans++
		if target&1 == 1 {
			target--
		} else {
			maxDoubles--
			target >>= 1
		}
	}
	ans += target - 1
	return
}
```

#### TypeScript

```ts
function minMoves(target: number, maxDoubles: number): number {
    let ans = 0;
    while (maxDoubles && target > 1) {
        ++ans;
        if (target & 1) {
            --target;
        } else {
            --maxDoubles;
            target >>= 1;
        }
    }
    ans += target - 1;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
