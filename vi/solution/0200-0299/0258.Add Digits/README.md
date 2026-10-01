---
comments: true
difficulty: Easy
tags:
    - Math
    - Number Theory
    - Simulation
---

<!-- problem:start -->

# [258. Add Digits](https://leetcode.com/problems/add-digits)

[中文文档](/solution/0200-0299/0258.Add%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>num</code>. Liên tục cộng các chữ số của số đó cho đến khi kết quả chỉ còn một chữ số, rồi trả về kết quả.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 38
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Quá trình thực hiện như sau:
38 --&gt; 3 + 8 --&gt; 11
11 --&gt; 1 + 1 --&gt; 2 
Vì 2 chỉ có một chữ số, hãy trả về giá trị này.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 0
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= num &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể giải bài này trong thời gian <code>O(1)</code> mà không dùng vòng lặp hay đệ quy không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cộng lặp đi lặp lại các chữ số sẽ cho ra digital root. Với số nguyên không âm, kết quả là $0$ khi $\textit{num}=0$, còn trong các trường hợp khác là $(\textit{num}-1)\bmod 9+1$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def addDigits(self, num: int) -> int:
        return 0 if num == 0 else (num - 1) % 9 + 1
```

#### Java

```java
class Solution {
    public int addDigits(int num) {
        return (num - 1) % 9 + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int addDigits(int num) {
        return (num - 1) % 9 + 1;
    }
};
```

#### Go

```go
func addDigits(num int) int {
	if num == 0 {
		return 0
	}
	return (num-1)%9 + 1
}
```

#### Rust

```rust
impl Solution {
    pub fn add_digits(mut num: i32) -> i32 {
        ((num - 1) % 9) + 1
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
