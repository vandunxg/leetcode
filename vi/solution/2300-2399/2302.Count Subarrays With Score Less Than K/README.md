---
comments: true
difficulty: Hard
rating: 1808
source: Biweekly Contest 80 Q4
tags:
    - Array
    - Binary Search
    - Prefix Sum
    - Sliding Window
---

<!-- problem:start -->

# [2302. Count Subarrays With Score Less Than K](https://leetcode.com/problems/count-subarrays-with-score-less-than-k)

[中文文档](/solution/2300-2399/2302.Count%20Subarrays%20With%20Score%20Less%20Than%20K/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Score</strong> của một mảng được định nghĩa là <strong>tích</strong> của tổng các phần tử và độ dài của mảng.</p>

<ul>
	<li>Ví dụ, score của <code>[1, 2, 3, 4, 5]</code> là <code>(1 + 2 + 3 + 4 + 5) * 5 = 75</code>.</li>
</ul>

<p>Cho một mảng số nguyên dương <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <em><strong>số lượng mảng con không rỗng</strong> của</em> <code>nums</code> <em>có score <strong>nhỏ hơn nghiêm ngặt</strong> </em><code>k</code>.</p>

<p><strong>Mảng con</strong> là một dãy liên tiếp các phần tử trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,4,3,5], k = 10
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
6 mảng con có score nhỏ hơn 10 là:
- [2] có score 2 * 1 = 2.
- [1] có score 1 * 1 = 1.
- [4] có score 4 * 1 = 4.
- [3] có score 3 * 1 = 3.
- [5] có score 5 * 1 = 5.
- [2,1] có score (2 + 1) * 2 = 6.
Lưu ý rằng các mảng con như [1,4] và [4,3,5] không được tính vì score của chúng lần lượt là 10 và 36, trong khi ta cần score nhỏ hơn nghiêm ngặt 10.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1], k = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong>
Mọi mảng con trừ [1,1,1] đều có score nhỏ hơn 5.
[1,1,1] có score (1 + 1 + 1) * 3 = 9, lớn hơn 5.
Vì vậy, có 5 mảng con có score nhỏ hơn 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Score của một mảng con là tích giữa tổng các phần tử và độ dài của nó. Duyệt qua mọi mảng con có độ phức tạp $O(n^2)$, không đáp ứng được với $n \le 10^5$. Vì mọi giá trị đều dương, với một điểm kết thúc cố định, cửa sổ càng dài thì score càng lớn, nên các điểm bắt đầu hợp lệ tạo thành một prefix.
>
> Tổng tiền tố cho phép tính tổng trên mọi đoạn trong thời gian hằng số. Ta tìm kiếm nhị phân độ dài lớn nhất $l$ thỏa mãn $(s[i]-s[i-l])\times l < k$. Với điểm kết thúc đó, có $l$ mảng con hợp lệ.

<!-- thinking:end -->

Trước tiên, ta tính mảng tổng tiền tố $s$ của mảng $\textit{nums}$, trong đó $s[i]$ biểu diễn tổng của $i$ phần tử đầu tiên trong $\textit{nums}$.

