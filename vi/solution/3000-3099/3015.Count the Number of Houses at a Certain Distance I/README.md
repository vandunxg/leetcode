---
comments: true
difficulty: Medium
rating: 1657
source: Weekly Contest 381 Q2
tags:
    - Breadth-First Search
    - Graph
    - Prefix Sum
---

<!-- problem:start -->

# [3015. Count the Number of Houses at a Certain Distance I](https://leetcode.com/problems/count-the-number-of-houses-at-a-certain-distance-i)

[中文文档](/solution/3000-3099/3015.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <strong>dương</strong> <code>n</code>, <code>x</code> và <code>y</code>.</p>

<p>Trong một thành phố, có các ngôi nhà được đánh số từ <code>1</code> đến <code>n</code>, được nối với nhau bởi <code>n</code> con đường. Có một con đường nối ngôi nhà số <code>i</code> với ngôi nhà số <code>i + 1</code> với mọi <code>1 &lt;= i &lt;= n - 1</code> . Một con đường bổ sung nối ngôi nhà số <code>x</code> với ngôi nhà số <code>y</code>.</p>

<p>Với mỗi <code>k</code>, sao cho <code>1 &lt;= k &lt;= n</code>, hãy tìm số <strong>cặp nhà</strong> <code>(house<sub>1</sub>, house<sub>2</sub>)</code> sao cho số con đường <strong>ít nhất</strong> cần đi để đến <code>house<sub>2</sub></code> từ <code>house<sub>1</sub></code> là <code>k</code>.</p>

<p>Trả về <em>một mảng được đánh chỉ số từ <strong>1</strong> </em><code>result</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>result[k]</code><em> biểu diễn <strong>tổng</strong> số cặp nhà sao cho số con đường <strong>ít nhất</strong> cần đi để đến một ngôi nhà từ ngôi nhà còn lại là </em><code>k</code>.</p>

<p><strong>Lưu ý</strong> rằng <code>x</code> và <code>y</code> có thể <strong>bằng nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3015.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20I/images/example2.png" style="width: 474px; height: 197px;" />
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
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3015.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20I/images/example3.png" style="width: 668px; height: 174px;" />
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
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3015.Count%20the%20Number%20of%20Houses%20at%20a%20Certain%20Distance%20I/images/example5.png" style="width: 544px; height: 130px;" />
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
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= x, y &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 100$, ta có thể liệt kê mọi cặp. Đồ thị là một đường đi cộng với cạnh bổ sung $(x,y)$.
>
> Đường đi ngắn nhất giữa $i$ và $j$ là giá trị nhỏ nhất trong khoảng cách trên đường đi và hai tuyến sử dụng cạnh bổ sung theo mỗi hướng.
>
> Ta liệt kê các cặp có thứ tự, lấy giá trị nhỏ nhất đó, rồi cộng $2$ vào ô tương ứng cho $(i,j)$ và $(j,i)$.

<!-- thinking:end -->

Ta có thể liệt kê từng cặp điểm $(i, j)$. Khoảng cách ngắn nhất từ $i$ đến $j$ là $min(|i - j|, |i - x| + 1 + |j - y|, |i - y| + 1 + |j - x|)$. Ta cộng $2$ vào số lượng của khoảng cách này vì cả $(i, j)$ và $(j, i)$ đều là các cặp điểm hợp lệ.

Độ phức tạp thời gian là $O(n^2)$, trong đó $n$ là giá trị $n$ được cho trong đề bài. Bỏ qua phần không gian dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countOfPairs(self, n: int, x: int, y: int) -> List[int]:
        x, y = x - 1, y - 1
        ans = [0] * n
        for i in range(n):
            for j in range(i + 1, n):
                a = j - i
                b = abs(i - x) + 1 + abs(j - y)
                c = abs(i - y) + 1 + abs(j - x)
                ans[min(a, b, c) - 1] += 2
        return ans
```

#### Java

```java
class Solution {
    public int[] countOfPairs(int n, int x, int y) {
        int[] ans = new int[n];
        x--;
        y--;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int a = j - i;
                int b = Math.abs(i - x) + 1 + Math.abs(j - y);
                int c = Math.abs(i - y) + 1 + Math.abs(j - x);
                ans[Math.min(a, Math.min(b, c)) - 1] += 2;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countOfPairs(int n, int x, int y) {
        vector<int> ans(n);
        x--;
        y--;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                int a = j - i;
                int b = abs(x - i) + abs(y - j) + 1;
                int c = abs(y - i) + abs(x - j) + 1;
                ans[min({a, b, c}) - 1] += 2;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countOfPairs(n int, x int, y int) []int {
	ans := make([]int, n)
	x, y = x-1, y-1
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			a := j - i
			b := abs(x-i) + abs(y-j) + 1
			c := abs(x-j) + abs(y-i) + 1
			ans[min(a, min(b, c))-1] += 2
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function countOfPairs(n: number, x: number, y: number): number[] {
    const ans: number[] = Array(n).fill(0);
    x--;
    y--;
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            const a = j - i;
            const b = Math.abs(x - i) + Math.abs(y - j) + 1;
            const c = Math.abs(y - i) + Math.abs(x - j) + 1;
            ans[Math.min(a, b, c) - 1] += 2;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
