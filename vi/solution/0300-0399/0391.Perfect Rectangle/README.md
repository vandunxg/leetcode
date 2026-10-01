---
comments: true
difficulty: Hard
tags:
    - Geometry
    - Array
    - Hash Table
    - Math
    - Sweep Line
---

<!-- problem:start -->

# [391. Perfect Rectangle](https://leetcode.com/problems/perfect-rectangle)

[中文文档](/solution/0300-0399/0391.Perfect%20Rectangle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>rectangles</code>, trong đó <code>rectangles[i] = [x<sub>i</sub>, y<sub>i</sub>, a<sub>i</sub>, b<sub>i</sub>]</code> biểu diễn một hình chữ nhật có các cạnh song song với trục tọa độ. Điểm dưới bên trái là <code>(x<sub>i</sub>, y<sub>i</sub>)</code>, còn điểm trên bên phải là <code>(a<sub>i</sub>, b<sub>i</sub>)</code>.</p>

<p>Trả về <code>true</code> <em>nếu tất cả các hình chữ nhật ghép lại tạo thành một hình chữ nhật lớn, không có phần thừa hay thiếu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0391.Perfect%20Rectangle/images/perectrec1-plane.jpg" style="width: 300px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> rectangles = [[1,1,3,3],[3,1,4,2],[3,2,4,4],[1,3,2,4],[2,3,3,4]]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Cả 5 hình chữ nhật ghép lại vừa khít thành một hình chữ nhật lớn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0391.Perfect%20Rectangle/images/perfectrec2-plane.jpg" style="width: 300px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> rectangles = [[1,1,2,3],[1,3,2,4],[3,1,4,2],[3,2,4,4]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Vì có một khoảng trống giữa hai vùng hình chữ nhật.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0300-0399/0391.Perfect%20Rectangle/images/perfecrrec4-plane.jpg" style="width: 300px; height: 294px;" />
<pre>
<strong>Đầu vào:</strong> rectangles = [[1,1,3,3],[3,1,4,2],[1,3,2,4],[2,2,4,4]]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Vì hai hình chữ nhật bị chồng lấn lên nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rectangles.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>rectangles[i].length == 4</code></li>
	<li><code>-10<sup>5</sup> &lt;= x<sub>i</sub> &lt; a<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= y<sub>i</sub> &lt; b<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Làm sao xác định các hình chữ nhật nhỏ ghép kín một hình lớn mà không có khoảng trống hay phần chồng lấn? Dùng sweep line khá phức tạp. Với một phép phủ hoàn hảo, tổng diện tích phải bằng diện tích hình lớn; mỗi đỉnh, ngoại trừ bốn góc ngoài cùng, xuất hiện số lần chẵn ($2$ hoặc $4$).
>
> Ta cộng diện tích và đếm số lần xuất hiện của từng đỉnh. Bốn góc của bounding box phải xuất hiện đúng một lần; mọi đỉnh khác phải xuất hiện $2$ hoặc $4$ lần; tổng diện tích phải bằng diện tích bounding box.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isRectangleCover(self, rectangles: List[List[int]]) -> bool:
        area = 0
        minX, minY = rectangles[0][0], rectangles[0][1]
        maxX, maxY = rectangles[0][2], rectangles[0][3]
        cnt = defaultdict(int)

        for r in rectangles:
            area += (r[2] - r[0]) * (r[3] - r[1])

            minX = min(minX, r[0])
            minY = min(minY, r[1])
            maxX = max(maxX, r[2])
            maxY = max(maxY, r[3])

            cnt[(r[0], r[1])] += 1
            cnt[(r[0], r[3])] += 1
            cnt[(r[2], r[3])] += 1
            cnt[(r[2], r[1])] += 1

        if (
            area != (maxX - minX) * (maxY - minY)
            or cnt[(minX, minY)] != 1
            or cnt[(minX, maxY)] != 1
            or cnt[(maxX, maxY)] != 1
            or cnt[(maxX, minY)] != 1
        ):
            return False

        del cnt[(minX, minY)], cnt[(minX, maxY)], cnt[(maxX, maxY)], cnt[(maxX, minY)]

        return all(c == 2 or c == 4 for c in cnt.values())
```

#### Java

```java
class Solution {
    public boolean isRectangleCover(int[][] rectangles) {
        long area = 0;
        int minX = rectangles[0][0], minY = rectangles[0][1];
        int maxX = rectangles[0][2], maxY = rectangles[0][3];
        Map<Pair, Integer> cnt = new HashMap<>();

        for (int[] r : rectangles) {
            area += (r[2] - r[0]) * (r[3] - r[1]);

            minX = Math.min(minX, r[0]);
            minY = Math.min(minY, r[1]);
            maxX = Math.max(maxX, r[2]);
            maxY = Math.max(maxY, r[3]);

            cnt.merge(new Pair(r[0], r[1]), 1, Integer::sum);
            cnt.merge(new Pair(r[0], r[3]), 1, Integer::sum);
            cnt.merge(new Pair(r[2], r[3]), 1, Integer::sum);
            cnt.merge(new Pair(r[2], r[1]), 1, Integer::sum);
        }

        if (area != (long) (maxX - minX) * (maxY - minY)
            || cnt.getOrDefault(new Pair(minX, minY), 0) != 1
            || cnt.getOrDefault(new Pair(minX, maxY), 0) != 1
            || cnt.getOrDefault(new Pair(maxX, maxY), 0) != 1
            || cnt.getOrDefault(new Pair(maxX, minY), 0) != 1) {
            return false;
        }

        cnt.remove(new Pair(minX, minY));
        cnt.remove(new Pair(minX, maxY));
        cnt.remove(new Pair(maxX, maxY));
        cnt.remove(new Pair(maxX, minY));

        return cnt.values().stream().allMatch(c -> c == 2 || c == 4);
    }

    private static class Pair {
        final int first;
        final int second;

        Pair(int first, int second) {
            this.first = first;
            this.second = second;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) {
                return true;
            }
            if (o == null || getClass() != o.getClass()) {
                return false;
            }
            Pair pair = (Pair) o;
            return first == pair.first && second == pair.second;
        }

        @Override
        public int hashCode() {
            return Objects.hash(first, second);
        }
    }
}
```

#### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        long long area = 0;
        int minX = rectangles[0][0], minY = rectangles[0][1];
        int maxX = rectangles[0][2], maxY = rectangles[0][3];

        using pii = pair<int, int>;
        map<pii, int> cnt;

        for (auto& r : rectangles) {
            area += (r[2] - r[0]) * (r[3] - r[1]);

            minX = min(minX, r[0]);
            minY = min(minY, r[1]);
            maxX = max(maxX, r[2]);
            maxY = max(maxY, r[3]);

            ++cnt[{r[0], r[1]}];
            ++cnt[{r[0], r[3]}];
            ++cnt[{r[2], r[3]}];
            ++cnt[{r[2], r[1]}];
        }

        if (area != (long long) (maxX - minX) * (maxY - minY) || cnt[{minX, minY}] != 1 || cnt[{minX, maxY}] != 1 || cnt[{maxX, maxY}] != 1 || cnt[{maxX, minY}] != 1) {
            return false;
        }

        cnt.erase({minX, minY});
        cnt.erase({minX, maxY});
        cnt.erase({maxX, maxY});
        cnt.erase({maxX, minY});

        return all_of(cnt.begin(), cnt.end(), [](pair<pii, int> e) {
            return e.second == 2 || e.second == 4;
        });
    }
};
```

#### Go

```go
type pair struct {
	first  int
	second int
}

func isRectangleCover(rectangles [][]int) bool {
	area := 0
	minX, minY := rectangles[0][0], rectangles[0][1]
	maxX, maxY := rectangles[0][2], rectangles[0][3]

	cnt := make(map[pair]int)
	for _, r := range rectangles {
		area += (r[2] - r[0]) * (r[3] - r[1])

		minX = min(minX, r[0])
		minY = min(minY, r[1])
		maxX = max(maxX, r[2])
		maxY = max(maxY, r[3])

		cnt[pair{r[0], r[1]}]++
		cnt[pair{r[0], r[3]}]++
		cnt[pair{r[2], r[3]}]++
		cnt[pair{r[2], r[1]}]++
	}

	if area != (maxX-minX)*(maxY-minY) ||
		cnt[pair{minX, minY}] != 1 ||
		cnt[pair{minX, maxY}] != 1 ||
		cnt[pair{maxX, maxY}] != 1 ||
		cnt[pair{maxX, minY}] != 1 {
		return false
	}

	delete(cnt, pair{minX, minY})
	delete(cnt, pair{minX, maxY})
	delete(cnt, pair{maxX, maxY})
	delete(cnt, pair{maxX, minY})

	for _, c := range cnt {
		if c != 2 && c != 4 {
			return false
		}
	}
	return true
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
