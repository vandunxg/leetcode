---
comments: true
difficulty: Hard
rating: 2709
source: Weekly Contest 381 Q4
tags:
    - Graph
    - Prefix Sum
---

<!-- problem:start -->

# [3017. Count the Number of Houses at a Certain Distance II](https://leetcode.com/problems/count-the-number-of-houses-at-a-certain-distance-ii)

[中文文档](/solution/3000-3099/3017.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <strong>dương</strong> <code>n</code>, <code>x</code> và <code>y</code>.</p>

<p>Trong một thành phố, có các ngôi nhà được đánh số từ <code>1</code> đến <code>n</code>, được nối với nhau bằng <code>n</code> con đường. Có một con đường nối ngôi nhà mang số <code>i</code> với ngôi nhà mang số <code>i + 1</code> với mọi <code>1 &lt;= i &lt;= n - 1</code> . Một con đường bổ sung nối ngôi nhà mang số <code>x</code> với ngôi nhà mang số <code>y</code>.</p>

<p>Với mỗi <code>k</code> thỏa mãn <code>1 &lt;= k &lt;= n</code>, hãy tìm số lượng <strong>cặp nhà</strong> <code>(house<sub>1</sub>, house<sub>2</sub>)</code> sao cho số con đường <strong>ít nhất</strong> cần đi để đến <code>house<sub>2</sub></code> từ <code>house<sub>1</sub></code> là <code>k</code>.</p>

<p>Hãy trả về <em>mảng được đánh chỉ số từ <strong>1</strong></em> <code>result</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>result[k]</code><em> biểu thị <strong>tổng</strong> số cặp nhà sao cho số con đường <strong>ít nhất</strong> cần đi để đi từ nhà này đến nhà kia là </em><code>k</code>.</p>

<p><strong>Lưu ý</strong> rằng <code>x</code> và <code>y</code> có thể <strong>bằng nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3017.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20II/images/example2.png" style="width: 474px; height: 197px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, x = 1, y = 3
<strong>Đầu ra:</strong> [6,0,0]
<strong>Giải thích:</strong> Xét từng cặp nhà:
- Với cặp (1, 2), ta có thể đi trực tiếp từ nhà 1 đến nhà 2.
- Với cặp (2, 1), ta có thể đi trực tiếp từ nhà 2 đến nhà 1.
- Với cặp (1, 3), ta có thể đi trực tiếp từ nhà 1 đến nhà 3.
- Với cặp (3, 1), ta có thể đi trực tiếp từ nhà 3 đến nhà 1.
- Với cặp (2, 3), ta có thể đi trực tiếp từ nhà 2 đến nhà 3.
- Với cặp (3, 2), ta có thể đi trực tiếp từ nhà 3 đến nhà 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3017.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20II/images/example3.png" style="width: 668px; height: 174px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, x = 2, y = 4
<strong>Đầu ra:</strong> [10,8,2,0,0]
<strong>Giải thích:</strong> Với mỗi khoảng cách k, các cặp nhà là:
- Với k == 1, các cặp là (1, 2), (2, 1), (2, 3), (3, 2), (2, 4), (4, 2), (3, 4), (4, 3), (4, 5) và (5, 4).
- Với k == 2, các cặp là (1, 3), (3, 1), (1, 4), (4, 1), (2, 5), (5, 2), (3, 5) và (5, 3).
- Với k == 3, các cặp là (1, 5) và (5, 1).
- Với k == 4 và k == 5, không có cặp nào.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3017.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20II/images/example5.png" style="width: 544px; height: 130px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, x = 1, y = 1
<strong>Đầu ra:</strong> [6,4,2,0]
<strong>Giải thích:</strong> Với mỗi khoảng cách k, các cặp nhà là:
- Với k == 1, các cặp là (1, 2), (2, 1), (2, 3), (3, 2), (3, 4) và (4, 3).
- Với k == 2, các cặp là (1, 3), (3, 1), (2, 4) và (4, 2).
- Với k == 3, các cặp là (1, 4) và (4, 1).
- Với k == 4, không có cặp nào.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= x, y &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n$ đã lên đến $10^5$, việc duyệt $O(n^2)$ như ở phần I không còn phù hợp. Đồ thị vẫn là một đường đi cộng thêm một cạnh, và histogram khoảng cách có công thức đóng.
>
> Nếu $|x-y| \le 1$, cạnh bổ sung không có tác dụng, nên ta đếm khoảng cách trên một đường đi. Nếu không, một chu trình có độ dài $|x-y|+1$ xuất hiện, với một nhánh ở mỗi phía.
>
> Ta cộng các histogram của các cặp trên đường đi, các cặp trên chu trình và các cặp từ nhánh đến chu trình, đồng thời xét riêng trường hợp độ dài chu trình chẵn/lẻ để không đếm trùng đường kính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfPairs(self, n: int, x: int, y: int) -> List[int]:
        if abs(x - y) <= 1:
            return [2 * x for x in reversed(range(n))]
        cycle_len = abs(x - y) + 1
        n2 = n - cycle_len + 2
        res = [2 * x for x in reversed(range(n2))]
        while len(res) < n:
            res.append(0)
        res2 = [cycle_len * 2] * (cycle_len >> 1)
        if not cycle_len & 1:
            res2[-1] = cycle_len
        res2[0] -= 2
        for i in range(len(res2)):
            res[i] += res2[i]
        if x > y:
            x, y = y, x
        tail1 = x - 1
        tail2 = n - y
        for tail in (tail1, tail2):
            if not tail:
                continue
            i_mx = tail + (cycle_len >> 1)
            val_mx = 4 * min((cycle_len - 3) >> 1, tail)
            i_mx2 = i_mx - (1 - (cycle_len & 1))
            res3 = [val_mx] * i_mx
            res3[0] = 0
            res3[1] = 0
            if not cycle_len & 1:
                res3[-1] = 0
            for i, j in enumerate(range(4, val_mx, 4)):
                res3[i + 2] = j
                res3[i_mx2 - i - 1] = j
            for i in range(1, tail + 1):
                res3[i] += 2
            if not cycle_len & 1:
                mn = cycle_len >> 1
                for i in range(mn, mn + tail):
                    res3[i] += 2
            for i in range(len(res3)):
                res[i] += res3[i]
        return res
```

#### Java

```java
class Solution {
    public long[] countOfPairs(int n, int x, int y) {
        --x;
        --y;
        if (x > y) {
        int temp = x;
        x = y;
        y = temp;
        }
        long[] diff = new long[n];
        for (int i = 0; i < n; ++i) {
        diff[0] += 1 + 1;
        ++diff[Math.min(Math.abs(i - x), Math.abs(i - y) + 1)];
        ++diff[Math.min(Math.abs(i - y), Math.abs(i - x) + 1)];
        --diff[Math.min(Math.abs(i - 0), Math.abs(i - y) + 1 + Math.abs(x - 0))];
        --diff[Math.min(Math.abs(i - (n - 1)),
                        Math.abs(i - x) + 1 + Math.abs(y - (n - 1)))];
        --diff[Math.max(x - i, 0) + Math.max(i - y, 0) + ((y - x) + 0) / 2];
        --diff[Math.max(x - i, 0) + Math.max(i - y, 0) + ((y - x) + 1) / 2];
        }
        for (int i = 0; i + 1 < n; ++i) {
        diff[i + 1] += diff[i];
        }
        return diff;
    }
}
```

#### C++

```cpp
class Solution {
public:
  vector<long long> countOfPairs(int n, int x, int y) {
    --x, --y;
    if (x > y) {
      swap(x, y);
    }
    vector<long long> diff(n);
    for (int i = 0; i < n; ++i) {
      diff[0] += 1 + 1;
      ++diff[min(abs(i - x), abs(i - y) + 1)];
      ++diff[min(abs(i - y), abs(i - x) + 1)];
      --diff[min(abs(i - 0), abs(i - y) + 1 + abs(x - 0))];
      --diff[min(abs(i - (n - 1)), abs(i - x) + 1 + abs(y - (n - 1)))];
      --diff[max(x - i, 0) + max(i - y, 0) + ((y - x) + 0) / 2];
      --diff[max(x - i, 0) + max(i - y, 0) + ((y - x) + 1) / 2];
    }
    for (int i = 0; i + 1 < n; ++i) {
      diff[i + 1] += diff[i];
    }
    return diff;
  }
};
```

#### Go

```go
func countOfPairs(n int, x int, y int) []int64 {
	if x > y {
		x, y = y, x
	}
	A := make([]int64, n)
	for i := 1; i <= n; i++ {
		A[0] += 2
		A[min(int64(i-1), int64(math.Abs(float64(i-y)))+int64(x))] -= 1
		A[min(int64(n-i), int64(math.Abs(float64(i-x)))+1+int64(n-y))] -= 1
		A[min(int64(math.Abs(float64(i-x))), int64(math.Abs(float64(y-i)))+1)] += 1
		A[min(int64(math.Abs(float64(i-x)))+1, int64(math.Abs(float64(y-i))))] += 1
		r := max(int64(x-i), 0) + max(int64(i-y), 0)
		A[r+int64((y-x+0)/2)] -= 1
		A[r+int64((y-x+1)/2)] -= 1
	}
	for i := 1; i < n; i++ {
		A[i] += A[i-1]
	}

	return A
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
