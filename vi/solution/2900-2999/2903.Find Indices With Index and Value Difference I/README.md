---
comments: true
difficulty: Easy
rating: 1157
source: Weekly Contest 367 Q1
tags:
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [2903. Find Indices With Index and Value Difference I](https://leetcode.com/problems/find-indices-with-index-and-value-difference-i)

[中文文档](/solution/2900-2999/2903.Find%20Indices%20With%20Index%20and%20Value%20Difference%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> <strong>được đánh chỉ số từ 0</strong> có độ dài <code>n</code>, một số nguyên <code>indexDifference</code> và một số nguyên <code>valueDifference</code>.</p>

<p>Nhiệm vụ của bạn là tìm <strong>hai</strong> chỉ số <code>i</code> và <code>j</code>, đều nằm trong khoảng <code>[0, n - 1]</code>, thỏa mãn các điều kiện sau:</p>

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
<strong>Giải thích:</strong> Có thể chứng minh rằng không thể tìm được hai chỉ số thỏa mãn cả hai điều kiện.
Do đó, trả về [-1,-1].</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 50</code></li>
	<li><code>0 &lt;= indexDifference &lt;= 100</code></li>
	<li><code>0 &lt;= valueDifference &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai con trỏ + Duy trì Giá trị lớn nhất và nhỏ nhất

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 100$ cho phép kiểm tra mọi cặp chỉ số. Một cặp hợp lệ cần $|i-j| \ge indexDifference$, vì vậy với mỗi điểm cuối phải $i$, ta chỉ cần tìm trong đoạn $[0, i-indexDifference]$ một giá trị đủ khác với $nums[i]$.
>
> Tiền tố đó được mô tả đầy đủ bởi các chỉ số $mi$ và $mx$ của giá trị nhỏ nhất và lớn nhất. Sau khi thêm $nums[j]$ vừa đủ điều kiện, ta kiểm tra $nums[i]-nums[mi]$ và $nums[mx]-nums[i]$. Chỉ cần một lượt duyệt là có thể tìm được một cặp hợp lệ.

<!-- thinking:end -->

Ta sử dụng hai con trỏ $i$ và $j$ để duy trì một cửa sổ trượt có khoảng cách là $indexDifference$, trong đó $j$ và $i$ lần lượt trỏ đến biên trái và biên phải của cửa sổ. Ban đầu, $i$ trỏ đến $indexDifference$, còn $j` points to $0$.

Ta sử dụng $mi$ và $mx$ để duy trì các chỉ số của giá trị nhỏ nhất và lớn nhất ở bên trái con trỏ $j$.

Khi con trỏ $i$ di chuyển sang phải, ta cần cập nhật $mi$ và $mx$. Nếu $nums[j] \lt nums[mi]$, ta cập nhật $mi$ thành $j$; nếu $nums[j] \gt nums[mx]$, ta cập nhật $mx$ thành $j$. Sau khi cập nhật $mi$ và $mx$, ta có thể xác định xem đã tìm được một cặp chỉ số thỏa mãn điều kiện hay chưa. Nếu $nums[i] - nums[mi] \ge valueDifference$, ta đã tìm được cặp chỉ số $[mi, i]$ thỏa mãn điều kiện; nếu $nums[mx] - nums[i] >= valueDifference$, ta đã tìm được cặp chỉ số $[mx, i]$ thỏa mãn điều kiện.

Nếu con trỏ $i$ đi đến cuối mảng mà vẫn chưa tìm được cặp chỉ số thỏa mãn điều kiện, ta trả về $[-1, -1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

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
