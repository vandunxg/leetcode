---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Divide and Conquer
---

<!-- problem:start -->

# [932. Beautiful Array](https://leetcode.com/problems/beautiful-array)

[中文文档](/solution/0900-0999/0932.Beautiful%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng <code>nums</code> có độ dài <code>n</code> được gọi là <strong>đẹp</strong> nếu:</p>

<ul>
	<li><code>nums</code> là một hoán vị của các số nguyên trong đoạn <code>[1, n]</code>.</li>
	<li>Với mọi <code>0 &lt;= i &lt; j &lt; n</code>, không tồn tại chỉ số <code>k</code> thỏa <code>i &lt; k &lt; j</code> và <code>2 * nums[k] == nums[i] + nums[j]</code>.</li>
</ul>

<p>Cho số nguyên <code>n</code>, hãy trả về <em>một mảng </em><code>nums</code><em> đẹp bất kỳ có độ dài </em><code>n</code>. Với <code>n</code> đã cho, luôn tồn tại ít nhất một đáp án hợp lệ.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> n = 4
<strong>Đầu ra:</strong> [2,1,4,3]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> n = 5
<strong>Đầu ra:</strong> [3,1,2,5,4]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng đẹp không cho phép $2A_k=A_i+A_j$. Vế trái là số chẵn, nên nếu $A_i,A_j$ gồm một số lẻ và một số chẵn thì điều kiện này luôn được đảm bảo. Biến đổi một mảng đẹp nhỏ hơn bằng phép ánh xạ affine để đưa các phần tử vào nhóm số lẻ và nhóm số chẵn, rồi nối hai nhóm lại. Phép biến đổi này bảo toàn tính chất đẹp; áp dụng chia để trị sẽ tạo được các số từ $1$ đến $n$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def beautifulArray(self, n: int) -> List[int]:
        if n == 1:
            return [1]
        left = self.beautifulArray((n + 1) >> 1)
        right = self.beautifulArray(n >> 1)
        left = [x * 2 - 1 for x in left]
        right = [x * 2 for x in right]
        return left + right
```

#### Java

```java
class Solution {
    public int[] beautifulArray(int n) {
        if (n == 1) {
            return new int[] {1};
        }
        int[] left = beautifulArray((n + 1) >> 1);
        int[] right = beautifulArray(n >> 1);
        int[] ans = new int[n];
        int i = 0;
        for (int x : left) {
            ans[i++] = x * 2 - 1;
        }
        for (int x : right) {
            ans[i++] = x * 2;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> beautifulArray(int n) {
        if (n == 1) return {1};
        vector<int> left = beautifulArray((n + 1) >> 1);
        vector<int> right = beautifulArray(n >> 1);
        vector<int> ans(n);
        int i = 0;
        for (int& x : left) ans[i++] = x * 2 - 1;
        for (int& x : right) ans[i++] = x * 2;
        return ans;
    }
};
```

#### Go

```go
func beautifulArray(n int) []int {
	if n == 1 {
		return []int{1}
	}
	left := beautifulArray((n + 1) >> 1)
	right := beautifulArray(n >> 1)
	var ans []int
	for _, x := range left {
		ans = append(ans, x*2-1)
	}
	for _, x := range right {
		ans = append(ans, x*2)
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
