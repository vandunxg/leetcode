---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [17.04. Missing Number](https://leetcode.cn/problems/missing-number-lcci)

[中文文档](/lcci/17.04.Missing%20Number/README.md)

## Mô tả

<!-- description:start -->

<p>Một mảng chứa tất cả các số nguyên từ 0 đến n, ngoại trừ một số bị thiếu. Hãy viết code để tìm số nguyên bị thiếu. Bạn có thể thực hiện trong thời gian O(n) không?</p>

<p><strong>Lưu ý: </strong>Bài toán này hơi khác so với bài toán trong cuốn sách gốc.</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong>[3,0,1]

<strong>Đầu ra: </strong>2</pre>

<p>&nbsp;</p>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong>[9,6,4,2,3,5,7,0,1]

<strong>Đầu ra: </strong>8

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ thiếu một số trong $0\ldots n$. Mảng đánh dấu boolean hoạt động, nhưng dùng thêm không gian tuyến tính.
>
> Với mảng đã sắp xếp, chỉ số phải bằng giá trị; phần tử đầu tiên không khớp chính là số bị thiếu, hoặc là $n$ nếu không có phần tử nào như vậy.
>
> `sort` rồi duyệt là lời giải đúng ngắn gọn nhất, với độ phức tạp $O(n\log n)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        nums.sort()
        for i, x in enumerate(nums):
            if i != x:
                return i
        return len(nums)
```

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        for (int i = 0; i < n; ++i) {
            if (i != nums[i]) {
                return i;
            }
        }
        return n;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            if (i != nums[i]) {
                return i;
            }
        }
        return n;
    }
};
```

#### Go

```go
func missingNumber(nums []int) int {
	sort.Ints(nums)
	for i, x := range nums {
		if i != x {
			return i
		}
	}
	return len(nums)
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_number(mut nums: Vec<i32>) -> i32 {
        nums.sort();
        let n = nums.len() as i32;
        for i in 0..n {
            if i != nums[i as usize] {
                return i;
            }
        }
        n
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
    nums.sort((a, b) => a - b);
    const n = nums.length;
    for (let i = 0; i < n; ++i) {
        if (i != nums[i]) {
            return i;
        }
    }
    return n;
};
```

#### Swift

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        let nums = nums.sorted()
        for (i, x) in nums.enumerated() {
            if i != x {
                return i
            }
        }
        return nums.count
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Không cần đến thừa số log của việc sắp xếp: tổng các số từ 0 đến n là $n(n+1)/2$, rồi trừ đi tổng các phần tử trong mảng.
>
> Chỉ cần cộng dồn một lần và dùng không gian bổ sung hằng số, không cần phép so sánh.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        return sum(range(len(nums) + 1)) - sum(nums)
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
        int ans = n;
        for (int i = 0; i < n; ++i) {
            ans += i - nums[i];
        }
        return ans;
    }
};
```

#### Go

```go
func missingNumber(nums []int) (ans int) {
	ans = len(nums)
	for i, x := range nums {
		ans += i - x
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_number(nums: Vec<i32>) -> i32 {
        let n = nums.len() as i32;
        let mut sum = 0;
        let mut max = 0;
        for num in nums {
            sum += num;
            max = max.max(num);
        }
        if max == n {
            ((1 + max) * max) / 2 - sum
        } else {
            n
        }
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

#### Swift

```swift
class Solution {
    func missingNumber(_ nums: [Int]) -> Int {
        let n = nums.count
        return n * (n + 1) / 2 - nums.reduce(0, +)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3

<!-- thinking:start -->

> **Tư duy**
>
> Tổng có thể bị tràn số trong kiểu số nguyên có phạm vi hẹp.
>
> XOR của $0\ldots n$ với mảng sẽ triệt tiêu các cặp và để lại giá trị bị thiếu, không cần carry hay bảng tra cứu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        ans = 0
        for i, x in enumerate(nums, 1):
            ans ^= i ^ x
        return ans
```

#### Java

```java
class Solution {
    public int missingNumber(int[] nums) {
        int ans = 0;
        for (int i = 1; i <= nums.length; ++i) {
            ans ^= i ^ nums[i - 1];
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
        int ans = 0;
        for (int i = 1; i <= nums.size(); ++i) {
            ans ^= i ^ nums[i - 1];
        }
        return ans;
    }
};
```

#### Go

```go
func missingNumber(nums []int) (ans int) {
	for i, x := range nums {
		ans ^= (i + 1) ^ x
	}
	return
}
```

#### Rust

```rust
impl Solution {
    pub fn missing_number(nums: Vec<i32>) -> i32 {
        let mut res = 0;
        let n = nums.len();
        for i in 0..n {
            res ^= nums[i] ^ ((i + 1) as i32);
        }
        res
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
    let ans = 0;
    for (let i = 1; i <= nums.length; ++i) {
        ans ^= i ^ nums[i - 1];
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
