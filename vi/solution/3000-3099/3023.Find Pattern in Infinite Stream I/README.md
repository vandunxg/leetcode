---
comments: true
difficulty: Medium
tags:
    - Array
    - Interactive
    - String Matching
    - Sliding Window
    - Hash Function
    - Rolling Hash
---

<!-- problem:start -->

# [3023. Find Pattern in Infinite Stream I 🔒](https://leetcode.com/problems/find-pattern-in-infinite-stream-i)

[中文文档](/solution/3000-3099/3023.Find%20Pattern%20in%20Infinite%20Stream%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng nhị phân <code>pattern</code> và một đối tượng <code>stream</code> thuộc lớp <code>InfiniteStream</code>, biểu diễn một stream vô hạn các bit được <strong>đánh chỉ số từ 0</strong>.</p>

<p>Lớp <code>InfiniteStream</code> có hàm sau:</p>

<ul>
	<li><code>int next()</code>: Đọc một bit <strong>duy nhất</strong> (là <code>0</code> hoặc <code>1</code>) từ stream và trả về bit đó.</li>
</ul>

<p>Hãy trả về <em><strong>chỉ số bắt đầu đầu tiên</strong> tại đó pattern khớp với các bit được đọc từ stream</em>. Ví dụ, nếu pattern là <code>[1, 0]</code>, lần khớp đầu tiên là phần được đánh dấu trong stream <code>[0, <strong><u>1, 0</u></strong>, 1, ...]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [1,1,1,0,1,1,1,...], pattern = [0,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [0,1] được đánh dấu trong stream [1,1,1,<strong><u>0,1</u></strong>,...], bắt đầu tại chỉ số 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [0,0,0,0,...], pattern = [0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [0] được đánh dấu trong stream [<strong><u>0</u></strong>,...], bắt đầu tại chỉ số 0.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> stream = [1,0,1,1,0,1,1,0,1,...], pattern = [1,1,0,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lần xuất hiện đầu tiên của pattern [1,1,0,1] được đánh dấu trong stream [1,0,<strong><u>1,1,0,1</u></strong>,...], bắt đầu tại chỉ số 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= pattern.length &lt;= 100</code></li>
	<li><code>pattern</code> chỉ gồm <code>0</code> và <code>1</code>.</li>
	<li><code>stream</code> chỉ gồm <code>0</code> và <code>1</code>.</li>
	<li>Đầu vào được tạo sao cho chỉ số bắt đầu của pattern tồn tại trong <code>10<sup>5</sup></code> bit đầu tiên của stream.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Pattern có độ dài tối đa là $100$ và stream không bị giới hạn, nên ta không thể lưu toàn bộ stream. Cách ngây thơ là so sánh tại mọi vị trí bắt đầu, có thể thực hiện khoảng $10^7$ phép so sánh.
>
> Độ dài $100$ vừa với hai số nguyên $64$ bit. Stream sẽ duy trì một cửa sổ trượt có cùng độ rộng.
>
> Mỗi bit mới sẽ dịch nửa bên phải; bit tràn đi vào nửa bên trái. Khi cửa sổ đã đầy, ta so sánh hai số nguyên tương ứng.

<!-- thinking:end -->

Ta nhận thấy độ dài của mảng $pattern$ không vượt quá $100$, do đó có thể dùng hai số nguyên $64$ bit $a$ và $b$ để biểu diễn các số nhị phân của nửa bên trái và nửa bên phải của $pattern$.

Tiếp theo, ta duyệt data stream, đồng thời duy trì hai số nguyên $64$ bit $x$ và $y$ để biểu diễn các số nhị phân của cửa sổ hiện tại có độ dài bằng $pattern$. Khi độ dài hiện tại đạt đến độ dài cửa sổ, ta kiểm tra xem $a$ và $x$ có bằng nhau hay không, đồng thời $b$ và $y$ có bằng nhau hay không. Nếu bằng nhau, ta trả về chỉ số của data stream hiện tại.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số phần tử trong data stream và $pattern$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
# Definition for an infinite stream.
# class InfiniteStream:
#     def next(self) -> int:
#         pass
class Solution:
    def findPattern(
        self, stream: Optional["InfiniteStream"], pattern: List[int]
    ) -> int:
        a = b = 0
        m = len(pattern)
        half = m >> 1
        mask1 = (1 << half) - 1
        mask2 = (1 << (m - half)) - 1
        for i in range(half):
            a |= pattern[i] << (half - 1 - i)
        for i in range(half, m):
            b |= pattern[i] << (m - 1 - i)
        x = y = 0
        for i in count(1):
            v = stream.next()
            y = y << 1 | v
            v = y >> (m - half) & 1
            y &= mask2
            x = x << 1 | v
            x &= mask1
            if i >= m and a == x and b == y:
                return i - m
```

#### Java

```java
/**
 * Definition for an infinite stream.
 * class InfiniteStream {
 *     public InfiniteStream(int[] bits);
 *     public int next();
 * }
 */
class Solution {
    public int findPattern(InfiniteStream infiniteStream, int[] pattern) {
        long a = 0, b = 0;
        int m = pattern.length;
        int half = m >> 1;
        long mask1 = (1L << half) - 1;
        long mask2 = (1L << (m - half)) - 1;
        for (int i = 0; i < half; ++i) {
            a |= (long) pattern[i] << (half - 1 - i);
        }
        for (int i = half; i < m; ++i) {
            b |= (long) pattern[i] << (m - 1 - i);
        }
        long x = 0, y = 0;
        for (int i = 1;; ++i) {
            int v = infiniteStream.next();
            y = y << 1 | v;
            v = (int) ((y >> (m - half)) & 1);
            y &= mask2;
            x = x << 1 | v;
            x &= mask1;
            if (i >= m && a == x && b == y) {
                return i - m;
            }
        }
    }
}
```

#### C++

```cpp
/**
 * Definition for an infinite stream.
 * class InfiniteStream {
 * public:
 *     InfiniteStream(vector<int> bits);
 *     int next();
 * };
 */
class Solution {
public:
    int findPattern(InfiniteStream* stream, vector<int>& pattern) {
        long long a = 0, b = 0;
        int m = pattern.size();
        int half = m >> 1;
        long long mask1 = (1LL << half) - 1;
        long long mask2 = (1LL << (m - half)) - 1;
        for (int i = 0; i < half; ++i) {
            a |= (long long) pattern[i] << (half - 1 - i);
        }
        for (int i = half; i < m; ++i) {
            b |= (long long) pattern[i] << (m - 1 - i);
        }
        long x = 0, y = 0;
        for (int i = 1;; ++i) {
            int v = stream->next();
            y = y << 1 | v;
            v = (int) ((y >> (m - half)) & 1);
            y &= mask2;
            x = x << 1 | v;
            x &= mask1;
            if (i >= m && a == x && b == y) {
                return i - m;
            }
        }
    }
};
```

#### Go

```go
/**
 * Definition for an infinite stream.
 * type InfiniteStream interface {
 *     Next() int
 * }
 */
 func findPattern(stream InfiniteStream, pattern []int) int {
	a, b := 0, 0
	m := len(pattern)
	half := m >> 1
	mask1 := (1 << half) - 1
	mask2 := (1 << (m - half)) - 1
	for i := 0; i < half; i++ {
		a |= pattern[i] << (half - 1 - i)
	}
	for i := half; i < m; i++ {
		b |= pattern[i] << (m - 1 - i)
	}
	x, y := 0, 0
	for i := 1; ; i++ {
		v := stream.Next()
		y = y<<1 | v
		v = (y >> (m - half)) & 1
		y &= mask2
		x = x<<1 | v
		x &= mask1
		if i >= m && a == x && b == y {
			return i - m
		}
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
