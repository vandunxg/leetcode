---
comments: true
difficulty: Easy
rating: 1191
source: Weekly Contest 353 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2769. Find the Maximum Achievable Number](https://leetcode.com/problems/find-the-maximum-achievable-number)

[中文文档](/solution/2700-2799/2769.Find%20the%20Maximum%20Achievable%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>num</code> và <code>t</code>. Một <strong>số </strong><code>x</code><strong> </strong>được<strong> xem là có thể đạt được</strong> nếu nó có thể trở nên bằng <code>num</code> sau khi thực hiện thao tác sau <strong>không quá</strong> <code>t</code> lần:</p>

<ul>
	<li>Tăng hoặc giảm <code>x</code> đi <code>1</code>, <em>đồng thời</em> tăng hoặc giảm <code>num</code> đi <code>1</code>.</li>
</ul>

<p>Trả về giá trị <code>x</code> <strong>lớn nhất</strong> có thể đạt được.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 4, t = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác sau một lần để số có thể đạt được lớn nhất bằng <code>num</code>:</p>

<ul>
	<li>Giảm số có thể đạt được lớn nhất đi 1, đồng thời tăng <code>num</code> thêm 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num = 3, t = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<p>Thực hiện thao tác sau hai lần để số có thể đạt được lớn nhất bằng <code>num</code>:</p>

<ul>
	<li>Giảm số có thể đạt được lớn nhất đi 1, đồng thời tăng <code>num</code> thêm 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num, t&nbsp;&lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác đưa $x$ và $num$ đi một bước theo hai hướng ngược nhau, thực hiện nhiều nhất $t$ lần; ta cần tìm $x$ lớn nhất có thể bằng $num$. Không cần mô phỏng từng bước đưa chúng lại gần nhau.
>
> Mỗi thao tác làm $x-num$ giảm $2$, nên $x$ lớn nhất có thể đạt được là $num+2t$.

<!-- thinking:end -->

Ta nhận thấy mỗi lần ta có thể giảm $x$ đi $1$ và tăng $num$ thêm $1$, khi đó hiệu giữa $x$ và $num$ sẽ giảm $2$. Vì có thể thực hiện thao tác này nhiều nhất $t$ lần, số có thể đạt được lớn nhất là $num + t \times 2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def theMaximumAchievableX(self, num: int, t: int) -> int:
        return num + t * 2
```

#### Java

```java
class Solution {
    public int theMaximumAchievableX(int num, int t) {
        return num + t * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int theMaximumAchievableX(int num, int t) {
        return num + t * 2;
    }
};
```

#### Go

```go
func theMaximumAchievableX(num int, t int) int {
	return num + t*2
}
```

#### TypeScript

```ts
function theMaximumAchievableX(num: number, t: number): number {
    return num + t * 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
