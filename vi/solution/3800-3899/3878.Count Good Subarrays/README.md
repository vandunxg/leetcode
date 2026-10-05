---
comments: true
difficulty: Hard
rating: 2230
source: Weekly Contest 494 Q4
tags:
    - Stack
    - Bit Manipulation
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [3878. Count Good Subarrays](https://leetcode.com/problems/count-good-subarrays)

[中文文档](/solution/3800-3899/3878.Count%20Good%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Một <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> được gọi là <strong>good</strong> nếu phép <strong>OR bitwise</strong> của tất cả các phần tử trong nó bằng với <strong>ít nhất một</strong> phần tử xuất hiện trong mảng con đó.</p>

<p>Hãy trả về số lượng mảng con good trong <code>nums</code>.</p>

<p>Trong đó, phép OR bitwise của hai số nguyên <code>a</code> và <code>b</code> được ký hiệu là <code>a | b</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con của <code>nums</code> là:</p>

<table style="border: 1px solid black;">
	<tbody>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Phép OR bitwise</th>
			<th style="border: 1px solid black;">Có trong mảng con</th>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[4]</code></td>
			<td style="border: 1px solid black;"><code>4 = 4</code></td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2]</code></td>
			<td style="border: 1px solid black;"><code>2 = 2</code></td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[3]</code></td>
			<td style="border: 1px solid black;"><code>3 = 3</code></td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[4, 2]</code></td>
			<td style="border: 1px solid black;"><code>4 | 2 = 6</code></td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 3]</code></td>
			<td style="border: 1px solid black;"><code>2 | 3 = 3</code></td>
			<td style="border: 1px solid black;">Có</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[4, 2, 3]</code></td>
			<td style="border: 1px solid black;"><code>4 | 2 | 3 = 7</code></td>
			<td style="border: 1px solid black;">Không</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, các mảng con good của <code>nums</code> là <code>[4]</code>, <code>[2]</code>, <code>[3]</code> và <code>[2, 3]</code>. Do đó, đáp án là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mọi mảng con của <code>nums</code> chứa 3 đều có phép OR bitwise bằng 3, còn các mảng con chỉ chứa 1 thì có phép OR bitwise bằng 1.</p>

<p>Trong cả hai trường hợp, kết quả đều xuất hiện trong mảng con, nên mọi mảng con đều là good và đáp án là 6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack + Contribution Enumeration

<!-- thinking:start -->

> **Tư duy**
>
> OR của một mảng con good bằng một phần tử nằm trong mảng con đó. Vì $n \le 10^5$, ta không thể liệt kê tất cả các đoạn.
>
> Nếu OR bằng $nums[i]$, mọi giá trị trong đoạn đều là tập con theo bit của $nums[i]$, và đoạn phải chứa $i$. Ta coi $i$ là phần tử đại diện cho kết quả OR.
>
> Monotonic stack tìm được biên trái và phải xa nhất sao cho các phần tử bên trong vẫn là tập con theo bit của $nums[i]$. Đóng góp là $(i-l[i])\cdot(r[i]-i)$.
>
> Mỗi phần tử được đếm trong các đoạn mà nó là phần tử điều khiển OR theo stack.

<!-- thinking:end -->

Ta có thể coi mỗi phần tử $\textit{nums}[i]$ là kết quả OR bitwise của một mảng con, rồi đếm số mảng con có kết quả OR bitwise đúng bằng $\textit{nums}[i]$.

Nếu phép OR bitwise của một mảng con bằng $\textit{nums}[i]$, thì mọi phần tử trong mảng con phải thỏa mãn:

$$
\textit{nums}[k] \mid \textit{nums}[i] = \textit{nums}[i]
$$

Điều đó có nghĩa là mọi phần tử trong mảng con đều phải là tập con của $\textit{nums}[i]$ (xét theo các bit). Ta có thể dùng monotonic stack để tìm biên trái $l[i]$ và biên phải $r[i]$ của mỗi phần tử $\textit{nums}[i]$, sao cho mọi phần tử trong đoạn $(l[i], r[i])$ đều thỏa mãn điều kiện trên, còn $\textit{nums}[l[i]]$ và $\textit{nums}[r[i]]$ thì không. Số lượng mảng con có $\textit{nums}[i]$ là kết quả OR bitwise được tính bằng $(i - l[i]) \cdot (r[i] - i)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countGoodSubarrays(self, nums: list[int]) -> int:
        n = len(nums)
        l = [-1] * n
        stk = []
        for i, x in enumerate(nums):
            while stk and nums[stk[-1]] < x and (nums[stk[-1]] | x) == x:
                stk.pop()
            l[i] = stk[-1] if stk else -1
            stk.append(i)
        r = [n] * n
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and (nums[stk[-1]] | nums[i]) == nums[i]:
                stk.pop()
            r[i] = stk[-1] if stk else n
            stk.append(i)
        return sum((i - l[i]) * (r[i] - i) for i in range(n))
