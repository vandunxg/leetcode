---
comments: true
difficulty: Medium
rating: 1508
source: Biweekly Contest 6 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [1151. Minimum Swaps to Group All 1's Together 🔒](https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together)

[中文文档](/solution/1100-1199/1151.Minimum%20Swaps%20to%20Group%20All%201%27s%20Together/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>data</code>, hãy trả về số lần swap ít nhất cần thiết để nhóm tất cả giá trị <code>1</code> trong mảng lại với nhau ở <strong>bất kỳ vị trí nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> data = [1,0,1,0,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có 3 cách nhóm tất cả giá trị 1 lại với nhau:
[1,1,1,0,0] dùng 1 lần swap.
[0,1,1,1,0] dùng 2 lần swap.
[0,0,1,1,1] dùng 1 lần swap.
Số lần ít nhất là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> data = [0,0,0,1,0]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Vì trong mảng chỉ có một giá trị 1 nên không cần swap.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> data = [1,0,1,0,1,0,0,1,1,0,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Một kết quả có thể đạt được bằng 3 lần swap là [0,0,0,0,0,1,1,1,1,1,1].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= data.length &lt;= 10<sup>5</sup></code></li>
	<li><code>data[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Tất cả giá trị $1$ cần nằm trong một window có độ dài $k$, bằng tổng số giá trị $1$. Mỗi số $0$ trong window cần được swap, tức số lần swap bằng $k$ trừ đi số giá trị $1$ đã có sẵn trong đó. Trượt window để tìm số lượng giá trị $1$ lớn nhất; đáp án là $k$ trừ số lượng lớn nhất đó.

<!-- thinking:end -->

Trước tiên, ta đếm số giá trị $1$ trong mảng, gọi là $k$. Sau đó dùng sliding window kích thước $k$, di chuyển biên phải của window từ trái sang phải và đếm số giá trị $1$ trong window, gọi là $t$. Mỗi lần trượt window, ta cập nhật $t$. Khi biên phải đến cuối mảng, số giá trị $1$ trong window là lớn nhất, gọi là $mx$. Đáp án cuối cùng là $k - mx$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, data: List[int]) -> int:
        k = data.count(1)
        mx = t = sum(data[:k])
        for i in range(k, len(data)):
            t += data[i]
            t -= data[i - k]
            mx = max(mx, t)
        return k - mx
```

#### Java

```java
class Solution {
    public int minSwaps(int[] data) {
        int k = 0;
        for (int v : data) {
            k += v;
        }
        int t = 0;
        for (int i = 0; i < k; ++i) {
            t += data[i];
        }
        int mx = t;
        for (int i = k; i < data.length; ++i) {
            t += data[i];
            t -= data[i - k];
            mx = Math.max(mx, t);
        }
        return k - mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwaps(vector<int>& data) {
        int k = 0;
        for (int& v : data) {
            k += v;
        }
        int t = 0;
        for (int i = 0; i < k; ++i) {
            t += data[i];
        }
        int mx = t;
        for (int i = k; i < data.size(); ++i) {
            t += data[i];
            t -= data[i - k];
            mx = max(mx, t);
        }
        return k - mx;
    }
};
```

#### Go

```go
func minSwaps(data []int) int {
	k := 0
	for _, v := range data {
		k += v
	}
	t := 0
	for _, v := range data[:k] {
		t += v
	}
	mx := t
	for i := k; i < len(data); i++ {
		t += data[i]
		t -= data[i-k]
		mx = max(mx, t)
	}
	return k - mx
}
```

#### TypeScript

```ts
function minSwaps(data: number[]): number {
    const k = data.reduce((acc, cur) => acc + cur, 0);
    let t = data.slice(0, k).reduce((acc, cur) => acc + cur, 0);
    let mx = t;
    for (let i = k; i < data.length; ++i) {
        t += data[i] - data[i - k];
        mx = Math.max(mx, t);
    }
    return k - mx;
}
```

#### C#

```cs
public class Solution {
    public int MinSwaps(int[] data) {
        int k = data.Count(x => x == 1);
        int t = data.Take(k).Sum();
        int mx = t;
        for (int i = k; i < data.Length; ++i) {
            t += data[i];
            t -= data[i - k];
            mx = Math.Max(mx, t);
        }
        return k - mx;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
