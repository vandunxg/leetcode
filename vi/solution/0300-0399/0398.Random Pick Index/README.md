---
comments: true
difficulty: Medium
tags:
    - Reservoir Sampling
    - Hash Table
    - Math
    - Randomized
---

<!-- problem:start -->

# [398. Random Pick Index](https://leetcode.com/problems/random-pick-index)

[中文文档](/solution/0300-0399/0398.Random%20Pick%20Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> có thể chứa các phần tử <strong>trùng lặp</strong>. Hãy chọn ngẫu nhiên chỉ số của một giá trị <code>target</code> cho trước. Có thể giả sử giá trị target luôn tồn tại trong mảng.</p>

<p>Hãy cài đặt class <code>Solution</code>:</p>

<ul>
	<li><code>Solution(int[] nums)</code> Khởi tạo object với mảng <code>nums</code>.</li>
	<li><code>int pick(int target)</code> Chọn ngẫu nhiên một chỉ số <code>i</code> trong <code>nums</code> sao cho <code>nums[i] == target</code>. Nếu có nhiều chỉ số thỏa mãn, mỗi chỉ số phải có xác suất được chọn như nhau.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;Solution&quot;, &quot;pick&quot;, &quot;pick&quot;, &quot;pick&quot;]
[[[1, 2, 3, 3, 3]], [3], [1], [3]]
<strong>Đầu ra</strong>
[null, 4, 0, 2]

<strong>Giải thích</strong>
Solution solution = new Solution([1, 2, 3, 3, 3]);
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(1); // It should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(3); // It should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>-2<sup>31</sup> &lt;= nums[i] &lt;= 2<sup>31</sup> - 1</code></li>
	<li><code>target</code> là một số nguyên có trong <code>nums</code>.</li>
	<li>Hàm <code>pick</code> được gọi nhiều nhất <code>10<sup>4</sup></code> lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Trả về một chỉ số của $target$ được chọn ngẫu nhiên với xác suất đồng đều. Lưu tất cả chỉ số cần $O(n)$ bộ nhớ; reservoir sampling xử lý các chỉ số theo luồng.
>
> Khi gặp kết quả khớp thứ $n$, thay đáp án hiện tại bằng chỉ số đó với xác suất $1/n$. Không cần lưu danh sách chỉ số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def __init__(self, nums: List[int]):
        self.nums = nums

    def pick(self, target: int) -> int:
        n = ans = 0
        for i, v in enumerate(self.nums):
            if v == target:
                n += 1
                x = random.randint(1, n)
                if x == n:
                    ans = i
        return ans


# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.pick(target)
```

#### Java

```java
class Solution {
    private int[] nums;
    private Random random = new Random();

    public Solution(int[] nums) {
        this.nums = nums;
    }

    public int pick(int target) {
        int n = 0, ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] == target) {
                ++n;
                int x = 1 + random.nextInt(n);
                if (x == n) {
                    ans = i;
                }
            }
        }
        return ans;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```

#### C++

```cpp
class Solution {
public:
    vector<int> nums;

    Solution(vector<int>& nums) {
        this->nums = nums;
    }

    int pick(int target) {
        int n = 0, ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == target) {
                ++n;
                int x = 1 + rand() % n;
                if (n == x) ans = i;
            }
        }
        return ans;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * int param_1 = obj->pick(target);
 */
```

#### Go

```go
type Solution struct {
	nums []int
}

func Constructor(nums []int) Solution {
	return Solution{nums}
}

func (this *Solution) Pick(target int) int {
	n, ans := 0, 0
	for i, v := range this.nums {
		if v == target {
			n++
			x := 1 + rand.Intn(n)
			if n == x {
				ans = i
			}
		}
	}
	return ans
}

/**
 * Your Solution object will be instantiated and called as such:
 * obj := Constructor(nums);
 * param_1 := obj.Pick(target);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
