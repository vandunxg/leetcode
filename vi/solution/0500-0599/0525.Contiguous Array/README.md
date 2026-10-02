---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [525. Contiguous Array](https://leetcode.com/problems/contiguous-array)

[中文文档](/solution/0500-0599/0525.Contiguous%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code>, hãy trả về <em>độ dài lớn nhất của mảng con liên tiếp có số lượng </em><code>0</code><em> và </em><code>1</code><em> bằng nhau</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> [0, 1] là mảng con liên tiếp dài nhất có số lượng 0 và 1 bằng nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> [0, 1] (hoặc [1, 0]) là một mảng con liên tiếp dài nhất có số lượng 0 và 1 bằng nhau.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1,1,1,1,0,0,0]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> [1,1,1,0,0,0] là mảng con liên tiếp dài nhất có số lượng 0 và 1 bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> bằng <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix sum + hash table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con dài nhất có số lượng $0$ và $1$ bằng nhau. Kiểm tra mọi cặp sẽ tốn $O(n^2)$, quá chậm với $n \le 10^5$.
>
> Xem $0$ là $-1$; khi đó mảng con có tổng bằng $0$ sẽ có số lượng $0$ và $1$ bằng nhau. Hai prefix sum bằng nhau xác định hai đầu của đoạn này. Lưu chỉ số xuất hiện đầu tiên của mỗi prefix sum; nếu gặp lại, ta cập nhật độ dài. Khởi tạo prefix sum $0$ tại chỉ số $-1$ để tính được cả các đoạn bắt đầu từ đầu mảng.

<!-- thinking:end -->

Theo đề bài, ta có thể xem mỗi $0$ trong mảng là $-1$. Khi gặp $0$, prefix sum $s$ giảm một; khi gặp $1$, $s$ tăng một. Vì vậy, nếu prefix sum $s$ tại chỉ số $j$ và $i$ bằng nhau với $j < i$, thì mảng con từ chỉ số $j + 1$ đến $i$ có số lượng $0$ và $1$ bằng nhau.

Ta dùng hash table để lưu các prefix sum cùng chỉ số xuất hiện đầu tiên. Ban đầu, ánh xạ prefix sum $0$ tới chỉ số $-1$.

Khi duyệt mảng, ta tính prefix sum $s$. Nếu $s$ đã có trong hash table, ta tìm được một mảng con có tổng bằng $0$ với độ dài $i - d[s]$, trong đó $d[s]$ là chỉ số đầu tiên mà $s$ xuất hiện. Nếu $s$ chưa có trong hash table, ta lưu $s$ cùng chỉ số $i$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxLength(self, nums: List[int]) -> int:
        d = {0: -1}
        ans = s = 0
        for i, x in enumerate(nums):
            s += 1 if x else -1
            if s in d:
                ans = max(ans, i - d[s])
            else:
                d[s] = i
        return ans
```

#### Java

```java
class Solution {
    public int findMaxLength(int[] nums) {
        Map<Integer, Integer> d = new HashMap<>();
        d.put(0, -1);
        int ans = 0, s = 0;
        for (int i = 0; i < nums.length; ++i) {
            s += nums[i] == 1 ? 1 : -1;
            if (d.containsKey(s)) {
                ans = Math.max(ans, i - d.get(s));
            } else {
                d.put(s, i);
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
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> d{{0, -1}};
        int ans = 0, s = 0;
        for (int i = 0; i < nums.size(); ++i) {
            s += nums[i] ? 1 : -1;
            if (d.contains(s)) {
                ans = max(ans, i - d[s]);
            } else {
                d[s] = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMaxLength(nums []int) int {
	d := map[int]int{0: -1}
	ans, s := 0, 0
	for i, x := range nums {
		if x == 0 {
			x = -1
		}
		s += x
		if j, ok := d[s]; ok {
			ans = max(ans, i-j)
		} else {
			d[s] = i
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findMaxLength(nums: number[]): number {
    const d: Record<number, number> = { 0: -1 };
    let ans = 0;
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i] ? 1 : -1;
        if (d.hasOwnProperty(s)) {
            ans = Math.max(ans, i - d[s]);
        } else {
            d[s] = i;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxLength = function (nums) {
    const d = { 0: -1 };
    let ans = 0;
    let s = 0;
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i] ? 1 : -1;
        if (d.hasOwnProperty(s)) {
            ans = Math.max(ans, i - d[s]);
        } else {
            d[s] = i;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
