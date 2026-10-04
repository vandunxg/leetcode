---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3682. Minimum Index Sum of Common Elements 🔒](https://leetcode.com/problems/minimum-index-sum-of-common-elements)

[中文文档](/solution/3600-3699/3682.Minimum%20Index%20Sum%20of%20Common%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có cùng độ dài <code>n</code>.</p>

<p>Ta gọi một cặp chỉ số <code>(i, j)</code> là một <strong>good pair</strong> nếu <code>nums1[i] == nums2[j]</code>.</p>

<p>Hãy trả về <strong>tổng chỉ số nhỏ nhất</strong> <code>i + j</code> trong tất cả các good pair có thể có. Nếu không tồn tại cặp nào như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [3,2,1], nums2 = [1,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử chung giữa <code>nums1</code> và <code>nums2</code> là 1 và 3.</li>
	<li>Với 3, <code>[i, j] = [0, 1]</code>, cho tổng chỉ số <code>i + j = 1</code>.</li>
	<li>Với 1, <code>[i, j] = [2, 0]</code>, cho tổng chỉ số <code>i + j = 2</code>.</li>
	<li>Tổng chỉ số nhỏ nhất là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [5,1,2], nums2 = [2,1,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Các phần tử chung giữa <code>nums1</code> và <code>nums2</code> là 1 và 2.</li>
	<li>Với 1, <code>[i, j] = [1, 1]</code>, cho tổng chỉ số <code>i + j = 2</code>.</li>
	<li>Với 2, <code>[i, j] = [2, 0]</code>, cho tổng chỉ số <code>i + j = 2</code>.</li>
	<li>Tổng chỉ số nhỏ nhất là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [6,4], nums2 = [7,8]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì không có phần tử chung giữa <code>nums1</code> và <code>nums2</code>, kết quả là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length == nums2.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng chỉ số nhỏ nhất trên các giá trị chung. Nếu duyệt $\textit{nums2}$ cho từng phần tử của $\textit{nums1}$ thì sẽ không đáp ứng được với $n\le 10^5$.
>
> Lưu chỉ số đầu tiên của mỗi giá trị trong $\textit{nums2}$, sau đó duyệt $\textit{nums1}$ và cập nhật bằng $i+d[x]$.
>
> Chỉ giữ lại lần xuất hiện đầu tiên sẽ tối thiểu hóa phía $\textit{nums2}$. Nếu không có phần tử chung, trả về $-1$.

<!-- thinking:end -->

Ta khởi tạo một biến $\textit{ans}$ bằng vô cùng, biểu diễn tổng chỉ số nhỏ nhất hiện tại, đồng thời sử dụng một hash map $\textit{d}$ để lưu chỉ số xuất hiện đầu tiên của mỗi phần tử trong mảng $\textit{nums2}$.

Sau đó, ta duyệt mảng $\textit{nums1}$. Với mỗi phần tử $\textit{nums1}[i]$, nếu phần tử đó tồn tại trong $\textit{d}$, ta tính tổng chỉ số $i + \textit{d}[\textit{nums1}[i]]$ và cập nhật $\textit{ans}$.

Cuối cùng, nếu $\textit{ans}$ vẫn bằng vô cùng, nghĩa là không tìm thấy phần tử chung nào, nên ta trả về -1; nếu không, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumSum(self, nums1: List[int], nums2: List[int]) -> int:
        d = {}
        for i, x in enumerate(nums2):
            if x not in d:
                d[x] = i
        ans = inf
        for i, x in enumerate(nums1):
            if x in d:
                ans = min(ans, i + d[x])
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minimumSum(int[] nums1, int[] nums2) {
        int n = nums1.length;
        final int inf = 1 << 30;
        Map<Integer, Integer> d = new HashMap<>();
        for (int i = 0; i < n; i++) {
            d.putIfAbsent(nums2[i], i);
        }
        int ans = inf;
        for (int i = 0; i < n; i++) {
            if (d.containsKey(nums1[i])) {
                ans = Math.min(ans, i + d.get(nums1[i]));
            }
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumSum(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        const int inf = INT_MAX;
        unordered_map<int, int> d;
        for (int i = 0; i < n; i++) {
            if (!d.contains(nums2[i])) {
                d[nums2[i]] = i;
            }
        }
        int ans = inf;
        for (int i = 0; i < n; i++) {
            if (d.contains(nums1[i])) {
                ans = min(ans, i + d[nums1[i]]);
            }
        }
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func minimumSum(nums1 []int, nums2 []int) int {
	const inf = 1 << 30
	d := make(map[int]int)
	for i, x := range nums2 {
		if _, ok := d[x]; !ok {
			d[x] = i
		}
	}
	ans := inf
	for i, x := range nums1 {
		if j, ok := d[x]; ok {
            ans = min(ans, i + j)
		}
	}
	if ans == inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumSum(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const inf = 1 << 30;
    const d = new Map<number, number>();
    for (let i = 0; i < n; i++) {
        if (!d.has(nums2[i])) {
            d.set(nums2[i], i);
        }
    }
    let ans = inf;
    for (let i = 0; i < n; i++) {
        if (d.has(nums1[i])) {
            ans = Math.min(ans, i + (d.get(nums1[i]) as number));
        }
    }
    return ans === inf ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
