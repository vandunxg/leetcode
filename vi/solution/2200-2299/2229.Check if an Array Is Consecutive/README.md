---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2229. Check if an Array Is Consecutive 🔒](https://leetcode.com/problems/check-if-an-array-is-consecutive)

[中文文档](/solution/2200-2299/2229.Check%20if%20an%20Array%20Is%20Consecutive/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <code>true</code> <em>nếu </em><code>nums</code><em> là một mảng <strong>liên tiếp</strong>, nếu không thì trả về </em><code>false</code><em>.</em></p>

<p>Một mảng được gọi là <strong>liên tiếp</strong> nếu nó chứa mọi số trong đoạn <code>[x, x + n - 1]</code> (<strong>bao gồm cả hai đầu mút</strong>), trong đó <code>x</code> là số nhỏ nhất trong mảng và <code>n</code> là độ dài của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,4,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Giá trị nhỏ nhất là 1 và độ dài của nums là 4.
Tất cả các giá trị trong đoạn [x, x + n - 1] = [1, 1 + 4 - 1] = [1, 4] = (1, 2, 3, 4) đều xuất hiện trong nums.
Do đó, nums là một mảng liên tiếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Giá trị nhỏ nhất là 1 và độ dài của nums là 2.
Giá trị 2 trong đoạn [x, x + n - 1] = [1, 1 + 2 - 1], = [1, 2] = (1, 2) không xuất hiện trong nums.
Do đó, nums không phải là một mảng liên tiếp.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,4]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
Giá trị nhỏ nhất là 3 và độ dài của nums là 3.
Tất cả các giá trị trong đoạn [x, x + n - 1] = [3, 3 + 3 - 1] = [3, 5] = (3, 4, 5) đều xuất hiện trong nums.
Do đó, nums là một mảng liên tiếp.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần kiểm tra xem mảng có phải là một hoán vị của một đoạn liên tiếp nào đó hay không. Sắp xếp rồi kiểm tra khoảng cách giữa các phần tử kề nhau có độ phức tạp $O(n\log n)$ và vẫn đáp ứng được ràng buộc, nhưng có một tiêu chí rẻ hơn: mọi giá trị phải khác nhau và $\max-\min+1$ phải bằng độ dài mảng.
>
> Một set đảm bảo tính duy nhất; kết hợp với giá trị nhỏ nhất và lớn nhất, đây là điều kiện cần và đủ.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{s}$ để lưu tất cả phần tử trong mảng $\textit{nums}$, đồng thời dùng hai biến $\textit{mi}$ và $\textit{mx}$ lần lượt biểu diễn giá trị nhỏ nhất và lớn nhất trong mảng.

Nếu tất cả phần tử trong mảng đều khác nhau và độ dài mảng bằng hiệu giữa giá trị lớn nhất và nhỏ nhất cộng thêm $1$, thì mảng là mảng liên tiếp và ta trả về $\textit{true}$; ngược lại, ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isConsecutive(self, nums: List[int]) -> bool:
        mi, mx = min(nums), max(nums)
        return len(set(nums)) == mx - mi + 1 == len(nums)
```

#### Java

```java
class Solution {
    public boolean isConsecutive(int[] nums) {
        int mi = nums[0], mx = 0;
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            if (!s.add(x)) {
                return false;
            }
            mi = Math.min(mi, x);
            mx = Math.max(mx, x);
        }
        return mx - mi + 1 == nums.length;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isConsecutive(vector<int>& nums) {
        unordered_set<int> s;
        int mi = nums[0], mx = 0;
        for (int x : nums) {
            if (s.contains(x)) {
                return false;
            }
            s.insert(x);
            mi = min(mi, x);
            mx = max(mx, x);
        }
        return mx - mi + 1 == nums.size();
    }
};
```

#### Go

```go
func isConsecutive(nums []int) bool {
	s := map[int]bool{}
	mi, mx := nums[0], 0
	for _, x := range nums {
		if s[x] {
			return false
		}
		s[x] = true
		mi = min(mi, x)
		mx = max(mx, x)
	}
	return mx-mi+1 == len(nums)
}
```

#### TypeScript

```ts
function isConsecutive(nums: number[]): boolean {
    let [mi, mx] = [nums[0], 0];
    const s = new Set<number>();
    for (const x of nums) {
        if (s.has(x)) {
            return false;
        }
        s.add(x);
        mi = Math.min(mi, x);
        mx = Math.max(mx, x);
    }
    return mx - mi + 1 === nums.length;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var isConsecutive = function (nums) {
    let [mi, mx] = [nums[0], 0];
    const s = new Set();
    for (const x of nums) {
        if (s.has(x)) {
            return false;
        }
        s.add(x);
        mi = Math.min(mi, x);
        mx = Math.max(mx, x);
    }
    return mx - mi + 1 === nums.length;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
