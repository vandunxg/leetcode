---
comments: true
difficulty: Medium
rating: 1701
source: Weekly Contest 230 Q2
tags:
    - Array
    - Dynamic Programming
    - Backtracking
    - Knapsack
    - Mixed Knapsack
---

<!-- problem:start -->

# [1774. Closest Dessert Cost](https://leetcode.com/problems/closest-dessert-cost)

[中文文档](/solution/1700-1799/1774.Closest%20Dessert%20Cost/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn muốn làm món tráng miệng và đang chuẩn bị mua nguyên liệu. Có <code>n</code> loại kem nền và <code>m</code> loại topping để lựa chọn. Khi làm món tráng miệng, cần tuân theo các quy tắc:</p>

<ul>
<li>Phải có <strong>đúng một</strong> kem nền.</li>
<li>Có thể thêm <strong>một hoặc nhiều</strong> loại topping, hoặc không thêm topping.</li>
<li>Mỗi loại topping được dùng <strong>tối đa hai</strong> lần, với <strong>mỗi loại</strong> không quá hai phần.</li>
</ul>

<p>Cho ba đầu vào:</p>

<ul>
	<li><code>baseCosts</code>, một mảng số nguyên độ dài <code>n</code>, trong đó mỗi <code>baseCosts[i]</code> là giá của vị kem nền thứ <code>i<sup>th</sup></code>.</li>
	<li><code>toppingCosts</code>, một mảng số nguyên độ dài <code>m</code>, trong đó mỗi <code>toppingCosts[i]</code> là giá của <strong>một</strong> phần topping thứ <code>i<sup>th</sup></code>.</li>
	<li><code>target</code>, một số nguyên biểu diễn giá mục tiêu của món tráng miệng.</li>
</ul>

<p>Bạn muốn làm món tráng miệng có tổng chi phí gần <code>target</code> nhất có thể.</p>

<p>Trả về <em>chi phí món tráng miệng gần </em><code>target</code> nhất có thể. Nếu có nhiều lựa chọn, trả về giá trị <em><strong>nhỏ hơn</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> baseCosts = [1,7], toppingCosts = [3,4], target = 10
<strong>Output:</strong> 10
<strong>Explanation:</strong> Consider the following combination (all 0-indexed):
- Choose base 1: cost 7
- Take 1 of topping 0: cost 1 x 3 = 3
- Take 0 of topping 1: cost 0 x 4 = 0
Total: 7 + 3 + 0 = 10.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> baseCosts = [2,3], toppingCosts = [4,5,100], target = 18
<strong>Output:</strong> 17
<strong>Explanation:</strong> Consider the following combination (all 0-indexed):
- Choose base 1: cost 3
- Take 1 of topping 0: cost 1 x 4 = 4
- Take 2 of topping 1: cost 2 x 5 = 10
- Take 0 of topping 2: cost 0 x 100 = 0
Total: 3 + 4 + 10 + 0 = 17. You cannot make a dessert with a total cost of 18.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> baseCosts = [3,10], toppingCosts = [2,5], target = 9
<strong>Output:</strong> 8
<strong>Explanation:</strong> It is possible to make desserts with cost 8 and 10. Return 8 as it is the lower cost.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>n == baseCosts.length</code></li>
	<li><code>m == toppingCosts.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 10</code></li>
	<li><code>1 &lt;= baseCosts[i], toppingCosts[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= target &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cần đúng một kem nền; mỗi topping được lấy $0$, $1$ hoặc $2$ lần. Chi phí cần gần $\textit{target}$ nhất. Số loại topping nhỏ nên có thể liệt kê các tổng tập con.
>
> Nhân đôi các topping, dùng DFS để tạo mọi tổng rồi sắp xếp chúng. Với mỗi kem nền và một tổng nửa đầu, tìm kiếm nhị phân tổng còn lại gần phần bù nhất; nếu hòa, chọn chi phí nhỏ hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def closestCost(
        self, baseCosts: List[int], toppingCosts: List[int], target: int
    ) -> int:
        def dfs(i, t):
            if i >= len(toppingCosts):
                arr.append(t)
                return
            dfs(i + 1, t)
            dfs(i + 1, t + toppingCosts[i])

        arr = []
        dfs(0, 0)
        arr.sort()
        d = ans = inf

        # 选择一种冰激淋基料
        for x in baseCosts:
            # 枚举子集和
            for y in arr:
                # 二分查找
                i = bisect_left(arr, target - x - y)
                for j in (i, i - 1):
                    if 0 <= j < len(arr):
                        t = abs(x + y + arr[j] - target)
                        if d > t or (d == t and ans > x + y + arr[j]):
                            d = t
                            ans = x + y + arr[j]
        return ans
```

#### Java

```java
class Solution {
    private List<Integer> arr = new ArrayList<>();
    private int[] ts;
    private int inf = 1 << 30;

    public int closestCost(int[] baseCosts, int[] toppingCosts, int target) {
        ts = toppingCosts;
        dfs(0, 0);
        Collections.sort(arr);
        int d = inf, ans = inf;

        // 选择一种冰激淋基料
        for (int x : baseCosts) {
            // 枚举子集和
            for (int y : arr) {
                // 二分查找
                int i = search(target - x - y);
                for (int j : new int[] {i, i - 1}) {
                    if (j >= 0 && j < arr.size()) {
                        int t = Math.abs(x + y + arr.get(j) - target);
                        if (d > t || (d == t && ans > x + y + arr.get(j))) {
                            d = t;
                            ans = x + y + arr.get(j);
                        }
                    }
                }
            }
        }
        return ans;
    }

    private int search(int x) {
        int left = 0, right = arr.size();
        while (left < right) {
            int mid = (left + right) >> 1;
            if (arr.get(mid) >= x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private void dfs(int i, int t) {
        if (i >= ts.length) {
            arr.add(t);
            return;
        }
        dfs(i + 1, t);
        dfs(i + 1, t + ts[i]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int inf = INT_MAX;
    int closestCost(vector<int>& baseCosts, vector<int>& toppingCosts, int target) {
        vector<int> arr;
        function<void(int, int)> dfs = [&](int i, int t) {
            if (i >= toppingCosts.size()) {
                arr.push_back(t);
                return;
            }
            dfs(i + 1, t);
            dfs(i + 1, t + toppingCosts[i]);
        };
        dfs(0, 0);
        sort(arr.begin(), arr.end());
        int d = inf, ans = inf;
        // 选择一种冰激淋基料
        for (int x : baseCosts) {
            // 枚举子集和
            for (int y : arr) {
                // 二分查找
                int i = lower_bound(arr.begin(), arr.end(), target - x - y) - arr.begin();
                for (int j = i - 1; j < i + 1; ++j) {
                    if (j >= 0 && j < arr.size()) {
                        int t = abs(x + y + arr[j] - target);
                        if (d > t || (d == t && ans > x + y + arr[j])) {
                            d = t;
                            ans = x + y + arr[j];
                        }
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func closestCost(baseCosts []int, toppingCosts []int, target int) int {
	arr := []int{}
	var dfs func(int, int)
	dfs = func(i, t int) {
		if i >= len(toppingCosts) {
			arr = append(arr, t)
			return
		}
		dfs(i+1, t)
		dfs(i+1, t+toppingCosts[i])
	}
	dfs(0, 0)
	sort.Ints(arr)
	const inf = 1 << 30
	ans, d := inf, inf
	// 选择一种冰激淋基料
	for _, x := range baseCosts {
		// 枚举子集和
		for _, y := range arr {
			// 二分查找
			i := sort.SearchInts(arr, target-x-y)
			for j := i - 1; j < i+1; j++ {
				if j >= 0 && j < len(arr) {
					t := abs(x + y + arr[j] - target)
					if d > t || (d == t && ans > x+y+arr[j]) {
						d = t
						ans = x + y + arr[j]
					}
				}
			}
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### JavaScript

```js
const closestCost = function (baseCosts, toppingCosts, target) {
    let closestDessertCost = -Infinity;
    function dfs(dessertCost, j) {
        const tarCurrDiff = Math.abs(target - dessertCost);
        const tarCloseDiff = Math.abs(target - closestDessertCost);
        if (tarCurrDiff < tarCloseDiff) {
            closestDessertCost = dessertCost;
        } else if (tarCurrDiff === tarCloseDiff && dessertCost < closestDessertCost) {
            closestDessertCost = dessertCost;
        }
        if (dessertCost > target) return;
        if (j === toppingCosts.length) return;
        for (let count = 0; count <= 2; count++) {
            dfs(dessertCost + count * toppingCosts[j], j + 1);
        }
    }
    for (let i = 0; i < baseCosts.length; i++) {
        dfs(baseCosts[i], 0);
    }
    return closestDessertCost;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
