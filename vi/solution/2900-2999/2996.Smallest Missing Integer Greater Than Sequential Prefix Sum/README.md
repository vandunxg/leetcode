---
comments: true
difficulty: Easy
rating: 1405
source: Biweekly Contest 121 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2996. Smallest Missing Integer Greater Than Sequential Prefix Sum](https://leetcode.com/problems/smallest-missing-integer-greater-than-sequential-prefix-sum)

[中文文档](/solution/2900-2999/2996.Smallest%20Missing%20Integer%20Greater%20Than%20Sequential%20Prefix%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code>.</p>

<p>Một tiền tố <code>nums[0..i]</code> là <strong>tuần tự</strong> nếu, với mọi <code>1 &lt;= j &lt;= i</code>, <code>nums[j] = nums[j - 1] + 1</code>. Đặc biệt, tiền tố chỉ gồm <code>nums[0]</code> cũng là <strong>tuần tự</strong>.</p>

<p><em>Trả về <strong>số nguyên nhỏ nhất</strong></em> <code>x</code> <em>không xuất hiện trong</em> <code>nums</code> <em>sao cho</em> <code>x</code> <em>lớn hơn hoặc bằng tổng của tiền tố tuần tự <strong>dài nhất</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,2,5]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Tiền tố tuần tự dài nhất của nums là [1,2,3] với tổng bằng 6. 6 không xuất hiện trong mảng, vì vậy 6 là số nguyên nhỏ nhất không xuất hiện và lớn hơn hoặc bằng tổng của tiền tố tuần tự dài nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,4,5,1,12,14,13]
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Tiền tố tuần tự dài nhất của nums là [3,4,5] với tổng bằng 12. 12, 13 và 14 đều thuộc mảng, còn 15 thì không. Do đó, 15 là số nguyên nhỏ nhất không xuất hiện và lớn hơn hoặc bằng tổng của tiền tố tuần tự dài nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 50</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tiền tố tuần tự dài nhất bắt đầu tại chỉ số $0$; gọi $s$ là tổng của tiền tố này. Ta cần số nguyên nhỏ nhất $\ge s$ không xuất hiện trong mảng. Vì $n \le 50$: duyệt để tính tổng tiền tố, sau đó kiểm tra lần lượt $s,s+1,\ldots$ trong một tập hợp.
>
> Miền giá trị nhỏ, nên chỉ cần tăng tuyến tính là sẽ tìm được khoảng trống.

<!-- thinking:end -->

Trước hết, ta tính tổng $s$ của tiền tố tuần tự dài nhất trong mảng $nums$. Sau đó, bắt đầu từ $s$, ta lần lượt xét số nguyên $x$. Nếu $x$ không xuất hiện trong mảng $nums$, thì $x$ là đáp án.

Vì $nums[i] \leq 50$ trong bài này, ta có thể dùng một mảng có độ dài $51$ (hoặc một hash table) để ghi nhận các số nguyên xuất hiện trong mảng, nhờ đó nhanh chóng xác định một số nguyên có nằm trong mảng $nums$ hay không.

Độ phức tạp thời gian là $O(n + M)$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là cận trên của các phần tử trong mảng, bằng $51$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingInteger(self, nums: List[int]) -> int:
        s = nums[0]
        for x, y in pairwise(nums):
            if x + 1 != y:
                break
            s += y
        st = set(nums)
        while s in st:
            s += 1
        return s
```

#### Java

```java
class Solution {
    public int missingInteger(int[] nums) {
        int s = nums[0];
        for (int j = 1; j < nums.length && nums[j] == nums[j - 1] + 1; ++j) {
            s += nums[j];
        }
        final int m = 51;
        boolean[] st = new boolean[m];
        for (int x : nums) {
            st[x] = true;
        }
        while (s < m && st[s]) {
            ++s;
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingInteger(vector<int>& nums) {
        int s = nums[0];
        for (int j = 1; j < nums.size() && nums[j] == nums[j - 1] + 1; ++j) {
            s += nums[j];
        }

        const int m = 51;
        bool st[m] = {};
        for (int x : nums) {
            st[x] = true;
        }

        while (s < m && st[s]) {
            ++s;
        }
        return s;
    }
};
```

#### Go

```go
func missingInteger(nums []int) int {
	s := nums[0]
	for j := 1; j < len(nums) && nums[j] == nums[j-1]+1; j++ {
		s += nums[j]
	}

	const m = 51
	st := make([]bool, m)
	for _, x := range nums {
		st[x] = true
	}

	for s < m && st[s] {
		s++
	}
	return s
}
```

#### TypeScript

```ts
function missingInteger(nums: number[]): number {
    let s = nums[0];
    for (let j = 1; j < nums.length && nums[j] === nums[j - 1] + 1; ++j) {
        s += nums[j];
    }

    const m = 51;
    const st = new Array<boolean>(m).fill(false);
    for (const x of nums) {
        st[x] = true;
    }

    while (s < m && st[s]) {
        ++s;
    }
    return s;
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_integer(nums: Vec<i32>) -> i32 {
        let mut s = nums[0];

        for j in 1..nums.len() {
            if nums[j] != nums[j - 1] + 1 {
                break;
            }
            s += nums[j];
        }

        const M: usize = 51;
        let mut st = [false; M];

        for &x in &nums {
            st[x as usize] = true;
        }

        while s < M as i32 && st[s as usize] {
            s += 1;
        }

        s
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
