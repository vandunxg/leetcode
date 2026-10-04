---
comments: true
difficulty: Medium
rating: 1763
source: Weekly Contest 367 Q3
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [2905. Find Indices With Index and Value Difference II](https://leetcode.com/problems/find-indices-with-index-and-value-difference-ii)

[中文文档](/solution/2900-2999/2905.Find%20Indices%20With%20Index%20and%20Value%20Difference%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>, có độ dài <code>n</code>, một số nguyên <code>indexDifference</code> và một số nguyên <code>valueDifference</code>.</p>

<p>Nhiệm vụ của bạn là tìm <strong>hai</strong> chỉ số <code>i</code> và <code>j</code>, đều thuộc đoạn <code>[0, n - 1]</code>, thỏa mãn các điều kiện sau:</p>

<ul>
	<li><code>abs(i - j) &gt;= indexDifference</code>, và</li>
	<li><code>abs(nums[i] - nums[j]) &gt;= valueDifference</code></li>
</ul>

<p>Trả về <em>một mảng số nguyên</em> <code>answer</code>, <em>trong đó</em> <code>answer = [i, j]</code> <em>nếu tồn tại hai chỉ số như vậy</em>, <em>và</em> <code>answer = [-1, -1]</code> <em>nếu không</em>. Nếu có nhiều lựa chọn cho hai chỉ số, hãy trả về <em>bất kỳ lựa chọn nào</em>.</p>

<p><strong>Lưu ý:</strong> <code>i</code> và <code>j</code> <strong>có thể bằng nhau</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,1,4,1], indexDifference = 2, valueDifference = 4
<strong>Đầu ra:</strong> [0,3]
<strong>Giải thích:</strong> Trong ví dụ này, có thể chọn i = 0 và j = 3.
abs(0 - 3) &gt;= 2 và abs(nums[0] - nums[3]) &gt;= 4.
Do đó, [0,3] là một đáp án hợp lệ.
[3,0] cũng là một đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1], indexDifference = 0, valueDifference = 0
<strong>Đầu ra:</strong> [0,0]
<strong>Giải thích:</strong> Trong ví dụ này, có thể chọn i = 0 và j = 0.
abs(0 - 0) &gt;= 0 và abs(nums[0] - nums[0]) &gt;= 0.
Do đó, [0,0] là một đáp án hợp lệ.
Các đáp án hợp lệ khác là [0,1], [1,0] và [1,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3], indexDifference = 2, valueDifference = 4
<strong>Đầu ra:</strong> [-1,-1]
<strong>Giải thích:</strong> Trong ví dụ này, có thể chứng minh rằng không thể tìm được hai chỉ số thỏa mãn cả hai điều kiện.
Do đó, trả về [-1,-1].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= indexDifference &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= valueDifference &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Phát biểu bài toán giống phần I, nhưng $n \le 10^5$ khiến việc dùng vòng lặp kép không khả thi. Với mỗi chỉ số bên phải $i$, tập chỉ số bên trái hợp lệ vẫn là tiền tố $[0, i-indexDifference]$, trong đó các giá trị cực trị tăng dần.
>
> Duy trì chỉ số của phần tử nhỏ nhất và lớn nhất trong tiền tố khi duyệt, rồi kiểm tra hiệu với $nums[i]$. Cài đặt $O(n)$ giống phần I; chỉ là các ràng buộc buộc ta phải dùng dạng tuyến tính này.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findIndices(
        self, nums: List[int], indexDifference: int, valueDifference: int
    ) -> List[int]:
        mi = mx = 0
        for i in range(indexDifference, len(nums)):
            j = i - indexDifference
            if nums[j] < nums[mi]:
                mi = j
            if nums[j] > nums[mx]:
                mx = j
            if nums[i] - nums[mi] >= valueDifference:
                return [mi, i]
            if nums[mx] - nums[i] >= valueDifference:
                return [mx, i]
        return [-1, -1]
```

#### Java

```java
class Solution {
    public int[] findIndices(int[] nums, int indexDifference, int valueDifference) {
        int mi = 0;
        int mx = 0;
        for (int i = indexDifference; i < nums.length; ++i) {
            int j = i - indexDifference;
            if (nums[j] < nums[mi]) {
                mi = j;
            }
            if (nums[j] > nums[mx]) {
                mx = j;
            }
            if (nums[i] - nums[mi] >= valueDifference) {
                return new int[] {mi, i};
            }
            if (nums[mx] - nums[i] >= valueDifference) {
                return new int[] {mx, i};
            }
        }
        return new int[] {-1, -1};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findIndices(vector<int>& nums, int indexDifference, int valueDifference) {
        int mi = 0, mx = 0;
        for (int i = indexDifference; i < nums.size(); ++i) {
            int j = i - indexDifference;
            if (nums[j] < nums[mi]) {
                mi = j;
            }
            if (nums[j] > nums[mx]) {
                mx = j;
            }
            if (nums[i] - nums[mi] >= valueDifference) {
                return {mi, i};
            }
            if (nums[mx] - nums[i] >= valueDifference) {
                return {mx, i};
            }
        }
        return {-1, -1};
    }
};
```

#### Go

```go
func findIndices(nums []int, indexDifference int, valueDifference int) []int {
	mi, mx := 0, 0
	for i := indexDifference; i < len(nums); i++ {
		j := i - indexDifference
		if nums[j] < nums[mi] {
			mi = j
		}
		if nums[j] > nums[mx] {
			mx = j
		}
		if nums[i]-nums[mi] >= valueDifference {
			return []int{mi, i}
		}
		if nums[mx]-nums[i] >= valueDifference {
			return []int{mx, i}
		}
	}
	return []int{-1, -1}
}
```

#### TypeScript

```ts
function findIndices(nums: number[], indexDifference: number, valueDifference: number): number[] {
    let [mi, mx] = [0, 0];
    for (let i = indexDifference; i < nums.length; ++i) {
        const j = i - indexDifference;
        if (nums[j] < nums[mi]) {
            mi = j;
        }
        if (nums[j] > nums[mx]) {
            mx = j;
        }
        if (nums[i] - nums[mi] >= valueDifference) {
            return [mi, i];
        }
        if (nums[mx] - nums[i] >= valueDifference) {
            return [mx, i];
        }
    }
    return [-1, -1];
}
```

#### Rust

```rust
impl Solution {
    pub fn find_indices(nums: Vec<i32>, index_difference: i32, value_difference: i32) -> Vec<i32> {
        let index_difference = index_difference as usize;
        let mut mi = 0;
        let mut mx = 0;

        for i in index_difference..nums.len() {
            let j = i - index_difference;

            if nums[j] < nums[mi] {
                mi = j;
            }

            if nums[j] > nums[mx] {
                mx = j;
            }

            if nums[i] - nums[mi] >= value_difference {
                return vec![mi as i32, i as i32];
            }

            if nums[mx] - nums[i] >= value_difference {
                return vec![mx as i32, i as i32];
            }
        }

        vec![-1, -1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
