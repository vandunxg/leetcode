---
comments: true
difficulty: Easy
tags:
    - Array
---

<!-- problem:start -->

# [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence)

[中文文档](/solution/0600-0699/0674.Longest%20Continuous%20Increasing%20Subsequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên chưa sắp xếp <code>nums</code>, hãy trả về <em>độ dài của <strong>dãy con tăng liên tiếp</strong> dài nhất (tức mảng con)</em>. Dãy con phải <strong>tăng nghiêm ngặt</strong>.</p>

<p><strong>Dãy con tăng liên tiếp</strong> được xác định bởi hai chỉ số <code>l</code> và <code>r</code> (<code>l &lt; r</code>), sao cho dãy là <code>[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]</code> và với mọi <code>l &lt;= i &lt; r</code>, <code>nums[i] &lt; nums[i + 1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,5,4,7]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Dãy con tăng liên tiếp dài nhất là [1,3,5], có độ dài 3.
Mặc dù [1,3,5,7] là một dãy con tăng, nó không liên tiếp vì phần tử 5 và 7 bị ngăn cách bởi phần tử
4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,2,2,2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Dãy con tăng liên tiếp dài nhất là [2], có độ dài 1. Lưu ý dãy phải tăng
nghiêm ngặt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Dãy con phải liên tiếp nên không cần dùng thuật toán LIS kinh điển.
>
> Theo dõi độ dài dãy hiện tại: tăng độ dài khi $nums[i-1]<nums[i]$, nếu không thì đặt lại thành $1$.

<!-- thinking:end -->

Ta duyệt mảng $nums$ và dùng biến $cnt$ để lưu độ dài dãy tăng liên tiếp hiện tại. Ban đầu, $cnt = 1$.

Sau đó, bắt đầu từ chỉ số $i = 1$ và duyệt mảng $nums$ sang phải. Mỗi lần duyệt, nếu $nums[i - 1] < nums[i]$, phần tử hiện tại có thể nối vào dãy tăng liên tiếp, nên tăng $cnt = cnt + 1$ rồi cập nhật đáp án thành $ans = \max(ans, cnt)$. Ngược lại, phần tử hiện tại không thể nối vào dãy, nên đặt lại $cnt = 1$.

Sau khi duyệt xong, trả về đáp án $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        ans = cnt = 1
        for i, x in enumerate(nums[1:]):
            if nums[i] < x:
                cnt += 1
                ans = max(ans, cnt)
            else:
                cnt = 1
        return ans
```

#### Java

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int ans = 1;
        for (int i = 1, cnt = 1; i < nums.length; ++i) {
            if (nums[i - 1] < nums[i]) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 1;
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
    int findLengthOfLCIS(vector<int>& nums) {
        int ans = 1;
        for (int i = 1, cnt = 1; i < nums.size(); ++i) {
            if (nums[i - 1] < nums[i]) {
                ans = max(ans, ++cnt);
            } else {
                cnt = 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findLengthOfLCIS(nums []int) int {
	ans, cnt := 1, 1
	for i, x := range nums[1:] {
		if nums[i] < x {
			cnt++
			ans = max(ans, cnt)
		} else {
			cnt = 1
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findLengthOfLCIS(nums: number[]): number {
    let [ans, cnt] = [1, 1];
    for (let i = 1; i < nums.length; ++i) {
        if (nums[i - 1] < nums[i]) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_length_of_lcis(nums: Vec<i32>) -> i32 {
        let mut ans = 1;
        let mut cnt = 1;
        for i in 1..nums.len() {
            if nums[i - 1] < nums[i] {
                ans = ans.max(cnt + 1);
                cnt += 1;
            } else {
                cnt = 1;
            }
        }
        ans
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function findLengthOfLCIS($nums) {
        $ans = 1;
        $cnt = 1;
        for ($i = 1; $i < count($nums); ++$i) {
            if ($nums[$i - 1] < $nums[$i]) {
                $ans = max($ans, ++$cnt);
            } else {
                $cnt = 1;
            }
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Có thể duyệt từng đoạn tăng bằng hai con trỏ: tăng $j$ khi mảng còn tăng, ghi nhận $j-i$, rồi đặt $i=j$.

<!-- thinking:end -->

Ta cũng có thể dùng hai con trỏ $i$ và $j$ để tìm từng dãy tăng liên tiếp, rồi lấy độ dài lớn nhất làm đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        ans, n = 1, len(nums)
        i = 0
        while i < n:
            j = i + 1
            while j < n and nums[j - 1] < nums[j]:
                j += 1
            ans = max(ans, j - i)
            i = j
        return ans
```

#### Java

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        int ans = 1;
        int n = nums.length;
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j - 1] < nums[j]) {
                ++j;
            }
            ans = Math.max(ans, j - i);
            i = j;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int ans = 1;
        int n = nums.size();
        for (int i = 0; i < n;) {
            int j = i + 1;
            while (j < n && nums[j - 1] < nums[j]) {
                ++j;
            }
            ans = max(ans, j - i);
            i = j;
        }
        return ans;
    }
};
```

#### Go

```go
func findLengthOfLCIS(nums []int) int {
	ans := 1
	n := len(nums)
	for i := 0; i < n; {
		j := i + 1
		for j < n && nums[j-1] < nums[j] {
			j++
		}
		ans = max(ans, j-i)
		i = j
	}
	return ans
}
```

#### TypeScript

```ts
function findLengthOfLCIS(nums: number[]): number {
    let ans = 1;
    const n = nums.length;
    for (let i = 0; i < n;) {
        let j = i + 1;
        while (j < n && nums[j - 1] < nums[j]) {
            ++j;
        }
        ans = Math.max(ans, j - i);
        i = j;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_length_of_lcis(nums: Vec<i32>) -> i32 {
        let mut ans = 1;
        let n = nums.len();
        let mut i = 0;
        while i < n {
            let mut j = i + 1;
            while j < n && nums[j - 1] < nums[j] {
                j += 1;
            }
            ans = ans.max(j - i);
            i = j;
        }
        ans as i32
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function findLengthOfLCIS($nums) {
        $ans = 1;
        $n = count($nums);
        $i = 0;
        while ($i < $n) {
            $j = $i + 1;
            while ($j < $n && $nums[$j - 1] < $nums[$j]) {
                $j++;
            }
            $ans = max($ans, $j - $i);
            $i = $j;
        }
        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
