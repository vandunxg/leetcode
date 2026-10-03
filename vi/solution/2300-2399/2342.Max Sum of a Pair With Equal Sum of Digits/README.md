---
comments: true
difficulty: Medium
rating: 1308
source: Weekly Contest 302 Q2
tags:
    - Array
    - Hash Table
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2342. Max Sum of a Pair With Equal Sum of Digits](https://leetcode.com/problems/max-sum-of-a-pair-with-equal-sum-of-digits)

[中文文档](/solution/2300-2399/2342.Max%20Sum%20of%20a%20Pair%20With%20Equal%20Sum%20of%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <strong>dương</strong> <code>nums</code> được đánh <strong>chỉ số từ 0</strong>. Bạn có thể chọn hai chỉ số <code>i</code> và <code>j</code> sao cho <code>i != j</code> và tổng các chữ số của số <code>nums[i]</code> bằng tổng các chữ số của <code>nums[j]</code>.</p>

<p>Hãy trả về giá trị <strong>lớn nhất</strong> của <em></em><code>nums[i] + nums[j]</code><em></em> có thể nhận được trong tất cả các cặp chỉ số <code>i</code> và <code>j</code> thỏa mãn điều kiện trên. Nếu không tồn tại cặp chỉ số nào như vậy, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [18,43,36,13,7]
<strong>Đầu ra:</strong> 54
<strong>Giải thích:</strong> Các cặp (i, j) thỏa mãn điều kiện là:
- (0, 2), cả hai số đều có tổng các chữ số bằng 9, và tổng của chúng là 18 + 36 = 54.
- (1, 4), cả hai số đều có tổng các chữ số bằng 7, và tổng của chúng là 43 + 7 = 50.
Vậy tổng lớn nhất có thể nhận được là 54.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,12,19,14]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có hai số nào thỏa mãn điều kiện, nên ta trả về -1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm tổng lớn nhất của hai số có cùng tổng các chữ số. Vì $n \le 10^5$, không thể kiểm tra từng cặp. Với mỗi tổng chữ số, ta chỉ cần giữ lại giá trị lớn nhất đã gặp.
>
> Với số $v$ có tổng chữ số là $x$, nếu $x$ đã xuất hiện, ta cập nhật đáp án bằng tổng của giá trị đã lưu và $v$, sau đó lưu $\max(record, v)$. Tổng các chữ số nhiều nhất là $81$, nên có thể dùng một mảng thay cho bảng băm.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $d$ để ghi lại giá trị lớn nhất tương ứng với mỗi tổng chữ số, đồng thời khởi tạo biến đáp án $ans = -1$.

Tiếp theo, ta duyệt mảng $nums$. Với mỗi số $v$, ta tính tổng các chữ số của nó là $x$. Nếu $x$ tồn tại trong bảng băm $d$, ta cập nhật đáp án $ans = \max(ans, d[x] + v)$. Sau đó cập nhật bảng băm $d[x] = \max(d[x], v)$.

Cuối cùng, trả về đáp án $ans$.

Vì phần tử lớn nhất trong $nums$ là $10^9$, tổng các chữ số lớn nhất là $9 \times 9 = 81$. Ta có thể định nghĩa trực tiếp một mảng $d$ có độ dài $100$ để thay thế bảng băm.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(D)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ và $D$ lần lượt là giá trị lớn nhất của các phần tử trong mảng $nums$ và giá trị lớn nhất của tổng chữ số. Trong bài này, $M \leq 10^9$, $D \leq 81$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        d = defaultdict(int)
        ans = -1
        for v in nums:
            x, y = 0, v
            while y:
                x += y % 10
                y //= 10
            if x in d:
                ans = max(ans, d[x] + v)
            d[x] = max(d[x], v)
        return ans
```

#### Java

```java
class Solution {
    public int maximumSum(int[] nums) {
        int[] d = new int[100];
        int ans = -1;
        for (int v : nums) {
            int x = 0;
            for (int y = v; y > 0; y /= 10) {
                x += y % 10;
            }
            if (d[x] > 0) {
                ans = Math.max(ans, d[x] + v);
            }
            d[x] = Math.max(d[x], v);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumSum(vector<int>& nums) {
        int d[100]{};
        int ans = -1;
        for (int v : nums) {
            int x = 0;
            for (int y = v; y; y /= 10) {
                x += y % 10;
            }
            if (d[x]) {
                ans = max(ans, d[x] + v);
            }
            d[x] = max(d[x], v);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSum(nums []int) int {
	d := [100]int{}
	ans := -1
	for _, v := range nums {
		x := 0
		for y := v; y > 0; y /= 10 {
			x += y % 10
		}
		if d[x] > 0 {
			ans = max(ans, d[x]+v)
		}
		d[x] = max(d[x], v)
	}
	return ans
}
```

#### TypeScript

```ts
function maximumSum(nums: number[]): number {
    const d: number[] = Array(100).fill(0);
    let ans = -1;
    for (const v of nums) {
        let x = 0;
        for (let y = v; y; y = (y / 10) | 0) {
            x += y % 10;
        }
        if (d[x]) {
            ans = Math.max(ans, d[x] + v);
        }
        d[x] = Math.max(d[x], v);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_sum(nums: Vec<i32>) -> i32 {
        let mut d = vec![0; 100];
        let mut ans = -1;

        for &v in nums.iter() {
            let mut x: usize = 0;
            let mut y = v;
            while y > 0 {
                x += (y % 10) as usize;
                y /= 10;
            }
            if d[x] > 0 {
                ans = ans.max(d[x] + v);
            }
            d[x] = d[x].max(v);
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
