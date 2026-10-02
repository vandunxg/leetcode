---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Queue
    - Array
    - Prefix Sum
    - Sliding Window
    - Brute-Force Search
---

<!-- problem:start -->

# [995. Minimum Number of K Consecutive Bit Flips](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips)

[中文文档](/solution/0900-0999/0995.Minimum%20Number%20of%20K%20Consecutive%20Bit%20Flips/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code> và số nguyên <code>k</code>.</p>

<p><strong>Lật k bit</strong> là chọn một <strong>mảng con</strong> có độ dài <code>k</code> trong <code>nums</code>, rồi đồng thời đổi mọi <code>0</code> trong mảng con thành <code>1</code> và mọi <code>1</code> thành <code>0</code>.</p>

<p>Hãy trả về <em>số lần <strong>lật k bit</strong> ít nhất cần thực hiện để mảng không còn </em><code>0</code><em>.</em> Nếu không thể, trả về <code>-1</code>.</p>

<p><strong>Mảng con</strong> là một phần <strong>liên tiếp</strong> của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,0], k = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Lật nums[0], sau đó lật nums[2].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,0], k = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Dù lật các mảng con có độ dài 2 theo cách nào, ta cũng không thể biến mảng thành [1,1,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0,1,0,1,1,0], k = 3
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 
Lật nums[0],nums[1],nums[2]: nums trở thành [1,1,1,1,0,1,1,0]
Lật nums[4],nums[5],nums[6]: nums trở thành [1,1,1,1,1,0,0,0]
Lật nums[5],nums[6],nums[7]: nums trở thành [1,1,1,1,1,1,1,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác lật $k$ bit liên tiếp; ta cần biến toàn bộ mảng thành 1 với số lần lật ít nhất. Vì $n\le 10^5$, mô phỏng từng lần lật quá chậm. Theo greedy, nếu sau các lần lật trước mà vị trí $i$ vẫn là $0$, ta buộc phải bắt đầu một lần lật mới tại đây. Mảng hiệu đánh dấu đoạn $[i,i+k)$ trong $O(1)$.

<!-- thinking:end -->

Ta nhận thấy kết quả của việc lật các đoạn liên tiếp không phụ thuộc vào thứ tự thực hiện. Vì vậy, ta có thể greedy xác định số lần lật cần thiết tại từng vị trí.

Ta có thể duyệt mảng từ trái sang phải.

Giả sử ta đang xử lý vị trí $i$ và các phần tử bên trái đã được xử lý. Nếu phần tử tại $i$ là $0$, ta phải thực hiện thao tác lật đoạn $[i,..i+k-1]$. Ta dùng mảng hiệu $d$ để theo dõi số lần lật tại mỗi vị trí. Để xác định vị trí $i$ hiện có cần lật hay không, chỉ cần xét $s = \sum_{j=0}^{i}d[j]$ và tính chẵn lẻ của $nums[i]$. Nếu $s$ và $nums[i]$ có cùng tính chẵn lẻ, phần tử tại $i$ vẫn là $0$ và cần được lật. Khi đó, ta kiểm tra $i+k$ có vượt quá độ dài mảng hay không. Nếu có, không thể đạt mục tiêu nên trả về $-1$. Nếu không, tăng $d[i]$ lên $1$, giảm $d[i+k]$ đi $1$, tăng đáp án lên $1$ và tăng $s$ lên $1$.

Như vậy, sau khi xử lý hết các phần tử trong mảng, ta có thể trả về đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minKBitFlips(self, nums: List[int], k: int) -> int:
        n = len(nums)
        d = [0] * (n + 1)
        ans = s = 0
        for i, x in enumerate(nums):
            s += d[i]
            if s % 2 == x:
                if i + k > n:
                    return -1
                d[i] += 1
                d[i + k] -= 1
                s += 1
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int n = nums.length;
        int[] d = new int[n + 1];
        int ans = 0, s = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            if (s % 2 == nums[i]) {
                if (i + k > n) {
                    return -1;
                }
                ++d[i];
                --d[i + k];
                ++s;
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
    int minKBitFlips(vector<int>& nums, int k) {
        int n = nums.size();
        int d[n + 1];
        memset(d, 0, sizeof(d));
        int ans = 0, s = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            if (s % 2 == nums[i]) {
                if (i + k > n) {
                    return -1;
                }
                ++d[i];
                --d[i + k];
                ++s;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minKBitFlips(nums []int, k int) (ans int) {
	n := len(nums)
	d := make([]int, n+1)
	s := 0
	for i, x := range nums {
		s += d[i]
		if s%2 == x {
			if i+k > n {
				return -1
			}
			d[i]++
			d[i+k]--
			s++
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function minKBitFlips(nums: number[], k: number): number {
    const n = nums.length;
    const d: number[] = Array(n + 1).fill(0);
    let [ans, s] = [0, 0];
    for (let i = 0; i < n; ++i) {
        s += d[i];
        if (s % 2 === nums[i]) {
            if (i + k > n) {
                return -1;
            }
            d[i]++;
            d[i + k]--;
            s++;
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_k_bit_flips(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut d = vec![0; n + 1];
        let mut ans = 0;
        let mut s = 0;
        for i in 0..n {
            s += d[i];
            if s % 2 == nums[i] {
                if i + (k as usize) > n {
                    return -1;
                }
                d[i] += 1;
                d[i + (k as usize)] -= 1;
                s += 1;
                ans += 1;
            }
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Mảng hiệu cần thêm bộ nhớ tuyến tính. Ta có thể thực hiện cùng chiến lược greedy ngay trên mảng bằng cách theo dõi tính chẵn lẻ hiện tại và đánh dấu $-1$ tại mỗi vị trí bắt đầu lật; loại bỏ tác động của lần lật bằng XOR khi cửa sổ trượt qua.

<!-- thinking:end -->

Ta có thể dùng biến $\textit{flipped}$ để cho biết vị trí hiện tại đã bị lật hay chưa. Nếu $\textit{flipped} = 1$, vị trí hiện tại đã bị lật; ngược lại, vị trí đó chưa bị lật. Với các vị trí đã lật, ta có thể gán giá trị $-1$ để phân biệt chúng.

Tiếp theo, ta duyệt mảng từ trái sang phải. Với mỗi vị trí $i$, nếu $i \geq k$ và phần tử tại $i-k$ là $-1$, trạng thái lật của vị trí hiện tại phải đổi ngược so với trạng thái lật của vị trí trước đó. Tức là, $\textit{flipped} = \textit{flipped} \oplus 1$. Nếu giá trị phần tử hiện tại bằng trạng thái lật hiện tại, ta cần lật vị trí đó. Khi ấy, kiểm tra xem $i+k$ có vượt quá độ dài mảng không. Nếu có, mục tiêu không thể đạt được nên trả về $-1$. Nếu không, đảo trạng thái lật hiện tại, tăng đáp án lên $1$ và gán phần tử tại vị trí hiện tại thành $-1$.

Sau khi xử lý tất cả phần tử trong mảng theo cách này, ta trả về đáp án.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minKBitFlips(self, nums: List[int], k: int) -> int:
        ans = flipped = 0
        for i, x in enumerate(nums):
            if i >= k and nums[i - k] == -1:
                flipped ^= 1
            if x == flipped:
                if i + k > len(nums):
                    return -1
                flipped ^= 1
                ans += 1
                nums[i] = -1
        return ans
```

#### Java

```java
class Solution {
    public int minKBitFlips(int[] nums, int k) {
        int n = nums.length;
        int ans = 0, flipped = 0;
        for (int i = 0; i < n; ++i) {
            if (i >= k && nums[i - k] == -1) {
                flipped ^= 1;
            }
            if (flipped == nums[i]) {
                if (i + k > n) {
                    return -1;
                }
                flipped ^= 1;
                ++ans;
                nums[i] = -1;
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
    int minKBitFlips(vector<int>& nums, int k) {
        int n = nums.size();
        int ans = 0, flipped = 0;
        for (int i = 0; i < n; ++i) {
            if (i >= k && nums[i - k] == -1) {
                flipped ^= 1;
            }
            if (flipped == nums[i]) {
                if (i + k > n) {
                    return -1;
                }
                flipped ^= 1;
                ++ans;
                nums[i] = -1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minKBitFlips(nums []int, k int) (ans int) {
	flipped := 0
	for i, x := range nums {
		if i >= k && nums[i-k] == -1 {
			flipped ^= 1
		}
		if flipped == x {
			if i+k > len(nums) {
				return -1
			}
			flipped ^= 1
			ans++
			nums[i] = -1
		}
	}
	return
}
```

#### TypeScript

```ts
function minKBitFlips(nums: number[], k: number): number {
    const n = nums.length;
    let [ans, flipped] = [0, 0];
    for (let i = 0; i < n; i++) {
        if (nums[i - k] === -1) {
            flipped ^= 1;
        }
        if (nums[i] === flipped) {
            if (i + k > n) {
                return -1;
            }
            flipped ^= 1;
            ++ans;
            nums[i] = -1;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_k_bit_flips(mut nums: Vec<i32>, k: i32) -> i32 {
        let mut ans = 0;
        let mut flipped = 0;
        let k = k as usize;

        for i in 0..nums.len() {
            if i >= k && nums[i - k] == -1 {
                flipped ^= 1;
            }
            if flipped == nums[i] {
                if i + k > nums.len() {
                    return -1;
                }
                flipped ^= 1;
                ans += 1;
                nums[i] = -1;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
