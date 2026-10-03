---
comments: true
difficulty: Medium
tags:
    - Math
    - Enumeration
---

<!-- problem:start -->

# [1999. Smallest Greater Multiple Made of Two Digits 🔒](https://leetcode.com/problems/smallest-greater-multiple-made-of-two-digits)

[中文文档](/solution/1900-1999/1999.Smallest%20Greater%20Multiple%20Made%20of%20Two%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba số nguyên <code>k</code>, <code>digit1</code> và <code>digit2</code>, hãy tìm số nguyên <strong>nhỏ nhất</strong> thỏa mãn các điều kiện sau:</p>

<ul>
	<li><strong>Lớn hơn</strong> <code>k</code>,</li>
	<li>Là <strong>bội số</strong> của <code>k</code>, và</li>
	<li><strong>Chỉ</strong> gồm các chữ số <code>digit1</code> và/hoặc <code>digit2</code>.</li>
</ul>

<p>Trả về <em>số nguyên như vậy <strong>nhỏ nhất</strong>. Nếu không tồn tại số nguyên nào hoặc số nguyên đó vượt quá giới hạn của số nguyên có dấu 32-bit (</em><code>2<sup>31</sup> - 1</code><em>), hãy trả về </em><code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2, digit1 = 0, digit2 = 2
<strong>Đầu ra:</strong> 20
<strong>Giải thích:</strong>
20 là số nguyên đầu tiên lớn hơn 2, là bội số của 2 và chỉ gồm các chữ số 0 và/hoặc 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 3, digit1 = 4, digit2 = 2
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong>
24 là số nguyên đầu tiên lớn hơn 3, là bội số của 3 và chỉ gồm các chữ số 4 và/hoặc 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> k = 2, digit1 = 0, digit2 = 0
<strong>Đầu ra:</strong> -1
<strong>Giải thích:
</strong>Không có số nguyên nào thỏa mãn yêu cầu, nên trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= 1000</code></li>
	<li><code>0 &lt;= digit1 &lt;= 9</code></li>
	<li><code>0 &lt;= digit2 &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên nhỏ nhất lớn hơn $k$, là bội của $k$ và chỉ sử dụng hai chữ số đã cho. Việc sinh các bội của $k$ có thể bỏ qua nhiều ràng buộc về chữ số; BFS trên các chữ số sẽ sinh mọi số hợp lệ theo đúng thứ tự.
>
> Sắp xếp hai chữ số rồi nối từng chữ số vào giá trị hiện tại. Hàng đợi được duyệt theo độ dài tăng dần, sau đó theo thứ tự từ điển, nên ứng viên đầu tiên $>k$ và chia hết cho $k$ là nhỏ nhất. Nếu vượt quá $2^{31}-1$ thì không tồn tại đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findInteger(self, k: int, digit1: int, digit2: int) -> int:
        if digit1 == 0 and digit2 == 0:
            return -1
        if digit1 > digit2:
            return self.findInteger(k, digit2, digit1)
        q = deque([0])
        while 1:
            x = q.popleft()
            if x > 2**31 - 1:
                return -1
            if x > k and x % k == 0:
                return x
            q.append(x * 10 + digit1)
            if digit1 != digit2:
                q.append(x * 10 + digit2)
```

#### Java

```java
class Solution {
    public int findInteger(int k, int digit1, int digit2) {
        if (digit1 == 0 && digit2 == 0) {
            return -1;
        }
        if (digit1 > digit2) {
            return findInteger(k, digit2, digit1);
        }
        Deque<Long> q = new ArrayDeque<>();
        q.offer(0L);
        while (true) {
            long x = q.poll();
            if (x > Integer.MAX_VALUE) {
                return -1;
            }
            if (x > k && x % k == 0) {
                return (int) x;
            }
            q.offer(x * 10 + digit1);
            if (digit1 != digit2) {
                q.offer(x * 10 + digit2);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findInteger(int k, int digit1, int digit2) {
        if (digit1 == 0 && digit2 == 0) {
            return -1;
        }
        if (digit1 > digit2) {
            swap(digit1, digit2);
        }
        queue<long long> q{{0}};
        while (1) {
            long long x = q.front();
            q.pop();
            if (x > INT_MAX) {
                return -1;
            }
            if (x > k && x % k == 0) {
                return x;
            }
            q.emplace(x * 10 + digit1);
            if (digit1 != digit2) {
                q.emplace(x * 10 + digit2);
            }
        }
    }
};
```

#### Go

```go
func findInteger(k int, digit1 int, digit2 int) int {
	if digit1 == 0 && digit2 == 0 {
		return -1
	}
	if digit1 > digit2 {
		digit1, digit2 = digit2, digit1
	}
	q := []int{0}
	for {
		x := q[0]
		q = q[1:]
		if x > math.MaxInt32 {
			return -1
		}
		if x > k && x%k == 0 {
			return x
		}
		q = append(q, x*10+digit1)
		if digit1 != digit2 {
			q = append(q, x*10+digit2)
		}
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
