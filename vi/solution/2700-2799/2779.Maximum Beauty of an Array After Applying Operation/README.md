---
comments: true
difficulty: Medium
rating: 1638
source: Weekly Contest 354 Q2
tags:
    - Array
    - Binary Search
    - Sorting
    - Sliding Window
---

<!-- problem:start -->

# [2779. Maximum Beauty of an Array After Applying Operation](https://leetcode.com/problems/maximum-beauty-of-an-array-after-applying-operation)

[中文文档](/solution/2700-2799/2779.Maximum%20Beauty%20of%20an%20Array%20After%20Applying%20Operation/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> được <strong>đánh chỉ số từ 0</strong> và một số nguyên <strong>không âm</strong> <code>k</code>.</p>

<p>Trong một thao tác, bạn có thể thực hiện như sau:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> <strong>chưa từng được chọn</strong> trong khoảng <code>[0, nums.length - 1]</code>.</li>
	<li>Thay <code>nums[i]</code> bằng một số nguyên bất kỳ trong khoảng <code>[nums[i] - k, nums[i] + k]</code>.</li>
</ul>

<p><strong>Độ đẹp</strong> của mảng là độ dài của dãy con dài nhất gồm các phần tử bằng nhau.</p>

<p>Hãy trả về <em><strong>độ đẹp lớn nhất</strong> có thể đạt được của mảng </em><code>nums</code><em> sau khi thực hiện thao tác một số lần bất kỳ.</em></p>

<p><strong>Lưu ý</strong> rằng bạn chỉ có thể thực hiện thao tác trên mỗi chỉ số <strong>một lần</strong>.</p>

<p><strong>Dãy con</strong> của một mảng là một mảng mới được tạo từ mảng ban đầu bằng cách xóa một số phần tử (có thể không xóa phần tử nào) mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,6,1,2], k = 2
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, ta thực hiện các thao tác sau:
- Chọn chỉ số 1, thay nó bằng 4 (từ khoảng [4,8]), nums = [4,4,1,2].
- Chọn chỉ số 3, thay nó bằng 4 (từ khoảng [0,4]), nums = [4,4,1,4].
Sau khi thực hiện các thao tác, độ đẹp của mảng nums là 3 (dãy con gồm các chỉ số 0, 1 và 3).
Có thể chứng minh rằng độ dài lớn nhất có thể đạt được là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1], k = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Trong ví dụ này, ta không cần thực hiện thao tác nào.
Độ đẹp của mảng nums là 4 (toàn bộ mảng).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i], k &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi giá trị có thể trở thành một số nguyên bất kỳ trong $[x-k,x+k]$; độ đẹp là số phần tử có thể cùng nhận một giá trị. Nếu thử mọi đích đến cho từng phần tử thì sẽ quá chậm khi cả miền giá trị và $n$ đều có thể đạt $10^5$.
>
> Sau khi dịch khoảng đi $k$, một phần tử phủ đoạn $[x,x+2k]$. Mảng hiệu cộng $1$ ở đầu trái và $-1$ ngay sau đầu phải; tổng tiền tố lớn nhất chính là số đoạn chồng lấn nhiều nhất.

<!-- thinking:end -->

Ta nhận thấy rằng với mỗi thao tác, tất cả các phần tử nằm trong khoảng $[nums[i]-k, nums[i]+k]$ sẽ tăng thêm $1$. Do đó, ta có thể dùng mảng hiệu để ghi nhận đóng góp của các thao tác vào độ đẹp.

Trong bài toán này, $nums[i]-k$ có thể âm. Ta cộng thêm $k$ vào tất cả các phần tử để đảm bảo kết quả không âm. Vì vậy, ta có thể tạo một mảng hiệu $d$ có độ dài $\max(nums) + k \times 2 + 2$.

Tiếp theo, ta duyệt qua mảng $nums$. Với phần tử hiện tại $x$, ta tăng $d[x]$ lên $1$ và giảm $d[x+k\times2+1]$ đi $1$. Nhờ đó, ta có thể tính tổng tiền tố tại mỗi vị trí bằng mảng $d$, biểu diễn giá trị độ đẹp tại vị trí đó. Sau đó, ta tìm được độ đẹp lớn nhất.

Độ phức tạp thời gian là $O(M + 2 \times k + n)$, độ phức tạp không gian là $O(M + 2 \times k)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBeauty(self, nums: List[int], k: int) -> int:
        m = max(nums) + k * 2 + 2
        d = [0] * m
        for x in nums:
            d[x] += 1
            d[x + k * 2 + 1] -= 1
        return max(accumulate(d))
```

#### Java

```java
class Solution {
    public int maximumBeauty(int[] nums, int k) {
        int m = Arrays.stream(nums).max().getAsInt() + k * 2 + 2;
        int[] d = new int[m];
        for (int x : nums) {
            d[x]++;
            d[x + k * 2 + 1]--;
        }
        int ans = 0, s = 0;
        for (int x : d) {
            s += x;
            ans = Math.max(ans, s);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximumBeauty(vector<int>& nums, int k) {
        int m = *max_element(nums.begin(), nums.end()) + k * 2 + 2;
        vector<int> d(m);
        for (int x : nums) {
            d[x]++;
            d[x + k * 2 + 1]--;
        }
        int ans = 0, s = 0;
        for (int x : d) {
            s += x;
            ans = max(ans, s);
        }
        return ans;
    }
};
```

#### Go

```go
func maximumBeauty(nums []int, k int) (ans int) {
	m := slices.Max(nums)
	m += k*2 + 2
	d := make([]int, m)
	for _, x := range nums {
		d[x]++
		d[x+k*2+1]--
	}
	s := 0
	for _, x := range d {
		s += x
		if ans < s {
			ans = s
		}
	}
	return
}
```

#### TypeScript

```ts
function maximumBeauty(nums: number[], k: number): number {
    const m = Math.max(...nums) + k * 2 + 2;
    const d: number[] = Array(m).fill(0);
    for (const x of nums) {
        d[x]++;
        d[x + k * 2 + 1]--;
    }
    let ans = 0;
    let s = 0;
    for (const x of d) {
        s += x;
        ans = Math.max(ans, s);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
