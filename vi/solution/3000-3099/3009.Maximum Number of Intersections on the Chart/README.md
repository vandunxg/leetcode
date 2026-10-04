---
comments: true
difficulty: Hard
tags:
    - Binary Indexed Tree
    - Geometry
    - Array
    - Hash Table
    - Math
    - Sorting
    - Sweep Line
---

<!-- problem:start -->

# [3009. Maximum Number of Intersections on the Chart 🔒](https://leetcode.com/problems/maximum-number-of-intersections-on-the-chart)

[中文文档](/solution/3000-3099/3009.Maximum%20Number%20of%20Intersections%20on%20the%20Chart/README.md)

## Mô tả

<!-- description:start -->

<p>Có một biểu đồ đường gồm <code>n</code> điểm được nối với nhau bằng các đoạn thẳng. Bạn được cho một mảng số nguyên <code>y</code> <strong>đánh chỉ số từ 1</strong>. Điểm <code>k<sup>th</sup></code> có tọa độ <code>(k, y[k])</code>. Không có đường nằm ngang, nghĩa là không có hai điểm liên tiếp nào có cùng tọa độ y.</p>

<p>Ta có thể vẽ một đường thẳng nằm ngang dài vô hạn. Hãy trả về <em>số giao điểm <strong>lớn nhất</strong> của đường thẳng với biểu đồ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3009.Maximum%20Number%20of%20Intersections%20on%20the%20Chart/images/20231208-020549.jpeg" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; height: 217px; width: 600px;" /></strong>

<pre>
<strong>Đầu vào:</strong> y = [1,2,1,2,1,3,2]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Như bạn có thể thấy trong hình trên, đường thẳng y = 1.5 có 5 giao điểm với biểu đồ (được đánh dấu bằng các dấu chéo màu đỏ). Đường thẳng y = 2 cũng cắt biểu đồ tại 4 điểm (được đánh dấu bằng các dấu chéo màu đỏ). Có thể chứng minh rằng không có đường thẳng nằm ngang nào cắt biểu đồ tại hơn 5 điểm. Vì vậy, đáp án là 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3009.Maximum%20Number%20of%20Intersections%20on%20the%20Chart/images/20231208-020557.jpeg" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 400px; height: 404px;" /></strong>

<pre>
<strong>Đầu vào:</strong> y = [2,1,3,4,5]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Như bạn có thể thấy trong hình trên, đường thẳng y = 1.5 có 2 giao điểm với biểu đồ (được đánh dấu bằng các dấu chéo màu đỏ). Đường thẳng y = 2 cũng cắt biểu đồ tại 2 điểm (được đánh dấu bằng các dấu chéo màu đỏ). Có thể chứng minh rằng không có đường thẳng nằm ngang nào cắt biểu đồ tại hơn 2 điểm. Vì vậy, đáp án là 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= y.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= y[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>y[i] != y[i + 1]</code> với <code>i</code> thuộc khoảng <code>[1, n - 1]</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có $n \le 10^5$ đoạn thẳng, nên việc xét từng cặp đoạn thẳng là quá chậm. Ta cần tìm đường nằm ngang $y=k+0.5$ cắt qua nhiều đoạn thẳng nhất.
>
> Chiếu mỗi đoạn thẳng lên một khoảng nửa kín theo phương dọc biến bài toán thành tìm độ phủ lớn nhất của các khoảng. Ta nhân đôi tọa độ, đồng thời thu nhỏ các đầu mút không ở cuối đi $1$ để các giá trị $y$ nguyên không bị đếm trùng.
>
> Một mảng hiệu trên TreeMap ghi nhận $+1/-1$ tại các đầu mút; quét prefix sẽ cho độ phủ lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxIntersectionCount(self, y: List[int]) -> int:
        n = len(y)
        line = defaultdict(int)
        for i in range(1, n):
            start = 2 * y[i - 1]
            end = 2 * y[i] + (0 if i == n - 1 else (-1 if y[i] > y[i - 1] else 1))
            line[min(start, end)] += 1
            line[max(start, end) + 1] -= 1
        ans = intersection = 0
        for _, count in sorted(line.items()):
            intersection += count
            ans = max(ans, intersection)
        return ans
```

#### Java

```java
class Solution {
    public int maxIntersectionCount(int[] y) {
        final int n = y.length;
        int ans = 0;
        int intersectionCount = 0;
        TreeMap<Integer, Integer> line = new TreeMap<>();

        for (int i = 1; i < n; ++i) {
            final int start = 2 * y[i - 1];
            final int end = 2 * y[i] + (i == n - 1 ? 0 : y[i] > y[i - 1] ? -1 : 1);
            line.merge(Math.min(start, end), 1, Integer::sum);
            line.merge(Math.max(start, end) + 1, -1, Integer::sum);
        }

        for (final int count : line.values()) {
            intersectionCount += count;
            ans = Math.max(ans, intersectionCount);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxIntersectionCount(vector<int>& y) {
        int n = y.size();
        map<int, int> line;
        for (int i = 1; i < n; ++i) {
            int start = 2 * y[i - 1];
            int end = 2 * y[i] + (i == n - 1 ? 0 : (y[i] > y[i - 1] ? -1 : 1));
            ++line[min(start, end)];
            --line[max(start, end) + 1];
        }
        int ans = 0, intersection = 0;
        for (auto& [_, count] : line) {
            intersection += count;
            ans = max(ans, intersection);
        }
        return ans;
    }
};
```

#### Go

```go
func maxIntersectionCount(y []int) (ans int) {
	n := len(y)
	line := map[int]int{}
	for i := 1; i < n; i++ {
		start := 2 * y[i-1]
		end := 2 * y[i]
		if i != n-1 {
			if y[i] > y[i-1] {
				end--
			} else {
				end++
			}
		}
		a, b := min(start, end), max(start, end)
		line[a]++
		line[b+1]--
	}
	keys := make([]int, 0, len(line))
	for k := range line {
		keys = append(keys, k)
	}
	sort.Ints(keys)
	intersection := 0
	for _, k := range keys {
		intersection += line[k]
		if ans < intersection {
			ans = intersection
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
