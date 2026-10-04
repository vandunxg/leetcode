---
comments: true
difficulty: Medium
rating: 1916
source: Biweekly Contest 146 Q3
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [3394. Check if Grid can be Cut into Sections](https://leetcode.com/problems/check-if-grid-can-be-cut-into-sections)

[中文文档](/solution/3300-3399/3394.Check%20if%20Grid%20can%20be%20Cut%20into%20Sections/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu diễn kích thước của một lưới <code>n x n</code><!-- notionvc: fa9fe4ed-dff8-4410-8196-346f2d430795 -->, với gốc tọa độ ở góc dưới bên trái của lưới. Đồng thời, cho một mảng tọa độ hai chiều <code>rectangles</code>, trong đó <code>rectangles[i]</code> có dạng <code>[start<sub>x</sub>, start<sub>y</sub>, end<sub>x</sub>, end<sub>y</sub>]</code>, biểu diễn một hình chữ nhật trên lưới. Mỗi hình chữ nhật được xác định như sau:</p>

<ul>
	<li><code>(start<sub>x</sub>, start<sub>y</sub>)</code>: Góc dưới bên trái của hình chữ nhật.</li>
	<li><code>(end<sub>x</sub>, end<sub>y</sub>)</code>: Góc trên bên phải của hình chữ nhật.</li>
</ul>

<p><strong>Lưu ý </strong>rằng các hình chữ nhật không chồng lấn. Nhiệm vụ của bạn là xác định liệu có thể tạo <strong>hai đường cắt ngang hoặc hai đường cắt dọc</strong> trên lưới sao cho:</p>

<ul>
	<li>Mỗi trong ba phần được tạo bởi các đường cắt chứa <strong>ít nhất</strong> một hình chữ nhật.</li>
	<li>Mỗi hình chữ nhật thuộc về <strong>chính xác</strong> một phần.</li>
</ul>

<p>Trả về <code>true</code> nếu có thể thực hiện các đường cắt như vậy; ngược lại, trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, rectangles = [[1,0,5,2],[0,2,2,4],[3,2,5,3],[0,4,4,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3394.Check%20if%20Grid%20can%20be%20Cut%20into%20Sections/images/tt1drawio.png" style="width: 285px; height: 280px;" /></p>

<p>Lưới được minh họa trong hình. Ta có thể tạo các đường cắt ngang tại <code>y = 2</code> và <code>y = 4</code>. Do đó, kết quả là true.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, rectangles = [[0,0,1,1],[2,0,3,4],[0,2,2,3],[3,0,4,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3394.Check%20if%20Grid%20can%20be%20Cut%20into%20Sections/images/tc2drawio.png" style="width: 240px; height: 240px;" /></p>

<p>Ta có thể tạo các đường cắt dọc tại <code>x = 2</code> và <code>x = 3</code>. Do đó, kết quả là true.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, rectangles = [[0,2,2,4],[1,0,3,2],[2,2,3,4],[3,0,4,2],[3,2,4,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta không thể tạo hai đường cắt ngang hoặc hai đường cắt dọc thỏa mãn các điều kiện. Do đó, kết quả là false.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>3 &lt;= rectangles.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= rectangles[i][0] &lt; rectangles[i][2] &lt;= n</code></li>
	<li><code>0 &lt;= rectangles[i][1] &lt; rectangles[i][3] &lt;= n</code></li>
	<li>Không có hai hình chữ nhật nào chồng lấn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các hình chữ nhật không chồng lấn; ta cần xác định liệu hai đường cắt song song với trục có thể chia chúng thành ba phần không rỗng hay không. Tọa độ có thể lên tới $10^9$, nên ta làm việc trên các phép chiếu.
>
> Mỗi đoạn đóng góp một điểm bắt đầu $+1$ và một điểm kết thúc $-1$. Với các tọa độ bằng nhau, xử lý điểm kết thúc trước để việc chạm nhau tạo thành một khoảng trống.
>
> Khi độ phủ trở về $0$, ta có một đường phân cách hoàn chỉnh. Nếu một trong hai hướng có ít nhất ba đường phân cách (tương ứng với hai đường cắt) thì hợp lệ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countLineIntersections(self, coordinates: List[tuple[int, int]]) -> bool:
        lines = 0
        overlap = 0
        for value, marker in coordinates:
            if marker == 0:
                overlap -= 1
            else:
                overlap += 1

            if overlap == 0:
                lines += 1

        return lines >= 3

    def checkValidCuts(self, n: int, rectangles: List[List[int]]) -> bool:
        y_coordinates = []
        x_coordinates = []

        for rect in rectangles:
            x1, y1, x2, y2 = rect
            y_coordinates.append((y1, 1))  # start
            y_coordinates.append((y2, 0))  # end

            x_coordinates.append((x1, 1))  # start
            x_coordinates.append((x2, 0))  # end

        # Sort by coordinate value, and for tie, put end (0) before start (1)
        y_coordinates.sort(key=lambda x: (x[0], x[1]))
        x_coordinates.sort(key=lambda x: (x[0], x[1]))

        return self.countLineIntersections(
            y_coordinates
        ) or self.countLineIntersections(x_coordinates)
```

#### Java

```java
class Solution {
    // Helper class to mimic C++ pair<int, int>
    static class Pair {
        int value;
        int type;

        Pair(int value, int type) {
            this.value = value;
            this.type = type;
        }
    }

    private boolean countLineIntersections(List<Pair> coordinates) {
        int lines = 0;
        int overlap = 0;

        for (Pair coord : coordinates) {
            if (coord.type == 0) {
                overlap--;
            } else {
                overlap++;
            }

            if (overlap == 0) {
                lines++;
            }
        }

        return lines >= 3;
    }

    public boolean checkValidCuts(int n, int[][] rectangles) {
        List<Pair> yCoordinates = new ArrayList<>();
        List<Pair> xCoordinates = new ArrayList<>();

        for (int[] rectangle : rectangles) {
            // rectangle = [x1, y1, x2, y2]
            yCoordinates.add(new Pair(rectangle[1], 1)); // y1, start
            yCoordinates.add(new Pair(rectangle[3], 0)); // y2, end

            xCoordinates.add(new Pair(rectangle[0], 1)); // x1, start
            xCoordinates.add(new Pair(rectangle[2], 0)); // x2, end
        }

        Comparator<Pair> comparator = (a, b) -> {
            if (a.value != b.value) return Integer.compare(a.value, b.value);
            return Integer.compare(a.type, b.type); // End (0) before Start (1)
        };

        Collections.sort(yCoordinates, comparator);
        Collections.sort(xCoordinates, comparator);

        return countLineIntersections(yCoordinates) || countLineIntersections(xCoordinates);
    }
}
```

#### C++

```cpp
class Solution {
#define pii pair<int, int>

    bool countLineIntersections(vector<pii>& coordinates) {
        int lines = 0;
        int overlap = 0;
        for (int i = 0; i < coordinates.size(); ++i) {
            if (coordinates[i].second == 0)
                overlap--;
            else
                overlap++;
            if (overlap == 0)
                lines++;
        }
        return lines >= 3;
    }

public:
    bool checkValidCuts(int n, vector<vector<int>>& rectangles) {
        vector<pii> y_cordinates, x_cordinates;
        for (auto& rectangle : rectangles) {
            y_cordinates.push_back(make_pair(rectangle[1], 1));
            y_cordinates.push_back(make_pair(rectangle[3], 0));
            x_cordinates.push_back(make_pair(rectangle[0], 1));
            x_cordinates.push_back(make_pair(rectangle[2], 0));
        }
        sort(y_cordinates.begin(), y_cordinates.end());
        sort(x_cordinates.begin(), x_cordinates.end());

        // Line-Sweep on x and y cordinates
        return (countLineIntersections(y_cordinates) or countLineIntersections(x_cordinates));
    }
};
```

#### Go

```go
type Pair struct {
	val int
	typ int // 1 = start, 0 = end
}

func countLineIntersections(coords []Pair) bool {
	lines := 0
	overlap := 0
	for _, p := range coords {
		if p.typ == 0 {
			overlap--
		} else {
			overlap++
		}
		if overlap == 0 {
			lines++
		}
	}
	return lines >= 3
}

func checkValidCuts(n int, rectangles [][]int) bool {
	var xCoords []Pair
	var yCoords []Pair

	for _, rect := range rectangles {
		x1, y1, x2, y2 := rect[0], rect[1], rect[2], rect[3]

		yCoords = append(yCoords, Pair{y1, 1}) // start
		yCoords = append(yCoords, Pair{y2, 0}) // end

		xCoords = append(xCoords, Pair{x1, 1})
		xCoords = append(xCoords, Pair{x2, 0})
	}

	sort.Slice(yCoords, func(i, j int) bool {
		if yCoords[i].val == yCoords[j].val {
			return yCoords[i].typ < yCoords[j].typ // end before start
		}
		return yCoords[i].val < yCoords[j].val
	})

	sort.Slice(xCoords, func(i, j int) bool {
		if xCoords[i].val == xCoords[j].val {
			return xCoords[i].typ < xCoords[j].typ
		}
		return xCoords[i].val < xCoords[j].val
	})

	return countLineIntersections(yCoords) || countLineIntersections(xCoords)
}
```

#### TypeScript

```ts
function checkValidCuts(n: number, rectangles: number[][]): boolean {
    const check = (arr: number[][], getVals: (x: number[]) => number[]) => {
        let [c, longest] = [3, 0];

        for (const x of arr) {
            const [start, end] = getVals(x);

            if (start < longest) {
                longest = Math.max(longest, end);
            } else {
                longest = end;
                if (--c === 0) return true;
            }
        }

        return false;
    };

    const sortByX = ([a]: number[], [b]: number[]) => a - b;
    const sortByY = ([, a]: number[], [, b]: number[]) => a - b;
    const getX = ([x1, , x2]: number[]) => [x1, x2];
    const getY = ([, y1, , y2]: number[]) => [y1, y2];

    return check(rectangles.toSorted(sortByX), getX) || check(rectangles.toSorted(sortByY), getY);
}
```

#### JavaScript

```js
function checkValidCuts(n, rectangles) {
    const check = (arr, getVals) => {
        let [c, longest] = [3, 0];

        for (const x of arr) {
            const [start, end] = getVals(x);

            if (start < longest) {
                longest = Math.max(longest, end);
            } else {
                longest = end;
                if (--c === 0) return true;
            }
        }

        return false;
    };

    const sortByX = ([a], [b]) => a - b;
    const sortByY = ([, a], [, b]) => a - b;
    const getX = ([x1, , x2]) => [x1, x2];
    const getY = ([, y1, , y2]) => [y1, y2];

    return check(rectangles.toSorted(sortByX), getX) || check(rectangles.toSorted(sortByY), getY);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
