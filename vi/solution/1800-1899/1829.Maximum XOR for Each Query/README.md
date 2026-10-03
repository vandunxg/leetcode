---
comments: true
difficulty: Medium
rating: 1523
source: Biweekly Contest 50 Q3
tags:
    - Bit Manipulation
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1829. Maximum XOR for Each Query](https://leetcode.com/problems/maximum-xor-for-each-query)

[中文文档](/solution/1800-1899/1829.Maximum%20XOR%20for%20Each%20Query/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm <code>n</code> số nguyên không âm đã được <strong>sắp xếp</strong> và một số nguyên <code>maximumBit</code>. Bạn cần thực hiện query sau <code>n</code> <strong>lần</strong>:</p>

<ol>
<li>Tìm số nguyên không âm <code>k &lt; 2<sup>maximumBit</sup></code> sao cho <code>nums[0] XOR nums[1] XOR ... XOR nums[nums.length-1] XOR k</code> đạt giá trị <strong>lớn nhất</strong>. <code>k</code> là đáp án của query thứ <code>i<sup>th</sup></code>.</li>
<li>Xóa phần tử <strong>cuối cùng</strong> khỏi mảng hiện tại <code>nums</code>.</li>
</ol>

<p>Trả về <em>một mảng</em> <code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là đáp án của </em><code>i<sup>th</sup></code><em> query</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1,3], maximumBit = 2
<strong>Đầu ra:</strong> [0,3,2,3]
<strong>Giải thích</strong>: Các query được trả lời như sau:
Query thứ <sup>1st</sup>: nums = [0,1,1,3], k = 0 vì 0 XOR 1 XOR 1 XOR 3 XOR 0 = 3.
Query thứ <sup>2nd</sup>: nums = [0,1,1], k = 3 vì 0 XOR 1 XOR 1 XOR 3 = 3.
Query thứ <sup>3rd</sup>: nums = [0,1], k = 2 vì 0 XOR 1 XOR 2 = 3.
Query thứ <sup>4th</sup>: nums = [0], k = 3 vì 0 XOR 3 = 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,4,7], maximumBit = 3
<strong>Đầu ra:</strong> [5,2,6,5]
<strong>Giải thích</strong>: Các query được trả lời như sau:
Query thứ <sup>1st</sup>: nums = [2,3,4,7], k = 5 vì 2 XOR 3 XOR 4 XOR 7 XOR 5 = 7.
Query thứ <sup>2nd</sup>: nums = [2,3,4], k = 2 vì 2 XOR 3 XOR 4 XOR 2 = 7.
Query thứ <sup>3rd</sup>: nums = [2,3], k = 6 vì 2 XOR 3 XOR 6 = 7.
Query thứ <sup>4th</sup>: nums = [2], k = 5 vì 2 XOR 5 = 7.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,2,2,5,7], maximumBit = 3
<strong>Đầu ra:</strong> [4,3,6,4,6,7]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == n</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= maximumBit &lt;= 20</code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>maximumBit</sup></code></li>
	<li><code>nums</code>​​​ được sắp xếp theo thứ tự <strong>tăng dần</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi query yêu cầu một $k<2^{\textit{maximumBit}}$ để tối đa hóa XOR với XOR tiền tố hiện tại, sau đó xóa phần tử cuối. Tính lại XOR từ đầu có độ phức tạp $O(n^2)$. Với $n\le 10^5$, cách này sẽ không đủ nhanh.
>
> Tính trước XOR $xs$ của toàn bộ mảng. Khi xóa từ cuối, $k$ cần đảo mọi bit $0$ của $xs$ trong phạm vi bit cho phép. Xây dựng $k$ theo từng bit, rồi loại giá trị vừa xóa khỏi $xs$.

<!-- thinking:end -->

Đầu tiên, ta tính trước tổng XOR $xs$ của mảng `nums`, tức là $xs=nums[0] \oplus nums[1] \oplus \cdots \oplus nums[n-1]$.

Tiếp theo, ta duyệt từng phần tử $x$ trong mảng `nums` từ cuối lên đầu. Tổng XOR hiện tại là $xs$. Ta cần tìm số $k$ sao cho $xs \oplus k$ lớn nhất có thể và $k \lt 2^{maximumBit}$.

Nói cách khác, ta bắt đầu từ bit $maximumBit - 1$ của $xs$ và duyệt xuống các bit thấp hơn. Nếu một bit của $xs$ bằng $0$, ta đặt bit tương ứng của $k$ bằng $1$. Ngược lại, ta đặt bit tương ứng của $k$ bằng $0$. Như vậy, $k$ cuối cùng là đáp án của query hiện tại. Sau đó, cập nhật $xs$ thành $xs \oplus x$ và tiếp tục với phần tử kế tiếp.

Độ phức tạp thời gian là $O(n \times m)$, trong đó $n$ và $m$ lần lượt là độ dài mảng `nums` và giá trị `maximumBit`. Không tính không gian lưu đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaximumXor(self, nums: List[int], maximumBit: int) -> List[int]:
        ans = []
        xs = reduce(xor, nums)
        for x in nums[::-1]:
            k = 0
            for i in range(maximumBit - 1, -1, -1):
                if (xs >> i & 1) == 0:
                    k |= 1 << i
            ans.append(k)
            xs ^= x
        return ans
