---
comments: true
difficulty: Medium
rating: 1845
source: Weekly Contest 337 Q4
tags:
    - Greedy
    - Array
    - Hash Table
    - Math
---

<!-- problem:start -->

# [2598. Smallest Missing Non-negative Integer After Operations](https://leetcode.com/problems/smallest-missing-non-negative-integer-after-operations)

[中文文档](/solution/2500-2599/2598.Smallest%20Missing%20Non-negative%20Integer%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>value</code>.</p>

<p>Trong một thao tác, bạn có thể cộng hoặc trừ <code>value</code> khỏi bất kỳ phần tử nào của <code>nums</code>.</p>

<ul>
	<li>Ví dụ, nếu <code>nums = [1,2,3]</code> và <code>value = 2</code>, bạn có thể chọn trừ <code>value</code> khỏi <code>nums[0]</code> để biến <code>nums = [-1,2,3]</code>.</li>
</ul>

<p>MEX (giá trị không xuất hiện nhỏ nhất) của một mảng là số nguyên <strong>không âm</strong> nhỏ nhất không có trong mảng.</p>

<ul>
	<li>Ví dụ, MEX của <code>[-1,2,3]</code> là <code>0</code>, còn MEX của <code>[1,0,3]</code> là <code>2</code>.</li>
</ul>

<p>Hãy trả về <em>MEX lớn nhất của </em><code>nums</code><em> sau khi thực hiện thao tác trên <strong>một số lần bất kỳ</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-10,7,13,6,8], value = 5
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có thể đạt được kết quả này bằng cách thực hiện các thao tác sau:
- Cộng value vào nums[1] hai lần để biến nums = [1,<strong><u>0</u></strong>,7,13,6,8]
- Trừ value khỏi nums[2] một lần để biến nums = [1,0,<strong><u>2</u></strong>,13,6,8]
- Trừ value khỏi nums[3] hai lần để biến nums = [1,0,2,<strong><u>3</u></strong>,6,8]
MEX của nums là 4. Có thể chứng minh rằng 4 là MEX lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,-10,7,13,6,8], value = 7
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Có thể đạt được kết quả này bằng cách thực hiện thao tác sau:
- Trừ value khỏi nums[2] một lần để biến nums = [1,-10,<u><strong>0</strong></u>,13,6,8]
MEX của nums là 2. Có thể chứng minh rằng 2 là MEX lớn nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length, value &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Việc cộng hoặc trừ $\textit{value}$ một số lần bất kỳ sẽ biến một số thành một số nguyên bất kỳ trong cùng lớp dư của nó. Ta muốn tìm $mex$ nhỏ nhất sau các thao tác này.
>
> Số dư $r$ có thể cung cấp các giá trị $r,r+\textit{value},r+2\textit{value},\ldots$. Ta lần lượt cần các số từ $0$ trở lên và sử dụng các lớp dư tương ứng; lớp dư đầu tiên cạn kiệt chính là $mex$. Vì vậy, ta đếm số lượng từng lớp dư rồi duyệt qua chúng.

<!-- thinking:end -->

Ta sử dụng một hash table $\textit{cnt}$ để đếm số lượng số dư khi lấy từng số trong mảng modulo $\textit{value}$.

Sau đó, ta duyệt từ $0$. Với số hiện tại $i$, nếu $\textit{cnt}[i \bmod \textit{value}]$ bằng $0$, điều đó có nghĩa là trong mảng không còn số nào có số dư khi modulo $\textit{value}$ bằng $i$, nên $i$ là MEX của mảng và ta có thể trả về ngay. Nếu không, ta giảm $\textit{cnt}[i \bmod \textit{value}]$ đi $1$ rồi tiếp tục duyệt.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(\textit{value})$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSmallestInteger(self, nums: List[int], value: int) -> int:
        cnt = Counter(x % value for x in nums)
        for i in range(len(nums) + 1):
            if cnt[i % value] == 0:
                return i
            cnt[i % value] -= 1
```

#### Java

```java
class Solution {
    public int findSmallestInteger(int[] nums, int value) {
        int[] cnt = new int[value];
        for (int x : nums) {
            ++cnt[(x % value + value) % value];
        }
        for (int i = 0;; ++i) {
            if (cnt[i % value]-- == 0) {
                return i;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findSmallestInteger(vector<int>& nums, int value) {
        int cnt[value];
        memset(cnt, 0, sizeof(cnt));
        for (int x : nums) {
            ++cnt[(x % value + value) % value];
        }
        for (int i = 0;; ++i) {
            if (cnt[i % value]-- == 0) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func findSmallestInteger(nums []int, value int) int {
	cnt := make([]int, value)
	for _, x := range nums {
		cnt[(x%value+value)%value]++
	}
	for i := 0; ; i++ {
		if cnt[i%value] == 0 {
			return i
		}
		cnt[i%value]--
	}
}
```

#### TypeScript

```ts
function findSmallestInteger(nums: number[], value: number): number {
    const cnt: number[] = new Array(value).fill(0);
    for (const x of nums) {
        ++cnt[((x % value) + value) % value];
    }
    for (let i = 0; ; ++i) {
        if (cnt[i % value]-- === 0) {
            return i;
        }
    }
}
```

#### Rust

```rust
impl Solution {
    pub fn find_smallest_integer(nums: Vec<i32>, value: i32) -> i32 {
        let mut cnt = vec![0; value as usize];
        for &x in &nums {
            let idx = ((x % value + value) % value) as usize;
            cnt[idx] += 1;
        }

        let mut i = 0;
        loop {
            let idx = (i % value) as usize;
            if cnt[idx] == 0 {
                return i;
            }
            cnt[idx] -= 1;
            i += 1;
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @param {number} value
 * @return {number}
 */
var findSmallestInteger = function (nums, value) {
    const cnt = Array(value).fill(0);
    for (const x of nums) {
        ++cnt[((x % value) + value) % value];
    }
    for (let i = 0; ; ++i) {
        if (cnt[i % value]-- === 0) {
            return i;
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
