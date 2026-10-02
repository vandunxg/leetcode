---
comments: true
difficulty: Medium
rating: 1386
source: Weekly Contest 165 Q2
tags:
    - Math
---

<!-- problem:start -->

# [1276. Number of Burgers with No Waste of Ingredients](https://leetcode.com/problems/number-of-burgers-with-no-waste-of-ingredients)

[中文文档](/solution/1200-1299/1276.Number%20of%20Burgers%20with%20No%20Waste%20of%20Ingredients/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>tomatoSlices</code> và <code>cheeseSlices</code>. Nguyên liệu cần dùng cho mỗi loại burger như sau:</p>

<ul>
	<li><strong>Jumbo Burger:</strong> <code>4</code> lát cà chua và <code>1</code> lát phô mai.</li>
	<li><strong>Small Burger:</strong> <code>2</code> lát cà chua và <code>1</code> lát phô mai.</li>
</ul>

<p>Trả về <code>[total_jumbo, total_small]</code> sao cho số <code>tomatoSlices</code> còn dư bằng <code>0</code> và số <code>cheeseSlices</code> còn dư cũng bằng <code>0</code>. Nếu không thể dùng hết cả <code>tomatoSlices</code> lẫn <code>cheeseSlices</code>, hãy trả về <code>[]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> tomatoSlices = 16, cheeseSlices = 7
<strong>Output:</strong> [1,6]
<strong>Giải thích:</strong> Để làm một jumbo burger và 6 small burger, ta cần 4*1 + 2*6 = 16 lát cà chua và 1 + 6 = 7 lát phô mai.
Không còn nguyên liệu nào dư.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> tomatoSlices = 17, cheeseSlices = 4
<strong>Output:</strong> []
<strong>Giải thích:</strong> Không có cách nào dùng hết nguyên liệu để làm các small burger và jumbo burger.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> tomatoSlices = 4, cheeseSlices = 17
<strong>Output:</strong> []
<strong>Giải thích:</strong> Nếu làm 1 jumbo burger thì còn dư 16 lát phô mai; nếu làm 2 small burger thì còn dư 15 lát phô mai.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= tomatoSlices, cheeseSlices &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Một jumbo burger cần $4$ lát cà chua và $1$ lát phô mai; một small burger cần $2$ lát cà chua và $1$ lát phô mai, và không được dư nguyên liệu. Ta có hai phương trình với hai ẩn và cần nghiệm nguyên không âm. Số lát cà chua có thể lên tới $10^7$, nên thử lần lượt số lượng từng loại burger sẽ chậm; công thức trực tiếp có độ phức tạp $O(1)$.

<!-- thinking:end -->

Gọi số Jumbo Burger là $x$ và số Small Burger là $y$, ta có:

$$
\begin{aligned}
4x + 2y &= tomatoSlices \\
x + y &= cheeseSlices
\end{aligned}
$$

Biến đổi hai phương trình trên, ta được:

$$
\begin{aligned}
y = (4 \times cheeseSlices - tomatoSlices) / 2 \\
x = cheeseSlices - y
\end{aligned}
$$

Trong đó, $x$ và $y$ phải là các số nguyên không âm.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numOfBurgers(self, tomatoSlices: int, cheeseSlices: int) -> List[int]:
        k = 4 * cheeseSlices - tomatoSlices
        y = k // 2
        x = cheeseSlices - y
        return [] if k % 2 or y < 0 or x < 0 else [x, y]
```

#### Java

```java
class Solution {
    public List<Integer> numOfBurgers(int tomatoSlices, int cheeseSlices) {
        int k = 4 * cheeseSlices - tomatoSlices;
        int y = k / 2;
        int x = cheeseSlices - y;
        return k % 2 != 0 || y < 0 || x < 0 ? List.of() : List.of(x, y);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numOfBurgers(int tomatoSlices, int cheeseSlices) {
        int k = 4 * cheeseSlices - tomatoSlices;
        int y = k / 2;
        int x = cheeseSlices - y;
        return k % 2 || x < 0 || y < 0 ? vector<int>{} : vector<int>{x, y};
    }
};
```

#### Go

```go
func numOfBurgers(tomatoSlices int, cheeseSlices int) []int {
	k := 4*cheeseSlices - tomatoSlices
	y := k / 2
	x := cheeseSlices - y
	if k%2 != 0 || x < 0 || y < 0 {
		return []int{}
	}
	return []int{x, y}
}
```

#### TypeScript

```ts
function numOfBurgers(tomatoSlices: number, cheeseSlices: number): number[] {
    const k = 4 * cheeseSlices - tomatoSlices;
    const y = k >> 1;
    const x = cheeseSlices - y;
    return k % 2 || y < 0 || x < 0 ? [] : [x, y];
}
```

#### Rust

```rust
impl Solution {
    pub fn num_of_burgers(tomato_slices: i32, cheese_slices: i32) -> Vec<i32> {
        let k = 4 * cheese_slices - tomato_slices;
        let y = k / 2;
        let x = cheese_slices - y;
        if k % 2 != 0 || y < 0 || x < 0 {
            Vec::new()
        } else {
            vec![x, y]
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
