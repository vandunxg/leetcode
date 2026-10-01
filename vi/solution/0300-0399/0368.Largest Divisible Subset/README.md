---
comments: true
difficulty: Medium
tags:
    - Array
    - Math
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [368. Largest Divisible Subset](https://leetcode.com/problems/largest-divisible-subset)

[中文文档](/solution/0300-0399/0368.Largest%20Divisible%20Subset/README.md)

## Mô tả

<!-- description:start -->

<p>Cho tập hợp các số nguyên dương <strong>khác nhau</strong> <code>nums</code>. Hãy trả về tập con lớn nhất <code>answer</code> sao cho mọi cặp phần tử <code>(answer[i], answer[j])</code> trong tập con này thỏa mãn:</p>

<ul>
	<li><code>answer[i] % answer[j] == 0</code>, hoặc</li>
	<li><code>answer[j] % answer[i] == 0</code></li>
</ul>

<p>Nếu có nhiều đáp án, bạn có thể trả về bất kỳ đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> [1,2]
<strong>Giải thích:</strong> [1,3] cũng là đáp án hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,4,8]
<strong>Đầu ra:</strong> [1,2,4,8]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 2 * 10<sup>9</sup></code></li>
	<li>Tất cả số nguyên trong <code>nums</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cặp số trong tập con phải có quan hệ chia hết. Khi chưa sắp xếp, các giá trị chưa tạo thành một chuỗi. Sau khi sắp xếp, mỗi giá trị lớn hơn chỉ cần là bội số của một giá trị nhỏ hơn — cấu trúc tương tự LIS.
>
> $f[i]$ là độ dài tập con chia hết dài nhất kết thúc tại $nums[i]$. Nếu $nums[i]\% nums[j]=0$, ta có thể nối thêm để được $f[j]+1$. Ghi nhớ chỉ số tốt nhất rồi duyệt ngược, lần lượt khôi phục độ dài $m,m-1,\ldots$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestDivisibleSubset(self, nums: List[int]) -> List[int]:
        nums.sort()
        n = len(nums)
        f = [1] * n
        k = 0
        for i in range(n):
            for j in range(i):
                if nums[i] % nums[j] == 0:
                    f[i] = max(f[i], f[j] + 1)
            if f[k] < f[i]:
                k = i
        m = f[k]
        i = k
        ans = []
        while m:
            if nums[k] % nums[i] == 0 and f[i] == m:
                ans.append(nums[i])
                k, m = i, m - 1
            i -= 1
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int[] f = new int[n];
        Arrays.fill(f, 1);
        int k = 0;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] % nums[j] == 0) {
                    f[i] = Math.max(f[i], f[j] + 1);
                }
            }
            if (f[k] < f[i]) {
                k = i;
            }
        }
        int m = f[k];
        List<Integer> ans = new ArrayList<>();
        for (int i = k; m > 0; --i) {
            if (nums[k] % nums[i] == 0 && f[i] == m) {
                ans.add(nums[i]);
                k = i;
                --m;
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
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        ranges::sort(nums);
        int n = nums.size();
        int f[n];
        int k = 0;
        for (int i = 0; i < n; ++i) {
            f[i] = 1;
            for (int j = 0; j < i; ++j) {
                if (nums[i] % nums[j] == 0) {
                    f[i] = max(f[i], f[j] + 1);
                }
            }
            if (f[k] < f[i]) {
                k = i;
            }
        }
        int m = f[k];
        vector<int> ans;
        for (int i = k; m > 0; --i) {
            if (nums[k] % nums[i] == 0 && f[i] == m) {
                ans.push_back(nums[i]);
                k = i;
                --m;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func largestDivisibleSubset(nums []int) (ans []int) {
	sort.Ints(nums)
	n := len(nums)
	f := make([]int, n)
	k := 0
	for i := 0; i < n; i++ {
		f[i] = 1
		for j := 0; j < i; j++ {
			if nums[i]%nums[j] == 0 {
				f[i] = max(f[i], f[j]+1)
			}
		}
		if f[k] < f[i] {
			k = i
		}
	}
	m := f[k]
	for i := k; m > 0; i-- {
		if nums[k]%nums[i] == 0 && f[i] == m {
			ans = append(ans, nums[i])
			k = i
			m--
		}
	}
	return
}
```

#### TypeScript

```ts
function largestDivisibleSubset(nums: number[]): number[] {
    nums.sort((a, b) => a - b);
    const n = nums.length;
    const f: number[] = Array(n).fill(1);
    let k = 0;

    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < i; ++j) {
            if (nums[i] % nums[j] === 0) {
                f[i] = Math.max(f[i], f[j] + 1);
            }
        }
        if (f[k] < f[i]) {
            k = i;
        }
    }

    let m = f[k];
    const ans: number[] = [];
    for (let i = k; m > 0; --i) {
        if (nums[k] % nums[i] === 0 && f[i] === m) {
            ans.push(nums[i]);
            k = i;
            --m;
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn largest_divisible_subset(nums: Vec<i32>) -> Vec<i32> {
        let mut nums = nums;
        nums.sort();

        let n = nums.len();
        let mut f = vec![1; n];
        let mut k = 0;

        for i in 0..n {
            for j in 0..i {
                if nums[i] % nums[j] == 0 {
                    f[i] = f[i].max(f[j] + 1);
                }
            }
            if f[k] < f[i] {
                k = i;
            }
        }

        let mut m = f[k];
        let mut ans = Vec::new();

        for i in (0..=k).rev() {
            if nums[k] % nums[i] == 0 && f[i] == m {
                ans.push(nums[i]);
                k = i;
                m -= 1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
