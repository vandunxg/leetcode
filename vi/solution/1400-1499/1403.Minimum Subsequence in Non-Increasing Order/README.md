---
comments: true
difficulty: Easy
rating: 1288
source: Weekly Contest 183 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1403. Minimum Subsequence in Non-Increasing Order](https://leetcode.com/problems/minimum-subsequence-in-non-increasing-order)

[中文文档](/solution/1400-1499/1403.Minimum%20Subsequence%20in%20Non-Increasing%20Order/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code>, hãy lấy một dãy con của mảng sao cho tổng các phần tử của dãy con <strong>lớn hơn nghiêm ngặt</strong> tổng các phần tử không được chọn trong dãy con đó.&nbsp;</p>

<p>Nếu có nhiều đáp án, hãy trả về dãy con có <strong>kích thước nhỏ nhất</strong>; nếu vẫn có nhiều đáp án, hãy trả về dãy con có <strong>tổng lớn nhất</strong> của tất cả các phần tử. Có thể thu được một dãy con của mảng bằng cách xóa một số phần tử (có thể không xóa phần tử nào) khỏi mảng.&nbsp;</p>

<p>Lưu ý rằng với các ràng buộc đã cho, đáp án được đảm bảo là <strong>duy nhất</strong>. Đồng thời, hãy trả về đáp án theo thứ tự <strong>không tăng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,10,9,8]
<strong>Đầu ra:</strong> [10,9] 
<strong>Giải thích:</strong> Các dãy con [10,9] và [10,8] đều có kích thước nhỏ nhất sao cho tổng các phần tử của chúng lớn hơn nghiêm ngặt tổng các phần tử không được chọn. Tuy nhiên, dãy con [10,9] có tổng các phần tử lớn nhất.&nbsp;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,4,7,6,7]
<strong>Đầu ra:</strong> [7,7,6] 
<strong>Giải thích:</strong> Dãy con [7,7] có tổng các phần tử bằng 14, không lớn hơn nghiêm ngặt tổng các phần tử không được chọn (14 = 4 + 4 + 6). Vì vậy, dãy con [7,6,7] là dãy nhỏ nhất thỏa mãn các điều kiện. Lưu ý rằng dãy con phải được trả về theo thứ tự không tăng.  
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 500$ cho phép liệt kê các dãy con, nhưng ta cũng cần dãy ngắn nhất, sau đó là dãy có tổng lớn nhất và được viết theo thứ tự không tăng.
>
> Để vượt qua phần bù với ít phần tử nhất, hãy chọn các giá trị lớn nhất trước. Duyệt mảng theo thứ tự giảm dần và dừng khi tổng đang có $t$ lớn hơn $s-t$; prefix đã thu thập chính là dãy con cần tìm.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp mảng $nums$ theo thứ tự giảm dần, sau đó thêm các phần tử vào mảng từ lớn nhất đến nhỏ nhất. Sau mỗi lần thêm, ta kiểm tra xem tổng các phần tử hiện tại có lớn hơn tổng các phần tử còn lại hay không. Nếu có, ta trả về mảng hiện tại.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSubsequence(self, nums: List[int]) -> List[int]:
        ans = []
        s, t = sum(nums), 0
        for x in sorted(nums, reverse=True):
            t += x
            ans.append(x)
            if t > s - t:
                break
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> minSubsequence(int[] nums) {
        Arrays.sort(nums);
        List<Integer> ans = new ArrayList<>();
        int s = Arrays.stream(nums).sum();
        int t = 0;
        for (int i = nums.length - 1; i >= 0; i--) {
            t += nums[i];
            ans.add(nums[i]);
            if (t > s - t) {
                break;
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
    vector<int> minSubsequence(vector<int>& nums) {
        sort(nums.rbegin(), nums.rend());
        int s = accumulate(nums.begin(), nums.end(), 0);
        int t = 0;
        vector<int> ans;
        for (int x : nums) {
            t += x;
            ans.push_back(x);
            if (t > s - t) {
                break;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minSubsequence(nums []int) (ans []int) {
	sort.Ints(nums)
	s, t := 0, 0
	for _, x := range nums {
		s += x
	}
	for i := len(nums) - 1; ; i-- {
		t += nums[i]
		ans = append(ans, nums[i])
		if t > s-t {
			return
		}
	}
}
```

#### TypeScript

```ts
function minSubsequence(nums: number[]): number[] {
    nums.sort((a, b) => b - a);
    const s = nums.reduce((r, c) => r + c);
    let t = 0;
    for (let i = 0; ; ++i) {
        t += nums[i];
        if (t > s - t) {
            return nums.slice(0, i + 1);
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn min_subsequence(mut nums: Vec<i32>) -> Vec<i32> {
        nums.sort_by(|a, b| b.cmp(a));
        let sum = nums.iter().sum::<i32>();
        let mut res = vec![];
        let mut t = 0;
        for num in nums.into_iter() {
            t += num;
            res.push(num);
            if t > sum - t {
                break;
            }
        }
        res
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
