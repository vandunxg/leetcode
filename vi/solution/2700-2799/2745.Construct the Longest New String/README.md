---
comments: true
difficulty: Medium
rating: 1607
source: Biweekly Contest 107 Q2
tags:
    - Greedy
    - Brainteaser
    - Math
    - Dynamic Programming
---

<!-- problem:start -->

# [2745. Construct the Longest New String](https://leetcode.com/problems/construct-the-longest-new-string)

[中文文档](/solution/2700-2799/2745.Construct%20the%20Longest%20New%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <code>x</code>, <code>y</code> và <code>z</code>.</p>

<p>Bạn có <code>x</code> chuỗi bằng <code>&quot;AA&quot;</code>, <code>y</code> chuỗi bằng <code>&quot;BB&quot;</code> và <code>z</code> chuỗi bằng <code>&quot;AB&quot;</code>. Bạn muốn chọn một số chuỗi (có thể chọn tất cả hoặc không chọn chuỗi nào) rồi nối chúng theo một thứ tự bất kỳ để tạo thành một chuỗi mới. Chuỗi mới này không được chứa <code>&quot;AAA&quot;</code> hoặc <code>&quot;BBB&quot;</code> dưới dạng chuỗi con.</p>

<p>Trả về <em>độ dài lớn nhất có thể của chuỗi mới</em>.</p>

<p><b>Chuỗi con</b> là một dãy ký tự liên tiếp <strong>không rỗng</strong> trong một chuỗi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 2, y = 5, z = 1
<strong>Đầu ra:</strong> 12
<strong>Giải thích: </strong>Ta có thể nối các chuỗi &quot;BB&quot;, &quot;AA&quot;, &quot;BB&quot;, &quot;AA&quot;, &quot;BB&quot; và &quot;AB&quot; theo thứ tự đó. Khi đó, chuỗi mới là &quot;BBAABBAABBAB&quot;.
Chuỗi này có độ dài 12, và có thể chứng minh rằng không thể tạo được chuỗi có độ dài lớn hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> x = 3, y = 2, z = 2
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Ta có thể nối các chuỗi &quot;AB&quot;, &quot;AB&quot;, &quot;AA&quot;, &quot;BB&quot;, &quot;AA&quot;, &quot;BB&quot; và &quot;AA&quot; theo thứ tự đó. Khi đó, chuỗi mới là &quot;ABABAABBAABBAA&quot;.
Chuỗi này có độ dài 14, và có thể chứng minh rằng không thể tạo được chuỗi có độ dài lớn hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= x, y, z &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Xét các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Nối $x$ bản sao của $AA$, $y$ bản sao của $BB$ và $z$ bản sao của $AB$ mà không tạo ra $AAA$ hoặc $BBB$, đồng thời tối đa hóa độ dài. Việc thử tất cả thứ tự trở nên tốn kém khi số lượng lên đến $50$.
>
> $AB$ an toàn ở cả hai đầu và có thể được đặt tự do; $AA$ và $BB$ phải được đặt xen kẽ. Số lượng lớn hơn trong $x$ và $y$ nhiều hơn số lượng còn lại nhiều nhất là một. Ba trường hợp này dẫn đến một công thức có thời gian hằng số.

<!-- thinking:end -->

Ta nhận thấy chuỗi 'AA' chỉ có thể được theo sau bởi 'BB', còn chuỗi 'AB' có thể được đặt ở đầu hoặc cuối chuỗi. Do đó:

- Nếu $x < y$, trước tiên ta có thể đặt xen kẽ 'BBAABBAA..BB', sử dụng tổng cộng $x$ chuỗi 'AA' và $x+1$ chuỗi 'BB', sau đó đặt $z$ chuỗi 'AB' còn lại, với tổng độ dài là $(x \times 2 + z + 1) \times 2$;
- Nếu $x > y$, trước tiên ta có thể đặt xen kẽ 'AABBAABB..AA', sử dụng tổng cộng $y$ chuỗi 'BB' và $y+1$ chuỗi 'AA', sau đó đặt $z$ chuỗi 'AB' còn lại, với tổng độ dài là $(y \times 2 + z + 1) \times 2$;
- Nếu $x = y$, ta chỉ cần đặt xen kẽ 'AABB', sử dụng tổng cộng $x$ chuỗi 'AA' và $y$ chuỗi 'BB', sau đó đặt $z$ chuỗi 'AB' còn lại, với tổng độ dài là $(x + y + z) \times 2$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestString(self, x: int, y: int, z: int) -> int:
        if x < y:
            return (x * 2 + z + 1) * 2
        if x > y:
            return (y * 2 + z + 1) * 2
        return (x + y + z) * 2
```

#### Java

```java
class Solution {
    public int longestString(int x, int y, int z) {
        if (x < y) {
            return (x * 2 + z + 1) * 2;
        }
        if (x > y) {
            return (y * 2 + z + 1) * 2;
        }
        return (x + y + z) * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int longestString(int x, int y, int z) {
        if (x < y) {
            return (x * 2 + z + 1) * 2;
        }
        if (x > y) {
            return (y * 2 + z + 1) * 2;
        }
        return (x + y + z) * 2;
    }
};
```

#### Go

```go
func longestString(x int, y int, z int) int {
	if x < y {
		return (x*2 + z + 1) * 2
	}
	if x > y {
		return (y*2 + z + 1) * 2
	}
	return (x + y + z) * 2
}
```

#### TypeScript

```ts
function longestString(x: number, y: number, z: number): number {
    if (x < y) {
        return (x * 2 + z + 1) * 2;
    }
    if (x > y) {
        return (y * 2 + z + 1) * 2;
    }
    return (x + y + z) * 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
