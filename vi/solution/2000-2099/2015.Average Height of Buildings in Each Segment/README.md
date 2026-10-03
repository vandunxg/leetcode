---
comments: true
difficulty: Medium
tags:
    - Array
    - Prefix Sum
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2015. Average Height of Buildings in Each Segment 🔒](https://leetcode.com/problems/average-height-of-buildings-in-each-segment)

[中文文档](/solution/2000-2099/2015.Average%20Height%20of%20Buildings%20in%20Each%20Segment/README.md)

## Mô tả

<!-- description:start -->

<p>Một con đường thẳng hoàn toàn được biểu diễn bằng một trục số. Trên đường có các tòa nhà, được biểu diễn bằng mảng số nguyên 2 chiều <code>buildings</code>, trong đó <code>buildings[i] = [start<sub>i</sub>, end<sub>i</sub>, height<sub>i</sub>]</code>. Điều này có nghĩa là có một tòa nhà cao <code>height<sub>i</sub></code> trên <strong>đoạn nửa kín</strong> <code>[start<sub>i</sub>, end<sub>i</sub>)</code>.</p>

<p>Bạn muốn <strong>mô tả</strong> chiều cao của các tòa nhà trên đường bằng số lượng <strong>ít nhất</strong> các <strong>đoạn</strong> không chồng lấn. Con đường có thể được biểu diễn bằng mảng số nguyên 2 chiều <code>street</code>, trong đó <code>street[j] = [left<sub>j</sub>, right<sub>j</sub>, average<sub>j</sub>]</code> mô tả <strong>đoạn nửa kín</strong> <code>[left<sub>j</sub>, right<sub>j</sub>)</code> của con đường, nơi chiều cao <strong>trung bình</strong> của các tòa nhà trong<strong> đoạn</strong> đó là <code>average<sub>j</sub></code>.</p>

<ul>
	<li>Ví dụ, nếu <code>buildings = [[1,5,2],[3,10,4]],</code> thì có thể biểu diễn con đường bằng <code>street = [[1,3,2],[3,5,3],[5,10,4]]</code> vì:

    <ul>
    <li>Từ 1 đến 3, chỉ có tòa nhà đầu tiên với chiều cao trung bình là <code>2 / 1 = 2</code>.</li>
    <li>Từ 3 đến 5, có cả tòa nhà đầu tiên và tòa nhà thứ hai với chiều cao trung bình là <code>(2+4) / 2 = 3</code>.</li>
    <li>Từ 5 đến 10, chỉ có tòa nhà thứ hai với chiều cao trung bình là <code>4 / 1 = 4</code>.</li>
    </ul>
    </li>

</ul>

<p>Cho <code>buildings</code>, hãy trả về <em>mảng số nguyên 2 chiều </em><code>street</code><em> như mô tả ở trên (<strong>không bao gồm</strong> những khu vực không có tòa nhà trên đường). Bạn có thể trả về mảng theo <strong>bất kỳ thứ tự nào</strong></em>.</p>

<p><strong>Giá trị trung bình</strong> của <code>n</code> phần tử là <strong>tổng</strong> của <code>n</code> phần tử đó chia cho <code>n</code> (<strong>phép chia nguyên</strong>).</p>

<p><strong>Đoạn nửa kín</strong> <code>[a, b)</code> là phần của trục số nằm giữa hai điểm <code>a</code> và <code>b</code>, <strong>bao gồm</strong> điểm <code>a</code> và <strong>không bao gồm</strong> điểm <code>b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2015.Average%20Height%20of%20Buildings%20in%20Each%20Segment/images/image-20210921224001-2.png" style="width: 500px; height: 349px;" />
<pre>
<strong>Đầu vào:</strong> buildings = [[1,4,2],[3,9,4]]
<strong>Đầu ra:</strong> [[1,3,2],[3,4,3],[4,9,4]]
<strong>Giải thích:</strong>
Từ 1 đến 3, chỉ có tòa nhà đầu tiên với chiều cao trung bình là 2 / 1 = 2.
Từ 3 đến 4, có cả tòa nhà đầu tiên và tòa nhà thứ hai với chiều cao trung bình là (2+4) / 2 = 3.
Từ 4 đến 9, chỉ có tòa nhà thứ hai với chiều cao trung bình là 4 / 1 = 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> buildings = [[1,3,2],[2,5,3],[2,8,3]]
<strong>Đầu ra:</strong> [[1,3,2],[3,8,3]]
<strong>Giải thích:</strong>
Từ 1 đến 2, chỉ có tòa nhà đầu tiên với chiều cao trung bình là 2 / 1 = 2.
Từ 2 đến 3, có cả ba tòa nhà với chiều cao trung bình là (2+3+3) / 3 = 2.
Từ 3 đến 5, có cả tòa nhà thứ hai và tòa nhà thứ ba với chiều cao trung bình là (3+3) / 2 = 3.
Từ 5 đến 8, chỉ có tòa nhà cuối cùng với chiều cao trung bình là 3 / 1 = 3.
Chiều cao trung bình từ 1 đến 3 giống nhau, nên ta có thể gộp chúng thành một đoạn.
Chiều cao trung bình từ 3 đến 8 giống nhau, nên ta có thể gộp chúng thành một đoạn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> buildings = [[1,2,1],[5,6,1]]
<strong>Đầu ra:</strong> [[1,2,1],[5,6,1]]
<strong>Giải thích:</strong>
Từ 1 đến 2, chỉ có tòa nhà đầu tiên với chiều cao trung bình là 1 / 1 = 1.
Từ 2 đến 5 không có tòa nhà nào, nên không được đưa vào kết quả.
Từ 5 đến 6, chỉ có tòa nhà thứ hai với chiều cao trung bình là 1 / 1 = 1.
Ta không thể gộp các đoạn này vì giữa chúng có một khoảng trống không có tòa nhà.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= buildings.length &lt;= 10<sup>5</sup></code></li>
	<li><code>buildings[i].length == 3</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt; end<sub>i</sub> &lt;= 10<sup>8</sup></code></li>
	<li><code>1 &lt;= height<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Các đầu mút của tòa nhà thưa, nên dùng mảng hiệu đặc sẽ lãng phí. Mỗi tòa nhà cộng chiều cao tại điểm bắt đầu và trừ tại điểm kết thúc; số lượng tòa nhà cũng được xử lý tương tự.
>
> Quét các tọa độ đã sắp xếp, duy trì tổng chiều cao $s$ và số lượng $m$, rồi tính chiều cao trung bình $s//m$. Gộp các đoạn liền kề có cùng chiều cao trung bình.
>
> Hash map lưu các thay đổi; sau một lần sắp xếp và duyệt, ta thu được kết quả.

<!-- thinking:end -->

Ta có thể sử dụng ý tưởng mảng hiệu, dùng một hash table $\textit{cnt}$ để ghi nhận thay đổi về số lượng tòa nhà tại mỗi vị trí, và một hash table khác $\textit{d}$ để ghi nhận thay đổi về chiều cao tại mỗi vị trí.

Tiếp theo, ta sắp xếp hash table $\textit{d}$ theo các key, dùng biến $\textit{s}$ để lưu tổng chiều cao hiện tại và biến $\textit{m}$ để lưu số lượng tòa nhà hiện tại.

Sau đó, ta duyệt qua hash table $\textit{d}$. Với mỗi vị trí, nếu $\textit{m}$ khác 0, nghĩa là có tòa nhà ở các vị trí trước đó, ta tính chiều cao trung bình. Nếu chiều cao trung bình của các tòa nhà tại vị trí hiện tại giống với các tòa nhà trước đó, ta gộp chúng; nếu không, ta thêm vị trí hiện tại vào kết quả.

Cuối cùng, ta trả về kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng tòa nhà.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def averageHeightOfBuildings(self, buildings: List[List[int]]) -> List[List[int]]:
        cnt = defaultdict(int)
        d = defaultdict(int)
        for start, end, height in buildings:
            cnt[start] += 1
            cnt[end] -= 1
            d[start] += height
            d[end] -= height
        s = m = 0
        last = -1
        ans = []
        for k, v in sorted(d.items()):
            if m:
                avg = s // m
                if ans and ans[-1][2] == avg and ans[-1][1] == last:
                    ans[-1][1] = k
                else:
                    ans.append([last, k, avg])
            s += v
            m += cnt[k]
            last = k
        return ans
```

#### Java

```java
class Solution {
    public int[][] averageHeightOfBuildings(int[][] buildings) {
        Map<Integer, Integer> cnt = new HashMap<>();
        TreeMap<Integer, Integer> d = new TreeMap<>();
        for (var e : buildings) {
            int start = e[0], end = e[1], height = e[2];
            cnt.merge(start, 1, Integer::sum);
            cnt.merge(end, -1, Integer::sum);
            d.merge(start, height, Integer::sum);
            d.merge(end, -height, Integer::sum);
        }
        int s = 0, m = 0;
        int last = -1;
        List<int[]> ans = new ArrayList<>();
        for (var e : d.entrySet()) {
            int k = e.getKey(), v = e.getValue();
            if (m > 0) {
                int avg = s / m;
                if (!ans.isEmpty() && ans.get(ans.size() - 1)[2] == avg
                    && ans.get(ans.size() - 1)[1] == last) {
                    ans.get(ans.size() - 1)[1] = k;
                } else {
                    ans.add(new int[] {last, k, avg});
                }
            }
            s += v;
            m += cnt.get(k);
            last = k;
        }
        return ans.toArray(new int[0][]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> averageHeightOfBuildings(vector<vector<int>>& buildings) {
        unordered_map<int, int> cnt;
        map<int, int> d;

        for (const auto& e : buildings) {
            int start = e[0], end = e[1], height = e[2];
            cnt[start]++;
            cnt[end]--;
            d[start] += height;
            d[end] -= height;
        }

        int s = 0, m = 0;
        int last = -1;
        vector<vector<int>> ans;

        for (const auto& [k, v] : d) {
            if (m > 0) {
                int avg = s / m;
                if (!ans.empty() && ans.back()[2] == avg && ans.back()[1] == last) {
                    ans.back()[1] = k;
                } else {
                    ans.push_back({last, k, avg});
                }
            }
            s += v;
            m += cnt[k];
            last = k;
        }

        return ans;
    }
};
```

#### Go

```go
func averageHeightOfBuildings(buildings [][]int) [][]int {
	cnt := make(map[int]int)
	d := make(map[int]int)

	for _, e := range buildings {
		start, end, height := e[0], e[1], e[2]
		cnt[start]++
		cnt[end]--
		d[start] += height
		d[end] -= height
	}

	s, m := 0, 0
	last := -1
	var ans [][]int

	keys := make([]int, 0, len(d))
	for k := range d {
		keys = append(keys, k)
	}
	sort.Ints(keys)

	for _, k := range keys {
		v := d[k]
		if m > 0 {
			avg := s / m
			if len(ans) > 0 && ans[len(ans)-1][2] == avg && ans[len(ans)-1][1] == last {
				ans[len(ans)-1][1] = k
			} else {
				ans = append(ans, []int{last, k, avg})
			}
		}
		s += v
		m += cnt[k]
		last = k
	}

	return ans
}
```

#### TypeScript

```ts
function averageHeightOfBuildings(buildings: number[][]): number[][] {
    const cnt = new Map<number, number>();
    const d = new Map<number, number>();
    for (const [start, end, height] of buildings) {
        cnt.set(start, (cnt.get(start) || 0) + 1);
        cnt.set(end, (cnt.get(end) || 0) - 1);
        d.set(start, (d.get(start) || 0) + height);
        d.set(end, (d.get(end) || 0) - height);
    }
    let [s, m] = [0, 0];
    let last = -1;
    const ans: number[][] = [];
    const sortedKeys = Array.from(d.keys()).sort((a, b) => a - b);
    for (const k of sortedKeys) {
        const v = d.get(k)!;
        if (m > 0) {
            const avg = Math.floor(s / m);
            if (ans.length > 0 && ans.at(-1)![2] === avg && ans.at(-1)![1] === last) {
                ans[ans.length - 1][1] = k;
            } else {
                ans.push([last, k, avg]);
            }
        }
        s += v;
        m += cnt.get(k)!;
        last = k;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
