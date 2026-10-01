---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [349. Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays)

[中文文档](/solution/0300-0399/0349.Intersection%20of%20Two%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, hãy trả về <em>mảng gồm các phần tử thuộc <span data-keyword="array-intersection">phần giao</span> của chúng</em>. Mỗi phần tử trong kết quả phải <strong>duy nhất</strong>; bạn có thể trả về kết quả theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,2,1], nums2 = [2,2]
<strong>Đầu ra:</strong> [2]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [4,9,5], nums2 = [9,4,9,8,4]
<strong>Đầu ra:</strong> [9,4]
<strong>Giải thích:</strong> [4,9] cũng là đáp án hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums1.length, nums2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table hoặc Array

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm giao không trùng lặp của hai mảng. Duyệt lồng nhau tốn $O(nm)$. Set (hoặc một bảng nhỏ) cho phép kiểm tra phần tử có tồn tại hay không trong $O(1)$.
>
> Đưa một mảng vào set rồi duyệt mảng còn lại; khi tìm thấy phần tử chung, xóa nó khỏi set để tránh trùng lặp. Về bản chất, đây là phép giao hai set.

<!-- thinking:end -->

Trước tiên, ta dùng hash table hoặc mảng $s$ có độ dài $1001$ để đánh dấu các phần tử xuất hiện trong mảng $nums1$. Sau đó, ta duyệt từng phần tử của $nums2$. Nếu phần tử $x$ có trong $s$, ta thêm $x$ vào đáp án rồi xóa dấu của $x$ khỏi $s$.

Sau khi duyệt xong, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(n+m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của các mảng $nums1$ và $nums2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return list(set(nums1) & set(nums2))
```

#### Java

```java
class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        boolean[] s = new boolean[1001];
        for (int x : nums1) {
            s[x] = true;
        }
        List<Integer> ans = new ArrayList<>();
        for (int x : nums2) {
            if (s[x]) {
                ans.add(x);
                s[x] = false;
            }
        }
        return ans.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        bool s[1001];
        memset(s, false, sizeof(s));
        for (int x : nums1) {
            s[x] = true;
        }
        vector<int> ans;
        for (int x : nums2) {
            if (s[x]) {
                ans.push_back(x);
                s[x] = false;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func intersection(nums1 []int, nums2 []int) (ans []int) {
	s := [1001]bool{}
	for _, x := range nums1 {
		s[x] = true
	}
	for _, x := range nums2 {
		if s[x] {
			ans = append(ans, x)
			s[x] = false
		}
	}
	return
}
```

#### TypeScript

```ts
function intersection(nums1: number[], nums2: number[]): number[] {
    const s = new Set(nums1);
    return [...new Set(nums2.filter(x => s.has(x)))];
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums1
 * @param {number[]} nums2
 * @return {number[]}
 */
var intersection = function (nums1, nums2) {
    const s = new Set(nums1);
    return [...new Set(nums2.filter(x => s.has(x)))];
};
```

#### C#

```cs
public class Solution {
    public int[] Intersection(int[] nums1, int[] nums2) {
        HashSet<int> s1 = new HashSet<int>(nums1);
        HashSet<int> s2 = new HashSet<int>(nums2);
        s1.IntersectWith(s2);
        int[] ans = new int[s1.Count];
        s1.CopyTo(ans);
        return ans;
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums1
     * @param Integer[] $nums2
     * @return Integer[]
     */
    function intersection($nums1, $nums2) {
        $s1 = array_unique($nums1);
        $s2 = array_unique($nums2);
        $ans = array_intersect($s1, $s2);
        return array_values($ans);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
