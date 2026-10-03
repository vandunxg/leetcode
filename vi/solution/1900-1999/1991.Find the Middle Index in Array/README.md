---
comments: true
difficulty: Easy
rating: 1302
source: Biweekly Contest 60 Q1
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [1991. Find the Middle Index in Array](https://leetcode.com/problems/find-the-middle-index-in-array)

[中文文档](/solution/1900-1999/1991.Find%20the%20Middle%20Index%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được <strong>đánh chỉ số từ 0</strong>, hãy tìm <strong>ngoài cùng bên trái</strong> <code>middleIndex</code> (tức là chỉ số nhỏ nhất trong tất cả các chỉ số có thể có).</p>

<p><code>middleIndex</code> là một chỉ số sao cho <code>nums[0] + nums[1] + ... + nums[middleIndex-1] == nums[middleIndex+1] + nums[middleIndex+2] + ... + nums[nums.length-1]</code>.</p>

<p>Nếu <code>middleIndex == 0</code>, tổng bên trái được xem là <code>0</code>. Tương tự, nếu <code>middleIndex == nums.length - 1</code>, tổng bên phải được xem là <code>0</code>.</p>

<p>Trả về <em><strong>ngoài cùng bên trái</strong> </em><code>middleIndex</code><em> thỏa mãn điều kiện, hoặc </em><code>-1</code><em> nếu không có chỉ số nào như vậy</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,-1,<u>8</u>,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Tổng các số trước chỉ số 3 là: 2 + 3 + -1 = 4
Tổng các số sau chỉ số 3 là: 4 = 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-1,<u>4</u>]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Tổng các số trước chỉ số 2 là: 1 + -1 = 0
Tổng các số sau chỉ số 2 là: 0
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,5]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có middleIndex hợp lệ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Lưu ý:</strong> Bài này giống với bài 724: <a href="https://leetcode.com/problems/find-pivot-index/" target="_blank">https://leetcode.com/problems/find-pivot-index/</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Một chỉ số giữa cân bằng tổng bên trái và bên phải. Sau khi tính tổng toàn bộ, ta duyệt từ trái sang phải: trừ giá trị hiện tại khỏi tổng bên phải, so sánh, rồi cộng giá trị đó vào tổng bên trái.
>
> Không cần tạo mảng tổng tiền tố.

<!-- thinking:end -->

Ta định nghĩa hai biến $l$ và $r$, lần lượt biểu diễn tổng các phần tử ở bên trái và bên phải của chỉ số $i$ trong mảng $\textit{nums}$. Ban đầu, $l = 0$ và $r = \sum_{i = 0}^{n - 1} \textit{nums}[i]$.

Ta duyệt mảng $\textit{nums}$, với số hiện tại $x$, cập nhật $r = r - x$. Nếu $l = r$ tại thời điểm này, điều đó có nghĩa là chỉ số $i$ hiện tại là middle index, và ta trả về chỉ số đó ngay. Nếu không, ta cập nhật $l = l + x$ rồi tiếp tục với phần tử kế tiếp.

Nếu duyệt hết mảng mà không tìm thấy middle index, trả về $-1$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

Bài toán liên quan:

- [0724. Find Pivot Index](https://github.com/doocs/leetcode/blob/main/solution/0700-0799/0724.Find%20Pivot%20Index/README_EN.md)
- [2574. Left and Right Sum Differences](https://github.com/doocs/leetcode/blob/main/solution/2500-2599/2574.Left%20and%20Right%20Sum%20Differences/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMiddleIndex(self, nums: List[int]) -> int:
        l, r = 0, sum(nums)
        for i, x in enumerate(nums):
            r -= x
            if l == r:
                return i
            l += x
        return -1
```

#### Java

```java
class Solution {
    public int findMiddleIndex(int[] nums) {
        int l = 0, r = Arrays.stream(nums).sum();
        for (int i = 0; i < nums.length; ++i) {
            r -= nums[i];
            if (l == r) {
                return i;
            }
            l += nums[i];
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findMiddleIndex(vector<int>& nums) {
        int l = 0, r = accumulate(nums.begin(), nums.end(), 0);
        for (int i = 0; i < nums.size(); ++i) {
            r -= nums[i];
            if (l == r) {
                return i;
            }
            l += nums[i];
        }
        return -1;
    }
};
```

#### Go

```go
func findMiddleIndex(nums []int) int {
	l, r := 0, 0
	for _, x := range nums {
		r += x
	}
	for i, x := range nums {
		r -= x
		if l == r {
			return i
		}
		l += x
	}
	return -1
}
```

#### TypeScript

```ts
function findMiddleIndex(nums: number[]): number {
    let l = 0;
    let r = nums.reduce((a, b) => a + b, 0);
    for (let i = 0; i < nums.length; ++i) {
        r -= nums[i];
        if (l === r) {
            return i;
        }
        l += nums[i];
    }
    return -1;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_middle_index(nums: Vec<i32>) -> i32 {
        let mut l = 0;
        let mut r: i32 = nums.iter().sum();

        for (i, &x) in nums.iter().enumerate() {
            r -= x;
            if l == r {
                return i as i32;
            }
            l += x;
        }

        -1
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMiddleIndex = function (nums) {
    let l = 0;
    let r = nums.reduce((a, b) => a + b, 0);
    for (let i = 0; i < nums.length; ++i) {
        r -= nums[i];
        if (l === r) {
            return i;
        }
        l += nums[i];
    }
    return -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
