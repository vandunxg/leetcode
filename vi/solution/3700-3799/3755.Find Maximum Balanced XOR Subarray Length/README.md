---
comments: true
difficulty: Medium
rating: 1663
source: Weekly Contest 477 Q2
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [3755. Find Maximum Balanced XOR Subarray Length](https://leetcode.com/problems/find-maximum-balanced-xor-subarray-length)

[中文文档](/solution/3700-3799/3755.Find%20Maximum%20Balanced%20XOR%20Subarray%20Length/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, hãy trả về <strong>độ dài</strong> của <strong><span data-keyword="subarray-nonempty">mảng con</span> dài nhất</strong> có XOR theo bit bằng 0 và chứa số lượng <strong>số chẵn</strong> và <strong>số lẻ</strong> <strong>bằng nhau</strong>. Nếu không tồn tại mảng con như vậy, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,3,2,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng con <code>[1, 3, 2, 0]</code> có XOR theo bit <code>1 XOR 3 XOR 2 XOR 0 = 0</code> và chứa 2 số chẵn cùng 2 số lẻ.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,2,8,5,4,14,9,15]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Toàn bộ mảng có XOR theo bit bằng <code>0</code> và chứa 4 số chẵn cùng 4 số lẻ.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có mảng con không rỗng nào thỏa mãn cả hai điều kiện.</p>
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

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện XOR bằng 0 và số lượng số chẵn, số lẻ bằng nhau đều có thể biểu diễn bằng hiệu giữa các prefix. Ta lưu vị trí đầu tiên của mỗi cặp $(\textit{xor},\textit{even}-\textit{odd})$; khi một trạng thái lặp lại, đoạn tương ứng sẽ thỏa mãn cả hai điều kiện.

<!-- thinking:end -->

Ta sử dụng một hash table để ghi lại vị trí xuất hiện đầu tiên của mỗi trạng thái $(a, b)$, trong đó $a$ biểu diễn XOR tiền tố, còn $b$ biểu diễn hiệu giữa số lượng số chẵn trong tiền tố và số lượng số lẻ trong tiền tố. Khi gặp lại cùng trạng thái $(a, b)$ trong quá trình duyệt mảng, điều đó có nghĩa là mảng con từ lần xuất hiện trước của trạng thái này đến vị trí hiện tại thỏa mãn cả hai điều kiện: XOR theo bit bằng 0 và số lượng số chẵn, số lẻ bằng nhau. Khi đó, ta cập nhật đáp án bằng cách lấy độ dài lớn nhất. Nếu không, ta lưu trạng thái này cùng vị trí hiện tại vào hash table.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxBalancedSubarray(self, nums: List[int]) -> int:
        d = {(0, 0): -1}
        a = b = 0
        ans = 0
        for i, x in enumerate(nums):
            a ^= x
            b += 1 if x % 2 == 0 else -1
            if (a, b) in d:
                ans = max(ans, i - d[(a, b)])
            else:
                d[(a, b)] = i
        return ans
```

#### Java

```java
class Solution {
    public int maxBalancedSubarray(int[] nums) {
        Map<Long, Integer> d = new HashMap<>();
        int ans = 0;
        int a = 0, b = nums.length;
        d.put((long) b, -1);
        for (int i = 0; i < nums.length; ++i) {
            a ^= nums[i];
            b += nums[i] % 2 == 0 ? 1 : -1;
            long key = (1L * a << 32) | b;
            if (d.containsKey(key)) {
                ans = Math.max(ans, i - d.get(key));
            } else {
                d.put(key, i);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxBalancedSubarray(vector<int>& nums) {
        unordered_map<long long, int> d;
        int ans = 0;
        int a = 0, b = nums.size();
        d[(long long) b] = -1;
        for (int i = 0; i < nums.size(); ++i) {
            a ^= nums[i];
            b += nums[i] % 2 == 0 ? 1 : -1;
            long long key = (1LL * a << 32) | b;
            if (d.contains(key)) {
                ans = max(ans, i - d[key]);
            } else {
                d[key] = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxBalancedSubarray(nums []int) (ans int) {
	d := map[int64]int{}
	a := 0
	b := len(nums)
	d[int64(b)] = -1
	for i, x := range nums {
		a ^= x
		if x%2 == 0 {
			b++
		} else {
			b--
		}
		key := int64(a)<<32 | int64(b)
		if j, ok := d[key]; ok {
			ans = max(ans, i-j)
		} else {
			d[key] = i
		}
	}
	return
}
```

#### TypeScript

```ts
function maxBalancedSubarray(nums: number[]): number {
    const d = new Map<bigint, number>();
    let ans = 0;
    let a = 0;
    let b = nums.length;
    d.set(BigInt(b), -1);
    for (let i = 0; i < nums.length; ++i) {
        a ^= nums[i];
        b += nums[i] % 2 === 0 ? 1 : -1;
        const key = (BigInt(a) << 32n) | BigInt(b);
        if (d.has(key)) {
            ans = Math.max(ans, i - d.get(key)!);
        } else {
            d.set(key, i);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn max_balanced_subarray(nums: Vec<i32>) -> i32 {
        let mut d = HashMap::new();
        let mut ans = 0;
        let mut a = 0;
        let mut b = nums.len() as i32;

        d.insert(b as i64, -1);

        for (i, &num) in nums.iter().enumerate() {
            a ^= num;
            b += if num % 2 == 0 { 1 } else { -1 };

            let key = ((a as i64) << 32) | b as i64;
            if let Some(&idx) = d.get(&key) {
                ans = ans.max(i as i32 - idx);
            } else {
                d.insert(key, i as i32);
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxBalancedSubarray = function (nums) {
    const d = new Map();
    let ans = 0;
    let a = 0;
    let b = nums.length;
    d.set(BigInt(b), -1);
    for (let i = 0; i < nums.length; ++i) {
        a ^= nums[i];
        b += nums[i] % 2 === 0 ? 1 : -1;
        const key = (BigInt(a) << 32n) | BigInt(b);
        if (d.has(key)) {
            ans = Math.max(ans, i - d.get(key));
        } else {
            d.set(key, i);
        }
    }
    return ans;
};
```

#### C#

```cs
public class Solution {
    public int MaxBalancedSubarray(int[] nums) {
        var d = new Dictionary<long, int>();
        int ans = 0;
        int a = 0;
        int b = nums.Length;

        d[(long)b] = -1;

        for (int i = 0; i < nums.Length; i++) {
            a ^= nums[i];
            b += nums[i] % 2 == 0 ? 1 : -1;

            long key = ((long)a << 32) | (uint)b;
            if (d.ContainsKey(key)) {
                ans = Math.Max(ans, i - d[key]);
            } else {
                d[key] = i;
            }
        }

        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