```

#### Java

```java
class Solution {
    public long countGoodSubarrays(int[] nums) {
        int n = nums.length;

        int[] l = new int[n];
        Arrays.fill(l, -1);
        Deque<Integer> stk = new ArrayDeque<>();

        for (int i = 0; i < n; i++) {
            int x = nums[i];
            while (!stk.isEmpty() && nums[stk.peek()] < x && (nums[stk.peek()] | x) == x) {
                stk.pop();
            }
            l[i] = stk.isEmpty() ? -1 : stk.peek();
            stk.push(i);
        }

        int[] r = new int[n];
        Arrays.fill(r, n);
        stk.clear();

        for (int i = n - 1; i >= 0; i--) {
            while (!stk.isEmpty() && (nums[stk.peek()] | nums[i]) == nums[i]) {
                stk.pop();
            }
            r[i] = stk.isEmpty() ? n : stk.peek();
            stk.push(i);
        }

        long ans = 0;
        for (int i = 0; i < n; i++) {
            ans += (long) (i - l[i]) * (r[i] - i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countGoodSubarrays(vector<int>& nums) {
        int n = nums.size();

        vector<int> l(n, -1);
        vector<int> stk;

        for (int i = 0; i < n; i++) {
            int x = nums[i];
            while (!stk.empty() && nums[stk.back()] < x && (nums[stk.back()] | x) == x) {
                stk.pop_back();
            }
            l[i] = stk.empty() ? -1 : stk.back();
            stk.push_back(i);
        }

        vector<int> r(n, n);
        stk.clear();

        for (int i = n - 1; i >= 0; i--) {
            while (!stk.empty() && (nums[stk.back()] | nums[i]) == nums[i]) {
                stk.pop_back();
            }
            r[i] = stk.empty() ? n : stk.back();
            stk.push_back(i);
        }

        long long ans = 0;
        for (int i = 0; i < n; i++) {
            ans += 1LL * (i - l[i]) * (r[i] - i);
        }
        return ans;
    }
};
```

#### Go

```go
func countGoodSubarrays(nums []int) int64 {
	n := len(nums)

	l := make([]int, n)
	for i := range l {
		l[i] = -1
	}
	stk := []int{}

	for i, x := range nums {
		for len(stk) > 0 && nums[stk[len(stk)-1]] < x &&
			(nums[stk[len(stk)-1]]|x) == x {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			l[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}

	r := make([]int, n)
	for i := range r {
		r[i] = n
	}
	stk = stk[:0]

	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 &&
			(nums[stk[len(stk)-1]]|nums[i]) == nums[i] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			r[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}

	var ans int64
	for i := 0; i < n; i++ {
		ans += int64(i-l[i]) * int64(r[i]-i)
	}
	return ans
}
```

#### TypeScript

```ts
function countGoodSubarrays(nums: number[]): number {
    const n = nums.length;

    const l = new Array(n).fill(-1);
    const stk: number[] = [];

    for (let i = 0; i < n; i++) {
        const x = nums[i];
        while (
            stk.length &&
            nums[stk[stk.length - 1]] < x &&
            (nums[stk[stk.length - 1]] | x) === x
        ) {
            stk.pop();
        }
        l[i] = stk.length ? stk[stk.length - 1] : -1;
        stk.push(i);
    }

    const r = new Array(n).fill(n);
    stk.length = 0;

    for (let i = n - 1; i >= 0; i--) {
        while (stk.length && (nums[stk[stk.length - 1]] | nums[i]) === nums[i]) {
            stk.pop();
        }
        r[i] = stk.length ? stk[stk.length - 1] : n;
        stk.push(i);
    }

    let ans = 0;
    for (let i = 0; i < n; i++) {
        ans += (i - l[i]) * (r[i] - i);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
