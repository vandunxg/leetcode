---
comments: true
difficulty: Easy
rating: 1229
source: Weekly Contest 362 Q1
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2848. Points That Intersect With Cars](https://leetcode.com/problems/points-that-intersect-with-cars)

[中文文档](/solution/2800-2899/2848.Points%20That%20Intersect%20With%20Cars/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên 2D <code>nums</code> được đánh chỉ số từ <strong>0</strong>, biểu diễn tọa độ của những chiếc xe đỗ trên một trục số. Với mỗi chỉ số <code>i</code>, <code>nums[i] = [start<sub>i</sub>, end<sub>i</sub>]</code>, trong đó <code>start<sub>i</sub></code> là điểm bắt đầu của chiếc xe thứ <code>i<sup>th</sup></code> và <code>end<sub>i</sub></code> là điểm kết thúc của chiếc xe thứ <code>i<sup>th</sup></code>.</p>

<p>Hãy trả về <em>số điểm nguyên trên trục số được phủ bởi <strong>ít nhất một phần</strong> của một chiếc xe.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[3,6],[1,5],[4,7]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Tất cả các điểm từ 1 đến 7 đều giao với ít nhất một chiếc xe, nên đáp án là 7.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [[1,3],[5,8]]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Các điểm giao với ít nhất một chiếc xe là 1, 2, 3, 5, 6, 7, 8. Có tổng cộng 7 điểm, nên đáp án là 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>nums[i].length == 2</code></li>
	<li><code><font face="monospace">1 &lt;= start<sub>i</sub>&nbsp;&lt;= end<sub>i</sub>&nbsp;&lt;= 100</font></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Tọa độ không vượt quá $100$, nên ta có thể dùng mảng hiệu: tăng tại điểm bắt đầu, giảm tại vị trí ngay sau điểm kết thúc, rồi đếm những vị trí có tổng tiền tố lớn hơn 0.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần cộng thêm một xe vào mỗi khoảng $[\textit{start}_i, \textit{end}_i]$. Ta có thể dùng mảng hiệu để thực hiện việc này.

Ta định nghĩa một mảng $d$ có độ dài 102. Với mỗi khoảng $[\textit{start}_i, \textit{end}_i]$, ta tăng $d[\textit{start}_i]$ lên 1 và giảm $d[\textit{end}_i + 1]$ đi 1.

Cuối cùng, ta tính tổng tiền tố trên $d$ và đếm số phần tử trong tổng tiền tố lớn hơn 0.

Độ phức tạp thời gian là $O(n + m)$, còn độ phức tạp không gian là $O(m)$, trong đó $n$ là độ dài của mảng đã cho và $m$ là giá trị lớn nhất trong mảng. Trong bài toán này, $m \leq 102$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPoints(self, nums: List[List[int]]) -> int:
        m = 102
        d = [0] * m
        for start, end in nums:
            d[start] += 1
            d[end + 1] -= 1
        return sum(s > 0 for s in accumulate(d))
```

#### Java

```java
class Solution {
    public int numberOfPoints(List<List<Integer>> nums) {
        int[] d = new int[102];
        for (var e : nums) {
            int start = e.get(0), end = e.get(1);
            ++d[start];
            --d[end + 1];
        }
        int ans = 0, s = 0;
        for (int x : d) {
            s += x;
            if (s > 0) {
                ++ans;
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
    int numberOfPoints(vector<vector<int>>& nums) {
        int d[102]{};
        for (const auto& e : nums) {
            int start = e[0], end = e[1];
            ++d[start];
            --d[end + 1];
        }
        int ans = 0, s = 0;
        for (int x : d) {
            s += x;
            ans += s > 0;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfPoints(nums [][]int) (ans int) {
	d := [102]int{}
	for _, e := range nums {
		start, end := e[0], e[1]
		d[start]++
		d[end+1]--
	}
	s := 0
	for _, x := range d {
		s += x
		if s > 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfPoints(nums: number[][]): number {
    const d: number[] = Array(102).fill(0);
    for (const [start, end] of nums) {
        ++d[start];
        --d[end + 1];
    }
    let ans = 0;
    let s = 0;
    for (const x of d) {
        s += x;
        ans += s > 0 ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hash Table + Mảng hiệu + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 cần biết trước cận trên của tọa độ. Lưu các điểm có hiệu khác 0 trong một hash map và duyệt chúng theo thứ tự giúp dùng không gian tuyến tính theo số khoảng.

<!-- thinking:end -->

Nếu miền của các khoảng trong bài toán lớn, ta có thể dùng một hash table để lưu các điểm bắt đầu và kết thúc của các khoảng. Sau đó, ta sắp xếp các khóa của hash table và tính tổng tiền tố.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPoints(self, nums: List[List[int]]) -> int:
        d = defaultdict(int)
        for start, end in nums:
            d[start] += 1
            d[end + 1] -= 1
        ans = s = last = 0
        for cur, v in sorted(d.items()):
            if s > 0:
                ans += cur - last
            s += v
            last = cur
        return ans
```

#### Java

```java
class Solution {
    public int numberOfPoints(List<List<Integer>> nums) {
        TreeMap<Integer, Integer> d = new TreeMap<>();
        for (var e : nums) {
            int start = e.get(0), end = e.get(1);
            d.merge(start, 1, Integer::sum);
            d.merge(end + 1, -1, Integer::sum);
        }
        int ans = 0, s = 0, last = 0;
        for (var e : d.entrySet()) {
            int cur = e.getKey(), v = e.getValue();
            if (s > 0) {
                ans += cur - last;
            }
            s += v;
            last = cur;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfPoints(vector<vector<int>>& nums) {
        map<int, int> d;
        for (const auto& e : nums) {
            int start = e[0], end = e[1];
            ++d[start];
            --d[end + 1];
        }
        int ans = 0, s = 0, last = 0;
        for (const auto& [cur, v] : d) {
            if (s > 0) {
                ans += cur - last;
            }
            s += v;
            last = cur;
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfPoints(nums [][]int) (ans int) {
	d := map[int]int{}
	for _, e := range nums {
		start, end := e[0], e[1]
		d[start]++
		d[end+1]--
	}
	keys := []int{}
	for k := range d {
		keys = append(keys, k)
	}
	s, last := 0, 0
	sort.Ints(keys)
	for _, cur := range keys {
		if s > 0 {
			ans += cur - last
		}
		s += d[cur]
		last = cur
	}
	return
}
```

#### TypeScript

```ts
function numberOfPoints(nums: number[][]): number {
    const d = new Map<number, number>();
    for (const [start, end] of nums) {
        d.set(start, (d.get(start) || 0) + 1);
        d.set(end + 1, (d.get(end + 1) || 0) - 1);
    }
    const keys = [...d.keys()].sort((a, b) => a - b);
    let [ans, s, last] = [0, 0, 0];
    for (const cur of keys) {
        if (s > 0) {
            ans += cur - last;
        }
        s += d.get(cur)!;
        last = cur;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
