---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Dynamic Programming
---

<!-- problem:start -->

# [338. Counting Bits](https://leetcode.com/problems/counting-bits)

[中文文档](/solution/0300-0399/0338.Counting%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho số nguyên <code>n</code>, hãy trả về mảng <code>ans</code> có độ dài <code>n + 1</code>, sao cho với mỗi <code>i</code> (<code>0 &lt;= i &lt;= n</code>), <code>ans[i]</code> là <strong>số bit 1</strong> trong biểu diễn nhị phân của <code>i</code>.</p>
Không giải bài này bằng các hàm built-in (ví dụ như <code>__builtin_popcount</code> trong C++).
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2
<strong>Đầu ra:</strong> [0,1,1]
<strong>Giải thích:</strong>
0 --&gt; 0
1 --&gt; 1
2 --&gt; 10
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> [0,1,1,2,1,2]
<strong>Giải thích:</strong>
0 --&gt; 0
1 --&gt; 1
2 --&gt; 10
3 --&gt; 11
4 --&gt; 100
5 --&gt; 101
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 10<sup>5</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong></p>

<ul>
	<li>Rất dễ nghĩ ra lời giải có thời gian chạy <code>O(n log n)</code>. Bạn có thể giải trong thời gian tuyến tính <code>O(n)</code>, có thể chỉ với một lượt duyệt không?</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số bit 1 của từng số nguyên trong $0\ldots n$. Duyệt bit riêng cho mỗi số tốn $O(\log i)$. Với $n\le 10^5$, cách này vẫn dùng được, nhưng ta có thể làm tốt hơn ở phần sau.
>
> Gọi `bit_count` (hoặc dùng `lowbit` lặp lại) cho từng $i$ để điền vào mảng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        return [i.bit_count() for i in range(n + 1)]
```

#### Java

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];
        for (int i = 0; i <= n; ++i) {
            ans[i] = Integer.bitCount(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> ans(n + 1);
        for (int i = 0; i <= n; ++i) {
            ans[i] = __builtin_popcount(i);
        }
        return ans;
    }
};
```

#### Go

```go
func countBits(n int) []int {
	ans := make([]int, n+1)
	for i := 0; i <= n; i++ {
		ans[i] = bits.OnesCount(uint(i))
	}
	return ans
}
```

#### TypeScript

```ts
function countBits(n: number): number[] {
    const ans: number[] = Array(n + 1).fill(0);
    for (let i = 0; i <= n; ++i) {
        ans[i] = bitCount(i);
    }
    return ans;
}

function bitCount(n: number): number {
    let count = 0;
    while (n) {
        n &= n - 1;
        ++count;
    }
    return count;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 đếm lại bit của từng số nguyên từ đầu. Xóa bit 1 thấp nhất sẽ tạo ra $i\&(i-1)$, là số đã biết đáp án, nên $ans[i]=ans[i\&(i-1)]+1$. Duyệt tăng dần để tính trong $O(n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        ans = [0] * (n + 1)
        for i in range(1, n + 1):
            ans[i] = ans[i & (i - 1)] + 1
        return ans
```

#### Java

```java
class Solution {
    public int[] countBits(int n) {
        int[] ans = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            ans[i] = ans[i & (i - 1)] + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countBits(int n) {
        vector<int> ans(n + 1);
        for (int i = 1; i <= n; ++i) {
            ans[i] = ans[i & (i - 1)] + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countBits(n int) []int {
	ans := make([]int, n+1)
	for i := 1; i <= n; i++ {
		ans[i] = ans[i&(i-1)] + 1
	}
	return ans
}
```

#### TypeScript

```ts
function countBits(n: number): number[] {
    const ans: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        ans[i] = ans[i & (i - 1)] + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
