---
comments: true
difficulty: Hard
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [3231. Minimum Number of Increasing Subsequence to Be Removed 🔒](https://leetcode.com/problems/minimum-number-of-increasing-subsequence-to-be-removed)

[中文文档](/solution/3200-3299/3231.Minimum%20Number%20of%20Increasing%20Subsequence%20to%20Be%20Removed/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>, bạn được phép thực hiện thao tác sau với số lần tùy ý:</p>

<ul>
	<li>Xóa một <strong>dãy con tăng chặt</strong> <span data-keyword="subsequence-array">subsequence</span> khỏi mảng.</li>
</ul>

<p>Nhiệm vụ của bạn là tìm <strong>số thao tác ít nhất</strong> cần thực hiện để làm <strong>mảng rỗng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,3,1,4,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta xóa các dãy con <code>[1, 2]</code>, <code>[3, 4]</code>, <code>[5]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,4,3,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác xóa một dãy con tăng chặt, và ta muốn số thao tác ít nhất. $n\le 10^5$ nên không thể liệt kê các cách phân hoạch. Số thao tác ít nhất bằng số chain được tạo ra khi luôn thêm phần tử vào chain có phần tử cuối tăng mà vẫn phù hợp, tức là bằng độ dài của dãy con không tăng dài nhất.
>
> Ta duy trì các phần tử cuối của các chain theo thứ tự không tăng. Với mỗi $x$, ta tìm kiếm nhị phân phần tử cuối đầu tiên $< x$, hoặc mở một chain mới. Số phần tử cuối chính là đáp án.

<!-- thinking:end -->

Ta duyệt mảng $\textit{nums}$ từ trái sang phải. Với mỗi phần tử $x$, ta cần tham lam thêm nó vào sau phần tử cuối của dãy trước đó nhỏ hơn $x$. Nếu không tìm thấy phần tử như vậy, điều đó có nghĩa là phần tử hiện tại $x$ nhỏ hơn mọi phần tử trong các dãy trước đó, và ta cần bắt đầu một dãy mới với $x$.

Từ phân tích này, ta nhận thấy các phần tử cuối của các dãy trước đó có thứ tự giảm dần. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm vị trí của phần tử đầu tiên trong các dãy trước đó nhỏ hơn $x$, rồi đặt $x$ vào vị trí đó.

Cuối cùng, ta trả về số dãy.

Độ phức tạp thời gian là $O(n \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        g = []
        for x in nums:
            l, r = 0, len(g)
            while l < r:
                mid = (l + r) >> 1
                if g[mid] < x:
                    r = mid
                else:
                    l = mid + 1
            if l == len(g):
                g.append(x)
            else:
                g[l] = x
        return len(g)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        List<Integer> g = new ArrayList<>();
        for (int x : nums) {
            int l = 0, r = g.size();
            while (l < r) {
                int mid = (l + r) >> 1;
                if (g.get(mid) < x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (l == g.size()) {
                g.add(x);
            } else {
                g.set(l, x);
            }
        }
        return g.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        vector<int> g;
        for (int x : nums) {
            int l = 0, r = g.size();
            while (l < r) {
                int mid = (l + r) >> 1;
                if (g[mid] < x) {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if (l == g.size()) {
                g.push_back(x);
            } else {
                g[l] = x;
            }
        }
        return g.size();
    }
};
```

#### Go

```go
func minOperations(nums []int) int {
	g := []int{}
	for _, x := range nums {
		l, r := 0, len(g)
		for l < r {
			mid := (l + r) >> 1
			if g[mid] < x {
				r = mid
			} else {
				l = mid + 1
			}
		}
		if l == len(g) {
			g = append(g, x)
		} else {
			g[l] = x
		}
	}
	return len(g)
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    const g: number[] = [];
    for (const x of nums) {
        let [l, r] = [0, g.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (g[mid] < x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        if (l === g.length) {
            g.push(x);
        } else {
            g[l] = x;
        }
    }
    return g.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>) -> i32 {
        let mut g = Vec::new();
        for &x in nums.iter() {
            let mut l = 0;
            let mut r = g.len();
            while l < r {
                let mid = (l + r) / 2;
                if g[mid] < x {
                    r = mid;
                } else {
                    l = mid + 1;
                }
            }
            if l == g.len() {
                g.push(x);
            } else {
                g[l] = x;
            }
        }
        g.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
