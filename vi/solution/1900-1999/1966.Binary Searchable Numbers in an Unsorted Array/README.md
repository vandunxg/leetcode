---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Binary Search
    - Monotonic Stack
---

<!-- problem:start -->

# [1966. Binary Searchable Numbers in an Unsorted Array 🔒](https://leetcode.com/problems/binary-searchable-numbers-in-an-unsorted-array)

[中文文档](/solution/1900-1999/1966.Binary%20Searchable%20Numbers%20in%20an%20Unsorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy xét một hàm triển khai một thuật toán <strong>tương tự</strong> như <a href="https://leetcode.com/explore/learn/card/binary-search/" target="_blank">Tìm kiếm nhị phân</a>. Hàm có hai tham số đầu vào: <code>sequence</code> là một dãy số nguyên, còn <code>target</code> là một giá trị nguyên. Mục đích của hàm là tìm xem <code>target</code> có tồn tại trong <code>sequence</code> hay không.</p>

<p>Mã giả của hàm như sau:</p>

<pre>
func(sequence, target)
  while sequence is not empty
    <strong>randomly</strong> choose an element from sequence as the pivot
    if pivot = target, return <strong>true</strong>
    else if pivot &lt; target, remove pivot and all elements to its left from the sequence
    else, remove pivot and all elements to its right from the sequence
  end while
  return <strong>false</strong>
</pre>

<p>Khi <code>sequence</code> đã được sắp xếp, hàm hoạt động chính xác với <strong>mọi</strong> giá trị. Khi <code>sequence</code> chưa được sắp xếp, hàm không hoạt động chính xác với mọi giá trị, nhưng vẫn có thể hoạt động với <strong>một số</strong> giá trị.</p>

<p>Cho một mảng số nguyên <code>nums</code> biểu diễn <code>sequence</code>, trong đó các phần tử là những số <strong>khác nhau</strong> và mảng <strong>có thể đã hoặc chưa được sắp xếp</strong>, hãy trả về <em>số lượng giá trị <strong>chắc chắn</strong> được tìm thấy khi sử dụng hàm, với <strong>mọi cách</strong> chọn pivot khả dĩ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7]
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>:
Việc tìm giá trị 7 chắc chắn sẽ thành công.
Vì sequence chỉ có một phần tử, 7 sẽ được chọn làm pivot. Do pivot bằng target, hàm sẽ trả về true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [-1,5,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích</strong>:
Việc tìm giá trị -1 chắc chắn sẽ thành công.
Nếu -1 được chọn làm pivot, hàm sẽ trả về true.
Nếu 5 được chọn làm pivot, 5 và 2 sẽ bị xóa. Ở vòng lặp tiếp theo, sequence chỉ còn -1 và hàm sẽ trả về true.
Nếu 2 được chọn làm pivot, 2 sẽ bị xóa. Ở vòng lặp tiếp theo, sequence sẽ gồm -1 và 5. Dù số nào được chọn làm pivot tiếp theo, hàm cũng sẽ tìm thấy -1 và trả về true.

Việc tìm giá trị 5 KHÔNG chắc chắn sẽ thành công.
Nếu 2 được chọn làm pivot, -1, 5 và 2 sẽ bị xóa. Sequence trở nên rỗng và hàm sẽ trả về false.

Việc tìm giá trị 2 KHÔNG chắc chắn sẽ thành công.
Nếu 5 được chọn làm pivot, 5 và 2 sẽ bị xóa. Ở vòng lặp tiếp theo, sequence chỉ còn -1 và hàm sẽ trả về false.

Vì chỉ có -1 là chắc chắn được tìm thấy, ta cần trả về 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các giá trị của <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu <code>nums</code> có <strong>phần tử trùng lặp</strong>, bạn có thay đổi thuật toán không? Nếu có thì thay đổi như thế nào?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân loại bỏ một phía của điểm giữa. Một giá trị chỉ có thể được tìm thấy nếu nó lớn hơn mọi phần tử bên trái và nhỏ hơn mọi phần tử bên phải; nếu không, nửa chứa giá trị đó có thể bị loại bỏ do chọn pivot không phù hợp.
>
> Một lượt duyệt từ trái sang phải để ghi nhận giá trị lớn nhất của tiền tố và một lượt duyệt từ phải sang trái để ghi nhận giá trị nhỏ nhất của hậu tố sẽ đánh dấu các vị trí không hợp lệ; sau đó đếm các vị trí còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def binarySearchableNumbers(self, nums: List[int]) -> int:
        n = len(nums)
        ok = [1] * n
        mx, mi = -1000000, 1000000
        for i, x in enumerate(nums):
            if x < mx:
                ok[i] = 0
            else:
                mx = x
        for i in range(n - 1, -1, -1):
            if nums[i] > mi:
                ok[i] = 0
            else:
                mi = nums[i]
        return sum(ok)
```

#### Java

```java
class Solution {
    public int binarySearchableNumbers(int[] nums) {
        int n = nums.length;
        int[] ok = new int[n];
        Arrays.fill(ok, 1);
        int mx = -1000000, mi = 1000000;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < mx) {
                ok[i] = 0;
            }
            mx = Math.max(mx, nums[i]);
        }
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (nums[i] > mi) {
                ok[i] = 0;
            }
            mi = Math.min(mi, nums[i]);
            ans += ok[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int binarySearchableNumbers(vector<int>& nums) {
        int n = nums.size();
        vector<int> ok(n, 1);
        int mx = -1000000, mi = 1000000;
        for (int i = 0; i < n; ++i) {
            if (nums[i] < mx) {
                ok[i] = 0;
            }
            mx = max(mx, nums[i]);
        }
        int ans = 0;
        for (int i = n - 1; i >= 0; --i) {
            if (nums[i] > mi) {
                ok[i] = 0;
            }
            mi = min(mi, nums[i]);
            ans += ok[i];
        }
        return ans;
    }
};
```

#### Go

```go
func binarySearchableNumbers(nums []int) (ans int) {
	n := len(nums)
	ok := make([]int, n)
	for i := range ok {
		ok[i] = 1
	}
	mx, mi := -1000000, 1000000
	for i, x := range nums {
		if x < mx {
			ok[i] = 0
		} else {
			mx = x
		}
	}
	for i := n - 1; i >= 0; i-- {
		if nums[i] > mi {
			ok[i] = 0
		} else {
			mi = nums[i]
		}
		ans += ok[i]
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
