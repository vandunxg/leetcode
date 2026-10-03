---
comments: true
difficulty: Medium
rating: 1334
source: Biweekly Contest 78 Q2
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2270. Number of Ways to Split Array](https://leetcode.com/problems/number-of-ways-to-split-array)

[中文文档](/solution/2200-2299/2270.Number%20of%20Ways%20to%20Split%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có <strong>chỉ số bắt đầu từ 0</strong>, có độ dài <code>n</code>.</p>

<p><code>nums</code> có một <strong>cách chia hợp lệ</strong> tại chỉ số <code>i</code> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li>Tổng của <code>i + 1</code> phần tử đầu tiên <strong>lớn hơn hoặc bằng</strong> tổng của <code>n - i - 1</code> phần tử cuối cùng.</li>
	<li>Có <strong>ít nhất một</strong> phần tử ở bên phải <code>i</code>. Nghĩa là <code>0 &lt;= i &lt; n - 1</code>.</li>
</ul>

<p>Hãy trả về <em>số lượng <strong>cách chia hợp lệ</strong> trong</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,4,-8,7]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Có ba cách chia nums thành hai phần không rỗng:
- Chia nums tại chỉ số 0. Khi đó, phần thứ nhất là [10], có tổng là 10. Phần thứ hai là [4,-8,7], có tổng là 3. Vì 10 &gt;= 3, i = 0 là một cách chia hợp lệ.
- Chia nums tại chỉ số 1. Khi đó, phần thứ nhất là [10,4], có tổng là 14. Phần thứ hai là [-8,7], có tổng là -1. Vì 14 &gt;= -1, i = 1 là một cách chia hợp lệ.
- Chia nums tại chỉ số 2. Khi đó, phần thứ nhất là [10,4,-8], có tổng là 6. Phần thứ hai là [7], có tổng là 7. Vì 6 &lt; 7, i = 2 không phải là một cách chia hợp lệ.
Vì vậy, số lượng cách chia hợp lệ trong nums là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Có hai cách chia hợp lệ trong nums:
- Chia nums tại chỉ số 1. Khi đó, phần thứ nhất là [2,3], có tổng là 5. Phần thứ hai là [1,0], có tổng là 1. Vì 5 &gt;= 1, i = 1 là một cách chia hợp lệ.
- Chia nums tại chỉ số 2. Khi đó, phần thứ nhất là [2,3,1], có tổng là 6. Phần thứ hai là [0], có tổng là 0. Vì 6 &gt;= 0, i = 2 là một cách chia hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Một cách chia hợp lệ cần tổng bên trái lớn hơn hoặc bằng tổng bên phải, đồng thời phần bên phải không được rỗng. Với $n \le 10^5$, ta không thể tính tổng lại từ đầu cho mỗi cách chia. Tổng của hai phần bằng tổng $s$ của cả mảng, nên điều kiện kiểm tra là tổng tiền tố $t \ge s-t$.
>
> Ta tính $s$, sau đó duyệt qua $n-1$ phần tử đầu tiên, cộng dồn $t$ và đếm số cách chia hợp lệ.

<!-- thinking:end -->

Trước tiên, ta tính tổng $s$ của toàn bộ mảng $\textit{nums}$. Sau đó, ta duyệt qua $n-1$ phần tử đầu tiên của mảng $\textit{nums}$, sử dụng biến $t$ để lưu tổng tiền tố. Nếu $t \geq s - t$, ta tăng đáp án lên một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def waysToSplitArray(self, nums: List[int]) -> int:
        s = sum(nums)
        ans = t = 0
        for x in nums[:-1]:
            t += x
            ans += t >= s - t
        return ans
```

#### Java

```java
class Solution {
    public int waysToSplitArray(int[] nums) {
        long s = 0;
        for (int x : nums) {
            s += x;
        }
        long t = 0;
        int ans = 0;
        for (int i = 0; i + 1 < nums.length; ++i) {
            t += nums[i];
            ans += t >= s - t ? 1 : 0;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int waysToSplitArray(vector<int>& nums) {
        long long s = accumulate(nums.begin(), nums.end(), 0LL);
        long long t = 0;
        int ans = 0;
        for (int i = 0; i + 1 < nums.size(); ++i) {
            t += nums[i];
            ans += t >= s - t;
        }
        return ans;
    }
};
```

#### Go

```go
func waysToSplitArray(nums []int) (ans int) {
	var s, t int
	for _, x := range nums {
		s += x
	}
	for _, x := range nums[:len(nums)-1] {
		t += x
		if t >= s-t {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function waysToSplitArray(nums: number[]): number {
    const s = nums.reduce((acc, cur) => acc + cur, 0);
    let [ans, t] = [0, 0];
    for (const x of nums.slice(0, -1)) {
        t += x;
        if (t >= s - t) {
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