Tiếp theo, ta lần lượt chọn mỗi phần tử của $\textit{nums}$ làm phần tử cuối của một mảng con. Với mỗi phần tử, ta có thể dùng tìm kiếm nhị phân để tìm độ dài lớn nhất $l$ sao cho $s[i] - s[i - l] \times l < k$. Số lượng mảng con kết thúc tại phần tử này là $l$, và tổng tất cả các giá trị $l$ là đáp án cuối cùng.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        s = list(accumulate(nums, initial=0))
        ans = 0
        for i in range(1, len(s)):
            l, r = 0, i
            while l < r:
                mid = (l + r + 1) >> 1
                if (s[i] - s[i - mid]) * mid < k:
                    l = mid
                else:
                    r = mid - 1
            ans += l
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        long ans = 0;
        for (int i = 1; i <= n; ++i) {
            int l = 0, r = i;
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                if ((s[i] - s[i - mid]) * mid < k) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            ans += l;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, long long k) {
        int n = nums.size();
        long long s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        long long ans = 0;
        for (int i = 1; i <= n; ++i) {
            int l = 0, r = i;
            while (l < r) {
                int mid = (l + r + 1) >> 1;
                if ((s[i] - s[i - mid]) * mid < k) {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            ans += l;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int64) (ans int64) {
	n := len(nums)
	s := make([]int64, n+1)
	for i, x := range nums {
		s[i+1] = s[i] + int64(x)
	}
	for i := 1; i <= n; i++ {
		l, r := 0, i
		for l < r {
			mid := (l + r + 1) >> 1
			if (s[i]-s[i-mid])*int64(mid) < k {
				l = mid
			} else {
				r = mid - 1
			}
		}
		ans += int64(l)
	}
	return
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    const n = nums.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        s[i + 1] = s[i] + nums[i];
    }
    let ans = 0;
    for (let i = 1; i <= n; ++i) {
        let [l, r] = [0, i];
        while (l < r) {
            const mid = (l + r + 1) >> 1;
            if ((s[i] - s[i - mid]) * mid < k) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        ans += l;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_subarrays(nums: Vec<i32>, k: i64) -> i64 {
        let n = nums.len();
        let mut s = vec![0i64; n + 1];
        for i in 0..n {
            s[i + 1] = s[i] + nums[i] as i64;
        }
        let mut ans = 0i64;
        for i in 1..=n {
            let mut l = 0;
            let mut r = i;
            while l < r {
                let mid = (l + r + 1) / 2;
                let sum = s[i] - s[i - mid];
                if sum * (mid as i64) < k {
                    l = mid;
                } else {
                    r = mid - 1;
                }
            }
            ans += l as i64;
        }
        ans
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
> Phương pháp 1 lưu một mảng tổng tiền tố và phải trả chi phí logarit cho mỗi điểm kết thúc. Vì các giá trị đều dương, score giảm khi điểm bắt đầu dịch sang phải, nên con trỏ trái chỉ tiến về phía trước. Ta duy trì tổng của cửa sổ và đếm các mảng con hợp lệ chỉ trong một lượt duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể dùng kỹ thuật hai con trỏ để duy trì một cửa sổ trượt sao cho tổng các phần tử trong cửa sổ nhỏ hơn $k$. Số lượng mảng con kết thúc tại phần tử hiện tại bằng độ dài của cửa sổ. Tổng tất cả độ dài cửa sổ là đáp án cuối cùng.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        ans = s = j = 0
        for i, x in enumerate(nums):
            s += x
            while s * (i - j + 1) >= k:
                s -= nums[j]
                j += 1
            ans += i - j + 1
        return ans
```

#### Java

```java
class Solution {
    public long countSubarrays(int[] nums, long k) {
        long ans = 0, s = 0;
        for (int i = 0, j = 0; i < nums.length; ++i) {
            s += nums[i];
            while (s * (i - j + 1) >= k) {
                s -= nums[j++];
            }
            ans += i - j + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, long long k) {
        long long ans = 0, s = 0;
        for (int i = 0, j = 0; i < nums.size(); ++i) {
            s += nums[i];
            while (s * (i - j + 1) >= k) {
                s -= nums[j++];
            }
            ans += i - j + 1;
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int64) (ans int64) {
	s, j := 0, 0
	for i, x := range nums {
		s += x
		for int64(s*(i-j+1)) >= k {
			s -= nums[j]
			j++
		}
		ans += int64(i - j + 1)
	}
	return
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    let [ans, s, j] = [0, 0, 0];
    for (let i = 0; i < nums.length; ++i) {
        s += nums[i];
        while (s * (i - j + 1) >= k) {
            s -= nums[j++];
        }
        ans += i - j + 1;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_subarrays(nums: Vec<i32>, k: i64) -> i64 {
        let mut ans = 0i64;
        let mut s = 0i64;
        let mut j = 0;

        for i in 0..nums.len() {
            s += nums[i] as i64;
            while s * (i as i64 - j as i64 + 1) >= k {
                s -= nums[j] as i64;
                j += 1;
            }
            ans += i as i64 - j as i64 + 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
