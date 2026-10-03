---
comments: true
difficulty: Hard
tags:
    - Stack
    - Array
    - Dynamic Programming
    - Monotonic Stack
---

<!-- problem:start -->

# [2355. Maximum Number of Books You Can Take 🔒](https://leetcode.com/problems/maximum-number-of-books-you-can-take)

[中文文档](/solution/2300-2399/2355.Maximum%20Number%20of%20Books%20You%20Can%20Take/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>books</code> có chỉ số bắt đầu từ <strong>0</strong>, với độ dài <code>n</code>, trong đó <code>books[i]</code> biểu thị số sách trên kệ thứ <code>i<sup>th</sup></code> của một giá sách.</p>

<p>Bạn sẽ lấy sách từ một đoạn <strong>liên tiếp</strong> của giá sách, trải dài từ <code>l</code> đến <code>r</code>, trong đó <code>0 &lt;= l &lt;= r &lt; n</code>. Với mỗi chỉ số <code>i</code> trong phạm vi <code>l &lt;= i &lt; r</code>, bạn phải lấy số sách từ kệ <code>i</code> <strong>ít hơn nghiêm ngặt</strong> số sách lấy từ kệ <code>i + 1</code>.</p>

<p>Trả về <em><strong>số sách tối đa</strong> bạn có thể lấy từ giá sách.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> books = [8,5,2,7,9]
<strong>Đầu ra:</strong> 19
<strong>Giải thích:</strong>
- Lấy 1 quyển sách từ kệ 1.
- Lấy 2 quyển sách từ kệ 2.
- Lấy 7 quyển sách từ kệ 3.
- Lấy 9 quyển sách từ kệ 4.
Bạn đã lấy 19 quyển sách, vì vậy trả về 19.
Có thể chứng minh rằng 19 là số sách tối đa bạn có thể lấy.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> books = [7,0,3,4,5]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
- Lấy 3 quyển sách từ kệ 2.
- Lấy 4 quyển sách từ kệ 3.
- Lấy 5 quyển sách từ kệ 4.
Bạn đã lấy 12 quyển sách, vì vậy trả về 12.
Có thể chứng minh rằng 12 là số sách tối đa bạn có thể lấy.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> books = [8,2,3,7,3,4,0,1,4,3]
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong>
- Lấy 1 quyển sách từ kệ 0.
- Lấy 2 quyển sách từ kệ 1.
- Lấy 3 quyển sách từ kệ 2.
- Lấy 7 quyển sách từ kệ 3.
Bạn đã lấy 13 quyển sách, vì vậy trả về 13.
Có thể chứng minh rằng 13 là số sách tối đa bạn có thể lấy.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= books.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= books[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Sách được lấy từ một đoạn kệ liên tiếp, với số lượng giảm ít nhất một đơn vị khi đi về bên trái. Việc tìm điểm ngắt bằng brute force cho mọi điểm kết thúc bên phải là quá chậm.
>
> Đặt $nums[i]=books[i]-i$. Điểm ngắt là $nums[j]$ nhỏ hơn gần nhất ở bên trái. Monotonic stack điền $left[i]$; $dp[i]$ là số sách tốt nhất khi kết thúc tại $i$: một đoạn cấp số cộng ở giữa cộng với $dp[j]$ ở bên trái.

<!-- thinking:end -->

Chúng ta trực tiếp so sánh từng hàng và cột của ma trận $grid$. Nếu chúng bằng nhau, đó là một cặp hàng-cột bằng nhau, và ta tăng đáp án lên một.

Độ phức tạp thời gian là $O(n^3)$, trong đó $n$ là số hàng hoặc số cột của ma trận $grid$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBooks(self, books: List[int]) -> int:
        nums = [v - i for i, v in enumerate(books)]
        n = len(nums)
        left = [-1] * n
        stk = []
        for i, v in enumerate(nums):
            while stk and nums[stk[-1]] >= v:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        ans = 0
        dp = [0] * n
        dp[0] = books[0]
        for i, v in enumerate(books):
            j = left[i]
            cnt = min(v, i - j)
            u = v - cnt + 1
            s = (u + v) * cnt // 2
            dp[i] = s + (0 if j == -1 else dp[j])
            ans = max(ans, dp[i])
        return ans
```

#### Java

```java
class Solution {
    public long maximumBooks(int[] books) {
        int n = books.length;
        int[] nums = new int[n];
        for (int i = 0; i < n; ++i) {
            nums[i] = books[i] - i;
        }
        int[] left = new int[n];
        Arrays.fill(left, -1);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        long ans = 0;
        long[] dp = new long[n];
        dp[0] = books[0];
        for (int i = 0; i < n; ++i) {
            int j = left[i];
            int v = books[i];
            int cnt = Math.min(v, i - j);
            int u = v - cnt + 1;
            long s = (long) (u + v) * cnt / 2;
            dp[i] = s + (j == -1 ? 0 : dp[j]);
            ans = Math.max(ans, dp[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

class Solution {
public:
    long long maximumBooks(vector<int>& books) {
        int n = books.size();
        vector<int> nums(n);
        for (int i = 0; i < n; ++i) nums[i] = books[i] - i;
        vector<int> left(n, -1);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) stk.pop();
            if (!stk.empty()) left[i] = stk.top();
            stk.push(i);
        }
        vector<ll> dp(n);
        dp[0] = books[0];
        ll ans = 0;
        for (int i = 0; i < n; ++i) {
            int v = books[i];
            int j = left[i];
            int cnt = min(v, i - j);
            int u = v - cnt + 1;
            ll s = 1ll * (u + v) * cnt / 2;
            dp[i] = s + (j == -1 ? 0 : dp[j]);
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBooks(books []int) int64 {
	n := len(books)
	nums := make([]int, n)
	left := make([]int, n)
	for i, v := range books {
		nums[i] = v - i
		left[i] = -1
	}
	stk := []int{}
	for i, v := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= v {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	dp := make([]int, n)
	dp[0] = books[0]
	ans := 0
	for i, v := range books {
		j := left[i]
		cnt := min(v, i-j)
		u := v - cnt + 1
		s := (u + v) * cnt / 2
		dp[i] = s
		if j != -1 {
			dp[i] += dp[j]
		}
		ans = max(ans, dp[i])
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
