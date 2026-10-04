---
comments: true
difficulty: Medium
rating: 1489
source: Weekly Contest 453 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [3576. Transform Array to All Equal Elements](https://leetcode.com/problems/transform-array-to-all-equal-elements)

[中文文档](/solution/3500-3599/3576.Transform%20Array%20to%20All%20Equal%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> có kích thước <code>n</code>, chỉ chứa <code>1</code> và <code>-1</code>, cùng một số nguyên <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác sau nhiều nhất <code>k</code> lần:</p>

<ul>
	<li>
	<p>Chọn một chỉ số <code>i</code> (<code>0 &lt;= i &lt; n - 1</code>), rồi <strong>nhân</strong> cả <code>nums[i]</code> và <code>nums[i + 1]</code> với <code>-1</code>.</p>
	</li>
</ul>

<p><strong>Lưu ý</strong> rằng bạn có thể chọn cùng một chỉ số <code data-end="459" data-start="456">i</code> nhiều hơn một lần trong các thao tác <strong>khác nhau</strong>.</p>

<p>Trả về <code>true</code> nếu có thể làm cho tất cả phần tử của mảng <strong>bằng nhau</strong> sau nhiều nhất <code>k</code> thao tác, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,1,-1,1], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể làm cho tất cả phần tử trong mảng bằng nhau bằng 2 thao tác như sau:</p>

<ul>
	<li>Chọn chỉ số <code>i = 1</code>, rồi nhân cả <code>nums[1]</code> và <code>nums[2]</code> với -1. Khi đó <code>nums = [1,1,-1,-1,1]</code>.</li>
	<li>Chọn chỉ số <code>i = 2</code>, rồi nhân cả <code>nums[2]</code> và <code>nums[3]</code> với -1. Khi đó <code>nums = [1,1,1,1,1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,-1,-1,1,1,1], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể làm cho tất cả phần tử của mảng bằng nhau bằng nhiều nhất 5 thao tác.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> là -1 hoặc 1.</li>
	<li><code>1 &lt;= k &lt;= n</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc đổi dấu của hai phần tử kề nhau tương đương với một phép chuyển vị các dấu trừ kề nhau. Các mảng hằng duy nhất có thể đạt được là mảng gồm toàn $nums[0]$ hoặc toàn $-nums[0]$.
>
> Duyệt từ trái sang phải: nếu dấu hiện tại khác với đích, thực hiện phép đổi dấu tại đây (khi đó phần còn lại bị đảo dấu) và tăng bộ đếm. Chấp nhận nếu phần tử cuối cùng khớp và số lần thực hiện không vượt quá $k$. Thử cả hai đích.

<!-- thinking:end -->

Theo mô tả bài toán, để làm cho tất cả phần tử trong mảng bằng nhau, mọi phần tử phải là $\textit{nums}[0]$ hoặc $-\textit{nums}[0]$. Vì vậy, ta xây dựng hàm $\textit{check}$ để xác định liệu có thể biến đổi mảng thành mảng mà mọi phần tử đều bằng $\textit{target}$ bằng nhiều nhất $k$ thao tác hay không.

Ý tưởng của hàm này là duyệt qua mảng và đếm số thao tác cần thực hiện. Mỗi phần tử được thay đổi một lần hoặc không thay đổi. Nếu phần tử hiện tại bằng giá trị đích, ta không cần sửa đổi và tiếp tục với phần tử kế tiếp. Nếu phần tử hiện tại khác giá trị đích, ta cần thực hiện một thao tác, tăng bộ đếm và đảo dấu, cho biết các phần tử tiếp theo cần thực hiện thao tác ngược lại.

Sau khi duyệt xong, nếu bộ đếm nhỏ hơn hoặc bằng $k$ và dấu của phần tử cuối cùng khớp với giá trị đích, trả về $\textit{true}$; ngược lại, trả về $\textit{false}$.

Đáp án cuối cùng là kết quả của $\textit{check}(\textit{nums}[0], k)$ hoặc $\textit{check}(-\textit{nums}[0], k)$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canMakeEqual(self, nums: List[int], k: int) -> bool:
        def check(target: int, k: int) -> bool:
            cnt, sign = 0, 1
            for i in range(len(nums) - 1):
                x = nums[i] * sign
                if x == target:
                    sign = 1
                else:
                    sign = -1
                    cnt += 1
            return cnt <= k and nums[-1] * sign == target

        return check(nums[0], k) or check(-nums[0], k)
```

#### Java

```java
class Solution {
    public boolean canMakeEqual(int[] nums, int k) {
        return check(nums, nums[0], k) || check(nums, -nums[0], k);
    }

    private boolean check(int[] nums, int target, int k) {
        int cnt = 0, sign = 1;
        for (int i = 0; i < nums.length - 1; ++i) {
            int x = nums[i] * sign;
            if (x == target) {
                sign = 1;
            } else {
                sign = -1;
                ++cnt;
            }
        }
        return cnt <= k && nums[nums.length - 1] * sign == target;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canMakeEqual(vector<int>& nums, int k) {
        auto check = [&](int target, int k) -> bool {
            int n = nums.size();
            int cnt = 0, sign = 1;
            for (int i = 0; i < n - 1; ++i) {
                int x = nums[i] * sign;
                if (x == target) {
                    sign = 1;
                } else {
                    sign = -1;
                    ++cnt;
                }
            }
            return cnt <= k && nums[n - 1] * sign == target;
        };
        return check(nums[0], k) || check(-nums[0], k);
    }
};
```

#### Go

```go
func canMakeEqual(nums []int, k int) bool {
	check := func(target, k int) bool {
		cnt, sign := 0, 1
		for i := 0; i < len(nums)-1; i++ {
			x := nums[i] * sign
			if x == target {
				sign = 1
			} else {
				sign = -1
				cnt++
			}
		}
		return cnt <= k && nums[len(nums)-1]*sign == target
	}
	return check(nums[0], k) || check(-nums[0], k)
}
```

#### TypeScript

```ts
function canMakeEqual(nums: number[], k: number): boolean {
    function check(target: number, k: number): boolean {
        let [cnt, sign] = [0, 1];
        for (let i = 0; i < nums.length - 1; i++) {
            const x = nums[i] * sign;
            if (x === target) {
                sign = 1;
            } else {
                sign = -1;
                cnt++;
            }
        }
        return cnt <= k && nums[nums.length - 1] * sign === target;
    }

    return check(nums[0], k) || check(-nums[0], k);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_make_equal(nums: Vec<i32>, k: i32) -> bool {
        fn check(target: i32, k: i32, nums: &Vec<i32>) -> bool {
            let mut cnt = 0;
            let mut sign = 1;
            for i in 0..nums.len() - 1 {
                let x = nums[i] * sign;
                if x == target {
                    sign = 1;
                } else {
                    sign = -1;
                    cnt += 1;
                }
            }
            cnt <= k && nums[nums.len() - 1] * sign == target
        }

        check(nums[0], k, &nums) || check(-nums[0], k, &nums)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
