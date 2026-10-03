---
comments: true
difficulty: Easy
rating: 1223
source: Weekly Contest 318 Q1
tags:
    - Array
    - Two Pointers
    - Simulation
---

<!-- problem:start -->

# [2460. Apply Operations to an Array](https://leetcode.com/problems/apply-operations-to-an-array)

[中文文档](/solution/2400-2499/2460.Apply%20Operations%20to%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, có kích thước <code>n</code> và gồm các số nguyên <strong>không âm</strong>.</p>

<p>Bạn cần thực hiện <code>n - 1</code> thao tác trên mảng này. Trong thao tác <code>i<sup>th</sup></code> (<strong>đánh chỉ số từ 0</strong>), bạn sẽ thực hiện như sau với phần tử thứ <code>i<sup>th</sup></code> của <code>nums</code>:</p>

<ul>
	<li>Nếu <code>nums[i] == nums[i + 1]</code>, nhân <code>nums[i]</code> với <code>2</code> và gán <code>nums[i + 1]</code> bằng <code>0</code>. Nếu không, bỏ qua thao tác này.</li>
</ul>

<p>Sau khi thực hiện <strong>tất cả</strong> các thao tác, hãy <strong>chuyển</strong> tất cả số <code>0</code> về <strong>cuối</strong> mảng.</p>

<ul>
	<li>Ví dụ, mảng <code>[1,0,2,0,0,1]</code> sau khi chuyển tất cả số <code>0</code> về cuối sẽ trở thành <code>[1,2,1,0,0,0]</code>.</li>
</ul>

<p>Trả về <em>mảng sau cùng</em>.</p>

<p><strong>Lưu ý</strong> rằng các thao tác được thực hiện <strong>tuần tự</strong>, không phải đồng thời.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,1,1,0]
<strong>Đầu ra:</strong> [1,4,2,0,0,0]
<strong>Giải thích:</strong> Ta thực hiện các thao tác sau:
- i = 0: nums[0] và nums[1] không bằng nhau, nên bỏ qua thao tác này.
- i = 1: nums[1] và nums[2] bằng nhau, nhân nums[1] với 2 và thay đổi nums[2] thành 0. Mảng trở thành [1,<strong><u>4</u></strong>,<strong><u>0</u></strong>,1,1,0].
- i = 2: nums[2] và nums[3] không bằng nhau, nên bỏ qua thao tác này.
- i = 3: nums[3] và nums[4] bằng nhau, nhân nums[3] với 2 và thay đổi nums[4] thành 0. Mảng trở thành [1,4,0,<strong><u>2</u></strong>,<strong><u>0</u></strong>,0].
- i = 4: nums[4] và nums[5] bằng nhau, nhân nums[4] với 2 và thay đổi nums[5] thành 0. Mảng trở thành [1,4,0,2,<strong><u>0</u></strong>,<strong><u>0</u></strong>].
Sau đó, chuyển các số 0 về cuối, ta được mảng [1,4,2,0,0,0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1]
<strong>Đầu ra:</strong> [1,0]
<strong>Giải thích:</strong> Không thể thực hiện thao tác nào, ta chỉ cần chuyển số 0 về cuối.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 2000</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Với $n\le 2000$, duyệt từ trái sang phải: nếu hai phần tử kề nhau bằng nhau thì nhân đôi phần tử bên trái và gán phần tử bên phải bằng 0. Sau đó, dồn ổn định các giá trị khác 0 về đầu mảng. Ta cần hai lượt duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp theo mô tả của đề bài.

Trước hết, ta duyệt qua mảng $nums$. Với mỗi cặp phần tử kề nhau $nums[i]$ và $nums[i+1]$, nếu $nums[i] = nums[i+1]$, ta nhân đôi giá trị của $nums[i]$ và thay đổi giá trị của $nums[i+1]$ thành $0$.

Sau đó, ta tạo một mảng kết quả $ans$ có độ dài $n$, rồi lần lượt đưa tất cả phần tử khác 0 của $nums$ vào $ans$.

Cuối cùng, ta trả về mảng kết quả $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Không tính phần bộ nhớ dùng cho mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def applyOperations(self, nums: List[int]) -> List[int]:
        n = len(nums)
        for i in range(n - 1):
            if nums[i] == nums[i + 1]:
                nums[i] <<= 1
                nums[i + 1] = 0
        ans = [0] * n
        i = 0
        for x in nums:
            if x:
                ans[i] = x
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] applyOperations(int[] nums) {
        int n = nums.length;
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                nums[i] <<= 1;
                nums[i + 1] = 0;
            }
        }
        int[] ans = new int[n];
        int i = 0;
        for (int x : nums) {
            if (x > 0) {
                ans[i++] = x;
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
    vector<int> applyOperations(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n - 1; ++i) {
            if (nums[i] == nums[i + 1]) {
                nums[i] <<= 1;
                nums[i + 1] = 0;
            }
        }
        vector<int> ans(n);
        int i = 0;
        for (int& x : nums) {
            if (x) {
                ans[i++] = x;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func applyOperations(nums []int) []int {
	n := len(nums)
	for i := 0; i < n-1; i++ {
		if nums[i] == nums[i+1] {
			nums[i] <<= 1
			nums[i+1] = 0
		}
	}
	ans := make([]int, n)
	i := 0
	for _, x := range nums {
		if x > 0 {
			ans[i] = x
			i++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function applyOperations(nums: number[]): number[] {
    const n = nums.length;
    for (let i = 0; i < n - 1; ++i) {
        if (nums[i] === nums[i + 1]) {
            nums[i] <<= 1;
            nums[i + 1] = 0;
        }
    }
    const ans: number[] = Array(n).fill(0);
    let i = 0;
    for (const x of nums) {
        if (x !== 0) {
            ans[i++] = x;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn apply_operations(nums: Vec<i32>) -> Vec<i32> {
        let mut nums = nums;

        for i in 0..nums.len() - 1 {
            if nums[i] == nums[i + 1] {
                nums[i] <<= 1;
                nums[i + 1] = 0;
            }
        }

        let mut cur = 0;
        for i in 0..nums.len() {
            if nums[i] != 0 {
                nums.swap(i, cur);
                cur += 1;
            }
        }

        nums
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
