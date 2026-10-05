---
comments: true
difficulty: Easy
rating: 1287
source: Biweekly Contest 177 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3852. Smallest Pair With Different Frequencies](https://leetcode.com/problems/smallest-pair-with-different-frequencies)

[中文文档](/solution/3800-3899/3852.Smallest%20Pair%20With%20Different%20Frequencies/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Xét tất cả các cặp giá trị <strong>khác nhau</strong> <code>x</code> và <code>y</code> trong <code>nums</code> sao cho:</p>

<ul>
	<li><code>x &lt; y</code></li>
	<li><code>x</code> và <code>y</code> có <span data-keyword="frequency-array">tần suất</span> khác nhau trong <code>nums</code>.</li>
</ul>

<p>Trong tất cả các cặp như vậy:</p>

<ul>
	<li>Chọn cặp có giá trị <code>x</code> nhỏ nhất có thể.</li>
	<li>Nếu có nhiều cặp có cùng <code>x</code>, chọn cặp có giá trị <code>y</code> nhỏ nhất có thể.</li>
</ul>

<p>Trả về một mảng số nguyên <code>[x, y]</code>. Nếu không tồn tại cặp hợp lệ, trả về <code>[-1, -1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Giá trị nhỏ nhất là 1 với tần suất bằng 2, và giá trị nhỏ nhất lớn hơn 1 có tần suất khác với 1 là 3 với tần suất bằng 1. Vì vậy, đáp án là <code>[1, 3]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả hai giá trị đều có cùng tần suất, nên không tồn tại cặp hợp lệ. Trả về <code>[-1, -1]</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng chỉ có một giá trị, nên không tồn tại cặp hợp lệ. Trả về <code>[-1, -1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm $x<y$ có tần suất khác nhau, đồng thời tối thiểu hóa $x$ rồi đến $y$. Cả độ dài và các giá trị đều không vượt quá $100$.
>
> Giá trị nhỏ nhất trong mảng phải là $x$, vì trước tiên ta cần tối thiểu hóa $x$.
>
> Đếm tần suất, chọn khóa nhỏ nhất làm $x$, sau đó trong các khóa còn lại chọn $y$ nhỏ nhất có số lần xuất hiện khác.
>
> Nếu không tồn tại giá trị nào như vậy, trả về $[-1,-1]$.

<!-- thinking:end -->

Ta dùng một hash table $\textit{cnt}$ để đếm tần suất của mỗi giá trị trong mảng. Sau đó, ta tìm giá trị nhỏ nhất $x$ và giá trị nhỏ nhất $y$ lớn hơn $x$ có tần suất khác với $x$. Nếu không tồn tại $y$ như vậy, trả về $[-1, -1]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minDistinctFreqPair(self, nums: list[int]) -> list[int]:
        cnt = Counter(nums)
        x = min(cnt.keys())
        min_y = inf
        for y in cnt.keys():
            if y < min_y and cnt[x] != cnt[y]:
                min_y = y
        return [-1, -1] if min_y == inf else [x, min_y]
```

#### Java

```java
class Solution {
    public int[] minDistinctFreqPair(int[] nums) {
        final int inf = Integer.MAX_VALUE;
        Map<Integer, Integer> cnt = new HashMap<>();
        int x = inf;
        for (int v : nums) {
            cnt.merge(v, 1, Integer::sum);
            x = Math.min(x, v);
        }
        int minY = inf;
        for (int y : cnt.keySet()) {
            if (y < minY && cnt.get(x) != cnt.get(y)) {
                minY = y;
            }
        }
        return minY == inf ? new int[] {-1, -1} : new int[] {x, minY};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minDistinctFreqPair(vector<int>& nums) {
        const int inf = INT_MAX;
        unordered_map<int, int> cnt;
        int x = inf;

        for (int v : nums) {
            cnt[v]++;
            x = min(x, v);
        }

        int minY = inf;
        for (auto& [y, _] : cnt) {
            if (y < minY && cnt[x] != cnt[y]) {
                minY = y;
            }
        }

        if (minY == inf) {
            return {-1, -1};
        }
        return {x, minY};
    }
};
```

#### Go

```go
func minDistinctFreqPair(nums []int) []int {
	const inf = math.MaxInt
	cnt := make(map[int]int)

	for _, v := range nums {
		cnt[v]++
	}

	x := slices.Min(nums)

	minY := inf
	for y := range cnt {
		if y < minY && cnt[x] != cnt[y] {
			minY = y
		}
	}

	if minY == inf {
		return []int{-1, -1}
	}
	return []int{x, minY}
}
```

#### TypeScript

```ts
function minDistinctFreqPair(nums: number[]): number[] {
    const inf = Number.MAX_SAFE_INTEGER;
    const cnt = new Map<number, number>();

    let x = inf;
    for (const v of nums) {
        cnt.set(v, (cnt.get(v) ?? 0) + 1);
        x = Math.min(x, v);
    }

    let minY = inf;
    for (const [y] of cnt) {
        if (y < minY && cnt.get(x)! !== cnt.get(y)!) {
            minY = y;
        }
    }

    return minY === inf ? [-1, -1] : [x, minY];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
