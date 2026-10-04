---
comments: true
difficulty: Medium
rating: 1917
source: Weekly Contest 379 Q3
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3002. Maximum Size of a Set After Removals](https://leetcode.com/problems/maximum-size-of-a-set-after-removals)

[中文文档](/solution/3000-3099/3002.Maximum%20Size%20of%20a%20Set%20After%20Removals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, được đánh chỉ số theo kiểu <strong>0-indexed</strong> và có cùng độ dài chẵn <code>n</code>.</p>

<p>Bạn phải xóa <code>n / 2</code> phần tử khỏi <code>nums1</code> và <code>n / 2</code> phần tử khỏi <code>nums2</code>. Sau khi xóa, đưa các phần tử còn lại của <code>nums1</code> và <code>nums2</code> vào một set <code>s</code>.</p>

<p>Trả về <em><strong>kích thước lớn nhất</strong> có thể có của set</em> <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,1,2], nums2 = [1,1,1,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta xóa hai lần xuất hiện của 1 khỏi nums1 và nums2. Sau khi xóa, hai mảng trở thành nums1 = [2,2] và nums2 = [1,1]. Khi đó, s = {1,2}.
Có thể chứng minh rằng 2 là kích thước lớn nhất có thể có của set s sau khi xóa.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,2,3,4,5,6], nums2 = [2,3,2,3,2,3]
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Ta xóa 2, 3 và 6 khỏi nums1, đồng thời xóa 2 và hai lần xuất hiện của 3 khỏi nums2. Sau khi xóa, hai mảng trở thành nums1 = [1,4,5] và nums2 = [2,3,2]. Khi đó, s = {1,2,3,4,5}.
Có thể chứng minh rằng 5 là kích thước lớn nhất có thể có của set s sau khi xóa.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums1 = [1,1,2,2,3,3], nums2 = [4,4,5,5,6,6]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta xóa 1, 2 và 3 khỏi nums1, đồng thời xóa 4, 5 và 6 khỏi nums2. Sau khi xóa, hai mảng trở thành nums1 = [1,2,3] và nums2 = [4,5,6]. Khi đó, s = {1,2,3,4,5,6}.
Có thể chứng minh rằng 6 là kích thước lớn nhất có thể có của set s sau khi xóa.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>n</code> là số chẵn.</li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Cả hai mảng có độ dài chẵn $n \le 2 \times 10^4$ và ta phải loại bỏ $n/2$ phần tử khỏi mỗi mảng. Duyệt qua mọi cách xóa là không thể.
>
> Hợp được tạo từ các giá trị chỉ xuất hiện trong $\textit{nums}_1$, chỉ xuất hiện trong $\textit{nums}_2$ và các giá trị thuộc giao. Mỗi phía chỉ có $n/2$ vị trí.
>
> Do đó, trước tiên ta điền hạn ngạch của mỗi phía bằng các giá trị riêng, sau đó bổ sung bằng các giá trị thuộc giao, đồng thời giới hạn tổng ở $n$. Phép hiệu và phép giao cho ta ba số lượng này trong một lượt duyệt.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSetSize(self, nums1: List[int], nums2: List[int]) -> int:
        s1 = set(nums1)
        s2 = set(nums2)
        n = len(nums1)
        a = min(len(s1 - s2), n // 2)
        b = min(len(s2 - s1), n // 2)
        return min(a + b + len(s1 & s2), n)
```

#### Java

```java
class Solution {
    public int maximumSetSize(int[] nums1, int[] nums2) {
        Set<Integer> s1 = new HashSet<>();
        Set<Integer> s2 = new HashSet<>();
        for (int x : nums1) {
            s1.add(x);
        }
        for (int x : nums2) {
            s2.add(x);
        }
        int n = nums1.length;
        int a = 0, b = 0, c = 0;
        for (int x : s1) {
            if (!s2.contains(x)) {
                ++a;
            }
        }
        for (int x : s2) {
            if (!s1.contains(x)) {
                ++b;
            } else {
                ++c;
            }
        }
        a = Math.min(a, n / 2);
        b = Math.min(b, n / 2);
        return Math.min(a + b + c, n);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSetSize(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> s1(nums1.begin(), nums1.end());
        unordered_set<int> s2(nums2.begin(), nums2.end());
        int n = nums1.size();
        int a = 0, b = 0, c = 0;
        for (int x : s1) {
            if (!s2.count(x)) {
                ++a;
            }
        }
        for (int x : s2) {
            if (!s1.count(x)) {
                ++b;
            } else {
                ++c;
            }
        }
        a = min(a, n / 2);
        b = min(b, n / 2);
        return min(a + b + c, n);
    }
};
```

#### Go

```go
func maximumSetSize(nums1 []int, nums2 []int) int {
	s1 := map[int]bool{}
	s2 := map[int]bool{}
	for _, x := range nums1 {
		s1[x] = true
	}
	for _, x := range nums2 {
		s2[x] = true
	}
	a, b, c := 0, 0, 0
	for x := range s1 {
		if !s2[x] {
			a++
		}
	}
	for x := range s2 {
		if !s1[x] {
			b++
		} else {
			c++
		}
	}
	n := len(nums1)
	a = min(a, n/2)
	b = min(b, n/2)
	return min(a+b+c, n)
}
```

#### TypeScript

```ts
function maximumSetSize(nums1: number[], nums2: number[]): number {
    const s1: Set<number> = new Set(nums1);
    const s2: Set<number> = new Set(nums2);
    const n = nums1.length;
    let [a, b, c] = [0, 0, 0];
    for (const x of s1) {
        if (!s2.has(x)) {
            ++a;
        }
    }
    for (const x of s2) {
        if (!s1.has(x)) {
            ++b;
        } else {
            ++c;
        }
    }
    a = Math.min(a, n >> 1);
    b = Math.min(b, n >> 1);
    return Math.min(a + b + c, n);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
