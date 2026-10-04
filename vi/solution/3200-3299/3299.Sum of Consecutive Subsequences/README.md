---
comments: true
difficulty: Hard
tags:
    - Array
    - Hash Table
    - Dynamic Programming
---

<!-- problem:start -->

# [3299. Sum of Consecutive Subsequences 🔒](https://leetcode.com/problems/sum-of-consecutive-subsequences)

[中文文档](/solution/3200-3299/3299.Sum%20of%20Consecutive%20Subsequences/README.md)

## Mô tả

<!-- description:start -->

<p>Ta gọi một mảng <code>arr</code> có độ dài <code>n</code> là <strong>liên tiếp</strong> nếu thỏa mãn một trong các điều kiện sau:</p>

<ul>
	<li><code>arr[i] - arr[i - 1] == 1</code> với <em>mọi</em> <code>1 &lt;= i &lt; n</code>.</li>
	<li><code>arr[i] - arr[i - 1] == -1</code> với <em>mọi</em> <code>1 &lt;= i &lt; n</code>.</li>
</ul>

<p><strong>Giá trị</strong> của một mảng là tổng các phần tử của nó.</p>

<p>Ví dụ, <code>[3, 4, 5]</code> là một mảng liên tiếp có giá trị bằng 12 và <code>[9, 8]</code> cũng là một mảng có giá trị bằng 17. Trong khi đó, <code>[3, 4, 3]</code> và <code>[8, 6]</code> không phải là mảng liên tiếp.</p>

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <em>tổng</em> <strong>giá trị</strong> của tất cả <strong>các </strong><em>dãy con liên tiếp, không rỗng</em> <span data-keyword="subsequence-array">này</span>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án <strong>modulo</strong> <code>10<sup>9 </sup>+ 7.</code></p>

<p><strong>Lưu ý</strong> rằng một mảng có độ dài 1 cũng được xem là liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con liên tiếp là: <code>[1]</code>, <code>[2]</code>, <code>[1, 2]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">31</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các dãy con liên tiếp là: <code>[1]</code>, <code>[4]</code>, <code>[2]</code>, <code>[3]</code>, <code>[1, 2]</code>, <code>[2, 3]</code>, <code>[4, 3]</code>, <code>[1, 2, 3]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê đóng góp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi dãy con liên tiếp thay đổi $\pm 1$ ở mỗi bước (các chỉ số không nhất thiết phải kề nhau). Ta cần tính đóng góp của các dãy có độ dài ít nhất $2$ cùng với mọi dãy chỉ có một phần tử. Với $n\le 10^5$, không thể liệt kê các dãy con. Đóng góp của một phần tử là số chuỗi tăng hoặc giảm chứa phần tử đó.
>
> Với các chuỗi tăng, ta nhân số chuỗi kết thúc tại $x-1$ ở bên trái với số chuỗi bắt đầu tại $x+1$ ở bên phải, rồi cộng các phần mở rộng một phía. Hai lượt duyệt bằng hash map sẽ điền $left$ và $right$; sau đó cộng $(l+r+lr)\times x$. Đảo ngược mảng để xử lý các chuỗi giảm, rồi cộng tổng của tất cả phần tử.

<!-- thinking:end -->

Ta hãy đếm số lần mỗi phần tử $\textit{nums}[i]$ xuất hiện trong một dãy con liên tiếp có độ dài lớn hơn 1. Sau đó, nhân số lần này với $\textit{nums}[i]$ sẽ cho đóng góp của $\textit{nums}[i]$ trong tất cả các dãy con liên tiếp có độ dài lớn hơn 1. Cộng các đóng góp này với tổng của tất cả phần tử, ta thu được đáp án.

Trước tiên, ta có thể tính đóng góp của các dãy con tăng nghiêm ngặt, sau đó tính đóng góp của các dãy con giảm nghiêm ngặt, cuối cùng cộng tổng của tất cả phần tử.

Để thực hiện, ta định nghĩa hàm $\textit{calc}(\textit{nums})$, trong đó $\textit{nums}$ là một mảng. Hàm này trả về tổng của tất cả các dãy con liên tiếp có độ dài lớn hơn 1 trong $\textit{nums}$.

Trong hàm này, ta sử dụng hai mảng $\textit{left}$ và $\textit{right}$ để ghi nhận số dãy con tăng nghiêm ngặt kết thúc tại $\textit{nums}[i] - 1$ ở bên trái mỗi phần tử $\textit{nums}[i]$, và số dãy con tăng nghiêm ngặt bắt đầu tại $\textit{nums}[i] + 1$ ở bên phải mỗi phần tử $\textit{nums}[i]$. Nhờ đó, ta có thể tính đóng góp của $\textit{nums}$ trong tất cả các dãy con liên tiếp có độ dài lớn hơn 1 với độ phức tạp thời gian $O(n)$.

