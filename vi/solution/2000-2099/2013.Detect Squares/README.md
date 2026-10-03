---
comments: true
difficulty: Medium
rating: 1841
source: Weekly Contest 259 Q3
tags:
    - Design
    - Array
    - Hash Table
    - Counting
    - Data Stream
---

<!-- problem:start -->

# [2013. Detect Squares](https://leetcode.com/problems/detect-squares)

[中文文档](/solution/2000-2099/2013.Detect%20Squares/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một stream các điểm trên mặt phẳng X-Y. Hãy thiết kế một thuật toán có thể:</p>

<ul>
	<li><strong>Thêm</strong> các điểm mới từ stream vào một data structure. Các điểm <strong>trùng nhau</strong> được phép và phải được xem là những điểm khác nhau.</li>
	<li>Với một điểm truy vấn, <strong>đếm</strong> số cách chọn ba điểm từ data structure sao cho ba điểm đó và điểm truy vấn tạo thành một <strong>hình vuông song song với các trục tọa độ</strong>, có <strong>diện tích dương</strong>.</li>
</ul>

<p><strong>Hình vuông song song với các trục tọa độ</strong> là hình vuông có các cạnh đều song song hoặc vuông góc với trục x và trục y.</p>

<p>Hãy cài đặt class <code>DetectSquares</code>:</p>

<ul>
	<li><code>DetectSquares()</code> Khởi tạo object với một data structure rỗng.</li>
	<li><code>void add(int[] point)</code> Thêm một điểm mới <code>point = [x, y]</code> vào data structure.</li>
	<li><code>int count(int[] point)</code> Đếm số cách tạo thành <strong>hình vuông song song với các trục tọa độ</strong> với điểm <code>point = [x, y]</code> như mô tả ở trên.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2013.Detect%20Squares/images/image.png" style="width: 869px; height: 504px;" />
<pre>
<strong>Input</strong>
[&quot;DetectSquares&quot;, &quot;add&quot;, &quot;add&quot;, &quot;add&quot;, &quot;count&quot;, &quot;count&quot;, &quot;add&quot;, &quot;count&quot;]
[[], [[3, 10]], [[11, 2]], [[3, 2]], [[11, 10]], [[14, 8]], [[11, 2]], [[11, 10]]]
<strong>Output</strong>
[null, null, null, null, 1, 0, null, 2]

<strong>Giải thích</strong>
DetectSquares detectSquares = new DetectSquares();
detectSquares.add([3, 10]);
detectSquares.add([11, 2]);
detectSquares.add([3, 2]);
detectSquares.count([11, 10]); // return 1. You can choose:
// - The first, second, and third points
detectSquares.count([14, 8]); // return 0. The query point cannot form a square with any points in the data structure.
detectSquares.add([11, 2]); // Adding duplicate points is allowed.
detectSquares.count([11, 10]); // return 2. You can choose:
// - The first, second, and third points
// - The first, third, and fourth points
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>point.length == 2</code></li>
	<li><code>0 &lt;= x, y &lt;= 1000</code></li>
	<li>Có nhiều nhất <code>3000</code> lần gọi <strong>tổng cộng</strong> đến <code>add</code> và <code>count</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $5000$ lần gọi và các tọa độ nằm trong $[0,1000]$. Việc liệt kê mọi bộ ba điểm cho mỗi lần `count` khá vụng về. Một hình vuông song song với các trục tọa độ được xác định bởi một cạnh nằm ngang.
>
> Lưu tần suất vào $cnt[x][y]$. Với điểm truy vấn $(x_1,y_1)$, liệt kê các $x_2 \ne x_1$ đã tồn tại với cạnh $d=x_2-x_1$; hai đỉnh còn lại của các hình vuông phía trên/phía dưới được suy ra từ đó.
>
> Tích của ba tần suất là số cách. `add` có độ phức tạp $O(1)$; `count` có độ phức tạp tuyến tính theo số $x$ phân biệt.

<!-- thinking:end -->

Ta có thể sử dụng một hash table $cnt$ để lưu toàn bộ thông tin của các điểm, trong đó $cnt[x][y]$ biểu thị số lần xuất hiện của điểm $(x, y)$.

Khi gọi phương thức $add(x, y)$, ta tăng giá trị của $cnt[x][y]$ lên $1$.

Khi gọi phương thức $count(x_1, y_1)$, ta cần tìm ba điểm khác để tạo thành một hình vuông song song với các trục tọa độ. Ta có thể liệt kê điểm $(x_2, y_1)$ song song với trục $x$ và cách $(x_1, y_1)$ một khoảng $d$. Nếu điểm đó tồn tại, dựa trên hai điểm này, ta có thể xác định hai điểm còn lại là $(x_1, y_1 + d)$ và $(x_2, y_1 + d)$, hoặc $(x_1, y_1 - d)$ và $(x_2, y_1 - d)$. Ta cộng số cách của hai trường hợp này.

Về độ phức tạp thời gian, độ phức tạp thời gian khi gọi phương thức $add(x, y)$ là $O(1)$, còn khi gọi phương thức $count(x_1, y_1)$ là $O(n)$; độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số điểm trong data stream.

<!-- tabs:start -->

#### Python3

```python
class DetectSquares:
    def __init__(self):
        self.cnt = defaultdict(Counter)

    def add(self, point: List[int]) -> None:
        x, y = point
        self.cnt[x][y] += 1

    def count(self, point: List[int]) -> int:
        x1, y1 = point
        if x1 not in self.cnt:
            return 0
        ans = 0
        for x2 in self.cnt.keys():
            if x2 != x1:
                d = x2 - x1
                ans += self.cnt[x2][y1] * self.cnt[x1][y1 + d] * self.cnt[x2][y1 + d]
                ans += self.cnt[x2][y1] * self.cnt[x1][y1 - d] * self.cnt[x2][y1 - d]
        return ans


# Your DetectSquares object will be instantiated and called as such:
# obj = DetectSquares()
# obj.add(point)
# param_2 = obj.count(point)
```

#### Java

```java
class DetectSquares {
    private Map<Integer, Map<Integer, Integer>> cnt = new HashMap<>();

    public DetectSquares() {
    }

    public void add(int[] point) {
        int x = point[0], y = point[1];
        cnt.computeIfAbsent(x, k -> new HashMap<>()).merge(y, 1, Integer::sum);
    }

    public int count(int[] point) {
        int x1 = point[0], y1 = point[1];
        if (!cnt.containsKey(x1)) {
            return 0;
        }
        int ans = 0;
        for (var e : cnt.entrySet()) {
            int x2 = e.getKey();
            if (x2 != x1) {
                int d = x2 - x1;
                var cnt1 = cnt.get(x1);
                var cnt2 = e.getValue();
                ans += cnt2.getOrDefault(y1, 0) * cnt1.getOrDefault(y1 + d, 0)
                    * cnt2.getOrDefault(y1 + d, 0);
                ans += cnt2.getOrDefault(y1, 0) * cnt1.getOrDefault(y1 - d, 0)
                    * cnt2.getOrDefault(y1 - d, 0);
            }
        }
        return ans;
    }
}

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares obj = new DetectSquares();
 * obj.add(point);
 * int param_2 = obj.count(point);
 */
```

#### C++

```cpp
class DetectSquares {
public:
    DetectSquares() {
    }

    void add(vector<int> point) {
        int x = point[0], y = point[1];
        ++cnt[x][y];
    }

    int count(vector<int> point) {
        int x1 = point[0], y1 = point[1];
        if (!cnt.count(x1)) {
            return 0;
        }
        int ans = 0;
        for (auto& [x2, cnt2] : cnt) {
            if (x2 != x1) {
                int d = x2 - x1;
                auto& cnt1 = cnt[x1];
                ans += cnt2[y1] * cnt1[y1 + d] * cnt2[y1 + d];
                ans += cnt2[y1] * cnt1[y1 - d] * cnt2[y1 - d];
            }
        }
        return ans;
    }

private:
    unordered_map<int, unordered_map<int, int>> cnt;
};

/**
 * Your DetectSquares object will be instantiated and called as such:
 * DetectSquares* obj = new DetectSquares();
 * obj->add(point);
 * int param_2 = obj->count(point);
 */
```

#### Go

```go
type DetectSquares struct {
	cnt map[int]map[int]int
}

func Constructor() DetectSquares {
	return DetectSquares{map[int]map[int]int{}}
}

func (this *DetectSquares) Add(point []int) {
	x, y := point[0], point[1]
	if _, ok := this.cnt[x]; !ok {
		this.cnt[x] = map[int]int{}
	}
	this.cnt[x][y]++
}

func (this *DetectSquares) Count(point []int) (ans int) {
	x1, y1 := point[0], point[1]
	if cnt1, ok := this.cnt[x1]; ok {
		for x2, cnt2 := range this.cnt {
			if x2 != x1 {
				d := x2 - x1
				ans += cnt2[y1] * cnt1[y1+d] * cnt2[y1+d]
				ans += cnt2[y1] * cnt1[y1-d] * cnt2[y1-d]
			}
		}
	}
	return
}

/**
 * Your DetectSquares object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Add(point);
 * param_2 = obj.Count(point);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
