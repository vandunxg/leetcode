---
comments: true
difficulty: Easy
rating: 1216
source: Biweekly Contest 97 Q1
tags:
    - Array
    - Simulation
---

<!-- problem:start -->

# [2553. Separate the Digits in an Array](https://leetcode.com/problems/separate-the-digits-in-an-array)

[中文文档](/solution/2500-2599/2553.Separate%20the%20Digits%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên dương <code>nums</code>, hãy trả về <em>một mảng </em><code>answer</code><em> gồm các chữ số của từng số nguyên trong </em><code>nums</code><em> sau khi tách chúng theo <strong>đúng thứ tự</strong> xuất hiện trong </em><code>nums</code>.</p>

<p>Tách các chữ số của một số nguyên là lấy tất cả các chữ số của nó theo đúng thứ tự.</p>

<ul>
	<li>Ví dụ, với số nguyên <code>10921</code>, các chữ số sau khi tách là <code>[1,0,9,2,1]</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [13,25,83,77]
<strong>Đầu ra:</strong> [1,3,2,5,8,3,7,7]
<strong>Giải thích:</strong>
- Các chữ số của 13 là [1,3].
- Các chữ số của 25 là [2,5].
- Các chữ số của 83 là [8,3].
- Các chữ số của 77 là [7,7].
answer = [1,3,2,5,8,3,7,7]. Lưu ý rằng answer chứa các chữ số theo đúng thứ tự ban đầu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [7,1,3,9]
<strong>Đầu ra:</strong> [7,1,3,9]
<strong>Giải thích:</strong> Các chữ số của mỗi số nguyên trong nums chính là số nguyên đó.
answer = [7,1,3,9].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Tách mỗi số nguyên thành các chữ số thập phân, đồng thời giữ nguyên thứ tự. Phép chia cho mười cho các chữ số theo thứ tự ngược, vì vậy cần đảo ngược từng bộ đệm trước khi thêm vào kết quả.

<!-- thinking:end -->

Tách mỗi số trong mảng thành các chữ số, sau đó đưa các chữ số đã tách vào mảng kết quả theo đúng thứ tự.

Độ phức tạp thời gian là $O(n \times \log_{10} M)$, và độ phức tạp không gian là $O(n \times \log_{10} M)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def separateDigits(self, nums: List[int]) -> List[int]:
        ans = []
        for x in nums:
            t = []
            while x:
                t.append(x % 10)
                x //= 10
            ans.extend(t[::-1])
        return ans
```

#### Java

```java
class Solution {
    public int[] separateDigits(int[] nums) {
        List<Integer> res = new ArrayList<>();
        for (int x : nums) {
            List<Integer> t = new ArrayList<>();
            for (; x > 0; x /= 10) {
                t.add(x % 10);
            }
            Collections.reverse(t);
            res.addAll(t);
        }
        int[] ans = new int[res.size()];
        for (int i = 0; i < ans.length; ++i) {
            ans[i] = res.get(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> separateDigits(vector<int>& nums) {
        vector<int> ans;
        for (int x : nums) {
            vector<int> t;
            for (; x; x /= 10) {
                t.push_back(x % 10);
            }
            while (t.size()) {
                ans.push_back(t.back());
                t.pop_back();
            }
        }
        return ans;
    }
};
```

#### Go

```go
func separateDigits(nums []int) (ans []int) {
	for _, x := range nums {
		t := []int{}
		for ; x > 0; x /= 10 {
			t = append(t, x%10)
		}
		for i, j := 0, len(t)-1; i < j; i, j = i+1, j-1 {
			t[i], t[j] = t[j], t[i]
		}
		ans = append(ans, t...)
	}
	return
}
```

#### TypeScript

```ts
function separateDigits(nums: number[]): number[] {
    const ans: number[] = [];
    for (let num of nums) {
        const t: number[] = [];
        while (num) {
            t.push(num % 10);
            num = Math.floor(num / 10);
        }
        ans.push(...t.reverse());
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn separate_digits(nums: Vec<i32>) -> Vec<i32> {
        let mut ans = Vec::new();
        for &num in nums.iter() {
            let mut num = num;
            let mut t = Vec::new();
            while num != 0 {
                t.push(num % 10);
                num /= 10;
            }
            t.into_iter().rev().for_each(|v| ans.push(v));
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* separateDigits(int* nums, int numsSize, int* returnSize) {
    int n = 0;
    for (int i = 0; i < numsSize; i++) {
        int t = nums[i];
        while (t != 0) {
            t /= 10;
            n++;
        }
    }
    int* ans = malloc(sizeof(int) * n);
    for (int i = numsSize - 1, j = n - 1; i >= 0; i--) {
        int t = nums[i];
        while (t != 0) {
            ans[j--] = t % 10;
            t /= 10;
        }
    }
    *returnSize = n;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 tách chữ số bằng phép toán số học. Việc chuyển số thành chuỗi sẽ duyệt các chữ số từ cao xuống thấp và không cần đảo ngược; kết quả thu được là như nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Rust

```rust
impl Solution {
    pub fn separate_digits(nums: Vec<i32>) -> Vec<i32> {
        let mut ans = vec![];

        for n in nums {
            let mut t = vec![];
            let mut x = n;

            while x != 0 {
                t.push(x % 10);
                x /= 10;
            }

            for i in (0..t.len()).rev() {
                ans.push(t[i]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
