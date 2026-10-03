---
comments: true
difficulty: Hard
rating: 2392
source: Weekly Contest 280 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [2172. Maximum AND Sum of Array](https://leetcode.com/problems/maximum-and-sum-of-array)

[中文文档](/solution/2100-2199/2172.Maximum%20AND%20Sum%20of%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> có độ dài <code>n</code> và một số nguyên <code>numSlots</code> sao cho <code>2 * numSlots &gt;= n</code>. Có <code>numSlots</code> slot được đánh số từ <code>1</code> đến <code>numSlots</code>.</p>

<p>Hãy đặt tất cả <code>n</code> số nguyên vào các slot sao cho mỗi slot chứa <strong>nhiều nhất</strong> hai số. <strong>Tổng AND</strong> của một cách đặt là tổng của phép <strong>bitwise</strong> <code>AND</code> giữa mỗi số và số thứ tự của slot tương ứng.</p>

<ul>
	<li>Ví dụ, <strong>tổng AND</strong> khi đặt các số <code>[1, 3]</code> vào slot <u><code>1</code></u> và <code>[4, 6]</code> vào slot <u><code>2</code></u> bằng <code>(1 AND <u>1</u>) + (3 AND <u>1</u>) + (4 AND <u>2</u>) + (6 AND <u>2</u>) = 1 + 1 + 0 + 2 = 4</code>.</li>
</ul>

<p>Hãy trả về <em><strong>tổng AND</strong> lớn nhất có thể của </em><code>nums</code><em> khi sử dụng </em><code>numSlots</code><em> slot.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5,6], numSlots = 3
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong> Một cách đặt là [1, 4] vào slot <u>1</u>, [2, 6] vào slot <u>2</u> và [3, 5] vào slot <u>3</u>.
Điều này cho tổng AND lớn nhất là (1 AND <u>1</u>) + (4 AND <u>1</u>) + (2 AND <u>2</u>) + (6 AND <u>2</u>) + (3 AND <u>3</u>) + (5 AND <u>3</u>) = 1 + 0 + 2 + 2 + 3 + 1 = 9.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,10,4,7,1], numSlots = 9
<strong>Đầu ra:</strong> 24
<strong>Giải thích:</strong> Một cách đặt là [1, 1] vào slot <u>1</u>, [3] vào slot <u>3</u>, [4] vào slot <u>4</u>, [7] vào slot <u>7</u> và [10] vào slot <u>9</u>.
Điều này cho tổng AND lớn nhất là (1 AND <u>1</u>) + (1 AND <u>1</u>) + (3 AND <u>3</u>) + (4 AND <u>4</u>) + (7 AND <u>7</u>) + (10 AND <u>9</u>) = 1 + 1 + 3 + 4 + 7 + 8 = 24.
Lưu ý rằng các slot 2, 5, 6 và 8 được phép để trống.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= numSlots &lt;= 9</code></li>
	<li><code>1 &lt;= n &lt;= 2 * numSlots</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 15</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi trong $k$ slot chứa nhiều nhất hai số; điểm số là $\sum(a\mathbin{\&}\textit{slot})$. Tách mỗi slot thành hai vị trí đơn để một bit mask có thể đánh dấu trạng thái đã dùng. $k\le 9$ nên có nhiều nhất $18$ slot.
>
> $f[S]$ là tổng AND tốt nhất khi dùng tập slot $S$ để đặt $|S|$ số đầu tiên. Nếu slot cuối cùng được dùng là $j$, ta chuyển từ $S\setminus\{j\}$ bằng cách cộng $\textit{nums}[|S|-1]\mathbin{\&}(j/2+1)$.
>
> Đáp án là giá trị $f$ lớn nhất.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumANDSum(self, nums: List[int], numSlots: int) -> int:
        n = len(nums)
        m = numSlots << 1
        f = [0] * (1 << m)
        for i in range(1 << m):
            cnt = i.bit_count()
            if cnt > n:
                continue
            for j in range(m):
                if i >> j & 1:
                    f[i] = max(f[i], f[i ^ (1 << j)] + (nums[cnt - 1] & (j // 2 + 1)))
        return max(f)
```

#### Java

```java
class Solution {
    public int maximumANDSum(int[] nums, int numSlots) {
        int n = nums.length;
        int m = numSlots << 1;
        int[] f = new int[1 << m];
        int ans = 0;
        for (int i = 0; i < 1 << m; ++i) {
            int cnt = Integer.bitCount(i);
            if (cnt > n) {
                continue;
            }
            for (int j = 0; j < m; ++j) {
                if ((i >> j & 1) == 1) {
                    f[i] = Math.max(f[i], f[i ^ (1 << j)] + (nums[cnt - 1] & (j / 2 + 1)));
                }
            }
            ans = Math.max(ans, f[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumANDSum(vector<int>& nums, int numSlots) {
        int n = nums.size();
        int m = numSlots << 1;
        int f[1 << m];
        memset(f, 0, sizeof(f));
        for (int i = 0; i < 1 << m; ++i) {
            int cnt = __builtin_popcount(i);
            if (cnt > n) {
                continue;
            }
            for (int j = 0; j < m; ++j) {
                if (i >> j & 1) {
                    f[i] = max(f[i], f[i ^ (1 << j)] + (nums[cnt - 1] & (j / 2 + 1)));
                }
            }
        }
        return *max_element(f, f + (1 << m));
    }
};
```

#### Go

```go
func maximumANDSum(nums []int, numSlots int) int {
	n := len(nums)
	m := numSlots << 1
	f := make([]int, 1<<m)
	for i := range f {
		cnt := bits.OnesCount(uint(i))
		if cnt > n {
			continue
		}
		for j := 0; j < m; j++ {
			if i>>j&1 == 1 {
				f[i] = max(f[i], f[i^(1<<j)]+(nums[cnt-1]&(j/2+1)))
			}
		}
	}
	return slices.Max(f)
}
```

#### TypeScript

```ts
function maximumANDSum(nums: number[], numSlots: number): number {
    const n = nums.length;
    const m = numSlots << 1;
    const f: number[] = new Array(1 << m).fill(0);
    for (let i = 0; i < 1 << m; ++i) {
        const cnt = i
            .toString(2)
            .split('')
            .filter(c => c === '1').length;
        if (cnt > n) {
            continue;
        }
        for (let j = 0; j < m; ++j) {
            if (((i >> j) & 1) === 1) {
                f[i] = Math.max(f[i], f[i ^ (1 << j)] + (nums[cnt - 1] & ((j >> 1) + 1)));
            }
        }
    }
    let ans = 0;
    for (const x of f) {
        ans = Math.max(ans, x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
