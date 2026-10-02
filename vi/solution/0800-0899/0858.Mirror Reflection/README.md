---
comments: true
difficulty: Medium
tags:
    - Geometry
    - Math
    - Greatest Common Divisor
    - Number Theory
    - Least Common Multiple
---

<!-- problem:start -->

# [858. Mirror Reflection](https://leetcode.com/problems/mirror-reflection)

[中文文档](/solution/0800-0899/0858.Mirror%20Reflection/README.md)

## Mô tả

<!-- description:start -->

<p>Có một căn phòng hình vuông đặc biệt với gương trên cả bốn bức tường. Ngoại trừ góc tây nam, mỗi góc còn lại đều có một bộ thu, được đánh số <code>0</code>, <code>1</code> và <code>2</code>.</p>

<p>Các cạnh của căn phòng hình vuông có độ dài <code>p</code>. Tia laser phát ra từ góc tây nam lần đầu chạm vào tường phía đông tại điểm cách bộ thu thứ <code>0<sup>th</sup></code> một khoảng <code>q</code>.</p>

<p>Cho hai số nguyên <code>p</code> và <code>q</code>, hãy trả về <em>số hiệu của bộ thu mà tia laser gặp đầu tiên</em>.</p>

<p>Các bộ test được đảm bảo sao cho cuối cùng tia laser sẽ gặp một bộ thu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0858.Mirror%20Reflection/images/reflection.png" style="width: 218px; height: 217px;" />
<pre>
<strong>Đầu vào:</strong> p = 2, q = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Tia laser gặp bộ thu 2 lần đầu tiên khi nó phản xạ trở lại tường bên trái.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> p = 3, q = 1
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= q &lt;= p &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có thể trải phẳng các lần phản xạ trong căn phòng thành một đường thẳng trên lưới. Bộ thu đầu tiên tia laser chạm đến được quyết định bởi số phòng đi qua theo chiều $x$ và chiều $y$ là chẵn hay lẻ.
>
> Rút gọn $p$ và $q$ theo $\gcd(p,q)$ rồi xét tính chẵn lẻ: nếu cả hai đều lẻ thì bộ thu là $1$; nếu $p$ lẻ và $q$ chẵn thì là $0$; nếu $p$ chẵn và $q$ lẻ thì là $2$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def mirrorReflection(self, p: int, q: int) -> int:
        g = gcd(p, q)
        p = (p // g) % 2
        q = (q // g) % 2
        if p == 1 and q == 1:
            return 1
        return 0 if p == 1 else 2
```

#### Java

```java
class Solution {
    public int mirrorReflection(int p, int q) {
        int g = gcd(p, q);
        p = (p / g) % 2;
        q = (q / g) % 2;
        if (p == 1 && q == 1) {
            return 1;
        }
        return p == 1 ? 0 : 2;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int mirrorReflection(int p, int q) {
        int g = __gcd(p, q);
        p = (p / g) % 2;
        q = (q / g) % 2;
        if (p == 1 && q == 1) {
            return 1;
        }
        return p == 1 ? 0 : 2;
    }
};
```

#### Go

```go
func mirrorReflection(p int, q int) int {
	g := gcd(p, q)
	p = (p / g) % 2
	q = (q / g) % 2
	if p == 1 && q == 1 {
		return 1
	}
	if p == 1 {
		return 0
	}
	return 2
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function mirrorReflection(p: number, q: number): number {
    const g = gcd(p, q);
    p = Math.floor(p / g) % 2;
    q = Math.floor(q / g) % 2;
    if (p === 1 && q === 1) {
        return 1;
    }
    return p === 1 ? 0 : 2;
}

function gcd(a: number, b: number): number {
    return b === 0 ? a : gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
