---
comments: true
difficulty: Easy
rating: 1372
source: Biweekly Contest 115 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2899. Last Visited Integers](https://leetcode.com/problems/last-visited-integers)

[中文文档](/solution/2800-2899/2899.Last%20Visited%20Integers/README.md)

## Mô tả

<!-- description:start -->
<p>Cho một mảng số nguyên <code>nums</code>, trong đó <code>nums[i]</code> là một số nguyên dương hoặc <code>-1</code>. Với mỗi <code>-1</code>, chúng ta cần tìm số nguyên dương tương ứng, gọi là số nguyên được truy cập gần nhất.</p>

<p>Để thực hiện mục tiêu này, hãy định nghĩa hai mảng rỗng: <code>seen</code> và <code>ans</code>.</p>

<p>Bắt đầu duyệt mảng <code>nums</code> từ đầu.</p>

<ul>
	<li>Nếu gặp một số nguyên dương, chèn số đó vào <strong>đầu</strong> của <code>seen</code>.</li>
	<li>Nếu gặp <code>-1</code>, gọi <code>k</code> là số lượng <code>-1</code> <strong>liên tiếp</strong> đã gặp tính cả <code>-1</code> hiện tại,
	<ul>
		<li>Nếu <code>k</code> nhỏ hơn hoặc bằng độ dài của <code>seen</code>, thêm phần tử thứ <code>k</code> của <code>seen</code> vào <code>ans</code>.</li>
		<li>Nếu <code>k</code> lớn hơn độ dài của <code>seen</code>, thêm <code>-1</code> vào <code>ans</code>.</li>
	</ul>
	</li>
</ul>

<p>Trả về mảng <em> </em><code>ans</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,-1,-1,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bắt đầu với <code>seen = []</code> và <code>ans = []</code>.</p>

<ol>
	<li>Xử lý <code>nums[0]</code>: Phần tử đầu tiên trong nums là <code>1</code>. Chèn nó vào đầu của <code>seen</code>. Khi đó, <code>seen == [1]</code>.</li>
	<li>Xử lý <code>nums[1]</code>: Phần tử tiếp theo là <code>2</code>. Chèn nó vào đầu của <code>seen</code>. Khi đó, <code>seen == [2, 1]</code>.</li>
	<li>Xử lý <code>nums[2]</code>: Phần tử tiếp theo là <code>-1</code>. Đây là lần đầu xuất hiện <code>-1</code>, nên <code>k == 1</code>. Ta tìm phần tử đầu tiên trong seen. Thêm <code>2</code> vào <code>ans</code>. Khi đó, <code>ans == [2]</code>.</li>
	<li>Xử lý <code>nums[3]</code>: Một <code>-1</code> khác. Đây là <code>-1</code> liên tiếp thứ hai, nên <code>k == 2</code>. Phần tử thứ hai trong <code>seen</code> là <code>1</code>, vì vậy ta thêm <code>1</code> vào <code>ans</code>. Khi đó, <code>ans == [2, 1]</code>.</li>
	<li>Xử lý <code>nums[4]</code>: Lại một <code>-1</code>, là phần tử thứ ba liên tiếp, nên <code>k = 3</code>. Tuy nhiên, <code>seen</code> chỉ có hai phần tử (<code>[2, 1]</code>). Vì <code>k</code> lớn hơn số phần tử của <code>seen</code>, ta thêm <code>-1</code> vào <code>ans</code>. Cuối cùng, <code>ans == [2, 1, -1]</code>.</li>
</ol>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,-1,2,-1,-1]</span></p>