```

#### Java

```java
class Solution {
    public int[] getMaximumXor(int[] nums, int maximumBit) {
        int n = nums.length;
        int xs = 0;
        for (int x : nums) {
            xs ^= x;
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = 0;
            for (int j = maximumBit - 1; j >= 0; --j) {
                if (((xs >> j) & 1) == 0) {
                    k |= 1 << j;
                }
            }
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) {
        int xs = 0;
        for (int& x : nums) {
            xs ^= x;
        }
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = 0;
            for (int j = maximumBit - 1; ~j; --j) {
                if ((xs >> j & 1) == 0) {
                    k |= 1 << j;
                }
            }
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
};
```

#### Go

```go
func getMaximumXor(nums []int, maximumBit int) (ans []int) {
	xs := 0
	for _, x := range nums {
		xs ^= x
	}
	for i := range nums {
		x := nums[len(nums)-i-1]
		k := 0
		for j := maximumBit - 1; j >= 0; j-- {
			if xs>>j&1 == 0 {
				k |= 1 << j
			}
		}
		ans = append(ans, k)
		xs ^= x
	}
	return
}
```

#### TypeScript

```ts
function getMaximumXor(nums: number[], maximumBit: number): number[] {
    let xs = 0;
    for (const x of nums) {
        xs ^= x;
    }
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const x = nums[n - i - 1];
        let k = 0;
        for (let j = maximumBit - 1; j >= 0; --j) {
            if (((xs >> j) & 1) == 0) {
                k |= 1 << j;
            }
        }
        ans[i] = k;
        xs ^= x;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} maximumBit
 * @return {number[]}
 */
var getMaximumXor = function (nums, maximumBit) {
    let xs = 0;
    for (const x of nums) {
        xs ^= x;
    }
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const x = nums[n - i - 1];
        let k = 0;
        for (let j = maximumBit - 1; j >= 0; --j) {
            if (((xs >> j) & 1) == 0) {
                k |= 1 << j;
            }
        }
        ans[i] = k;
        xs ^= x;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] GetMaximumXor(int[] nums, int maximumBit) {
        int xs = 0;
        foreach (int x in nums) {
            xs ^= x;
        }
        int n = nums.Length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = 0;
            for (int j = maximumBit - 1; j >= 0; --j) {
                if ((xs >> j & 1) == 0) {
                    k |= 1 << j;
                }
            }
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tối ưu phép liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 duyệt qua $maximumBit$ bit. Đáp án tối ưu đặt mọi bit được phép của $xs\oplus k$ thành $1$, nên $k=xs\oplus(2^{\textit{maximumBit}}-1)$. Mỗi query chỉ cần một phép XOR, giúp việc duyệt có thời gian tuyến tính.

<!-- thinking:end -->

Tương tự Lời giải 1, trước tiên ta tính trước tổng XOR $xs$ của mảng `nums`, tức là $xs=nums[0] \oplus nums[1] \oplus \cdots \oplus nums[n-1]$.

Tiếp theo, ta tính $2^{maximumBit} - 1$, tức $2^{maximumBit}$ trừ $1$, ký hiệu là $mask$. Sau đó, ta duyệt từng phần tử $x$ trong mảng `nums` từ cuối lên đầu. Tổng XOR hiện tại là $xs$, khi đó $k=xs \oplus mask$ là đáp án của query hiện tại. Tiếp theo, cập nhật $xs$ thành $xs \oplus x$ và tiếp tục với phần tử kế tiếp.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng `nums`. Không tính không gian lưu đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getMaximumXor(self, nums: List[int], maximumBit: int) -> List[int]:
        ans = []
        xs = reduce(xor, nums)
        mask = (1 << maximumBit) - 1
        for x in nums[::-1]:
            k = xs ^ mask
            ans.append(k)
            xs ^= x
        return ans
```

#### Java

```java
class Solution {
    public int[] getMaximumXor(int[] nums, int maximumBit) {
        int xs = 0;
        for (int x : nums) {
            xs ^= x;
        }
        int mask = (1 << maximumBit) - 1;
        int n = nums.length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = xs ^ mask;
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getMaximumXor(vector<int>& nums, int maximumBit) {
        int xs = 0;
        for (int& x : nums) {
            xs ^= x;
        }
        int mask = (1 << maximumBit) - 1;
        int n = nums.size();
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = xs ^ mask;
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
};
```

#### Go

```go
func getMaximumXor(nums []int, maximumBit int) (ans []int) {
	xs := 0
	for _, x := range nums {
		xs ^= x
	}
	mask := (1 << maximumBit) - 1
	for i := range nums {
		x := nums[len(nums)-i-1]
		k := xs ^ mask
		ans = append(ans, k)
		xs ^= x
	}
	return
}
```

#### TypeScript

```ts
function getMaximumXor(nums: number[], maximumBit: number): number[] {
    let xs = 0;
    for (const x of nums) {
        xs ^= x;
    }
    const mask = (1 << maximumBit) - 1;
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const x = nums[n - i - 1];
        let k = xs ^ mask;
        ans[i] = k;
        xs ^= x;
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} maximumBit
 * @return {number[]}
 */
var getMaximumXor = function (nums, maximumBit) {
    let xs = 0;
    for (const x of nums) {
        xs ^= x;
    }
    const mask = (1 << maximumBit) - 1;
    const n = nums.length;
    const ans = new Array(n);
    for (let i = 0; i < n; ++i) {
        const x = nums[n - i - 1];
        let k = xs ^ mask;
        ans[i] = k;
        xs ^= x;
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int[] GetMaximumXor(int[] nums, int maximumBit) {
        int xs = 0;
        foreach (int x in nums) {
            xs ^= x;
        }
        int mask = (1 << maximumBit) - 1;
        int n = nums.Length;
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            int x = nums[n - i - 1];
            int k = xs ^ mask;
            ans[i] = k;
            xs ^= x;
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
