---
comments: true
difficulty: Hard
rating: 2917
source: Weekly Contest 382 Q4
tags:
    - Greedy
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [3022. Minimize OR of Remaining Elements Using Operations](https://leetcode.com/problems/minimize-or-of-remaining-elements-using-operations)

[中文文档](/solution/3000-3099/3022.Minimize%20OR%20of%20Remaining%20Elements%20Using%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể chọn một chỉ số <code>i</code> bất kỳ của <code>nums</code> sao cho <code>0 &lt;= i &lt; nums.length - 1</code>, rồi thay thế <code>nums[i]</code> và <code>nums[i + 1]</code> bằng một lần xuất hiện duy nhất của <code>nums[i] &amp; nums[i + 1]</code>, trong đó <code>&amp;</code> biểu diễn phép toán <code>AND</code> theo bit.</p>

<p>Trả về <em>giá trị <strong>nhỏ nhất</strong> có thể có của phép OR theo bit </em><code>OR</code><em> trên các phần tử còn lại của</em> <code>nums</code> <em>sau khi thực hiện <strong>nhiều nhất</strong></em> <code>k</code> <em>thao tác</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,5,3,2,7], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
1. Thay thế nums[0] và nums[1] bằng (nums[0] &amp; nums[1]), khi đó nums trở thành [1,3,2,7].
2. Thay thế nums[2] và nums[3] bằng (nums[2] &amp; nums[3]), khi đó nums trở thành [1,3,2].
Phép OR theo bit của mảng cuối cùng là 3.
Có thể chứng minh rằng 3 là giá trị nhỏ nhất có thể có của phép OR theo bit trên các phần tử còn lại của nums sau khi thực hiện nhiều nhất k thao tác.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,3,15,14,2,8], k = 4
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
1. Thay thế nums[0] và nums[1] bằng (nums[0] &amp; nums[1]), khi đó nums trở thành [3,15,14,2,8].
2. Thay thế nums[0] và nums[1] bằng (nums[0] &amp; nums[1]), khi đó nums trở thành [3,14,2,8].
3. Thay thế nums[0] và nums[1] bằng (nums[0] &amp; nums[1]), khi đó nums trở thành [2,2,8].
4. Thay thế nums[1] và nums[2] bằng (nums[1] &amp; nums[2]), khi đó nums trở thành [2,0].
Phép OR theo bit của mảng cuối cùng là 2.
Có thể chứng minh rằng 2 là giá trị nhỏ nhất có thể có của phép OR theo bit trên các phần tử còn lại của nums sau khi thực hiện nhiều nhất k thao tác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,7,10,3,9,14,9,4], k = 1
<strong>Đầu ra:</strong> 15
<strong>Giải thích:</strong> Nếu không thực hiện thao tác nào, phép OR theo bit của nums là 15.
Có thể chứng minh rằng 15 là giá trị nhỏ nhất có thể có của phép OR theo bit trên các phần tử còn lại của nums sau khi thực hiện nhiều nhất k thao tác.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>30</sup></code></li>
	<li><code>0 &lt;= k &lt; nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác thay thế hai giá trị kề nhau bằng phép AND theo bit của chúng, được thực hiện nhiều nhất $k$ lần, với $n \le 10^5$. Ta muốn phép OR của các phần tử còn lại nhỏ nhất có thể.
>
> Các bit lớn đóng góp nhiều hơn vào OR, vì vậy ta thử tắt các bit từ cao xuống thấp. Tắt một bit có nghĩa là gộp các đoạn có bit đó bằng $1$ bằng không quá $k$ phép AND.
>
> Với mỗi bit, ta xây dựng một mask để kiểm tra và đếm số lần gộp bổ sung. Nếu số lần này không vượt quá $k$ thì có thể xóa bit; nếu không, bit đó phải được giữ lại trong đáp án.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOrAfterOperations(self, nums: List[int], k: int) -> int:
        ans = 0
        rans = 0
        for i in range(29, -1, -1):
            test = ans + (1 << i)
            cnt = 0
            val = 0
            for num in nums:
                if val == 0:
                    val = test & num
                else:
                    val &= test & num
                if val:
                    cnt += 1
            if cnt > k:
                rans += 1 << i
            else:
                ans += 1 << i
        return rans
```

#### Java

```java
class Solution {
    public int minOrAfterOperations(int[] nums, int k) {
        int ans = 0, rans = 0;
        for (int i = 29; i >= 0; i--) {
            int test = ans + (1 << i);
            int cnt = 0;
            int val = 0;
            for (int num : nums) {
                if (val == 0) {
                    val = test & num;
                } else {
                    val &= test & num;
                }
                if (val != 0) {
                    cnt++;
                }
            }
            if (cnt > k) {
                rans += (1 << i);
            } else {
                ans += (1 << i);
            }
        }
        return rans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOrAfterOperations(vector<int>& nums, int k) {
        int ans = 0, rans = 0;
        for (int i = 29; i >= 0; i--) {
            int test = ans + (1 << i);
            int cnt = 0;
            int val = 0;
            for (auto it : nums) {
                if (val == 0) {
                    val = test & it;
                } else {
                    val &= test & it;
                }
                if (val) {
                    cnt++;
                }
            }
            if (cnt > k) {
                rans += (1 << i);
            } else {
                ans += (1 << i);
            }
        }
        return rans;
    }
};
```

#### Go

```go
func minOrAfterOperations(nums []int, k int) int {
    ans := 0
    rans := 0
    for i := 29; i >= 0; i-- {
        test := ans + (1 << i)
        cnt := 0
        val := 0
        for _, num := range nums {
            if val == 0 {
                val = test & num
            } else {
                val &= test & num
            }
            if val != 0 {
                cnt++
            }
        }
        if cnt > k {
            rans += (1 << i)
        } else {
            ans += (1 << i)
        }
    }
    return rans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
