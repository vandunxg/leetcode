---
comments: true
difficulty: Medium
rating: 1748
source: Weekly Contest 275 Q2
tags:
    - Array
    - Sliding Window
---

<!-- problem:start -->

# [2134. Minimum Swaps to Group All 1's Together II](https://leetcode.com/problems/minimum-swaps-to-group-all-1s-together-ii)

[中文文档](/solution/2100-2199/2134.Minimum%20Swaps%20to%20Group%20All%201%27s%20Together%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Một <strong>phép hoán đổi</strong> được định nghĩa là chọn hai vị trí <strong>khác nhau</strong> trong một mảng và đổi giá trị tại đó.</p>

<p>Một mảng <strong>dạng vòng</strong> được định nghĩa là mảng mà ta xem phần tử <strong>đầu tiên</strong> và phần tử <strong>cuối cùng</strong> là <strong>kề nhau</strong>.</p>

<p>Cho một mảng <strong>nhị phân</strong> <strong>dạng vòng</strong> <code>nums</code>, hãy trả về <em>số phép hoán đổi nhỏ nhất cần thực hiện để gom tất cả các số </em><code>1</code><em> xuất hiện trong mảng vào cùng một <strong>vị trí bất kỳ</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,0,1,1,0,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Dưới đây là một vài cách để gom tất cả các số 1 lại với nhau:
[0,<u>0</u>,<u>1</u>,1,1,0,0] với 1 phép hoán đổi.
[0,1,<u>1</u>,1,<u>0</u>,0,0] với 1 phép hoán đổi.
[1,1,0,0,0,0,1] với 2 phép hoán đổi (sử dụng tính chất vòng của mảng).
Không có cách nào gom tất cả các số 1 lại với nhau mà không cần phép hoán đổi nào.
Do đó, số phép hoán đổi nhỏ nhất cần thực hiện là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1,1,0,0,1,1,0]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Dưới đây là một vài cách để gom tất cả các số 1 lại với nhau:
[1,1,1,0,0,0,0,1,1] với 2 phép hoán đổi (sử dụng tính chất vòng của mảng).
[1,1,1,1,1,0,0,0,0] với 2 phép hoán đổi.
Không có cách nào gom tất cả các số 1 lại với nhau với 0 hoặc 1 phép hoán đổi.
Do đó, số phép hoán đổi nhỏ nhất cần thực hiện là 2.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,0,0,1]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Tất cả các số 1 đã được gom lại với nhau nhờ tính chất vòng của mảng.
Do đó, số phép hoán đổi nhỏ nhất cần thực hiện là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Việc gom tất cả các số $1$ trên một vòng tròn tương đương với việc tìm một cửa sổ có độ dài $k=\textit{count}(1)$ chứa nhiều số $1$ nhất; đáp án là $k$ trừ đi số lượng lớn nhất đó. Nếu đếm lại từng cửa sổ, độ phức tạp sẽ là bậc hai.
>
> Với cửa sổ vòng có độ dài cố định, ta trượt theo modulo thêm $k$ bước. Đếm số $1$ trong $k$ phần tử đầu tiên, sau đó cộng chỉ số phần tử đi vào và trừ chỉ số phần tử đi ra.
>
> Sau khi duyệt hết một vòng, trả về $k-\textit{mx}$.

<!-- thinking:end -->

Trước hết, ta đếm số lượng số $1$ trong mảng, ký hiệu là $k$. Bài toán thực chất yêu cầu tìm một mảng con dạng vòng có độ dài $k$ chứa nhiều số $1$ nhất. Vì vậy, số phép hoán đổi nhỏ nhất bằng $k$ trừ đi số lượng số $1$ lớn nhất trong mảng con đó.

Ta có thể giải bài toán bằng cửa sổ trượt. Trước tiên, ta đếm số $1$ trong $k$ phần tử đầu tiên của mảng, ký hiệu là $cnt$. Sau đó, ta duy trì một cửa sổ có độ dài $k$. Mỗi khi dịch cửa sổ sang phải một vị trí, ta cập nhật $cnt$ và đồng thời cập nhật giá trị lớn nhất của $cnt$, tức là $mx = \max(mx, cnt)$. Cuối cùng, đáp án là $k - mx$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        k = nums.count(1)
        mx = cnt = sum(nums[:k])
        n = len(nums)
        for i in range(k, n + k):
            cnt += nums[i % n]
            cnt -= nums[(i - k + n) % n]
            mx = max(mx, cnt)
        return k - mx
