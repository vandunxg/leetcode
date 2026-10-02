---
comments: true
difficulty: Easy
rating: 1327
source: Biweekly Contest 2 Q2
tags:
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1086. High Five 🔒](https://leetcode.com/problems/high-five)

[中文文档](/solution/1000-1099/1086.High%20Five/README.md)

## Mô tả

<!-- description:start -->

<p>Cho danh sách điểm của các học sinh <code>items</code>, trong đó <code>items[i] = [ID<sub>i</sub>, score<sub>i</sub>]</code> biểu thị một điểm số của học sinh có <code>ID<sub>i</sub></code>. Hãy tính <strong>điểm trung bình của năm điểm cao nhất</strong> cho mỗi học sinh.</p>

<p>Trả về kết quả dưới dạng mảng các cặp <code>result</code>, trong đó <code>result[j] = [ID<sub>j</sub>, topFiveAverage<sub>j</sub>]</code> biểu thị học sinh có <code>ID<sub>j</sub></code> và điểm trung bình năm điểm cao nhất của học sinh đó. Sắp xếp <code>result</code> theo <code>ID<sub>j</sub></code> theo <strong>thứ tự tăng dần</strong>.</p>

<p><strong>Điểm trung bình của năm điểm cao nhất</strong> được tính bằng cách lấy tổng năm điểm cao nhất rồi chia cho <code>5</code> bằng <strong>phép chia nguyên</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[1,91],[1,92],[2,93],[2,97],[1,60],[2,77],[1,65],[1,87],[1,100],[2,100],[2,76]]
<strong>Đầu ra:</strong> [[1,87],[2,88]]
<strong>Giải thích: </strong>
Học sinh có ID = 1 đạt các điểm 91, 92, 60, 65, 87 và 100. Điểm trung bình của năm điểm cao nhất là (100 + 92 + 91 + 87 + 65) / 5 = 87.
Học sinh có ID = 2 đạt các điểm 93, 97, 77, 100 và 76. Điểm trung bình của năm điểm cao nhất là (100 + 97 + 93 + 77 + 76) / 5 = 88.6; dùng phép chia nguyên nên kết quả là 88.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> items = [[1,100],[7,100],[1,100],[7,100],[1,100],[7,100],[1,100],[7,100],[1,100],[7,100]]
<strong>Đầu ra:</strong> [[1,100],[7,100]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= items.length &lt;= 1000</code></li>
	<li><code>items[i].length == 2</code></li>
	<li><code>1 &lt;= ID<sub>i</sub> &lt;= 1000</code></li>
	<li><code>0 &lt;= score<sub>i</sub> &lt;= 100</code></li>
	<li>Với mỗi <code>ID<sub>i</sub></code>, sẽ có <strong>ít nhất</strong> năm điểm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi học sinh có ít nhất năm điểm; cần tính trung bình nguyên của năm điểm cao nhất và sắp xếp theo ID. Gom điểm theo từng học sinh rồi chọn năm điểm lớn nhất.
>
> Dùng map để lưu danh sách điểm, còn $m$ là ID lớn nhất đã gặp. Với mỗi ID hiện có trong đoạn $1..m$, lấy `nlargest(5)`, tính tổng rồi chia cho $5$.
>
> Bỏ qua các ID không xuất hiện, nhờ đó kết quả đã được sắp xếp sẵn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def highFive(self, items: List[List[int]]) -> List[List[int]]:
        d = defaultdict(list)
        m = 0
        for i, x in items:
            d[i].append(x)
            m = max(m, i)
        ans = []
        for i in range(1, m + 1):
            if xs := d[i]:
                avg = sum(nlargest(5, xs)) // 5
                ans.append([i, avg])
        return ans
```

#### Java

```java
class Solution {
    public int[][] highFive(int[][] items) {
        int size = 0;
        PriorityQueue[] s = new PriorityQueue[101];
        int n = 5;
        for (int[] item : items) {
            int i = item[0], score = item[1];
            if (s[i] == null) {
                ++size;
                s[i] = new PriorityQueue<>(n);
            }
            s[i].offer(score);
            if (s[i].size() > n) {
                s[i].poll();
            }
        }
        int[][] res = new int[size][2];
        int j = 0;
        for (int i = 0; i < 101; ++i) {
            if (s[i] == null) {
                continue;
            }
            int avg = sum(s[i]) / n;
            res[j][0] = i;
            res[j++][1] = avg;
        }
        return res;
    }

    private int sum(PriorityQueue<Integer> q) {
        int s = 0;
        while (!q.isEmpty()) {
            s += q.poll();
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> highFive(vector<vector<int>>& items) {
        vector<int> d[1001];
        for (auto& item : items) {
            int i = item[0], x = item[1];
            d[i].push_back(x);
        }
        vector<vector<int>> ans;
        for (int i = 1; i <= 1000; ++i) {
            if (!d[i].empty()) {
                sort(d[i].begin(), d[i].end(), greater<int>());
                int s = 0;
                for (int j = 0; j < 5; ++j) {
                    s += d[i][j];
                }
                ans.push_back({i, s / 5});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func highFive(items [][]int) (ans [][]int) {
	d := make([][]int, 1001)
	for _, item := range items {
		i, x := item[0], item[1]
		d[i] = append(d[i], x)
	}
	for i := 1; i <= 1000; i++ {
		if len(d[i]) > 0 {
			sort.Ints(d[i])
			s := 0
			for j := len(d[i]) - 1; j >= len(d[i])-5; j-- {
				s += d[i][j]
			}
			ans = append(ans, []int{i, s / 5})
		}
	}
	return ans
}
```

#### TypeScript

```ts
function highFive(items: number[][]): number[][] {
    const d: number[][] = Array(1001)
        .fill(0)
        .map(() => Array(0));
    for (const [i, x] of items) {
        d[i].push(x);
    }
    const ans: number[][] = [];
    for (let i = 1; i <= 1000; ++i) {
        if (d[i].length > 0) {
            d[i].sort((a, b) => b - a);
            const s = d[i].slice(0, 5).reduce((a, b) => a + b);
            ans.push([i, Math.floor(s / 5)]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