Trong hàm chính, trước tiên ta gọi $\textit{calc}(\textit{nums})$ để tính đóng góp của các dãy con tăng nghiêm ngặt, sau đó đảo ngược $\textit{nums}$ và gọi lại $\textit{calc}(\textit{nums})$ để tính đóng góp của các dãy con giảm nghiêm ngặt. Cuối cùng, cộng tổng của tất cả phần tử để thu được đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getSum(self, nums: List[int]) -> int:
        def calc(nums: List[int]) -> int:
            n = len(nums)
            left = [0] * n
            right = [0] * n
            cnt = Counter()
            for i in range(1, n):
                cnt[nums[i - 1]] += 1 + cnt[nums[i - 1] - 1]
                left[i] = cnt[nums[i] - 1]
            cnt = Counter()
            for i in range(n - 2, -1, -1):
                cnt[nums[i + 1]] += 1 + cnt[nums[i + 1] + 1]
                right[i] = cnt[nums[i] + 1]
            return sum((l + r + l * r) * x for l, r, x in zip(left, right, nums)) % mod

        mod = 10**9 + 7
        x = calc(nums)
        nums.reverse()
        y = calc(nums)
        return (x + y + sum(nums)) % mod
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int getSum(int[] nums) {
        long x = calc(nums);
        for (int i = 0, j = nums.length - 1; i < j; ++i, --j) {
            int t = nums[i];
            nums[i] = nums[j];
            nums[j] = t;
        }
        long y = calc(nums);
        long s = Arrays.stream(nums).asLongStream().sum();
        return (int) ((x + y + s) % mod);
    }

    private long calc(int[] nums) {
        int n = nums.length;
        long[] left = new long[n];
        long[] right = new long[n];
        Map<Integer, Long> cnt = new HashMap<>();
        for (int i = 1; i < n; ++i) {
            cnt.merge(nums[i - 1], 1 + cnt.getOrDefault(nums[i - 1] - 1, 0L), Long::sum);
            left[i] = cnt.getOrDefault(nums[i] - 1, 0L);
        }
        cnt.clear();
        for (int i = n - 2; i >= 0; --i) {
            cnt.merge(nums[i + 1], 1 + cnt.getOrDefault(nums[i + 1] + 1, 0L), Long::sum);
            right[i] = cnt.getOrDefault(nums[i] + 1, 0L);
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = (ans + (left[i] + right[i] + left[i] * right[i] % mod) * nums[i] % mod) % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getSum(vector<int>& nums) {
        using ll = long long;
        const int mod = 1e9 + 7;
        auto calc = [&](const vector<int>& nums) -> ll {
            int n = nums.size();
            vector<ll> left(n), right(n);
            unordered_map<int, ll> cnt;

            for (int i = 1; i < n; ++i) {
                cnt[nums[i - 1]] += 1 + cnt[nums[i - 1] - 1];
                left[i] = cnt[nums[i] - 1];
            }

            cnt.clear();

            for (int i = n - 2; i >= 0; --i) {
                cnt[nums[i + 1]] += 1 + cnt[nums[i + 1] + 1];
                right[i] = cnt[nums[i] + 1];
            }

            ll ans = 0;
            for (int i = 0; i < n; ++i) {
                ans = (ans + (left[i] + right[i] + left[i] * right[i] % mod) * nums[i] % mod) % mod;
            }
            return ans;
        };

        ll x = calc(nums);
        reverse(nums.begin(), nums.end());
        ll y = calc(nums);
        ll s = accumulate(nums.begin(), nums.end(), 0LL);
        return static_cast<int>((x + y + s) % mod);
    }
};
```

#### Go

```go
func getSum(nums []int) int {
	const mod = 1e9 + 7

	calc := func(nums []int) int64 {
		n := len(nums)
		left := make([]int64, n)
		right := make([]int64, n)
		cnt := make(map[int]int64)

		for i := 1; i < n; i++ {
			cnt[nums[i-1]] += 1 + cnt[nums[i-1]-1]
			left[i] = cnt[nums[i]-1]
		}

		cnt = make(map[int]int64)

		for i := n - 2; i >= 0; i-- {
			cnt[nums[i+1]] += 1 + cnt[nums[i+1]+1]
			right[i] = cnt[nums[i]+1]
		}

		var ans int64
		for i, x := range nums {
			ans = (ans + (left[i]+right[i]+(left[i]*right[i]%mod))*int64(x)%mod) % mod
		}
		return ans
	}

	x := calc(nums)
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
	y := calc(nums)
	s := int64(0)
	for _, num := range nums {
		s += int64(num)
	}
	return int((x + y + s) % mod)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