```

#### Java

```java
class Solution {
    public int minSwaps(int[] nums) {
        int k = Arrays.stream(nums).sum();
        int n = nums.length;
        int cnt = 0;
        for (int i = 0; i < k; ++i) {
            cnt += nums[i];
        }
        int mx = cnt;
        for (int i = k; i < n + k; ++i) {
            cnt += nums[i % n] - nums[(i - k + n) % n];
            mx = Math.max(mx, cnt);
        }
        return k - mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSwaps(vector<int>& nums) {
        int k = accumulate(nums.begin(), nums.end(), 0);
        int n = nums.size();
        int cnt = accumulate(nums.begin(), nums.begin() + k, 0);
        int mx = cnt;
        for (int i = k; i < n + k; ++i) {
            cnt += nums[i % n] - nums[(i - k + n) % n];
            mx = max(mx, cnt);
        }
        return k - mx;
    }
};
```

#### Go

```go
func minSwaps(nums []int) int {
	k := 0
	for _, x := range nums {
		k += x
	}
	cnt := 0
	for i := 0; i < k; i++ {
		cnt += nums[i]
	}
	mx := cnt
	n := len(nums)
	for i := k; i < n+k; i++ {
		cnt += nums[i%n] - nums[(i-k+n)%n]
		mx = max(mx, cnt)
	}
	return k - mx
}
```

#### TypeScript

```ts
function minSwaps(nums: number[]): number {
    const n = nums.length;
    const k = nums.reduce((a, b) => a + b, 0);
    let cnt = k - nums.slice(0, k).reduce((a, b) => a + b, 0);
    let min = cnt;

    for (let i = k; i < n + k; i++) {
        cnt += nums[i - k] - nums[i % n];
        min = Math.min(min, cnt);
    }

    return min;
}
```

#### JavaScript

```js
function minSwaps(nums) {
    const n = nums.length;
    const k = nums.reduce((a, b) => a + b, 0);
    let cnt = k - nums.slice(0, k).reduce((a, b) => a + b, 0);
    let min = cnt;

    for (let i = k; i < n + k; i++) {
        cnt += nums[i - k] - nums[i % n];
        min = Math.min(min, cnt);
    }

    return min;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_swaps(nums: Vec<i32>) -> i32 {
        let k: i32 = nums.iter().sum();
        let n: usize = nums.len();
        let mut cnt: i32 = 0;
        for i in 0..k {
            cnt += nums[i as usize];
        }
        let mut mx: i32 = cnt;
        for i in k..(n as i32) + k {
            cnt += nums[(i % (n as i32)) as usize]
                - nums[((i - k + (n as i32)) % (n as i32)) as usize];
            mx = mx.max(cnt);
        }
        return k - mx;
    }
}
```

#### C#

```cs
public class Solution {
    public int MinSwaps(int[] nums) {
        int k = nums.Sum();
        int n = nums.Length;
        int cnt = 0;
        for (int i = 0; i < k; ++i) {
            cnt += nums[i];
        }
        int mx = cnt;
        for (int i = k; i < n + k; ++i) {
            cnt += nums[i % n] - nums[(i - k + n) % n];
            mx = Math.Max(mx, cnt);
        }
        return k - mx;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dùng phép lấy modulo để xử lý chỉ số. Trên mảng tuyến tính, tổng tiền tố cũng đếm được một giá trị $x$ trong mọi cửa sổ có độ dài bằng số lượng $x$ trên toàn mảng; số phép hoán đổi bằng độ dài đó trừ đi số lượng phần tử của cửa sổ.
>
> Việc gom tất cả các số $1$ trên một vòng tròn tương đương với góc nhìn lấy phần bù của việc gom tất cả các số $0$, vì vậy phần cài đặt lấy giá trị nhỏ hơn trong hai đáp án dùng tổng tiền tố.
>
> Cách này trình bày công thức tổng tiền tố cho cùng một bài toán cửa sổ.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function minSwaps(nums: number[]): number {
    const n = nums.length;

    const getMin = (x: 0 | 1) => {
        const prefixSum = Array(n + 1).fill(0);
        for (let i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + (nums[i - 1] === x);
        }

        const length = prefixSum[n];
        let ans = Number.POSITIVE_INFINITY;
        for (let l = 0, r = length; r <= n; l++, r++) {
            const min = length - (prefixSum[r] - prefixSum[l]);
            ans = Math.min(ans, min);
        }

        return ans;
    };

    return Math.min(getMin(0), getMin(1));
}
```

#### JavaScript

```js
function minSwaps(nums) {
    const n = nums.length;

    const getMin = x => {
        const prefixSum = Array(n + 1).fill(0);
        for (let i = 1; i <= n; i++) {
            prefixSum[i] = prefixSum[i - 1] + (nums[i - 1] === x);
        }

        const length = prefixSum[n];
        let ans = Number.POSITIVE_INFINITY;
        for (let l = 0, r = length; r <= n; l++, r++) {
            const min = length - (prefixSum[r] - prefixSum[l]);
            ans = Math.min(ans, min);
        }

        return ans;
    };

    return Math.min(getMin(0), getMin(1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
