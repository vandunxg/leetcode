---
comments: true
difficulty: Easy
rating: 1144
source: Weekly Contest 311 Q1
tags:
    - Math
    - Number Theory
---

<!-- problem:start -->

# [2413. Smallest Even Multiple](https://leetcode.com/problems/smallest-even-multiple)

[中文文档](/solution/2400-2499/2413.Smallest%20Even%20Multiple/README.md)

## Mô tả

<!-- description:start -->

Cho một số nguyên <strong>dương</strong> <code>n</code>, hãy trả về <em>số nguyên dương nhỏ nhất là bội của <strong>cả</strong> </em><code>2</code><em> và </em><code>n</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> 10
<strong>Giải thích:</strong> Bội nhỏ nhất của cả 5 và 2 là 10.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 6
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Bội nhỏ nhất của cả 6 và 2 là 6. Lưu ý rằng một số là bội của chính nó.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 150</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 150$, ta cần tính $\mathrm{lcm}(2,n)$. Nếu $n$ chẵn thì bản thân nó đã là bội của $2$; ngược lại, nhân nó với $2$. Kết quả được tính trong $O(1)$.

<!-- thinking:end -->

Nếu $n$ chẵn, bội chung nhỏ nhất (LCM) của $2$ và $n$ chính là $n$. Ngược lại, bội chung nhỏ nhất của $2$ và $n$ là $n \times 2$.

Độ phức tạp thời gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestEvenMultiple(self, n: int) -> int:
        return n if n % 2 == 0 else n * 2
```

#### Java

```java
class Solution {
    public int smallestEvenMultiple(int n) {
        return n % 2 == 0 ? n : n * 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestEvenMultiple(int n) {
        return n % 2 == 0 ? n : n * 2;
    }
};
```

#### Go

```go
func smallestEvenMultiple(n int) int {
	if n%2 == 0 {
		return n
	}
	return n * 2
}
```

#### TypeScript

```ts
function smallestEvenMultiple(n: number): number {
    return n % 2 === 0 ? n : n * 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn smallest_even_multiple(n: i32) -> i32 {
        if n % 2 == 0 {
            return n;
        }
        n * 2
    }
}
```

#### C

```c
int smallestEvenMultiple(int n) {
    return n % 2 == 0 ? n : n * 2;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
