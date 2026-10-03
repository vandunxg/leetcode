---
comments: true
difficulty: Medium
rating: 1969
source: Biweekly Contest 57 Q3
tags:
    - Array
    - Hash Table
    - Prefix Sum
    - Sorting
---

<!-- problem:start -->

# [1943. Describe the Painting](https://leetcode.com/problems/describe-the-painting)

[中文文档](/solution/1900-1999/1943.Describe%20the%20Painting/README.md)

## Mô tả

<!-- description:start -->

<p>Có một bức tranh dài và mảnh có thể được biểu diễn bằng một trục số. Bức tranh được vẽ bằng nhiều đoạn chồng lấn, trong đó mỗi đoạn được tô bằng một màu <strong>duy nhất</strong>. Bạn được cung cấp một mảng số nguyên 2D <code>segments</code>, trong đó <code>segments[i] = [start<sub>i</sub>, end<sub>i</sub>, color<sub>i</sub>]</code> biểu diễn <strong>đoạn nửa kín</strong> <code>[start<sub>i</sub>, end<sub>i</sub>)</code> có màu <code>color<sub>i</sub></code>.</p>

<p>Các màu trong những đoạn chồng lấn của bức tranh được <strong>trộn</strong> khi vẽ. Khi trộn hai hoặc nhiều màu, chúng tạo thành một màu mới có thể được biểu diễn bằng một <strong>tập hợp</strong> các màu đã trộn.</p>

<ul>
	<li>Ví dụ, nếu trộn các màu <code>2</code>, <code>4</code> và <code>6</code>, màu trộn thu được là <code>{2,4,6}</code>.</li>
</ul>

<p>Để đơn giản, bạn chỉ cần xuất ra <strong>tổng</strong> các phần tử trong tập hợp thay vì toàn bộ tập hợp.</p>

<p>Bạn muốn <strong>mô tả</strong> bức tranh bằng số lượng <strong>tối thiểu</strong> các <strong>đoạn nửa kín</strong> không chồng lấn, với các màu đã trộn. Các đoạn này có thể được biểu diễn bằng mảng 2D <code>painting</code>, trong đó <code>painting[j] = [left<sub>j</sub>, right<sub>j</sub>, mix<sub>j</sub>]</code> mô tả <strong>đoạn nửa kín</strong> <code>[left<sub>j</sub>, right<sub>j</sub>)</code> có màu trộn bằng <strong>tổng</strong> của <code>mix<sub>j</sub></code>.</p>

<ul>
	<li>Ví dụ, bức tranh được tạo bởi <code>segments = [[1,4,5],[1,7,7]]</code> có thể được mô tả bằng <code>painting = [[1,4,12],[4,7,7]]</code> vì:

    <ul>
    <li><code>[1,4)</code> được tô bằng <code>{5,7}</code> (có tổng là <code>12</code>) từ cả đoạn thứ nhất và đoạn thứ hai.</li>
    <li><code>[4,7)</code> chỉ được tô bằng <code>{7}</code> từ đoạn thứ hai.</li>
        </ul>
        </li>

</ul>