<p><strong>Đầu ra:</strong><span class="example-io"> [1,2,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Bắt đầu với <code>seen = []</code> và <code>ans = []</code>.</p>

<ol>
	<li>Xử lý <code>nums[0]</code>: Phần tử đầu tiên trong nums là <code>1</code>. Chèn nó vào đầu của <code>seen</code>. Khi đó, <code>seen == [1]</code>.</li>
	<li>Xử lý <code>nums[1]</code>: Phần tử tiếp theo là <code>-1</code>. Đây là lần đầu xuất hiện <code>-1</code>, nên <code>k == 1</code>. Ta tìm phần tử đầu tiên trong <code>seen</code>, đó là <code>1</code>. Thêm <code>1</code> vào <code>ans</code>. Khi đó, <code>ans == [1]</code>.</li>
	<li>Xử lý <code>nums[2]</code>: Phần tử tiếp theo là <code>2</code>. Chèn nó vào đầu của <code>seen</code>. Khi đó, <code>seen == [2, 1]</code>.</li>
	<li>Xử lý <code>nums[3]</code>: Phần tử tiếp theo là <code>-1</code>. <code>-1</code> này không liên tiếp với <code>-1</code> đầu tiên vì ở giữa có <code>2</code>. Do đó, <code>k</code> được đặt lại thành <code>1</code>. Phần tử đầu tiên trong <code>seen</code> là <code>2</code>, nên ta thêm <code>2</code> vào <code>ans</code>. Khi đó, <code>ans == [1, 2]</code>.</li>
	<li>Xử lý <code>nums[4]</code>: Lại một <code>-1</code>. Nó liên tiếp với <code>-1</code> trước đó, nên <code>k == 2</code>. Phần tử thứ hai trong <code>seen</code> là <code>1</code>, ta thêm <code>1</code> vào <code>ans</code>. Cuối cùng, <code>ans == [1, 2, 1]</code>.</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>nums[i] == -1</code> hoặc <code>1 &lt;= nums[i]&nbsp;&lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các số nguyên dương được thêm vào $seen$; một đoạn gồm các số $-1$ liên tiếp biểu thị giá trị thứ $k$ tính từ số nguyên được truy cập gần nhất. Bộ đếm $k$ tăng khi gặp $-1$ và được đặt lại khi gặp số nguyên dương; nếu $k$ vượt quá độ dài của $seen$, đáp án là $-1$.

<!-- thinking:end -->

Ta mô phỏng trực tiếp theo mô tả của đề bài.

Định nghĩa một mảng $\textit{seen}$ để lưu các số nguyên dương đã gặp, và một mảng $\textit{ans}$ để lưu đáp án. Ta cũng cần một biến $k$ để ghi nhận số lượng $-1$ liên tiếp.

Ta duyệt mảng $\textit{nums}$:

- Nếu phần tử hiện tại $x = -1$, tăng $k$ lên 1. Nếu $k$ lớn hơn độ dài của $\textit{seen}$, thêm $-1$ vào $\textit{ans}$; ngược lại, thêm phần tử thứ $k$ tính từ cuối của $\textit{seen}$ vào $\textit{ans}$.
- Nếu phần tử hiện tại $x$ là số nguyên dương, đặt lại $k$ về 0 và thêm $x$ vào cuối $\textit{seen}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastVisitedIntegers(self, nums: List[int]) -> List[int]:
        seen = []
        ans = []
        k = 0
        for x in nums:
            if x == -1:
                k += 1
                ans.append(-1 if k > len(seen) else seen[-k])
            else:
                k = 0
                seen.append(x)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> lastVisitedIntegers(int[] nums) {
        List<Integer> seen = new ArrayList<>();
        List<Integer> ans = new ArrayList<>();
        int k = 0;
        for (int x : nums) {
            if (x == -1) {
                if (++k > seen.size()) {
                    ans.add(-1);
                } else {
                    ans.add(seen.get(seen.size() - k));
                }
            } else {
                k = 0;
                seen.add(x);
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
    vector<int> lastVisitedIntegers(vector<int>& nums) {
        vector<int> seen;
        vector<int> ans;
        int k = 0;

        for (int x : nums) {
            if (x == -1) {
                if (++k > seen.size()) {
                    ans.push_back(-1);
                } else {
                    ans.push_back(seen[seen.size() - k]);
                }
            } else {
                k = 0;
                seen.push_back(x);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func lastVisitedIntegers(nums []int) []int {
	seen := []int{}
	ans := []int{}
	k := 0

	for _, x := range nums {
		if x == -1 {
			k++
			if k > len(seen) {
				ans = append(ans, -1)
			} else {
				ans = append(ans, seen[len(seen)-k])
			}
		} else {
			k = 0
			seen = append(seen, x)
		}
	}

	return ans
}
```

#### TypeScript

```ts
function lastVisitedIntegers(nums: number[]): number[] {
    const seen: number[] = [];
    const ans: number[] = [];
    let k = 0;

    for (const x of nums) {
        if (x === -1) {
            if (++k > seen.length) {
                ans.push(-1);
            } else {
                ans.push(seen.at(-k)!);
            }
        } else {
            k = 0;
            seen.push(x);
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn last_visited_integers(nums: Vec<i32>) -> Vec<i32> {
        let mut seen: Vec<i32> = Vec::new();
        let mut ans: Vec<i32> = Vec::new();
        let mut k: i32 = 0;

        for x in nums {
            if x == -1 {
                k += 1;
                if k as usize > seen.len() {
                    ans.push(-1);
                } else {
                    ans.push(seen[seen.len() - k as usize]);
                }
            } else {
                k = 0;
                seen.push(x);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
