---
comments: true
difficulty: Easy
rating: 1226
source: Biweekly Contest 5 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [1133. Largest Unique Number 🔒](https://leetcode.com/problems/largest-unique-number)

[中文文档](/solution/1100-1199/1133.Largest%20Unique%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về <em>số nguyên lớn nhất chỉ xuất hiện một lần</em>. Nếu không có số nào xuất hiện đúng một lần, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [5,7,3,9,4,9,8,3,1]
<strong>Output:</strong> 8
<strong>Giải thích:</strong> Số nguyên lớn nhất trong mảng là 9 nhưng số này bị lặp. Số 8 chỉ xuất hiện một lần nên đó là đáp án.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [9,9,8,8]
<strong>Output:</strong> -1
<strong>Giải thích:</strong> Không có số nào chỉ xuất hiện một lần.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Duyệt ngược

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số lớn nhất chỉ xuất hiện một lần, hoặc trả về $-1$. Đếm tần suất, giữ lại các giá trị có số lần xuất hiện bằng $1$, rồi lấy giá trị lớn nhất. Khi miền giá trị nhỏ, có thể dùng mảng kích thước $1001$ và duyệt từ lớn xuống nhỏ.

<!-- thinking:end -->

Dựa vào miền giá trị trong đề bài, ta có thể dùng mảng độ dài $1001$ để đếm số lần xuất hiện của từng số. Sau đó, duyệt mảng theo thứ tự ngược để tìm số đầu tiên chỉ xuất hiện một lần. Nếu không tìm thấy số nào như vậy, trả về $-1$.

Độ phức tạp thời gian là $O(n + M)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài mảng, còn $M$ là giá trị lớn nhất xuất hiện trong mảng. Với bài này, $M \leq 1000$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestUniqueNumber(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        return max((x for x, v in cnt.items() if v == 1), default=-1)
```

#### Java

```java
class Solution {
    public int largestUniqueNumber(int[] nums) {
        int[] cnt = new int[1001];
        for (int x : nums) {
            ++cnt[x];
        }
        for (int x = 1000; x >= 0; --x) {
            if (cnt[x] == 1) {
                return x;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestUniqueNumber(vector<int>& nums) {
        int cnt[1001]{};
        for (int& x : nums) {
            ++cnt[x];
        }
        for (int x = 1000; ~x; --x) {
            if (cnt[x] == 1) {
                return x;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func largestUniqueNumber(nums []int) int {
	cnt := [1001]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for x := 1000; x >= 0; x-- {
		if cnt[x] == 1 {
			return x
		}
	}
	return -1
}
```

#### TypeScript

```ts
function largestUniqueNumber(nums: number[]): number {
    const cnt = Array(1001).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    for (let x = 1000; x >= 0; --x) {
        if (cnt[x] === 1) {
            return x;
        }
    }
    return -1;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var largestUniqueNumber = function (nums) {
    const cnt = Array(1001).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    for (let x = 1000; x >= 0; --x) {
        if (cnt[x] === 1) {
            return x;
        }
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
