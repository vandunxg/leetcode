---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.14. Best Line](https://leetcode.cn/problems/best-line-lcci)

[中文文档](/lcci/16.14.Best%20Line/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị hai chiều có các điểm trên đó, hãy tìm một đường thẳng đi qua nhiều điểm nhất.</p>
<p>Giả sử tất cả các điểm mà đường thẳng đi qua được lưu trong danh sách <code>S</code> và được sắp xếp theo số hiệu. Bạn cần trả về <code>[S[0], S[1]]</code>, tức là hai điểm có số hiệu nhỏ nhất. Nếu có nhiều hơn một đường thẳng đi qua số điểm nhiều nhất, hãy chọn đường có <code>S[0].</code> nhỏ nhất. Nếu có nhiều hơn một đường có cùng <code>S[0]</code>, hãy chọn đường có <code>S[1]</code> nhỏ nhất.</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong> [[0,0],[1,1],[1,0],[2,0]]

<strong>Đầu ra: </strong> [0,2]

<strong>Giải thích: </strong> Số hiệu các điểm mà đường thẳng đi qua là [0,2,3].

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>2 &lt;= len(Points) &lt;= 300</code></li>
	<li><code>len(Points[i]) = 2</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Trả về hai chỉ số nhỏ nhất trên một đường thẳng đi qua nhiều điểm nhất. $n$ đủ nhỏ để có thể dùng $O(n^3)$.
>
> Hai điểm xác định một đường thẳng; điểm thứ ba thẳng hàng khi $(y_2-y_1)(x_3-x_1)=(y_3-y_1)(x_2-x_1)$, qua đó tránh phép chia.
>
> Liệt kê $i<j<k$, cập nhật số điểm lớn nhất và cặp $(i,j)$. Việc liệt kê các chỉ số nhỏ hơn trước đã tự nhiên đáp ứng điều kiện phá hòa.

<!-- thinking:end -->

Chúng ta có thể liệt kê bất kỳ hai điểm $(x_1, y_1), (x_2, y_2)$ nào, nối hai điểm này thành một đường thẳng, khi đó số điểm trên đường thẳng là 2. Sau đó, chúng ta liệt kê các điểm khác $(x_3, y_3)$ và xác định xem chúng có nằm trên cùng đường thẳng hay không. Nếu có, số điểm trên đường thẳng tăng thêm 1; nếu không, số điểm trên đường thẳng không đổi. Tìm số điểm lớn nhất trên một đường thẳng, hai chỉ số điểm nhỏ nhất tương ứng là đáp án.

Độ phức tạp thời gian là $O(n^3)$, độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài của mảng `points`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestLine(self, points: List[List[int]]) -> List[int]:
        n = len(points)
        mx = 0
        for i in range(n):
            x1, y1 = points[i]
            for j in range(i + 1, n):
                x2, y2 = points[j]
                cnt = 2
                for k in range(j + 1, n):
                    x3, y3 = points[k]
                    a = (y2 - y1) * (x3 - x1)
                    b = (y3 - y1) * (x2 - x1)
                    cnt += a == b
                if mx < cnt:
                    mx = cnt
                    x, y = i, j
        return [x, y]
```

#### Java

```java
class Solution {
    public int[] bestLine(int[][] points) {
        int n = points.length;
        int mx = 0;
        int[] ans = new int[2];
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int cnt = 2;
                for (int k = j + 1; k < n; ++k) {
                    int x3 = points[k][0], y3 = points[k][1];
                    int a = (y2 - y1) * (x3 - x1);
                    int b = (y3 - y1) * (x2 - x1);
                    if (a == b) {
                        ++cnt;
                    }
                }
                if (mx < cnt) {
                    mx = cnt;
                    ans[0] = i;
                    ans[1] = j;
                }
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
    vector<int> bestLine(vector<vector<int>>& points) {
        int n = points.size();
        int mx = 0;
        vector<int> ans(2);
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int cnt = 2;
                for (int k = j + 1; k < n; ++k) {
                    int x3 = points[k][0], y3 = points[k][1];
                    long a = (long) (y2 - y1) * (x3 - x1);
                    long b = (long) (y3 - y1) * (x2 - x1);
                    cnt += a == b;
                }
                if (mx < cnt) {
                    mx = cnt;
                    ans[0] = i;
                    ans[1] = j;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func bestLine(points [][]int) []int {
	n := len(points)
	ans := make([]int, 2)
	mx := 0
	for i := 0; i < n; i++ {
		x1, y1 := points[i][0], points[i][1]
		for j := i + 1; j < n; j++ {
			x2, y2 := points[j][0], points[j][1]
			cnt := 2
			for k := j + 1; k < n; k++ {
				x3, y3 := points[k][0], points[k][1]
				a := (y2 - y1) * (x3 - x1)
				b := (y3 - y1) * (x2 - x1)
				if a == b {
					cnt++
				}
			}
			if mx < cnt {
				mx = cnt
				ans[0], ans[1] = i, j
			}
		}
	}
	return ans
}
```

#### Swift

```swift
class Solution {
    func bestLine(_ points: [[Int]]) -> [Int] {
        let n = points.count
        var maxCount = 0
        var answer = [Int](repeating: 0, count: 2)

        for i in 0..<n {
            let x1 = points[i][0], y1 = points[i][1]
            for j in i + 1..<n {
                let x2 = points[j][0], y2 = points[j][1]
                var count = 2

                for k in j + 1..<n {
                    let x3 = points[k][0], y3 = points[k][1]
                    let a = (y2 - y1) * (x3 - x1)
                    let b = (y3 - y1) * (x2 - x1)
                    if a == b {
                        count += 1
                    }
                }

                if maxCount < count {
                    maxCount = count
                    answer = [i, j]
                }
            }
        }
        return answer
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê + Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Vòng lặp bậc ba sẽ đếm lặp lại cùng một đường thẳng nhiều lần khi $n$ lớn.
>
> Cố định một điểm và băm các hệ số góc đã rút gọn của các điểm còn lại; các key giống nhau tương ứng với các điểm thẳng hàng, với độ phức tạp $O(n^2\log m)$.

<!-- thinking:end -->

Chúng ta có thể liệt kê một điểm $(x_1, y_1)$, lưu hệ số góc của đường thẳng nối $(x_1, y_1)$ với tất cả các điểm khác $(x_2, y_2)$ vào một bảng băm. Các điểm có cùng hệ số góc nằm trên cùng một đường thẳng, key của bảng băm là hệ số góc và value là số điểm trên đường thẳng. Tìm giá trị lớn nhất trong bảng băm chính là đáp án. Để tránh vấn đề về độ chính xác, chúng ta có thể rút gọn hệ số góc $\frac{y_2 - y_1}{x_2 - x_1}$ bằng cách tìm ước chung lớn nhất, sau đó chia cả tử số và mẫu số cho ước chung lớn nhất. Tử số và mẫu số sau khi rút gọn được dùng làm key của bảng băm.

Độ phức tạp thời gian là $O(n^2 \times \log m)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng `points` và hiệu lớn nhất giữa tất cả các tọa độ ngang và dọc trong mảng `points`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def bestLine(self, points: List[List[int]]) -> List[int]:
        def gcd(a, b):
            return a if b == 0 else gcd(b, a % b)

        n = len(points)
        mx = 0
        for i in range(n):
            x1, y1 = points[i]
            cnt = defaultdict(list)
            for j in range(i + 1, n):
                x2, y2 = points[j]
                dx, dy = x2 - x1, y2 - y1
                g = gcd(dx, dy)
                k = (dx // g, dy // g)
                cnt[k].append((i, j))
                if mx < len(cnt[k]) or (mx == len(cnt[k]) and (x, y) > cnt[k][0]):
                    mx = len(cnt[k])
                    x, y = cnt[k][0]
        return [x, y]
```

#### Java

```java
class Solution {
    public int[] bestLine(int[][] points) {
        int n = points.length;
        int mx = 0;
        int[] ans = new int[2];
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            Map<String, List<int[]>> cnt = new HashMap<>();
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int dx = x2 - x1, dy = y2 - y1;
                int g = gcd(dx, dy);
                String key = (dx / g) + "." + (dy / g);
                cnt.computeIfAbsent(key, k -> new ArrayList<>()).add(new int[] {i, j});
                if (mx < cnt.get(key).size()
                    || (mx == cnt.get(key).size()
                        && (ans[0] > cnt.get(key).get(0)[0]
                            || (ans[0] == cnt.get(key).get(0)[0]
                                && ans[1] > cnt.get(key).get(0)[1])))) {
                    mx = cnt.get(key).size();
                    ans = cnt.get(key).get(0);
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> bestLine(vector<vector<int>>& points) {
        int n = points.size();
        int mx = 0;
        pair<int, int> ans = {0, 0};
        for (int i = 0; i < n; ++i) {
            int x1 = points[i][0], y1 = points[i][1];
            unordered_map<string, vector<pair<int, int>>> cnt;
            for (int j = i + 1; j < n; ++j) {
                int x2 = points[j][0], y2 = points[j][1];
                int dx = x2 - x1, dy = y2 - y1;
                int g = gcd(dx, dy);
                string k = to_string(dx / g) + "." + to_string(dy / g);
                cnt[k].push_back({i, j});
                if (mx < cnt[k].size() || (mx == cnt[k].size() && ans > cnt[k][0])) {
                    mx = cnt[k].size();
                    ans = cnt[k][0];
                }
            }
        }
        return vector<int>{ans.first, ans.second};
    }

    int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
};
```

#### Go

```go
func bestLine(points [][]int) []int {
	n := len(points)
	ans := make([]int, 2)
	type pair struct{ i, j int }
	mx := 0
	for i := 0; i < n; i++ {
		x1, y1 := points[i][0], points[i][1]
		cnt := map[pair][]pair{}
		for j := i + 1; j < n; j++ {
			x2, y2 := points[j][0], points[j][1]
			dx, dy := x2-x1, y2-y1
			g := gcd(dx, dy)
			k := pair{dx / g, dy / g}
			cnt[k] = append(cnt[k], pair{i, j})
			if mx < len(cnt[k]) || (mx == len(cnt[k]) && (ans[0] > cnt[k][0].i || (ans[0] == cnt[k][0].i && ans[1] > cnt[k][0].j))) {
				mx = len(cnt[k])
				ans[0], ans[1] = cnt[k][0].i, cnt[k][0].j
			}
		}
	}
	return ans
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
