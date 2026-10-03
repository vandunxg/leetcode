---
comments: true
difficulty: Medium
rating: 1218
source: Weekly Contest 315 Q2
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [2442. Count Number of Distinct Integers After Reverse Operations](https://leetcode.com/problems/count-number-of-distinct-integers-after-reverse-operations)

[中文文档](/solution/2400-2499/2442.Count%20Number%20of%20Distinct%20Integers%20After%20Reverse%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Bạn phải lấy từng số nguyên trong mảng, <strong>đảo ngược các chữ số</strong> của nó, rồi thêm số đó vào cuối mảng. Bạn nên áp dụng thao tác này cho các số nguyên ban đầu trong <code>nums</code>.</p>

<p>Trả về <em>số lượng số nguyên <strong>phân biệt</strong> trong mảng cuối cùng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,13,10,12,31]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Sau khi thêm số đảo ngược của từng số, mảng kết quả là [1,13,10,12,31,<u>1,31,1,21,13</u>].
Các số nguyên sau khi đảo ngược được thêm vào cuối mảng được gạch chân. Lưu ý rằng với số nguyên 10, sau khi đảo ngược, nó trở thành 01, tức là 1.
Số lượng số nguyên phân biệt trong mảng này là 6 (các số 1, 10, 12, 13, 21 và 31).</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sau khi thêm số đảo ngược của từng số, mảng kết quả là [2,2,2,<u>2,2,2</u>].
Số lượng số nguyên phân biệt trong mảng này là 1 (số 2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi số ban đầu và số nhận được sau khi đảo chữ số đều phải được đếm. Với $n\le 10^5$ và các giá trị $\le 10^6$, ta đảo chữ số bằng cách cắt chuỗi thập phân. Đưa mảng vào một set, sau đó thêm từng số đảo ngược; kích thước của set là đáp án.

<!-- thinking:end -->

Đầu tiên, chúng ta sử dụng một bảng băm để ghi lại tất cả số nguyên trong mảng. Sau đó, duyệt qua từng số nguyên trong mảng, đảo ngược số đó và thêm số nguyên sau khi đảo ngược vào bảng băm. Cuối cùng, trả về kích thước của bảng băm.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDistinctIntegers(self, nums: List[int]) -> int:
        s = set(nums)
        for x in nums:
            y = int(str(x)[::-1])
            s.add(y)
        return len(s)
```

#### Java

```java
class Solution {
    public int countDistinctIntegers(int[] nums) {
        Set<Integer> s = new HashSet<>();
        for (int x : nums) {
            s.add(x);
        }
        for (int x : nums) {
            int y = 0;
            while (x > 0) {
                y = y * 10 + x % 10;
                x /= 10;
            }
            s.add(y);
        }
        return s.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countDistinctIntegers(vector<int>& nums) {
        unordered_set<int> s(nums.begin(), nums.end());
        for (int x : nums) {
            int y = 0;
            while (x) {
                y = y * 10 + x % 10;
                x /= 10;
            }
            s.insert(y);
        }
        return s.size();
    }
};
```

#### Go

```go
func countDistinctIntegers(nums []int) int {
	s := map[int]struct{}{}
	for _, x := range nums {
		s[x] = struct{}{}
	}
	for _, x := range nums {
		y := 0
		for x > 0 {
			y = y*10 + x%10
			x /= 10
		}
		s[y] = struct{}{}
	}
	return len(s)
}
```

#### TypeScript

```ts
function countDistinctIntegers(nums: number[]): number {
    const n = nums.length;
    for (let i = 0; i < n; i++) {
        nums.push(Number([...(nums[i] + '')].reverse().join('')));
    }
    return new Set(nums).size;
}
```

#### Rust

```rust
use std::collections::HashSet;
impl Solution {
    pub fn count_distinct_integers(nums: Vec<i32>) -> i32 {
        let mut set = HashSet::new();
        for i in 0..nums.len() {
            let mut num = nums[i];
            set.insert(num);
            set.insert({
                let mut item = 0;
                while num > 0 {
                    item = item * 10 + (num % 10);
                    num /= 10;
                }
                item
            });
        }
        set.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
