---
comments: true
difficulty: Easy
tags:
    - Bit Manipulation
    - Array
    - Hash Table
    - Math
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [268. Missing Number](https://leetcode.com/problems/missing-number)

[中文文档](/solution/0200-0299/0268.Missing%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>nums</code> gồm <code>n</code> số khác nhau nằm trong đoạn <code>[0, n]</code>. Hãy trả về <em>số duy nhất trong đoạn này bị thiếu khỏi mảng.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì có 3 số nên <code>n = 3</code>, do đó mọi số đều nằm trong đoạn <code>[0,3]</code>. Số 2 bị thiếu trong đoạn này vì nó không xuất hiện trong <code>nums</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì có 2 số nên <code>n = 2</code>, do đó mọi số đều nằm trong đoạn <code>[0,2]</code>. Số 2 bị thiếu trong đoạn này vì nó không xuất hiện trong <code>nums</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,6,4,2,3,5,7,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<p>Vì có 9 số nên <code>n = 9</code>, do đó mọi số đều nằm trong đoạn <code>[0,9]</code>. Số 8 bị thiếu trong đoạn này vì nó không xuất hiện trong <code>nums</code>.</p>
</div>

<div class="simple-translate-system-theme" id="simple-translate">
<div>
<div class="simple-translate-button isShow" style="background-image: url(&quot;moz-extension://8a9ffb6b-7e69-4e93-aae1-436a1448eff6/icons/512.png&quot;); height: 22px; width: 22px; top: 318px; left: 36px;">&nbsp;</div>

<div class="simple-translate-panel " style="width: 300px; height: 200px; top: 0px; left: 0px; font-size: 13px;">
<div class="simple-translate-result-wrapper" style="overflow: hidden;">
<div class="simple-translate-move" draggable="true">&nbsp;</div>

<div class="simple-translate-result-contents">
<p class="simple-translate-result" dir="auto">&nbsp;</p>

<p class="simple-translate-candidate" dir="auto">&nbsp;</p>
</div>
</div>
</div>
</div>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= n</code></li>
	<li>Các số trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Bạn có thể xây dựng lời giải chỉ dùng thêm <code>O(1)</code> không gian và có độ phức tạp thời gian <code>O(n)</code> không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Thiếu một giá trị trong $[0,n]$. XOR các số từ $1..n$ với các phần tử của mảng; những cặp giá trị giống nhau triệt tiêu lẫn nhau, còn lại số bị thiếu.

<!-- thinking:end -->

Phép XOR có các tính chất sau:

- XOR một số với 0 vẫn cho ra chính số đó, tức là $x \oplus 0 = x$;
- XOR một số với chính nó cho ra 0, tức là $x \oplus x = 0$;

Vì vậy, ta có thể duyệt mảng, XOR từng phần tử với các số trong $[0,..n]$; kết quả cuối cùng chính là số bị thiếu.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return reduce(xor, (i ^ v for i, v in enumerate(nums, 1)))
```

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans ^= (i ^ nums[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans ^= (i ^ nums[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func missingNumber(nums []int) (ans int) {
	n := len(nums)
	ans = n
	for i, v := range nums {
		ans ^= (i ^ v)
	}
	return
}
```

#### TypeScript

```ts
function missingNumber(nums: number[]): number {
    const n = nums.length;
    let ans = n;
    for (let i = 0; i < n; ++i) {
        ans ^= i ^ nums[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_number(nums: Vec<i32>) -> i32 {
        let n = nums.len() as i32;
        let mut ans = n;
        for (i, v) in nums.iter().enumerate() {
            ans ^= (i as i32) ^ v;
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
var missingNumber = function (nums) {
    const n = nums.length;
    let ans = n;
    for (let i = 0; i < n; ++i) {
        ans ^= i ^ nums[i];
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
    function missingNumber($nums) {
        $n = count($nums);
        $sumN = (($n + 1) * $n) / 2;
        for ($i = 0; $i < $n; $i++) {
            $sumN -= $nums[$i];
        }
        return $sumN;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Phép XOR giúp tránh phép cộng. Công thức Gauss cũng dùng được: lấy $n(n+1)/2$ trừ tổng các phần tử trong mảng sẽ ra giá trị còn thiếu.

<!-- thinking:end -->

Ta cũng có thể giải bài này bằng toán học. Tính tổng các số trong $[0,..n]$, rồi trừ đi tổng các phần tử trong mảng để tìm số bị thiếu.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        return (1 + n) * n // 2 - sum(nums)
```

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        int n = nums.length;
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans += i - nums[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        return (1 + n) * n / 2 - accumulate(nums.begin(), nums.end(), 0);
    }
};
```

#### Go

```go
func missingNumber(nums []int) (ans int) {
	n := len(nums)
	ans = n
	for i, v := range nums {
		ans += i - v
	}
	return
}
```

#### TypeScript

```ts
function missingNumber(nums: number[]): number {
    const n = nums.length;
    let ans = n;
    for (let i = 0; i < n; ++i) {
        ans += i - nums[i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_number(nums: Vec<i32>) -> i32 {
        let n = nums.len() as i32;
        let mut ans = n;
        for (i, &v) in nums.iter().enumerate() {
            ans += (i as i32) - v;
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
var missingNumber = function (nums) {
    const n = nums.length;
    let ans = n;
    for (let i = 0; i < n; ++i) {
        ans += i - nums[i];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
