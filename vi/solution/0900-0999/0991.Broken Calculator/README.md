---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Math
---

<!-- problem:start -->

# [991. Broken Calculator](https://leetcode.com/problems/broken-calculator)

[中文文档](/solution/0900-0999/0991.Broken%20Calculator/README.md)

## Mô tả

<!-- description:start -->

<p>Có một chiếc máy tính bị hỏng, ban đầu màn hình hiển thị số nguyên <code>startValue</code>. Trong một thao tác, bạn có thể:</p>

<ul>
	<li>nhân số đang hiển thị với <code>2</code>, hoặc</li>
	<li>trừ <code>1</code> khỏi số đang hiển thị.</li>
</ul>

<p>Cho hai số nguyên <code>startValue</code> và <code>target</code>, hãy trả về số thao tác ít nhất cần thực hiện để máy tính hiển thị <code>target</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> startValue = 2, target = 3
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Nhân đôi rồi giảm đi một đơn vị: {2 -&gt; 4 -&gt; 3}.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> startValue = 5, target = 8
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Giảm đi một đơn vị rồi nhân đôi: {5 -&gt; 4 -&gt; 8}.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> startValue = 3, target = 10
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Nhân đôi, giảm đi một đơn vị, rồi nhân đôi {3 -&gt; 6 -&gt; 5 -&gt; 10}.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= startValue, target &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tính ngược

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể nhân đôi hoặc trừ một từ $\textit{startValue}$ để đạt $\textit{target}$ với ít thao tác nhất. Cả hai giá trị đều có thể lên tới $10^9$, nên tìm kiếm xuôi sẽ có phạm vi quá lớn. Khi tính ngược, nếu target chẵn thì chia đôi; nếu target lẻ thì phép ngược duy nhất là cộng một. Lặp lại đến khi target không lớn hơn giá trị ban đầu; phần chênh lệch còn lại tương ứng với các lần trừ thêm.

<!-- thinking:end -->

Bắt đầu từ $\textit{target}$ và tính ngược. Nếu $\textit{target}$ lẻ, ta cộng $1$; nếu chẵn, ta chia $2$. Đếm số thao tác cho đến khi $\textit{target} \leq \textit{startValue}$. Kết quả cuối cùng bằng số thao tác đã đếm cộng với $\textit{startValue} - \textit{target}$. 

Độ phức tạp thời gian là $O(\log n)$, trong đó $n$ là $\textit{target}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def brokenCalc(self, startValue: int, target: int) -> int:
        ans = 0
        while startValue < target:
            if target & 1:
                target += 1
            else:
                target >>= 1
            ans += 1
        ans += startValue - target
        return ans
```

#### Java

```java
class Solution {
    public int brokenCalc(int startValue, int target) {
        int ans = 0;
        while (startValue < target) {
            if ((target & 1) == 1) {
                target++;
            } else {
                target >>= 1;
            }
            ans += 1;
        }
        ans += startValue - target;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int brokenCalc(int startValue, int target) {
        int ans = 0;
        while (startValue < target) {
            if (target & 1) {
                target++;
            } else {
                target >>= 1;
            }
            ++ans;
        }
        ans += startValue - target;
        return ans;
    }
};
```

#### Go

```go
func brokenCalc(startValue int, target int) (ans int) {
	for startValue < target {
		if target&1 == 1 {
			target++
		} else {
			target >>= 1
		}
		ans++
	}
	ans += startValue - target
	return
}
```

#### TypeScript

```ts
function brokenCalc(startValue: number, target: number): number {
    let ans = 0;
    for (; startValue < target; ++ans) {
        if (target & 1) {
            ++target;
        } else {
            target >>= 1;
        }
    }
    ans += startValue - target;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
