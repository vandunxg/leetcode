---
comments: true
difficulty: Easy
rating: 1139
source: Weekly Contest 168 Q1
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [1295. Find Numbers with Even Number of Digits](https://leetcode.com/problems/find-numbers-with-even-number-of-digits)

[中文文档](/solution/1200-1299/1295.Find%20Numbers%20with%20Even%20Number%20of%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code>, hãy trả về số phần tử có <strong>số chữ số chẵn</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [12,345,2,6,7896]
<strong>Đầu ra:</strong> 2
<strong>Giải thích: 
</strong>12 có 2 chữ số (số chữ số chẵn).&nbsp;
345 có 3 chữ số (số chữ số lẻ).&nbsp;
2 có 1 chữ số (số chữ số lẻ).&nbsp;
6 có 1 chữ số (số chữ số lẻ).&nbsp;
7896 có 4 chữ số (số chữ số chẵn).&nbsp;
Vì vậy, chỉ 12 và 7896 có số chữ số chẵn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [555,901,482,1771]
<strong>Đầu ra:</strong> 1 
<strong>Giải thích: </strong>
Chỉ có 1771 là có số chữ số chẵn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 500</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n \le 500$, ta chuyển mỗi số thành chuỗi thập phân rồi kiểm tra tính chẵn lẻ của độ dài. Có thể đếm chữ số bằng cách chia liên tiếp cho $10$, nhưng chuyển thành chuỗi sẽ ngắn gọn hơn.

<!-- thinking:end -->

Ta duyệt từng phần tử $x$ trong mảng $\textit{nums}$. Với mỗi $x$, ta chuyển trực tiếp nó thành chuỗi rồi kiểm tra độ dài có chẵn hay không. Nếu có, tăng đáp án thêm một.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log M)$ và độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ là độ dài mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findNumbers(self, nums: List[int]) -> int:
        return sum(len(str(x)) % 2 == 0 for x in nums)
```

#### Java

```java
class Solution {
    public int findNumbers(int[] nums) {
        int ans = 0;
        for (int x : nums) {
            if (String.valueOf(x).length() % 2 == 0) {
                ++ans;
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
    int findNumbers(vector<int>& nums) {
        int ans = 0;
        for (int& x : nums) {
            ans += to_string(x).size() % 2 == 0;
        }
        return ans;
    }
};
```

#### Go

```go
func findNumbers(nums []int) (ans int) {
	for _, x := range nums {
		if len(strconv.Itoa(x))%2 == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function findNumbers(nums: number[]): number {
    return nums.filter(x => x.toString().length % 2 === 0).length;
}
```

#### Rust

```rust
impl Solution {
    pub fn find_numbers(nums: Vec<i32>) -> i32 {
        nums.iter().filter(|&x| x.to_string().len() % 2 == 0).count() as i32
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var findNumbers = function (nums) {
    return nums.filter(x => x.toString().length % 2 === 0).length;
};
```

#### C#

```cs
public class Solution {
    public int FindNumbers(int[] nums) {
        return nums.Count(x => x.ToString().Length % 2 == 0);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
