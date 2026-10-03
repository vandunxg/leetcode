---
comments: true
difficulty: Easy
rating: 1246
source: Biweekly Contest 92 Q1
tags:
    - Geometry
    - Math
---

<!-- problem:start -->

# [2481. Minimum Cuts to Divide a Circle](https://leetcode.com/problems/minimum-cuts-to-divide-a-circle)

[中文文档](/solution/2400-2499/2481.Minimum%20Cuts%20to%20Divide%20a%20Circle/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>nhát cắt hợp lệ</strong> trong hình tròn có thể là:</p>

<ul>
	<li>Nhát cắt được biểu diễn bởi một đường thẳng chạm vào hai điểm trên đường tròn và đi qua tâm, hoặc</li>
	<li>Nhát cắt được biểu diễn bởi một đường thẳng chạm vào một điểm trên đường tròn và đi qua tâm.</li>
</ul>

<p>Một số nhát cắt hợp lệ và không hợp lệ được minh họa trong các hình bên dưới.</p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2481.Minimum%20Cuts%20to%20Divide%20a%20Circle/images/alldrawio.png" style="width: 450px; height: 174px;" />
<p>Cho số nguyên <code>n</code>, hãy trả về số nhát cắt <em><strong>ít nhất</strong> cần thiết để chia hình tròn thành </em><code>n</code><em> phần bằng nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2481.Minimum%20Cuts%20to%20Divide%20a%20Circle/images/11drawio.png" style="width: 200px; height: 200px;" />
<pre>
<strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hình trên cho thấy cách cắt hình tròn hai lần qua tâm để chia nó thành 4 phần bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2481.Minimum%20Cuts%20to%20Divide%20a%20Circle/images/22drawio.png" style="width: 200px; height: 201px;" />
<pre>
<strong>Đầu vào:</strong> n = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Cần ít nhất 3 nhát cắt để chia hình tròn thành 3 phần bằng nhau.
Có thể chứng minh rằng không thể tạo ra 3 phần có cùng kích thước và hình dạng với ít hơn 3 nhát cắt.
Ngoài ra, lưu ý rằng nhát cắt đầu tiên sẽ không chia hình tròn thành các phần riêng biệt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thảo luận các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Một nhát cắt là một đường thẳng đi qua tâm. Với một phần thì không cần nhát cắt nào. Với $n$ lẻ, không có các bán kính đối nhau cùng nằm trên một đường thẳng, nên cần $n$ nhát cắt; với $n$ chẵn, các bán kính này ghép thành từng cặp, nên cần $n/2$ nhát cắt. Chỉ cần xét tính chẵn lẻ.

<!-- thinking:end -->

- Khi $n=1$, không cần cắt, nên số nhát cắt là $0$;
- Khi $n$ lẻ, không thể có các bán kính cùng nằm trên một đường thẳng, nên cần ít nhất $n$ nhát cắt;
- Khi $n$ chẵn, các bán kính có thể ghép thành từng cặp cùng nằm trên một đường thẳng, nên cần ít nhất $\frac{n}{2}$ nhát cắt.

Tóm lại, ta có:

$$
\textit{ans} = \begin{cases}
n, & n \gt 1 \textit{ and } n \textit{ is odd} \\
\frac{n}{2}, & n \textit{ is even} \\
\end{cases}
$$

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfCuts(self, n: int) -> int:
        return n if (n > 1 and n & 1) else n >> 1
```

#### Java

```java
class Solution {
    public int numberOfCuts(int n) {
        return n > 1 && n % 2 == 1 ? n : n >> 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfCuts(int n) {
        return n > 1 && n % 2 == 1 ? n : n >> 1;
    }
};
```

#### Go

```go
func numberOfCuts(n int) int {
	if n > 1 && n%2 == 1 {
		return n
	}
	return n >> 1
}
```

#### TypeScript

```ts
function numberOfCuts(n: number): number {
    return n > 1 && n & 1 ? n : n >> 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_cuts(n: i32) -> i32 {
        if n > 1 && n % 2 == 1 {
            return n;
        }
        n >> 1
    }
}
```

#### C#

```cs
public class Solution {
    public int NumberOfCuts(int n) {
        return n > 1 && n % 2 == 1 ? n : n >> 1;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
