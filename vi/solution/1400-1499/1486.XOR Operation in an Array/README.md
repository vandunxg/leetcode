---
comments: true
difficulty: Easy
rating: 1180
source: Weekly Contest 194 Q1
tags:
    - Bit Manipulation
    - Math
---

<!-- problem:start -->

# [1486. XOR Operation in an Array](https://leetcode.com/problems/xor-operation-in-an-array)

[中文文档](/solution/1400-1499/1486.XOR%20Operation%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code> và <code>start</code>.</p>

<p>Định nghĩa một mảng <code>nums</code> sao cho <code>nums[i] = start + 2 * i</code> (<strong>đánh chỉ số từ 0</strong>) và <code>n == nums.length</code>.</p>

<p>Trả về <em>phép XOR bit của tất cả các phần tử trong</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, start = 0
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Mảng nums bằng [0, 2, 4, 6, 8], trong đó (0 ^ 2 ^ 4 ^ 6 ^ 8) = 8.
Trong đó &quot;^&quot; tương ứng với toán tử XOR bit.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, start = 3
<strong>Đầu ra:</strong> 8
<strong>Giải thích:</strong> Mảng nums bằng [3, 5, 7, 9], trong đó (3 ^ 5 ^ 7 ^ 9) = 8.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= start &lt;= 1000</code></li>
	<li><code>n == nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 1000$. Sinh các giá trị $start+2i$ rồi XOR chúng lại với nhau.

<!-- thinking:end -->

Có thể mô phỏng trực tiếp để tính kết quả XOR của tất cả các phần tử trong mảng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def xorOperation(self, n: int, start: int) -> int:
        return reduce(xor, ((start + 2 * i) for i in range(n)))
```

#### Java

```java
class Solution {
    public int xorOperation(int n, int start) {
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans ^= start + 2 * i;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int xorOperation(int n, int start) {
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans ^= start + 2 * i;
        }
        return ans;
    }
};
```

#### Go

```go
func xorOperation(n int, start int) (ans int) {
	for i := 0; i < n; i++ {
		ans ^= start + 2*i
	}
	return
}
```

#### TypeScript

```ts
function xorOperation(n: number, start: number): number {
    let ans = 0;
    for (let i = 0; i < n; ++i) {
        ans ^= start + 2 * i;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
