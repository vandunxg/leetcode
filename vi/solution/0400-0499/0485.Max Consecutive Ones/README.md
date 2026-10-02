---
comments: true
difficulty: Easy
tags:
    - Array
---

<!-- problem:start -->

# [485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones)

[中文文档](/solution/0400-0499/0485.Max%20Consecutive%20Ones/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code>, hãy trả về <em>số lượng </em><code>1</code><em> liên tiếp lớn nhất trong mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,0,1,1,1]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Hai phần tử đầu tiên hoặc ba phần tử cuối cùng là các số 1 liên tiếp. Số lượng số 1 liên tiếp lớn nhất là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,0,1,1,0,1]
<strong>Đầu ra:</strong> 2
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> có giá trị là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Cần tìm đoạn số 1 liên tiếp dài nhất. Kiểm tra mọi mảng con mất $O(n^2)$; chỉ cần duyệt một lượt.
>
> Gặp $1$ thì tăng độ dài đoạn hiện tại và cập nhật đáp án; gặp $0$ thì đặt lại bộ đếm.
>
> Các số 0 ngăn cách những đoạn số 1, vì vậy bộ đếm không gộp hai đoạn riêng biệt.

<!-- thinking:end -->

Ta có thể duyệt mảng, dùng biến $\textit{cnt}$ để ghi lại số lượng số 1 liên tiếp hiện tại và biến $\textit{ans}$ để ghi lại số lượng số 1 liên tiếp lớn nhất.

Khi gặp số 1, ta tăng $\textit{cnt}$ lên một rồi cập nhật $\textit{ans}$ bằng giá trị lớn hơn giữa $\textit{cnt}$ và chính $\textit{ans}$, tức là $\textit{ans} = \max(\textit{ans}, \textit{cnt})$. Nếu không, ta đặt lại $\textit{cnt}$ về 0.

Sau khi duyệt xong, ta trả về giá trị của $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        ans = cnt = 0
        for x in nums:
            if x:
                cnt += 1
                ans = max(ans, cnt)
            else:
                cnt = 0
        return ans
```

#### Java

```java
class Solution {
    public int findMaxConsecutiveOnes(int[] nums) {
        int ans = 0, cnt = 0;
        for (int x : nums) {
            if (x == 1) {
                ans = Math.max(ans, ++cnt);
            } else {
                cnt = 0;
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
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int ans = 0, cnt = 0;
        for (int x : nums) {
            if (x) {
                ans = max(ans, ++cnt);
            } else {
                cnt = 0;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMaxConsecutiveOnes(nums []int) (ans int) {
	cnt := 0
	for _, x := range nums {
		if x == 1 {
			cnt++
			ans = max(ans, cnt)
		} else {
			cnt = 0
		}
	}
	return
}
```

#### TypeScript

```ts
function findMaxConsecutiveOnes(nums: number[]): number {
    let [ans, cnt] = [0, 0];
    for (const x of nums) {
        if (x) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 0;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_max_consecutive_ones(nums: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut cnt = 0;

        for &x in nums.iter() {
            if x == 1 {
                cnt += 1;
                ans = ans.max(cnt);
            } else {
                cnt = 0;
            }
        }

        ans
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findMaxConsecutiveOnes = function (nums) {
    let [ans, cnt] = [0, 0];
    for (const x of nums) {
        if (x) {
            ans = Math.max(ans, ++cnt);
        } else {
            cnt = 0;
        }
    }
    return ans;
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $nums
     * @return Integer
     */
    function findMaxConsecutiveOnes($nums) {
        $ans = $cnt = 0;

        foreach ($nums as $x) {
            if ($x == 1) {
                $cnt += 1;
                $ans = max($ans, $cnt);
            } else {
                $cnt = 0;
            }
        }

        return $ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
