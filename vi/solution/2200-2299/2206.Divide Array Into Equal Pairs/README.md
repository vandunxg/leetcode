---
comments: true
difficulty: Easy
rating: 1223
source: Biweekly Contest 74 Q1
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2206. Divide Array Into Equal Pairs](https://leetcode.com/problems/divide-array-into-equal-pairs)

[中文文档](/solution/2200-2299/2206.Divide%20Array%20Into%20Equal%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> gồm <code>2 * n</code> số nguyên.</p>

<p>Bạn cần chia <code>nums</code> thành <code>n</code> cặp sao cho:</p>

<ul>
	<li>Mỗi phần tử thuộc <strong>đúng một</strong> cặp.</li>
	<li>Các phần tử trong một cặp <strong>bằng nhau</strong>.</li>
</ul>

<p>Trả về <code>true</code> <em>nếu nums có thể được chia thành</em> <code>n</code> <em>cặp, ngược lại trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,3,2,2,2]
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong>
nums có 6 phần tử, vì vậy cần chia thành 6 / 2 = 3 cặp.
Nếu nums được chia thành các cặp (2, 2), (3, 3) và (2, 2), tất cả điều kiện đều được thỏa mãn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong>
Không có cách nào chia nums thành 4 / 2 = 2 cặp sao cho các cặp thỏa mãn mọi điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>nums.length == 2 * n</code></li>
	<li><code>1 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 500</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng có độ dài $2n$ phải được chia thành $n$ cặp bằng nhau. Không cần xác định cụ thể cách ghép cặp, nhưng việc thử tất cả các cách ghép sẽ có số lượng tổ hợp tăng rất nhanh ngay cả khi $n \le 500$.
>
> Mỗi cặp sử dụng hai bản sao của cùng một giá trị, vì vậy số lần xuất hiện của mọi giá trị phải là số chẵn. Ta đếm số lần xuất hiện rồi kiểm tra xem mọi số đếm có chẵn hay không. Chỉ cần duyệt qua một hash map là đủ.

<!-- thinking:end -->

Theo mô tả bài toán, chỉ cần mỗi phần tử trong mảng xuất hiện một số lần chẵn thì mảng có thể được chia thành $n$ cặp.

Vì vậy, ta có thể sử dụng một hash table hoặc một mảng $\textit{cnt}$ để ghi nhận số lần xuất hiện của mỗi phần tử, sau đó duyệt qua $\textit{cnt}$. Nếu có phần tử xuất hiện số lần lẻ, trả về $\textit{false}$; ngược lại, trả về $\textit{true}$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divideArray(self, nums: List[int]) -> bool:
        cnt = Counter(nums)
        return all(v % 2 == 0 for v in cnt.values())
```

#### Java

```java
class Solution {
    public boolean divideArray(int[] nums) {
        int[] cnt = new int[510];
        for (int v : nums) {
            ++cnt[v];
        }
        for (int v : cnt) {
            if (v % 2 != 0) {
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
    bool divideArray(vector<int>& nums) {
        int cnt[510]{};
        for (int x : nums) {
            ++cnt[x];
        }
        for (int i = 1; i <= 500; ++i) {
            if (cnt[i] % 2) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func divideArray(nums []int) bool {
	cnt := [510]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for _, v := range cnt {
		if v%2 != 0 {
			return false
		}
	}
	return true
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn divide_array(nums: Vec<i32>) -> bool {
        let mut cnt = HashMap::new();
        for x in nums {
            *cnt.entry(x).or_insert(0) += 1;
        }
        cnt.values().all(|&v| v % 2 == 0)
    }
}
```

#### TypeScript

```ts
function divideArray(nums: number[]): boolean {
    const cnt = Array(501).fill(0);

    for (const x of nums) {
        cnt[x]++;
    }

    for (const x of cnt) {
        if (x & 1) return false;
    }

    return true;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var divideArray = function (nums) {
    const cnt = Array(501).fill(0);

    for (const x of nums) {
        cnt[x]++;
    }

    for (const x of cnt) {
        if (x & 1) return false;
    }

    return true;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
