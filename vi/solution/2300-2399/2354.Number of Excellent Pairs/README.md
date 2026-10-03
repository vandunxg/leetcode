---
comments: true
difficulty: Hard
rating: 2075
source: Weekly Contest 303 Q4
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Binary Search
---

<!-- problem:start -->

# [2354. Number of Excellent Pairs](https://leetcode.com/problems/number-of-excellent-pairs)

[中文文档](/solution/2300-2399/2354.Number%20of%20Excellent%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương được <strong>đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên dương <code>k</code>.</p>

<p>Một cặp số <code>(num1, num2)</code> được gọi là <strong>cặp xuất sắc</strong> nếu thỏa mãn các điều kiện sau:</p>

<ul>
	<li><strong>Cả hai</strong> số <code>num1</code> và <code>num2</code> đều tồn tại trong mảng <code>nums</code>.</li>
	<li>Tổng số bit được bật trong <code>num1 OR num2</code> và <code>num1 AND num2</code> lớn hơn hoặc bằng <code>k</code>, trong đó <code>OR</code> là phép toán <strong>OR bit</strong> và <code>AND</code> là phép toán <strong>AND bit</strong>.</li>
</ul>

<p>Hãy trả về <em>số lượng <strong>cặp xuất sắc phân biệt</strong></em>.</p>

<p>Hai cặp <code>(a, b)</code> và <code>(c, d)</code> được xem là phân biệt nếu <code>a != c</code> hoặc <code>b != d</code>. Ví dụ, <code>(1, 2)</code> và <code>(2, 1)</code> là hai cặp phân biệt.</p>

<p><strong>Lưu ý</strong> rằng cặp <code>(num1, num2)</code> với <code>num1 == num2</code> cũng có thể là cặp xuất sắc nếu mảng chứa <strong>ít nhất một</strong> lần xuất hiện của <code>num1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,1], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Các cặp xuất sắc là:
- (3, 3). (3 AND 3) và (3 OR 3) đều bằng (11) trong hệ nhị phân. Tổng số bit được bật là 2 + 2 = 4, lớn hơn hoặc bằng k = 3.
- (2, 3) và (3, 2). (2 AND 3) bằng (10) trong hệ nhị phân, còn (2 OR 3) bằng (11) trong hệ nhị phân. Tổng số bit được bật là 1 + 2 = 3.
- (1, 3) và (3, 1). (1 AND 3) bằng (01) trong hệ nhị phân, còn (1 OR 3) bằng (11) trong hệ nhị phân. Tổng số bit được bật là 1 + 2 = 3.
Vậy số lượng cặp xuất sắc là 5.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,1,1], k = 10
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có cặp xuất sắc nào trong mảng này.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 60</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một cặp là cặp xuất sắc khi và chỉ khi $\mathrm{popcount}(a\land b)+\mathrm{popcount}(a\lor b)\ge k$, biểu thức này bằng $\mathrm{popcount}(a)+\mathrm{popcount}(b)$. Vì $n \le 10^5$ và các cặp phụ thuộc vào tập hợp các giá trị, trước hết ta loại bỏ các giá trị trùng lặp.
>
> Đếm các giá trị phân biệt theo số bit được bật. Với mỗi $v$ có $t$ bit được bật, cộng mọi tần suất $i$ thỏa mãn $t+i\ge k$. Mỗi cặp có thứ tự, kể cả cặp gồm hai giá trị bằng nhau, được đếm đúng một lần.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countExcellentPairs(self, nums: List[int], k: int) -> int:
        s = set(nums)
        ans = 0
        cnt = Counter()
        for v in s:
            cnt[v.bit_count()] += 1
        for v in s:
            t = v.bit_count()
            for i, x in cnt.items():
                if t + i >= k:
                    ans += x
        return ans
```

#### Java

```java
class Solution {
    public long countExcellentPairs(int[] nums, int k) {
        Set<Integer> s = new HashSet<>();
        for (int v : nums) {
            s.add(v);
        }
        long ans = 0;
        int[] cnt = new int[32];
        for (int v : s) {
            int t = Integer.bitCount(v);
            ++cnt[t];
        }
        for (int v : s) {
            int t = Integer.bitCount(v);
            for (int i = 0; i < 32; ++i) {
                if (t + i >= k) {
                    ans += cnt[i];
                }
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
    long long countExcellentPairs(vector<int>& nums, int k) {
        unordered_set<int> s(nums.begin(), nums.end());
        vector<int> cnt(32);
        for (int v : s) ++cnt[__builtin_popcount(v)];
        long long ans = 0;
        for (int v : s) {
            int t = __builtin_popcount(v);
            for (int i = 0; i < 32; ++i) {
                if (t + i >= k) {
                    ans += cnt[i];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countExcellentPairs(nums []int, k int) int64 {
	s := map[int]bool{}
	for _, v := range nums {
		s[v] = true
	}
	cnt := make([]int, 32)
	for v := range s {
		t := bits.OnesCount(uint(v))
		cnt[t]++
	}
	ans := 0
	for v := range s {
		t := bits.OnesCount(uint(v))
		for i, x := range cnt {
			if t+i >= k {
				ans += x
			}
		}
	}
	return int64(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
