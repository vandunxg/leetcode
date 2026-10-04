---
comments: true
difficulty: Easy
rating: 1214
source: Biweekly Contest 119 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2956. Find Common Elements Between Two Arrays](https://leetcode.com/problems/find-common-elements-between-two-arrays)

[中文文档](/solution/2900-2999/2956.Find%20Common%20Elements%20Between%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code> có kích thước lần lượt là <code>n</code> và <code>m</code>. Hãy tính các giá trị sau:</p>

<ul>
	<li><code>answer1</code>: số lượng chỉ số <code>i</code> sao cho <code>nums1[i]</code> xuất hiện trong <code>nums2</code>.</li>
	<li><code>answer2</code>: số lượng chỉ số <code>i</code> sao cho <code>nums2[i]</code> xuất hiện trong <code>nums1</code>.</li>
</ul>

<p>Trả về <code>[answer1,answer2]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [2,3,2], nums2 = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2956.Find%20Common%20Elements%20Between%20Two%20Arrays/images/3488_find_common_elements_between_two_arrays-t1.gif" style="width: 225px; height: 150px;" /></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [4,3,2,3,1], nums2 = [2,2,5,2,3,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các phần tử ở các chỉ số 1, 2 và 3 trong <code>nums1</code> cũng xuất hiện trong <code>nums2</code>. Vì vậy, <code>answer1</code> bằng 3.</p>

<p>Các phần tử ở các chỉ số 0, 1, 3 và 4 trong <code>nums2</code> xuất hiện trong <code>nums1</code>. Vì vậy, <code>answer2</code> bằng 4.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [3,4,2,3], nums2 = [1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,0]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nào xuất hiện trong cả <code>nums1</code> và <code>nums2</code>, nên kết quả là [0,0].</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length</code></li>
	<li><code>m == nums2.length</code></li>
	<li><code>1 &lt;= n, m &lt;= 100</code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc Mảng

<!-- thinking:start -->

> **Tư duy**
>
> Đếm số giá trị trong $nums1$ xuất hiện trong $nums2$ và ngược lại; đây là bài toán kiểm tra phần tử có tồn tại, không phải đối chiếu chỉ số. Vì $n,m \le 100$, ta xây dựng hai tập hợp rồi duyệt mỗi mảng một lần.
>
> Miền giá trị có nhiều nhất là $100$, nên ta cũng có thể dùng một mảng boolean. Sau đó trả về cặp kết quả.

<!-- thinking:end -->

Ta có thể dùng hai hash table hoặc mảng $s1$ và $s2$ để ghi lại các phần tử lần lượt xuất hiện trong hai mảng.

Tiếp theo, ta tạo một mảng $ans$ có độ dài $2$, trong đó $ans[0]$ biểu thị số phần tử trong $nums1$ xuất hiện trong $s2$, còn $ans[1]$ biểu thị số phần tử trong $nums2$ xuất hiện trong $s1$.

Sau đó, ta duyệt từng phần tử $x$ trong mảng $nums1$. Nếu $x$ đã xuất hiện trong $s2$, ta tăng $ans[0]$. Tiếp theo, ta duyệt từng phần tử $x$ trong mảng $nums2$. Nếu $x$ đã xuất hiện trong $s1$, ta tăng $ans[1]$.

Cuối cùng, ta trả về mảng $ans$.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là độ dài của các mảng $nums1$ và $nums2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findIntersectionValues(self, nums1: List[int], nums2: List[int]) -> List[int]:
        s1, s2 = set(nums1), set(nums2)
        return [sum(x in s2 for x in nums1), sum(x in s1 for x in nums2)]
```

#### Java

```java
class Solution {
    public int[] findIntersectionValues(int[] nums1, int[] nums2) {
        int[] s1 = new int[101];
        int[] s2 = new int[101];
        for (int x : nums1) {
            s1[x] = 1;
        }
        for (int x : nums2) {
            s2[x] = 1;
        }
        int[] ans = new int[2];
        for (int x : nums1) {
            ans[0] += s2[x];
        }
        for (int x : nums2) {
            ans[1] += s1[x];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findIntersectionValues(vector<int>& nums1, vector<int>& nums2) {
        int s1[101]{};
        int s2[101]{};
        for (int& x : nums1) {
            s1[x] = 1;
        }
        for (int& x : nums2) {
            s2[x] = 1;
        }
        vector<int> ans(2);
        for (int& x : nums1) {
            ans[0] += s2[x];
        }
        for (int& x : nums2) {
            ans[1] += s1[x];
        }
        return ans;
    }
};
```

#### Go

```go
func findIntersectionValues(nums1 []int, nums2 []int) []int {
	s1 := [101]int{}
	s2 := [101]int{}
	for _, x := range nums1 {
		s1[x] = 1
	}
	for _, x := range nums2 {
		s2[x] = 1
	}
	ans := make([]int, 2)
	for _, x := range nums1 {
		ans[0] += s2[x]
	}
	for _, x := range nums2 {
		ans[1] += s1[x]
	}
	return ans
}
```

#### TypeScript

```ts
function findIntersectionValues(nums1: number[], nums2: number[]): number[] {
    const s1: number[] = Array(101).fill(0);
    const s2: number[] = Array(101).fill(0);
    for (const x of nums1) {
        s1[x] = 1;
    }
    for (const x of nums2) {
        s2[x] = 1;
    }
    const ans: number[] = Array(2).fill(0);
    for (const x of nums1) {
        ans[0] += s2[x];
    }
    for (const x of nums2) {
        ans[1] += s1[x];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
