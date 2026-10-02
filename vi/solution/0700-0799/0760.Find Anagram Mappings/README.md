---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [760. Find Anagram Mappings 🔒](https://leetcode.com/problems/find-anagram-mappings)

[中文文档](/solution/0700-0799/0760.Find%20Anagram%20Mappings/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, trong đó <code>nums2</code> là <strong>hoán vị</strong> của <code>nums1</code>. Cả hai mảng có thể chứa phần tử trùng lặp.</p>

<p>Hãy trả về <em>mảng ánh xạ chỉ số </em><code>mapping</code><em> từ </em><code>nums1</code><em> sang </em><code>nums2</code><em>, trong đó </em><code>mapping[i] = j</code><em> có nghĩa là phần tử thứ </em><code>i<sup>th</sup></code><em> trong </em><code>nums1</code><em> xuất hiện trong </em><code>nums2</code><em> tại chỉ số </em><code>j</code>. Nếu có nhiều đáp án, hãy trả về <strong>bất kỳ đáp án nào</strong>.</p>

<p>Mảng <code>a</code> là <strong>hoán vị</strong> của mảng <code>b</code> nghĩa là <code>b</code> được tạo ra bằng cách xáo trộn thứ tự các phần tử trong <code>a</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [12,28,46,32,50], nums2 = [50,12,32,46,28]
<strong>Đầu ra:</strong> [1,4,3,2,0]
<strong>Giải thích:</strong> mapping[0] = 1 vì phần tử thứ 0 của nums1 xuất hiện tại nums2[1], và mapping[1] = 4 vì phần tử thứ 1 của nums1 xuất hiện tại nums2[4], cứ tiếp tục như vậy.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [84,46], nums2 = [84,46]
<strong>Đầu ra:</strong> [0,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length &lt;= 100</code></li>
	<li><code>nums2.length == nums1.length</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>nums2</code> là hoán vị của <code>nums1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hai mảng là hoán vị của nhau; ta cần tìm một chỉ số trong $nums2$ cho mỗi giá trị của $nums1$. $n\le 100$, nhưng dùng map có thể xử lý trong thời gian tuyến tính.
>
> Ghi lại các chỉ số của $nums2$ (các giá trị trùng lặp xuất hiện sau sẽ ghi đè), rồi tra cứu từng $nums1[i]$. Bất kỳ ánh xạ hợp lệ nào cũng được chấp nhận.

<!-- thinking:end -->

Ta dùng hash table $\textit{d}$ để lưu mỗi phần tử trong mảng $\textit{nums2}$ cùng chỉ số tương ứng. Sau đó, ta duyệt mảng $\textit{nums1}$; với mỗi phần tử $\textit{nums1}[i]$, ta lấy chỉ số tương ứng từ hash table $\textit{d}$ và lưu vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def anagramMappings(self, nums1: List[int], nums2: List[int]) -> List[int]:
        d = {x: i for i, x in enumerate(nums2)}
        return [d[x] for x in nums1]
```

#### Java

```java
class Solution {
    public int[] anagramMappings(int[] nums1, int[] nums2) {
        int n = nums1.length;
        Map<Integer, Integer> d = new HashMap<>(n);
        for (int i = 0; i < n; ++i) {
            d.put(nums2[i], i);
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = d.get(nums1[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> anagramMappings(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        unordered_map<int, int> d;
        for (int i = 0; i < n; ++i) {
            d[nums2[i]] = i;
        }
        vector<int> ans;
        for (int x : nums1) {
            ans.push_back(d[x]);
        }
        return ans;
    }
};
```

#### Go

```go
func anagramMappings(nums1 []int, nums2 []int) []int {
	d := map[int]int{}
	for i, x := range nums2 {
		d[x] = i
	}
	ans := make([]int, len(nums1))
	for i, x := range nums1 {
		ans[i] = d[x]
	}
	return ans
}
```

#### TypeScript

```ts
function anagramMappings(nums1: number[], nums2: number[]): number[] {
    const d: Map<number, number> = new Map();
    for (let i = 0; i < nums2.length; ++i) {
        d.set(nums2[i], i);
    }
    return nums1.map(num => d.get(num)!);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
