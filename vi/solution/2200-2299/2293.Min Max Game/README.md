---
comments: true
difficulty: Easy
rating: 1241
source: Weekly Contest 296 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2293. Min Max Game](https://leetcode.com/problems/min-max-game)

[Tài liệu tiếng Trung](/solution/2200-2299/2293.Min%20Max%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> có độ dài là lũy thừa của <code>2</code>.</p>

<p>Hãy áp dụng thuật toán sau lên <code>nums</code>:</p>

<ol>
	<li>Gọi <code>n</code> là độ dài của <code>nums</code>. Nếu <code>n == 1</code>, <strong>kết thúc</strong> quá trình. Ngược lại, <strong>tạo</strong> một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>newNums</code> có độ dài <code>n / 2</code>.</li>
	<li>Với mọi chỉ số <strong>chẵn</strong> <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n / 2</code>, <strong>gán</strong> giá trị của <code>newNums[i]</code> bằng <code>min(nums[2 * i], nums[2 * i + 1])</code>.</li>
	<li>Với mọi chỉ số <strong>lẻ</strong> <code>i</code> thỏa mãn <code>0 &lt;= i &lt; n / 2</code>, <strong>gán</strong> giá trị của <code>newNums[i]</code> bằng <code>max(nums[2 * i], nums[2 * i + 1])</code>.</li>
	<li><strong>Thay thế</strong> mảng <code>nums</code> bằng <code>newNums</code>.</li>
	<li><strong>Lặp lại</strong> toàn bộ quá trình bắt đầu từ bước 1.</li>
</ol>

<p>Trả về <em>số cuối cùng còn lại trong </em><code>nums</code><em> sau khi áp dụng thuật toán.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2293.Min%20Max%20Game/images/example1drawio-1.png" style="width: 500px; height: 240px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,2,4,8,2,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Các mảng sau đây là kết quả của việc áp dụng lặp lại thuật toán.
Lần đầu: nums = [1,5,4,2]
Lần hai: nums = [1,4]
Lần ba: nums = [1]
1 là số cuối cùng còn lại, vì vậy ta trả về 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 3 vốn đã là số cuối cùng còn lại, vì vậy ta trả về 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1024</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>nums.length</code> là một lũy thừa của <code>2</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lượt thay thế các cặp phần tử liền kề bằng $\min$ hoặc $\max$ tùy theo chỉ số của cặp, cho đến khi chỉ còn lại một giá trị. Độ dài mảng là lũy thừa của hai và không vượt quá $1024$, nên mô phỏng là đủ.
>
> Ta ghi lượt tiếp theo vào nửa đầu của chính mảng đó: $n$ giảm đi một nửa sau mỗi lượt, và giá trị mới thứ $i$ được tạo từ $nums[2i]$ và $nums[2i+1]$. Phần tử còn lại là $nums[0]$.

<!-- thinking:end -->

Theo đề bài, ta có thể mô phỏng toàn bộ quá trình, và số còn lại sẽ là đáp án. Khi triển khai, ta không cần tạo thêm một mảng; có thể thao tác trực tiếp trên mảng ban đầu.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minMaxGame(self, nums: List[int]) -> int:
        n = len(nums)
        while n > 1:
            n >>= 1
            for i in range(n):
                a, b = nums[i << 1], nums[i << 1 | 1]
                nums[i] = min(a, b) if i % 2 == 0 else max(a, b)
        return nums[0]
```

#### Java

```java
class Solution {
    public int minMaxGame(int[] nums) {
        for (int n = nums.length; n > 1;) {
            n >>= 1;
            for (int i = 0; i < n; ++i) {
                int a = nums[i << 1], b = nums[i << 1 | 1];
                nums[i] = i % 2 == 0 ? Math.min(a, b) : Math.max(a, b);
            }
        }
        return nums[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minMaxGame(vector<int>& nums) {
        for (int n = nums.size(); n > 1;) {
            n >>= 1;
            for (int i = 0; i < n; ++i) {
                int a = nums[i << 1], b = nums[i << 1 | 1];
                nums[i] = i % 2 == 0 ? min(a, b) : max(a, b);
            }
        }
        return nums[0];
    }
};
```

#### Go

```go
func minMaxGame(nums []int) int {
	for n := len(nums); n > 1; {
		n >>= 1
		for i := 0; i < n; i++ {
			a, b := nums[i<<1], nums[i<<1|1]
			if i%2 == 0 {
				nums[i] = min(a, b)
			} else {
				nums[i] = max(a, b)
			}
		}
	}
	return nums[0]
}
```

#### TypeScript

```ts
function minMaxGame(nums: number[]): number {
    for (let n = nums.length; n > 1;) {
        n >>= 1;
        for (let i = 0; i < n; ++i) {
            const a = nums[i << 1];
            const b = nums[(i << 1) | 1];
            nums[i] = i % 2 == 0 ? Math.min(a, b) : Math.max(a, b);
        }
    }
    return nums[0];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_max_game(mut nums: Vec<i32>) -> i32 {
        let mut n = nums.len();
        while n != 1 {
            n >>= 1;
            for i in 0..n {
                nums[i] = (if (i & 1) == 1 { i32::max } else { i32::min })(
                    nums[i << 1],
                    nums[(i << 1) | 1],
                );
            }
        }
        nums[0]
    }
}
```

#### C

```c
#define min(a, b) (((a) < (b)) ? (a) : (b))
#define max(a, b) (((a) > (b)) ? (a) : (b))

int minMaxGame(int* nums, int numsSize) {
    while (numsSize != 1) {
        numsSize >>= 1;
        for (int i = 0; i < numsSize; i++) {
            int a = nums[i << 1];
            int b = nums[i << 1 | 1];
            nums[i] = i & 1 ? max(a, b) : min(a, b);
        }
    }
    return nums[0];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
