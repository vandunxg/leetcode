---
comments: true
difficulty: Medium
rating: 2051
source: Weekly Contest 240 Q3
tags:
    - Stack
    - Array
    - Cartesian Tree
    - Prefix Sum
    - Monotonic Stack
---

<!-- problem:start -->

# [1856. Maximum Subarray Min-Product](https://leetcode.com/problems/maximum-subarray-min-product)

[中文文档](/solution/1800-1899/1856.Maximum%20Subarray%20Min-Product/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Min-product</strong> của một mảng bằng <strong>giá trị nhỏ nhất</strong> trong mảng <strong>nhân với</strong> <strong>tổng</strong> của mảng.</p>

<ul>
	<li>Ví dụ, mảng <code>[3,2,5]</code> (giá trị nhỏ nhất là <code>2</code>) có min-product là <code>2 * (3+2+5) = 2 * 10 = 20</code>.</li>
</ul>

<p>Cho một mảng số nguyên <code>nums</code>, trả về <em><strong>min-product lớn nhất</strong> của một <strong>mảng con không rỗng</strong> bất kỳ của </em><code>nums</code>. Vì đáp án có thể lớn, hãy trả về nó theo <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>Lưu ý rằng phải tối đa hóa min-product <strong>trước</strong> khi thực hiện phép modulo. Các bộ kiểm thử được tạo sao cho min-product lớn nhất <strong>không</strong> lấy modulo vẫn vừa với một <strong>số nguyên có dấu 64 bit</strong>.</p>

<p><strong>Mảng con</strong> là một phần <strong>liên tiếp</strong> của một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,<u>2,3,2</u>]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Min-product lớn nhất đạt được với mảng con [2,3,2] (giá trị nhỏ nhất là 2).
2 * (2+3+2) = 2 * 7 = 14.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,<u>3,3</u>,1,2]
<strong>Đầu ra:</strong> 18
<strong>Giải thích:</strong> Min-product lớn nhất đạt được với mảng con [3,3] (giá trị nhỏ nhất là 3).
3 * (3+3) = 3 * 6 = 18.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,<u>5,6,4</u>,2]
<strong>Đầu ra:</strong> 60
<strong>Giải thích:</strong> Min-product lớn nhất đạt được với mảng con [5,6,4] (giá trị nhỏ nhất là 4).
4 * (5+6+4) = 4 * 15 = 60.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>7</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Min-product của một mảng con là giá trị nhỏ nhất nhân với tổng của nó. Liệt kê các đoạn có độ phức tạp $O(n^2)$, quá chậm khi $n\le 10^5$.
>
> Khi $nums[i]$ là giá trị nhỏ nhất, đoạn đó kéo dài đến giá trị nhỏ hơn nghiêm ngặt gần nhất ở bên trái và giá trị nhỏ hơn hoặc bằng gần nhất ở bên phải. Monotonic stack tìm được hai biên này; prefix sum cho phép tính tổng đoạn trong $O(1)$. Ta lấy lớn nhất trong mọi ứng viên.

<!-- thinking:end -->

Ta có thể xem mỗi phần tử $nums[i]$ là giá trị nhỏ nhất của mảng con, rồi tìm biên trái và phải $left[i]$ và $right[i]$ của mảng con. Trong đó, $left[i]$ là vị trí đầu tiên ở bên trái của $i$ có giá trị nhỏ hơn nghiêm ngặt $nums[i]$, còn $right[i]$ là vị trí đầu tiên ở bên phải của $i$ có giá trị nhỏ hơn hoặc bằng $nums[i]$.

Để thuận tiện tính tổng mảng con, ta tiền xử lý mảng tổng tiền tố $s$, trong đó $s[i]$ là tổng của $i$ phần tử đầu tiên trong $nums$.

Khi $nums[i]$ là giá trị nhỏ nhất của mảng con, min-product là $nums[i] \times (s[right[i]] - s[left[i] + 1])$. Ta duyệt từng phần tử $nums[i]$, tìm min-product khi $nums[i]$ là giá trị nhỏ nhất của mảng con, rồi lấy giá trị lớn nhất.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxSumMinProduct(self, nums: List[int]) -> int:
        n = len(nums)
        left = [-1] * n
        right = [n] * n
        stk = []
        for i, x in enumerate(nums):
            while stk and nums[stk[-1]] >= x:
                stk.pop()
            if stk:
                left[i] = stk[-1]
            stk.append(i)
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and nums[stk[-1]] > nums[i]:
                stk.pop()
            if stk:
                right[i] = stk[-1]
            stk.append(i)
        s = list(accumulate(nums, initial=0))
        mod = 10**9 + 7
        return max((s[right[i]] - s[left[i] + 1]) * x for i, x in enumerate(nums)) % mod
```

#### Java

```java
class Solution {
    public int maxSumMinProduct(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
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
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] > nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = Math.max(ans, nums[i] * (s[right[i]] - s[left[i] + 1]));
        }
        final int mod = (int) 1e9 + 7;
        return (int) (ans % mod);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxSumMinProduct(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] >= nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            while (!stk.empty() && nums[stk.top()] > nums[i]) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        long long s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans = max(ans, nums[i] * (s[right[i]] - s[left[i] + 1]));
        }
        const int mod = 1e9 + 7;
        return ans % mod;
    }
};
```

#### Go

```go
func maxSumMinProduct(nums []int) int {
	n := len(nums)
	left := make([]int, n)
	right := make([]int, n)
	for i := range left {
		left[i] = -1
		right[i] = n
	}
	stk := []int{}
	for i, x := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] >= x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && nums[stk[len(stk)-1]] > nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	s := make([]int, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + x
	}
	ans := 0
	for i, x := range nums {
		if t := x * (s[right[i]] - s[left[i]+1]); ans < t {
			ans = t
		}
	}
	const mod = 1e9 + 7
	return ans % mod
}
```

#### TypeScript

```ts
function maxSumMinProduct(nums: number[]): number {
    const n = nums.length;
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    const stk: number[] = [];
    for (let i = 0; i < n; ++i) {
        while (stk.length && nums[stk.at(-1)!] >= nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1)!;
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; i >= 0; --i) {
        while (stk.length && nums[stk.at(-1)!] > nums[i]) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1)!;
        }
        stk.push(i);
    }
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let ans: bigint = 0n;
    const mod = 10 ** 9 + 7;
    for (let i = 0; i < n; ++i) {
        const t = BigInt(nums[i]) * BigInt(s[right[i]] - s[left[i] + 1]);
        if (ans < t) {
            ans = t;
        }
    }
    return Number(ans % BigInt(mod));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
