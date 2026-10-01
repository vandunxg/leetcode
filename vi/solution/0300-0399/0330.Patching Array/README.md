---
comments: true
difficulty: Hard
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [330. Patching Array](https://leetcode.com/problems/patching-array)

[中文文档](/solution/0300-0399/0330.Patching%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên đã sắp xếp <code>nums</code> và số nguyên <code>n</code>. Hãy thêm các phần tử vào mảng sao cho mọi số trong đoạn <code>[1, n]</code> đều có thể tạo thành bằng tổng của một số phần tử trong mảng.</p>

<p>Hãy trả về <em>số phần tử cần thêm ít nhất</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3], n = 6
<strong>Đầu ra:</strong> 1
Giải thích:
Các tổ hợp phần tử của nums là [1], [3], [1,3], tạo được các tổng: 1, 3, 4.
Nếu thêm 2 vào nums, các tổ hợp sẽ là: [1], [2], [3], [1,3], [2,3], [1,2,3].
Các tổng có thể tạo được là 1, 2, 3, 4, 5, 6, nên lúc này đã phủ kín đoạn [1, 6].
Vì vậy, ta chỉ cần thêm 1 phần tử.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,5,10], n = 20
<strong>Đầu ra:</strong> 2
Giải thích: Hai phần tử được thêm có thể là [2, 4].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2], n = 5
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>nums</code> được sắp xếp theo <strong>thứ tự tăng dần</strong>.</li>
	<li><code>1 &lt;= n &lt;= 2<sup>31</sup> - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Cho mảng đã sắp xếp, ta cần thêm ít số dương nhất có thể để mọi số nguyên trong $[1,n]$ đều là tổng của một tập con. Xét từng khoảng một sẽ quá tốn kém.
>
> Giả sử các số trong $[1,x)$ đã tạo được. Nếu phần tử kế tiếp thỏa $nums[i]\le x$, ta dùng nó để mở rộng phạm vi thành $[1,x+nums[i])$; nếu không, thêm chính $x$ để phạm vi được nhân đôi. Thêm số nhỏ hơn sẽ phủ được ít hơn, vì vậy lấp khoảng trống hiện tại là tối ưu. Dừng khi $x>n$.

<!-- thinking:end -->

Giả sử $x$ là số nguyên dương nhỏ nhất chưa thể tạo thành. Khi đó, mọi số trong $[1,..x-1]$ đều tạo được. Để tạo được $x$, ta cần thêm một số nhỏ hơn hoặc bằng $x$:

- Nếu số được thêm bằng $x$, vì mọi số trong $[1,..x-1]$ đều tạo được nên sau khi thêm $x$, mọi số trong đoạn $[1,..2x-1]$ cũng tạo được; số nguyên dương nhỏ nhất chưa tạo được lúc này là $2x$.
- Nếu số được thêm nhỏ hơn $x$, gọi số đó là $x'$. Vì mọi số trong $[1,..x-1]$ đều tạo được nên sau khi thêm $x'$, mọi số trong đoạn $[1,..x+x'-1]$ cũng tạo được; số nguyên dương nhỏ nhất chưa tạo được lúc này là $x+x' \lt 2x$.

Vì vậy, ta nên thêm tham lam số $x$ để phủ được phạm vi lớn hơn.

Ta dùng biến $x$ để lưu số nguyên dương nhỏ nhất hiện chưa tạo được, khởi tạo bằng $1$. Khi đó, đoạn $[1,..x-1]$ rỗng, nghĩa là chưa phủ được số nào. Biến $i$ lưu chỉ số hiện tại trong mảng đang duyệt.

Ta lặp lại các thao tác sau:

- Nếu $i$ còn nằm trong phạm vi mảng và $nums[i] \le x$, phần tử hiện tại có thể được dùng để mở rộng phạm vi đã phủ; ta cộng $nums[i]$ vào $x$ rồi tăng $i$ lên $1$.
- Nếu không, nghĩa là chưa thể tạo được $x$, nên ta thêm $x$ vào mảng rồi cập nhật $x$ thành $2x$.
- Lặp lại các thao tác trên cho đến khi $x$ lớn hơn $n$.

Đáp án cuối cùng là số phần tử đã thêm.

Độ phức tạp thời gian là $O(m + \log n)$, trong đó $m$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPatches(self, nums: List[int], n: int) -> int:
        x = 1
        ans = i = 0
        while x <= n:
            if i < len(nums) and nums[i] <= x:
                x += nums[i]
                i += 1
            else:
                ans += 1
                x <<= 1
        return ans
```

#### Java

```java
class Solution {
    public int minPatches(int[] nums, int n) {
        long x = 1;
        int ans = 0;
        for (int i = 0; x <= n;) {
            if (i < nums.length && nums[i] <= x) {
                x += nums[i++];
            } else {
                ++ans;
                x <<= 1;
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
    int minPatches(vector<int>& nums, int n) {
        long long x = 1;
        int ans = 0;
        for (int i = 0; x <= n;) {
            if (i < nums.size() && nums[i] <= x) {
                x += nums[i++];
            } else {
                ++ans;
                x <<= 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minPatches(nums []int, n int) (ans int) {
	x := 1
	for i := 0; x <= n; {
		if i < len(nums) && nums[i] <= x {
			x += nums[i]
			i++
		} else {
			ans++
			x <<= 1
		}
	}
	return
}
```

#### TypeScript

```ts
function minPatches(nums: number[], n: number): number {
    let x = 1;
    let ans = 0;
    for (let i = 0; x <= n;) {
        if (i < nums.length && nums[i] <= x) {
            x += nums[i++];
        } else {
            ++ans;
            x *= 2;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
