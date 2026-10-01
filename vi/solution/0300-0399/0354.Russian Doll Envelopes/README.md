---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
    - Dynamic Programming
    - Sorting
    - Longest Increasing Subsequence
---

<!-- problem:start -->

# [354. Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes)

[中文文档](/solution/0300-0399/0354.Russian%20Doll%20Envelopes/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên 2D <code>envelopes</code>, trong đó <code>envelopes[i] = [w<sub>i</sub>, h<sub>i</sub>]</code> biểu diễn chiều rộng và chiều cao của một phong bì.</p>

<p>Một phong bì có thể lồng vào phong bì khác khi và chỉ khi cả chiều rộng lẫn chiều cao của nó đều nhỏ hơn chiều rộng và chiều cao của phong bì kia.</p>

<p>Hãy trả về <em>số phong bì nhiều nhất có thể lồng vào nhau (tức đặt phong bì này bên trong phong bì khác)</em>.</p>

<p><strong>Lưu ý:</strong> Không được xoay phong bì.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> envelopes = [[5,4],[6,4],[6,7],[2,3]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Số phong bì nhiều nhất có thể lồng vào nhau là <code>3</code> ([2,3] =&gt; [5,4] =&gt; [6,7]).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> envelopes = [[1,1],[1,1],[1,1]]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= envelopes.length &lt;= 10<sup>5</sup></code></li>
	<li><code>envelopes[i].length == 2</code></li>
	<li><code>1 &lt;= w<sub>i</sub>, h<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một phong bì chỉ có thể lồng vào phong bì khác nếu cả chiều rộng và chiều cao đều tăng; ta cần tìm chuỗi lồng dài nhất. LIS 2D với độ phức tạp $O(n^2)$ không phù hợp khi $n\le 10^5$. Sau khi sắp xếp theo chiều rộng, đáp án là LIS theo chiều cao; các phong bì có cùng chiều rộng không được lồng vào nhau.
>
> Sắp xếp chiều rộng tăng dần và chiều cao giảm dần, sau đó tìm LIS tham lam theo chiều cao: thêm vào cuối nếu giá trị lớn hơn phần tử cuối, nếu không thì dùng binary search để thay thế. Sắp xếp chiều cao giảm dần giúp mỗi chiều rộng chỉ đóng góp tối đa một phong bì.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        envelopes.sort(key=lambda x: (x[0], -x[1]))
        d = [envelopes[0][1]]
        for _, h in envelopes[1:]:
            if h > d[-1]:
                d.append(h)
            else:
                idx = bisect_left(d, h)
                d[idx] = h
        return len(d)
```

#### Java

```java
class Solution {
    public int maxEnvelopes(int[][] envelopes) {
        Arrays.sort(envelopes, (a, b) -> { return a[0] == b[0] ? b[1] - a[1] : a[0] - b[0]; });
        int n = envelopes.length;
        int[] d = new int[n + 1];
        d[1] = envelopes[0][1];
        int size = 1;
        for (int i = 1; i < n; ++i) {
            int x = envelopes[i][1];
            if (x > d[size]) {
                d[++size] = x;
            } else {
                int left = 1, right = size;
                while (left < right) {
                    int mid = (left + right) >> 1;
                    if (d[mid] >= x) {
                        right = mid;
                    } else {
                        left = mid + 1;
                    }
                }
                int p = d[left] >= x ? left : 1;
                d[p] = x;
            }
        }
        return size;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        sort(envelopes.begin(), envelopes.end(), [](const auto& e1, const auto& e2) {
            return e1[0] < e2[0] || (e1[0] == e2[0] && e1[1] > e2[1]);
        });
        int n = envelopes.size();
        vector<int> d{envelopes[0][1]};
        for (int i = 1; i < n; ++i) {
            int x = envelopes[i][1];
            if (x > d[d.size() - 1])
                d.push_back(x);
            else {
                int idx = lower_bound(d.begin(), d.end(), x) - d.begin();
                if (idx == d.size()) idx = 0;
                d[idx] = x;
            }
        }
        return d.size();
    }
};
```

#### Go

```go
func maxEnvelopes(envelopes [][]int) int {
	sort.Slice(envelopes, func(i, j int) bool {
		if envelopes[i][0] != envelopes[j][0] {
			return envelopes[i][0] < envelopes[j][0]
		}
		return envelopes[j][1] < envelopes[i][1]
	})
	n := len(envelopes)
	d := make([]int, n+1)
	d[1] = envelopes[0][1]
	size := 1
	for _, e := range envelopes[1:] {
		x := e[1]
		if x > d[size] {
			size++
			d[size] = x
		} else {
			left, right := 1, size
			for left < right {
				mid := (left + right) >> 1
				if d[mid] >= x {
					right = mid
				} else {
					left = mid + 1
				}
			}
			if d[left] < x {
				left = 1
			}
			d[left] = x
		}
	}
	return size
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
