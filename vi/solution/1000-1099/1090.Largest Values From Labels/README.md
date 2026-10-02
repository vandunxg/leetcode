---
comments: true
difficulty: Medium
rating: 1501
source: Weekly Contest 141 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
    - Sorting
---

<!-- problem:start -->

# [1090. Largest Values From Labels](https://leetcode.com/problems/largest-values-from-labels)

[中文文档](/solution/1000-1099/1090.Largest%20Values%20From%20Labels/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>values</code> và <code>labels</code>, lần lượt chứa giá trị và nhãn của <code>n</code> món đồ. Ngoài ra, bạn được cho hai số nguyên <code>numWanted</code> và <code>useLimit</code>.</p>

<p>Hãy tìm một tập con các món đồ sao cho tổng giá trị là <strong>lớn nhất</strong> và thỏa mãn:</p>

<ul>
	<li>Số món đồ được chọn <strong>không vượt quá</strong> <code>numWanted</code>.</li>
	<li>Số món đồ có cùng nhãn <strong>không vượt quá</strong> <code>useLimit</code>.</li>
</ul>

<p>Trả về tổng lớn nhất đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">values = [5,4,3,2,1], labels = [1,1,2,2,3], numWanted = 3, useLimit = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tập con được chọn gồm món đồ thứ nhất, thứ ba và thứ năm, có tổng giá trị là 5 + 3 + 1.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">values = [5,4,3,2,1], labels = [1,3,3,3,2], numWanted = 3, useLimit = 2</span></p>

<p><strong>Output:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tập con được chọn gồm món đồ thứ nhất, thứ hai và thứ ba, có tổng giá trị là 5 + 4 + 3.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">values = [9,8,8,7,6], labels = [0,0,0,1,1], numWanted = 3, useLimit = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<p>Tập con được chọn gồm món đồ thứ nhất và thứ tư, có tổng giá trị là 9 + 7.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == values.length == labels.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= values[i], labels[i] &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= numWanted, useLimit &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tập con có tối đa $\textit{numWanted}$ món đồ, mỗi nhãn xuất hiện không quá $\textit{useLimit}$ lần, và cần có tổng giá trị lớn nhất. Xét giá trị lớn trước, đồng thời dùng bộ đếm để giới hạn số món theo từng nhãn. Vì $n\le 2\times 10^4$, ta có thể sắp xếp rồi duyệt chọn một lượt.
>
> Sắp xếp các cặp $(value,label)$ theo thứ tự giảm dần; chọn món nếu nhãn của nó chưa đạt giới hạn, cho đến khi chọn đủ $\textit{numWanted}$ món.
>
> $cnt$ lưu số lần đã chọn món theo từng nhãn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestValsFromLabels(
        self, values: List[int], labels: List[int], numWanted: int, useLimit: int
    ) -> int:
        ans = num = 0
        cnt = Counter()
        for v, l in sorted(zip(values, labels), reverse=True):
            if cnt[l] < useLimit:
                cnt[l] += 1
                num += 1
                ans += v
                if num == numWanted:
                    break
        return ans
```

#### Java

```java
class Solution {
    public int largestValsFromLabels(int[] values, int[] labels, int numWanted, int useLimit) {
        int n = values.length;
        int[][] pairs = new int[n][2];
        for (int i = 0; i < n; ++i) {
            pairs[i] = new int[] {values[i], labels[i]};
        }
        Arrays.sort(pairs, (a, b) -> b[0] - a[0]);
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0, num = 0;
        for (int i = 0; i < n && num < numWanted; ++i) {
            int v = pairs[i][0], l = pairs[i][1];
            if (cnt.getOrDefault(l, 0) < useLimit) {
                cnt.merge(l, 1, Integer::sum);
                num += 1;
                ans += v;
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
    int largestValsFromLabels(vector<int>& values, vector<int>& labels, int numWanted, int useLimit) {
        int n = values.size();
        vector<pair<int, int>> pairs(n);
        for (int i = 0; i < n; ++i) {
            pairs[i] = {-values[i], labels[i]};
        }
        sort(pairs.begin(), pairs.end());
        unordered_map<int, int> cnt;
        int ans = 0, num = 0;
        for (int i = 0; i < n && num < numWanted; ++i) {
            int v = -pairs[i].first, l = pairs[i].second;
            if (cnt[l] < useLimit) {
                ++cnt[l];
                ++num;
                ans += v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestValsFromLabels(values []int, labels []int, numWanted int, useLimit int) (ans int) {
	n := len(values)
	pairs := make([][2]int, n)
	for i := 0; i < n; i++ {
		pairs[i] = [2]int{values[i], labels[i]}
	}
	sort.Slice(pairs, func(i, j int) bool { return pairs[i][0] > pairs[j][0] })
	cnt := map[int]int{}
	for i, num := 0, 0; i < n && num < numWanted; i++ {
		v, l := pairs[i][0], pairs[i][1]
		if cnt[l] < useLimit {
			cnt[l]++
			num++
			ans += v
		}
	}
	return
}
```

#### TypeScript

```ts
function largestValsFromLabels(
    values: number[],
    labels: number[],
    numWanted: number,
    useLimit: number,
): number {
    const n = values.length;
    const pairs = new Array(n);
    for (let i = 0; i < n; ++i) {
        pairs[i] = [values[i], labels[i]];
    }
    pairs.sort((a, b) => b[0] - a[0]);
    const cnt: Map<number, number> = new Map();
    let ans = 0;
    for (let i = 0, num = 0; i < n && num < numWanted; ++i) {
        const [v, l] = pairs[i];
        if ((cnt.get(l) || 0) < useLimit) {
            cnt.set(l, (cnt.get(l) || 0) + 1);
            ++num;
            ans += v;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
