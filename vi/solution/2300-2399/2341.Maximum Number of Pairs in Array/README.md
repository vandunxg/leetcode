---
comments: true
difficulty: Easy
rating: 1184
source: Weekly Contest 302 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2341. Maximum Number of Pairs in Array](https://leetcode.com/problems/maximum-number-of-pairs-in-array)

[中文文档](/solution/2300-2399/2341.Maximum%20Number%20of%20Pairs%20in%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong>. Trong một thao tác, bạn có thể thực hiện các việc sau:</p>

<ul>
	<li>Chọn <strong>hai</strong> số nguyên trong <code>nums</code> <strong>bằng nhau</strong>.</li>
	<li>Xóa cả hai số nguyên khỏi <code>nums</code>, tạo thành một <strong>cặp</strong>.</li>
</ul>

<p>Thực hiện thao tác này nhiều lần nhất có thể trên <code>nums</code>.</p>

<p>Trả về <em>một mảng số nguyên được đánh chỉ số từ <strong>0</strong> </em><code>answer</code><em> có kích thước </em><code>2</code><em>, trong đó </em><code>answer[0]</code><em> là số cặp được tạo thành và </em><code>answer[1]</code><em> là số nguyên còn lại trong </em><code>nums</code><em> sau khi thực hiện thao tác nhiều lần nhất có thể</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,2,1,3,2,2]
<strong>Đầu ra:</strong> [3,1]
<strong>Giải thích:</strong>
Tạo một cặp từ nums[0] và nums[3] rồi xóa chúng khỏi nums. Khi đó, nums = [3,2,3,2,2].
Tạo một cặp từ nums[0] và nums[2] rồi xóa chúng khỏi nums. Khi đó, nums = [2,2,2].
Tạo một cặp từ nums[0] và nums[1] rồi xóa chúng khỏi nums. Khi đó, nums = [2].
Không thể tạo thêm cặp nào. Tổng cộng có 3 cặp được tạo thành và còn lại 1 số trong nums.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1]
<strong>Đầu ra:</strong> [1,0]
<strong>Giải thích:</strong> Tạo một cặp từ nums[0] và nums[1] rồi xóa chúng khỏi nums. Khi đó, nums = [].
Không thể tạo thêm cặp nào. Tổng cộng có 1 cặp được tạo thành và còn lại 0 số trong nums.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0]
<strong>Đầu ra:</strong> [0,1]
<strong>Giải thích:</strong> Không thể tạo cặp nào và còn lại 1 số trong nums.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị bằng nhau sẽ ghép thành từng cặp; ta cần tìm số cặp và số phần tử còn lại. Vì $n \le 100$, chỉ cần đếm tần suất xuất hiện.
>
> Cộng $\lfloor v/2 \rfloor$ trên tần suất của mỗi giá trị để tính số cặp; số phần tử còn lại là $n-2s$.

<!-- thinking:end -->

Ta có thể đếm số lần xuất hiện của mỗi số $x$ trong mảng $\textit{nums}$ và lưu chúng vào hash table hoặc mảng $\textit{cnt}$.

Sau đó, ta duyệt qua $\textit{cnt}$. Với mỗi số $x$, nếu số lần xuất hiện $v$ của $x$ lớn hơn $1$, ta có thể chọn hai số $x$ trong mảng để tạo thành một cặp. Chia $v$ cho $2$ và lấy phần nguyên để thu được số cặp có thể tạo thành từ số $x$ hiện tại. Sau đó, ta cộng số này vào biến $s$.

Số phần tử còn lại là độ dài của mảng $\textit{nums}$ trừ đi số cặp được tạo thành nhân với $2$, tức là $n - s \times 2$.

Đáp án là $[s, n - s \times 2]$.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$, còn $C$ là miền giá trị của các số trong mảng $\textit{nums}$, tức là $101$ trong bài này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfPairs(self, nums: List[int]) -> List[int]:
        cnt = Counter(nums)
        s = sum(v // 2 for v in cnt.values())
        return [s, len(nums) - s * 2]
```

#### Java

```java
class Solution {
    public int[] numberOfPairs(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            ++cnt[x];
        }
        int s = 0;
        for (int v : cnt) {
            s += v / 2;
        }
        return new int[] {s, nums.length - s * 2};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> numberOfPairs(vector<int>& nums) {
        vector<int> cnt(101);
        for (int& x : nums) {
            ++cnt[x];
        }
        int s = 0;
        for (int& v : cnt) {
            s += v >> 1;
        }
        return {s, (int) nums.size() - s * 2};
    }
};
```

#### Go

```go
func numberOfPairs(nums []int) []int {
	cnt := [101]int{}
	for _, x := range nums {
		cnt[x]++
	}
	s := 0
	for _, v := range cnt {
		s += v / 2
	}
	return []int{s, len(nums) - s*2}
}
```

#### TypeScript

```ts
function numberOfPairs(nums: number[]): number[] {
    const n = nums.length;
    const count = new Array(101).fill(0);
    for (const num of nums) {
        count[num]++;
    }
    const sum = count.reduce((r, v) => r + (v >> 1), 0);
    return [sum, n - sum * 2];
}
```

#### Rust

```rust
impl Solution {
    pub fn number_of_pairs(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let mut count = [0; 101];
        for &v in nums.iter() {
            count[v as usize] += 1;
        }
        let mut sum = 0;
        for v in count.iter() {
            sum += v >> 1;
        }
        vec![sum as i32, (n - sum * 2) as i32]
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var numberOfPairs = function (nums) {
    const cnt = new Array(101).fill(0);
    for (const x of nums) {
        ++cnt[x];
    }
    const s = cnt.reduce((a, b) => a + (b >> 1), 0);
    return [s, nums.length - s * 2];
};
```

#### C#

```cs
public class Solution {
    public int[] NumberOfPairs(int[] nums) {
        int[] cnt = new int[101];
        foreach(int x in nums) {
            ++cnt[x];
        }
        int s = 0;
        foreach(int v in cnt) {
            s += v / 2;
        }
        return new int[] {s, nums.Length - s * 2};
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* numberOfPairs(int* nums, int numsSize, int* returnSize) {
    int count[101] = {0};
    for (int i = 0; i < numsSize; i++) {
        count[nums[i]]++;
    }
    int sum = 0;
    for (int i = 0; i < 101; i++) {
        sum += count[i] >> 1;
    }
    int* ans = malloc(sizeof(int) * 2);
    ans[0] = sum;
    ans[1] = numsSize - sum * 2;
    *returnSize = 2;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
