---
comments: true
difficulty: Hard
rating: 2943
source: Biweekly Contest 118 Q4
tags:
    - Stack
    - Queue
    - Array
    - Binary Search
    - Dynamic Programming
    - Prefix Sum
    - Monotonic Queue
    - Monotonic Stack
---

<!-- problem:start -->

# [2945. Find Maximum Non-decreasing Array Length](https://leetcode.com/problems/find-maximum-non-decreasing-array-length)

[Tài liệu tiếng Trung](/solution/2900-2999/2945.Find%20Maximum%20Non-decreasing%20Array%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>.</p>

<p>Bạn có thể thực hiện bất kỳ số lượng thao tác nào, trong đó mỗi thao tác chọn một <strong>mảng con</strong> rồi thay thế nó bằng <strong>tổng</strong> các phần tử của nó. Ví dụ, nếu mảng ban đầu là <code>[1,3,5,6]</code> và bạn chọn mảng con <code>[3,5]</code>, mảng sẽ trở thành <code>[1,8,6]</code>.</p>

<p>Hãy trả về <em>độ dài </em><strong><em>lớn nhất</em></strong><em> của một mảng </em><strong><em>không giảm</em></strong><em> có thể tạo ra sau khi thực hiện các thao tác.</em></p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp <strong>không rỗng</strong> trong một mảng.</p>

<p>&nbsp;</p>
<p><strong>Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,2,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mảng có độ dài 3 này không phải là mảng không giảm.
Có hai cách để đưa độ dài mảng về hai.
Đầu tiên, chọn mảng con [2,2] để biến mảng thành [5,4].
Thứ hai, chọn mảng con [5,2] để biến mảng thành [7,2].
Trong cả hai cách này, mảng đều không phải là mảng không giảm.
Nếu chọn mảng con [5,2,2] và thay nó bằng [9], mảng sẽ trở thành mảng không giảm.
Vì vậy, đáp án là 1.
</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Mảng đã cho là mảng không giảm. Vì vậy, đáp án là 4.
</pre>

<p><strong>Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,6]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Thay [3,2] bằng [5] sẽ biến mảng đã cho thành [4,5,6], là một mảng không giảm.
Vì mảng đã cho không phải là mảng không giảm, đáp án lớn nhất<!-- notionvc: 3447a505-d1ee-4411-8cae-e52162f53a55 --> có thể là 3.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị kề nhau có thể được gộp thành tổng của chúng; mảng cuối cùng phải không giảm và có độ dài lớn nhất có thể. Với $n \le 10^5$, không thể thử mọi cách phân hoạch. Gọi $f[i]$ là số phần nhiều nhất của tiền tố có độ dài $i$, đồng thời giữ phần cuối đủ nhỏ để các phần phía sau có thể nối tiếp; $pre$ lưu thông tin về phần cuối đó.
>
> Trên các tổng tiền tố $s$, phần tiếp theo phải lớn hơn hoặc bằng phần trước, nên ta tìm kiếm nhị phân $j$ nhỏ nhất sao cho $s[j]-s[i] \ge s[i]-s[pre[i]]$. Lấy giá trị lớn nhất tiền tố của $pre$ giúp kế thừa điểm bắt đầu tốt hơn. Một lần duyệt kết hợp tìm kiếm nhị phân cho ra $f[n]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaximumLength(self, nums: List[int]) -> int:
        n = len(nums)
        s = list(accumulate(nums, initial=0))
        f = [0] * (n + 1)
        pre = [0] * (n + 2)
        for i in range(1, n + 1):
            pre[i] = max(pre[i], pre[i - 1])
            f[i] = f[pre[i]] + 1
            j = bisect_left(s, s[i] * 2 - s[pre[i]])
            pre[j] = i
        return f[n]
```

#### Java

```java
class Solution {
    public int findMaximumLength(int[] nums) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int[] f = new int[n + 1];
        int[] pre = new int[n + 2];
        for (int i = 1; i <= n; ++i) {
            pre[i] = Math.max(pre[i], pre[i - 1]);
            f[i] = f[pre[i]] + 1;
            int j = Arrays.binarySearch(s, s[i] * 2 - s[pre[i]]);
            pre[j < 0 ? -j - 1 : j] = i;
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMaximumLength(vector<int>& nums) {
        int n = nums.size();
        int f[n + 1];
        int pre[n + 2];
        long long s[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        memset(f, 0, sizeof(f));
        memset(pre, 0, sizeof(pre));
        for (int i = 1; i <= n; ++i) {
            pre[i] = max(pre[i], pre[i - 1]);
            f[i] = f[pre[i]] + 1;
            int j = lower_bound(s, s + n + 1, s[i] * 2 - s[pre[i]]) - s;
            pre[j] = i;
        }
        return f[n];
    }
};
```

#### Go

```go
func findMaximumLength(nums []int) int {
	n := len(nums)
	f := make([]int, n+1)
	pre := make([]int, n+2)
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	for i := 1; i <= n; i++ {
		pre[i] = max(pre[i], pre[i-1])
		f[i] = f[pre[i]] + 1
		j := sort.SearchInts(s, s[i]*2-s[pre[i]])
		pre[j] = max(pre[j], i)
	}
	return f[n]
}
```

#### TypeScript

```ts
function findMaximumLength(nums: number[]): number {
    const n = nums.length;
    const f: number[] = Array(n + 1).fill(0);
    const pre: number[] = Array(n + 2).fill(0);
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        s[i] = s[i - 1] + nums[i - 1];
    }
    const search = (nums: number[], x: number): number => {
        let [l, r] = [0, nums.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (let i = 1; i <= n; ++i) {
        pre[i] = Math.max(pre[i], pre[i - 1]);
        f[i] = f[pre[i]] + 1;
        const j = search(s, s[i] * 2 - s[pre[i]]);
        pre[j] = i;
    }
    return f[n];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
