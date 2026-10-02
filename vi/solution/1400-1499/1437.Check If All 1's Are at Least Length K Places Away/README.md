---
comments: true
difficulty: Easy
rating: 1193
source: Weekly Contest 187 Q2
tags:
    - Array
---

<!-- problem:start -->

# [1437. Check If All 1's Are at Least Length K Places Away](https://leetcode.com/problems/check-if-all-1s-are-at-least-length-k-places-away)

[Tài liệu tiếng Trung](/solution/1400-1499/1437.Check%20If%20All%201%27s%20Are%20at%20Least%20Length%20K%20Places%20Away/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng nhị phân <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <code>true</code><em> nếu mọi </em><code>1</code><em> cách nhau ít nhất </em><code>k</code><em> vị trí, nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1437.Check%20If%20All%201%27s%20Are%20at%20Least%20Length%20K%20Places%20Away/images/sample_1_1791.png" style="width: 428px; height: 181px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,0,0,0,1,0,0,1], k = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Mỗi cặp số 1 cách nhau ít nhất 2 vị trí.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1400-1499/1437.Check%20If%20All%201%27s%20Are%20at%20Least%20Length%20K%20Places%20Away/images/sample_2_1791.png" style="width: 320px; height: 173px;" />
<pre>
<strong>Đầu vào:</strong> nums = [1,0,0,1,0,1], k = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Số 1 thứ hai và thứ ba chỉ cách nhau một vị trí.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= k &lt;= nums.length</code></li>
	<li><code>nums[i]</code> là <code>0</code> hoặc <code>1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 10^5$. Ghi nhớ số $1$ trước đó ở chỉ số $j$. Khi gặp một số $1$ mới, khoảng cách là $i-j-1$; trả về false nếu khoảng cách này nhỏ hơn $k$. Chỉ cần duyệt qua mảng một lần.

<!-- thinking:end -->

Ta có thể duyệt qua mảng <code>nums</code> và dùng biến $j$ để ghi lại chỉ số của số $1$ trước đó. Khi phần tử ở vị trí hiện tại $i$ là $1$, ta chỉ cần kiểm tra xem $i - j - 1$ có nhỏ hơn $k$ hay không. Nếu nhỏ hơn $k$, nghĩa là tồn tại một cặp số $1$ có ít hơn $k$ số 0 nằm giữa chúng, nên ta trả về $\text{false}$. Ngược lại, ta cập nhật $j$ thành $i$ và tiếp tục duyệt mảng.

Sau khi duyệt xong, ta trả về $\text{true}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kLengthApart(self, nums: List[int], k: int) -> bool:
        j = -inf
        for i, x in enumerate(nums):
            if x:
                if i - j - 1 < k:
                    return False
                j = i
        return True
```

#### Java

```java
class Solution {
    public boolean kLengthApart(int[] nums, int k) {
        int j = -(k + 1);
        for (int i = 0; i < nums.length; ++i) {
            if (nums[i] == 1) {
                if (i - j - 1 < k) {
                    return false;
                }
                j = i;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool kLengthApart(vector<int>& nums, int k) {
        int j = -(k + 1);
        for (int i = 0; i < nums.size(); ++i) {
            if (nums[i] == 1) {
                if (i - j - 1 < k) {
                    return false;
                }
                j = i;
            }
        }
        return true;
    }
};
```

#### Go

```go
func kLengthApart(nums []int, k int) bool {
	j := -(k + 1)
	for i, x := range nums {
		if x == 1 {
			if i-j-1 < k {
				return false
			}
			j = i
		}
	}
	return true
}
```

#### TypeScript

```ts
function kLengthApart(nums: number[], k: number): boolean {
    let j = -(k + 1);
    for (let i = 0; i < nums.length; ++i) {
        if (nums[i] === 1) {
            if (i - j - 1 < k) {
                return false;
            }
            j = i;
        }
    }
    return true;
}
```

#### Rust

```rust
impl Solution {
    pub fn k_length_apart(nums: Vec<i32>, k: i32) -> bool {
        let mut j = -(k + 1);
        for (i, &x) in nums.iter().enumerate() {
            if x == 1 {
                if (i as i32) - j - 1 < k {
                    return false;
                }
                j = i as i32;
            }
        }
        true
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
