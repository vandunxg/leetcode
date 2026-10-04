---
comments: true
difficulty: Easy
rating: 1376
source: Biweekly Contest 109 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [2784. Check if Array is Good](https://leetcode.com/problems/check-if-array-is-good)

[中文文档](/solution/2700-2799/2784.Check%20if%20Array%20is%20Good/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>. Một mảng được gọi là <strong>tốt</strong> nếu nó là một hoán vị của mảng <code>base[n]</code>.</p>

<p><code>base[n] = [1, 2, ..., n - 1, n, n] </code>(nói cách khác, đây là một mảng có độ dài <code>n + 1</code>, chứa các số từ <code>1</code> đến <code>n - 1 </code>đúng một lần và chứa <code>n</code> hai lần). Ví dụ, <code>base[1] = [1, 1]</code> và<code> base[3] = [1, 2, 3, 3]</code>.</p>

<p>Trả về <code>true</code> <em>nếu mảng đã cho là mảng tốt, nếu không thì trả về</em><em> </em><code>false</code>.</p>

<p><strong>Lưu ý: </strong>Một hoán vị của các số nguyên biểu diễn cách sắp xếp các số này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2, 1, 3]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Vì phần tử lớn nhất của mảng là 3, ứng viên duy nhất cho n để mảng này có thể là một hoán vị của base[n] là n = 3. Tuy nhiên, base[3] có bốn phần tử còn mảng nums chỉ có ba phần tử. Do đó, nums không thể là một hoán vị của base[3] = [1, 2, 3, 3]. Vì vậy, đáp án là false.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1, 3, 3, 2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Vì phần tử lớn nhất của mảng là 3, ứng viên duy nhất cho n để mảng này có thể là một hoán vị của base[n] là n = 3. Có thể thấy nums là một hoán vị của base[3] = [1, 2, 3, 3] (bằng cách đổi chỗ phần tử thứ hai và thứ tư trong nums, ta thu được base[3]). Do đó, đáp án là true.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1, 1]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Vì phần tử lớn nhất của mảng là 1, ứng viên duy nhất cho n để mảng này có thể là một hoán vị của base[n] là n = 1. Có thể thấy nums là một hoán vị của base[1] = [1, 1]. Do đó, đáp án là true.</pre>

<p><strong class="example">Ví dụ 4:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3, 4, 4, 1, 2, 1]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Vì phần tử lớn nhất của mảng là 4, ứng viên duy nhất cho n để mảng này có thể là một hoán vị của base[n] là n = 4. Tuy nhiên, base[4] có năm phần tử còn mảng nums có sáu phần tử. Do đó, nums không thể là một hoán vị của base[4] = [1, 2, 3, 4, 4]. Vì vậy, đáp án là false.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= num[i] &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng tốt gồm các số từ $1..n-1$ xuất hiện một lần và hai bản sao của $n$, trong đó $n=|nums|-1$. Có thể sắp xếp rồi so sánh với một mảng cơ sở được tạo sẵn, nhưng chỉ cần đếm tần suất là đủ.
>
> Sau khi đếm, $n$ phải xuất hiện hai lần và mọi chỉ số trong $1..n-1$ phải xuất hiện ít nhất một lần (khi đó độ dài mảng sẽ đảm bảo mỗi số xuất hiện đúng một lần).

<!-- thinking:end -->

Ta có thể dùng hash table hoặc mảng $cnt$ để ghi nhận số lần xuất hiện của mỗi phần tử trong mảng $nums$. Sau đó, ta kiểm tra xem hai điều kiện sau có được thỏa mãn hay không:

1. $cnt[n] = 2$, tức là phần tử lớn nhất trong mảng xuất hiện hai lần;
2. Với $1 \leq i \leq n-1$, ta có $cnt[i] = 1$, tức là mọi phần tử khác phần tử lớn nhất chỉ xuất hiện một lần.

Nếu hai điều kiện trên được thỏa mãn, mảng $nums$ là một mảng tốt, ngược lại thì không.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isGood(self, nums: List[int]) -> bool:
        cnt = Counter(nums)
        n = len(nums) - 1
        return cnt[n] == 2 and all(cnt[i] for i in range(1, n))
```

#### Java

```java
class Solution {
    public boolean isGood(int[] nums) {
        int n = nums.length - 1;
        int[] cnt = new int[201];
        for (int x : nums) {
            ++cnt[x];
        }
        if (cnt[n] != 2) {
            return false;
        }
        for (int i = 1; i < n; ++i) {
            if (cnt[i] != 1) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int n = nums.size() - 1;
        int cnt[201]{};
        for (int x : nums) {
            ++cnt[x];
        }
        if (cnt[n] != 2) {
            return false;
        }
        for (int i = 1; i < n; ++i) {
            if (cnt[i] != 1) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func isGood(nums []int) bool {
	n := len(nums) - 1
	cnt := [201]int{}
	for _, x := range nums {
		cnt[x]++
	}
	if cnt[n] != 2 {
		return false
	}
	for i := 1; i < n; i++ {
		if cnt[i] != 1 {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function isGood(nums: number[]): boolean {
    const n = nums.length - 1;
    const cnt: number[] = Array(201).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    if (cnt[n] !== 2) {
        return false;
    }
    for (let i = 1; i < n; ++i) {
        if (cnt[i] !== 1) {
            return false;
        }
    }
    return true;
}
```

#### C#

```cs
public class Solution {
    public bool IsGood(int[] nums) {
        int n = nums.Length - 1;
        int[] cnt = new int[201];
        foreach (int x in nums) {
            ++cnt[x];
        }
        if (cnt[n] != 2) {
            return false;
        }
        for (int i = 1; i < n; ++i) {
            if (cnt[i] != 1) {
                return false;
            }
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
