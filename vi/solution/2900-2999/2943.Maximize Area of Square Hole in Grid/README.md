---
comments: true
difficulty: Medium
rating: 1677
source: Biweekly Contest 118 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2943. Maximize Area of Square Hole in Grid](https://leetcode.com/problems/maximize-area-of-square-hole-in-grid)

[中文文档](/solution/2900-2999/2943.Maximize%20Area%20of%20Square%20Hole%20in%20Grid/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>n</code>, <code>m</code> và hai mảng số nguyên <code>hBars</code>, <code>vBars</code>. Lưới có <code>n + 2</code> thanh ngang và <code>m + 2</code> thanh dọc, tạo thành các ô đơn vị kích thước 1 x 1. Các thanh được đánh số bắt đầu từ <code>1</code>.</p>

<p>Bạn có thể <strong>gỡ bỏ</strong> một số thanh trong <code>hBars</code> khỏi các thanh ngang và một số thanh trong <code>vBars</code> khỏi các thanh dọc. Lưu ý rằng các thanh khác là cố định và không thể gỡ bỏ.</p>

<p>Hãy trả về một số nguyên biểu thị <strong>diện tích lớn nhất</strong> của một lỗ <em>hình vuông</em> trong lưới sau khi gỡ bỏ một số thanh (có thể không gỡ thanh nào).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2943.Maximize%20Area%20of%20Square%20Hole%20in%20Grid/images/screenshot-from-2023-11-05-22-40-25.png" style="width: 411px; height: 220px;" /></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">n = 2, m = 1, hBars = [2,3], vBars = [2]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Hình bên trái mô tả lưới ban đầu được tạo bởi các thanh. Các thanh ngang là <code>[1,2,3,4]</code>, còn các thanh dọc là&nbsp;<code>[1,2,3]</code>.</p>

<p>Một cách để tạo lỗ hình vuông lớn nhất là gỡ bỏ thanh ngang 2 và thanh dọc 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2943.Maximize%20Area%20of%20Square%20Hole%20in%20Grid/images/screenshot-from-2023-11-04-17-01-02.png" style="width: 368px; height: 145px;" /></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">n = 1, m = 1, hBars = [2], vBars = [2]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Để tạo lỗ hình vuông lớn nhất, ta gỡ bỏ thanh ngang 2 và thanh dọc 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2900-2999/2943.Maximize%20Area%20of%20Square%20Hole%20in%20Grid/images/unsaved-image-2.png" style="width: 648px; height: 218px;" /></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">n = 2, m = 3, hBars = [2,3], vBars = [2,4]</span></p>

<p><strong>Đầu ra: </strong><span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><span style="color: var(--text-secondary); font-size: 0.875rem;">Một cách để tạo lỗ hình vuông lớn nhất là gỡ bỏ thanh ngang 3 và thanh dọc 4.</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= m &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= hBars.length &lt;= 100</code></li>
	<li><code>2 &lt;= hBars[i] &lt;= n + 1</code></li>
	<li><code>1 &lt;= vBars.length &lt;= 100</code></li>
	<li><code>2 &lt;= vBars[i] &lt;= m + 1</code></li>
	<li>Tất cả giá trị trong <code>hBars</code> đều khác nhau.</li>
	<li>Tất cả giá trị trong <code>vBars</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Lỗ hình vuông bị giới hạn bởi dãy liên tiếp dài nhất gồm các thanh ngang có thể gỡ bỏ và dãy tương tự gồm các thanh dọc; cạnh hình vuông bằng một cộng với độ dài nhỏ hơn trong hai dãy đó. Vì $n$ và $m$ có thể lên tới $10^9$, ta không thể mô phỏng toàn bộ lưới; nhiều nhất chỉ có $100$ thanh có thể gỡ bỏ.
>
> Sắp xếp $hBars$ và $vBars$, duyệt dãy dài nhất có hiệu giữa hai phần tử kề nhau bằng $1$, cộng thêm một, lấy giá trị nhỏ hơn của hai cạnh rồi bình phương.

<!-- thinking:end -->

Về bản chất, bài toán yêu cầu tìm độ dài của dãy con tăng liên tiếp dài nhất trong mảng, sau đó cộng thêm $1$.

Ta định nghĩa hàm $f(\textit{nums})$ biểu thị độ dài của dãy con tăng liên tiếp dài nhất trong mảng $\textit{nums}$.

Với mảng $\textit{nums}$, trước tiên ta sắp xếp mảng, sau đó duyệt qua các phần tử. Nếu phần tử hiện tại $\textit{nums}[i]$ bằng phần tử trước đó $\textit{nums}[i - 1]$ cộng $1$, điều đó có nghĩa là phần tử hiện tại có thể được thêm vào dãy con tăng liên tiếp. Ngược lại, phần tử hiện tại không thể được thêm vào dãy con tăng liên tiếp, nên ta phải bắt đầu đếm lại độ dài dãy. Cuối cùng, ta trả về độ dài dãy con tăng liên tiếp cộng $1$.

Sau khi tìm được độ dài của các dãy con tăng liên tiếp dài nhất trong $\textit{hBars}$ và $\textit{vBars}$, ta lấy giá trị nhỏ hơn làm độ dài cạnh hình vuông, rồi tính diện tích hình vuông.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{hBars}$ hoặc $\textit{vBars}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximizeSquareHoleArea(
        self, n: int, m: int, hBars: List[int], vBars: List[int]
    ) -> int:
        def f(nums: List[int]) -> int:
            nums.sort()
            ans = cnt = 1
            for i in range(1, len(nums)):
                if nums[i] == nums[i - 1] + 1:
                    cnt += 1
                    ans = max(ans, cnt)
                else:
                    cnt = 1
            return ans + 1

        return min(f(hBars), f(vBars)) ** 2
```

#### Java

```java
class Solution {
    public int maximizeSquareHoleArea(int n, int m, int[] hBars, int[] vBars) {
        int x = Math.min(f(hBars), f(vBars));
        return x * x;
    }

    private int f(int[] nums) {
        Arrays.sort(nums);
        int ans = 1, cnt = 1;
        for (int i = 1; i < nums.length; ++i) {
            if (nums[i] == nums[i - 1] + 1) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 1;
            }
        }
        return ans + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximizeSquareHoleArea(int n, int m, vector<int>& hBars, vector<int>& vBars) {
        auto f = [](vector<int>& nums) {
            int ans = 1, cnt = 1;
            sort(nums.begin(), nums.end());
            for (int i = 1; i < nums.size(); ++i) {
                if (nums[i] == nums[i - 1] + 1) {
                    ans = max(ans, ++cnt);
                } else {
                    cnt = 1;
                }
            }
            return ans + 1;
        };
        int x = min(f(hBars), f(vBars));
        return x * x;
    }
};
```

#### Go

```go
func maximizeSquareHoleArea(n int, m int, hBars []int, vBars []int) int {
	f := func(nums []int) int {
		sort.Ints(nums)
		ans, cnt := 1, 1
		for i, x := range nums[1:] {
			if x == nums[i]+1 {
				cnt++
				ans = max(ans, cnt)
			} else {
				cnt = 1
			}
		}
		return ans + 1
	}
	x := min(f(hBars), f(vBars))
	return x * x
}
```

#### TypeScript

```ts
function maximizeSquareHoleArea(n: number, m: number, hBars: number[], vBars: number[]): number {
    const f = (nums: number[]): number => {
        nums.sort((a, b) => a - b);
        let [ans, cnt] = [1, 1];
        for (let i = 1; i < nums.length; ++i) {
            if (nums[i] === nums[i - 1] + 1) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 1;
            }
        }
        return ans + 1;
    };
    return Math.min(f(hBars), f(vBars)) ** 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn maximize_square_hole_area(n: i32, m: i32, h_bars: Vec<i32>, v_bars: Vec<i32>) -> i32 {
        let f = |nums: &mut Vec<i32>| -> i32 {
            let mut ans = 1;
            let mut cnt = 1;
            nums.sort();
            for i in 1..nums.len() {
                if nums[i] == nums[i - 1] + 1 {
                    cnt += 1;
                    ans = ans.max(cnt);
                } else {
                    cnt = 1;
                }
            }
            ans + 1
        };

        let mut h_bars = h_bars;
        let mut v_bars = v_bars;
        let x = f(&mut h_bars).min(f(&mut v_bars));
        x * x
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