<p>Hãy trả về <em>mảng 2D </em><code>painting</code><em> mô tả bức tranh đã hoàn thiện (loại bỏ mọi phần <strong>không </strong>được tô). Bạn có thể trả về các đoạn theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p><strong>Đoạn nửa kín</strong> <code>[a, b)</code> là phần của trục số nằm giữa các điểm <code>a</code> và <code>b</code>, <strong>bao gồm</strong> điểm <code>a</code> và <strong>không bao gồm</strong> điểm <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1943.Describe%20the%20Painting/images/1.png" style="width: 529px; height: 241px;" />
<pre>
<strong>Đầu vào:</strong> segments = [[1,4,5],[4,7,7],[1,7,9]]
<strong>Đầu ra:</strong> [[1,4,14],[4,7,16]]
<strong>Giải thích: </strong>Bức tranh có thể được mô tả như sau:
- [1,4) được tô bằng {5,9} (có tổng là 14) từ đoạn thứ nhất và đoạn thứ ba.
- [4,7) được tô bằng {7,9} (có tổng là 16) từ đoạn thứ hai và đoạn thứ ba.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1943.Describe%20the%20Painting/images/2.png" style="width: 532px; height: 219px;" />
<pre>
<strong>Đầu vào:</strong> segments = [[1,7,9],[6,8,15],[8,10,7]]
<strong>Đầu ra:</strong> [[1,6,9],[6,7,24],[7,8,15],[8,10,7]]
<strong>Giải thích: </strong>Bức tranh có thể được mô tả như sau:
- [1,6) được tô bằng 9 từ đoạn thứ nhất.
- [6,7) được tô bằng {9,15} (có tổng là 24) từ đoạn thứ nhất và đoạn thứ hai.
- [7,8) được tô bằng 15 từ đoạn thứ hai.
- [8,10) được tô bằng 7 từ đoạn thứ ba.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1943.Describe%20the%20Painting/images/c1.png" style="width: 529px; height: 289px;" />
<pre>
<strong>Đầu vào:</strong> segments = [[1,4,5],[1,4,7],[4,7,1],[4,7,11]]
<strong>Đầu ra:</strong> [[1,4,12],[4,7,12]]
<strong>Giải thích: </strong>Bức tranh có thể được mô tả như sau:
- [1,4) được tô bằng {5,7} (có tổng là 12) từ đoạn thứ nhất và đoạn thứ hai.
- [4,7) được tô bằng {1,11} (có tổng là 12) từ đoạn thứ ba và đoạn thứ tư.
Lưu ý rằng trả về một đoạn duy nhất [1,7) là không đúng vì các tập hợp màu trộn khác nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= segments.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>segments[i].length == 3</code></li>
	<li><code>1 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= color<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li>Mỗi <code>color<sub>i</sub></code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các đoạn chồng lấn sẽ trộn màu, và ta phải xuất ra các đoạn cực đại có màu trộn không đổi. Nếu cắt theo mọi cặp đầu mút thì độ phức tạp là $O(n^2)$.
>
> Màu chỉ thay đổi tại các đầu mút: cộng tại đầu trái, trừ tại đầu phải. Sau khi sắp xếp các đầu mút này, tổng tiền tố khác 0 giữa hai đầu mút liên tiếp chính là một đoạn màu trộn.
>
> Một hash map lưu mảng hiệu; sau đó duyệt một lượt theo thứ tự đã sắp xếp để tạo đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def splitPainting(self, segments: List[List[int]]) -> List[List[int]]:
        d = defaultdict(int)
        for l, r, c in segments:
            d[l] += c
            d[r] -= c
        s = sorted([[k, v] for k, v in d.items()])
        n = len(s)
        for i in range(1, n):
            s[i][1] += s[i - 1][1]
        return [[s[i][0], s[i + 1][0], s[i][1]] for i in range(n - 1) if s[i][1]]
```

#### Java

```java
class Solution {
    public List<List<Long>> splitPainting(int[][] segments) {
        TreeMap<Integer, Long> d = new TreeMap<>();
        for (int[] e : segments) {
            int l = e[0], r = e[1], c = e[2];
            d.put(l, d.getOrDefault(l, 0L) + c);
            d.put(r, d.getOrDefault(r, 0L) - c);
        }
        List<List<Long>> ans = new ArrayList<>();
        long i = 0, j = 0;
        long cur = 0;
        for (Map.Entry<Integer, Long> e : d.entrySet()) {
            if (Objects.equals(e.getKey(), d.firstKey())) {
                i = e.getKey();
            } else {
                j = e.getKey();
                if (cur > 0) {
                    ans.add(Arrays.asList(i, j, cur));
                }
                i = j;
            }
            cur += e.getValue();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<long long>> splitPainting(vector<vector<int>>& segments) {
        map<int, long long> d;
        for (auto& e : segments) {
            int l = e[0], r = e[1], c = e[2];
            d[l] += c;
            d[r] -= c;
        }
        vector<vector<long long>> ans;
        long long i, j, cur = 0;
        for (auto& it : d) {
            if (it == *d.begin())
                i = it.first;
            else {
                j = it.first;
                if (cur > 0) ans.push_back({i, j, cur});
                i = j;
            }
            cur += it.second;
        }
        return ans;
    }
};
```

#### Go

```go
func splitPainting(segments [][]int) [][]int64 {
	d := make(map[int]int64)
	for _, seg := range segments {
		d[seg[0]] += int64(seg[2])
		d[seg[1]] -= int64(seg[2])
	}
	dList := make([]int, 0, len(d))
	for k := range d {
		dList = append(dList, k)
	}
	sort.Ints(dList)

	var ans [][]int64

	i := dList[0]
	cur := d[i]
	for j := 1; j < len(dList); j++ {
		it := d[dList[j]]
		if cur > 0 {
			ans = append(ans, []int64{int64(i), int64(dList[j]), cur})
		}
		cur += it
		i = dList[j]
	}

	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
