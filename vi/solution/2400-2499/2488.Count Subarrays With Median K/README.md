---
comments: true
difficulty: Hard
rating: 1998
source: Weekly Contest 321 Q4
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [2488. Count Subarrays With Median K](https://leetcode.com/problems/count-subarrays-with-median-k)

[Tài liệu tiếng Trung](/solution/2400-2499/2488.Count%20Subarrays%20With%20Median%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một mảng <code>nums</code> có kích thước <code>n</code>, gồm các số nguyên <strong>khác nhau </strong> từ <code>1</code> đến <code>n</code> và một số nguyên dương <code>k</code>.</p>

<p>Trả về <em>số lượng mảng con không rỗng trong </em><code>nums</code><em> có <strong>median</strong> bằng </em><code>k</code>.</p>

<p><strong>Lưu ý</strong>:</p>

<ul>
	<li>Median của một mảng là phần tử <strong>ở giữa </strong>sau khi sắp xếp mảng theo thứ tự <strong>tăng dần </strong>. Nếu mảng có độ dài chẵn, median là phần tử <strong>ở giữa bên trái </strong>.

    <ul>
    <li>Ví dụ, median của <code>[2,3,1,4]</code> là <code>2</code>, còn median của <code>[8,4,3,5,1]</code> là <code>4</code>.</li>
    </ul>
    </li>
    <li>Mảng con là một phần liên tiếp của mảng.</li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,1,4,5], k = 4
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các mảng con có median bằng 4 là: [4], [4,5] và [1,4,5].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,1], k = 3
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> [3] là mảng con duy nhất có median bằng 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i], k &lt;= n</code></li>
	<li>Các số nguyên trong <code>nums</code> là khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> $nums$ là một hoán vị. Mảng con có median $k$ chứa $k$, và hiệu giữa số giá trị $>k$ và số giá trị $<k$ bằng $0$ (độ dài lẻ) hoặc $1$ (độ dài chẵn). Với $n\le 10^5$, duyệt sang phải từ $k$ để ghi lại balance $x$, sau đó duyệt sang trái và ghép với $-x$ và $-x+1$.
>
> Balance một phía thuộc $\{0,1\}$ cũng được tính. Một map lưu tần suất ở phía bên phải.

<!-- thinking:end -->

Trước hết, tìm vị trí $i$ của median $k$ trong mảng, sau đó bắt đầu duyệt từ $i$ về cả hai phía để đếm số mảng con có median bằng $k$.

Đặt biến kết quả $ans$, biểu thị số lượng mảng con có median bằng $k$. Ban đầu, $ans = 1$, nghĩa là hiện có một mảng con độ dài $1$ có median bằng $k$. Ngoài ra, đặt một bộ đếm $cnt$ để đếm hiệu giữa "số phần tử lớn hơn $k$" và "số phần tử nhỏ hơn $k$" trong mảng đang duyệt.

Tiếp theo, bắt đầu duyệt sang phải từ $i + 1$. Duy trì biến $x$, biểu thị hiệu giữa "số phần tử lớn hơn $k$" và "số phần tử nhỏ hơn $k$" trong mảng con bên phải hiện tại. Nếu $x \in [0, 1]$, median của mảng con hiện tại là $k$, nên tăng biến kết quả $ans$ thêm $1$. Sau đó, thêm giá trị $x$ vào bộ đếm $cnt$.

Tương tự, bắt đầu duyệt sang trái từ $i - 1$, đồng thời duy trì biến $x$, biểu thị hiệu giữa "số phần tử lớn hơn $k$" và "số phần tử nhỏ hơn $k$" trong mảng con bên trái hiện tại. Nếu $x \in [0, 1]$, median của mảng con hiện tại là $k$, nên tăng biến kết quả $ans$ thêm $1$. Nếu $-x$ hoặc $-x + 1$ cũng có trong bộ đếm, điều đó có nghĩa là hiện có một mảng con trải dài qua cả hai phía của $i$ và có median bằng $k$; khi đó, $ans$ tăng thêm giá trị tương ứng trong bộ đếm, tức là $ans += cnt[-x] + cnt[-x + 1]$.

Cuối cùng, trả về biến kết quả $ans$.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

> Khi lập trình, ta có thể dùng trực tiếp một mảng có độ dài $2 \times n + 1$ để đếm hiệu giữa "số phần tử lớn hơn $k$" và "số phần tử nhỏ hơn $k$" trong mảng hiện tại. Mỗi lần cộng hiệu thêm $n$, ta có thể chuyển miền giá trị của hiệu từ $[-n, n]$ thành $[0, 2n]$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: List[int], k: int) -> int:
        i = nums.index(k)
        cnt = Counter()
        ans = 1
        x = 0
        for v in nums[i + 1 :]:
            x += 1 if v > k else -1
            ans += 0 <= x <= 1
            cnt[x] += 1
        x = 0
        for j in range(i - 1, -1, -1):
            x += 1 if nums[j] > k else -1
            ans += 0 <= x <= 1
            ans += cnt[-x] + cnt[-x + 1]
        return ans
```

#### Java

```java
class Solution {
    public int countSubarrays(int[] nums, int k) {
        int n = nums.length;
        int i = 0;
        for (; nums[i] != k; ++i) {
        }
        int[] cnt = new int[n << 1 | 1];
        int ans = 1;
        int x = 0;
        for (int j = i + 1; j < n; ++j) {
            x += nums[j] > k ? 1 : -1;
            if (x >= 0 && x <= 1) {
                ++ans;
            }
            ++cnt[x + n];
        }
        x = 0;
        for (int j = i - 1; j >= 0; --j) {
            x += nums[j] > k ? 1 : -1;
            if (x >= 0 && x <= 1) {
                ++ans;
            }
            ans += cnt[-x + n] + cnt[-x + 1 + n];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countSubarrays(vector<int>& nums, int k) {
        int n = nums.size();
        int i = find(nums.begin(), nums.end(), k) - nums.begin();
        int cnt[n << 1 | 1];
        memset(cnt, 0, sizeof(cnt));
        int ans = 1;
        int x = 0;
        for (int j = i + 1; j < n; ++j) {
            x += nums[j] > k ? 1 : -1;
            if (x >= 0 && x <= 1) {
                ++ans;
            }
            ++cnt[x + n];
        }
        x = 0;
        for (int j = i - 1; ~j; --j) {
            x += nums[j] > k ? 1 : -1;
            if (x >= 0 && x <= 1) {
                ++ans;
            }
            ans += cnt[-x + n] + cnt[-x + 1 + n];
        }
        return ans;
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int) int {
	i, n := 0, len(nums)
	for nums[i] != k {
		i++
	}
	ans := 1
	cnt := make([]int, n<<1|1)
	x := 0
	for j := i + 1; j < n; j++ {
		if nums[j] > k {
			x++
		} else {
			x--
		}
		if x >= 0 && x <= 1 {
			ans++
		}
		cnt[x+n]++
	}
	x = 0
	for j := i - 1; j >= 0; j-- {
		if nums[j] > k {
			x++
		} else {
			x--
		}
		if x >= 0 && x <= 1 {
			ans++
		}
		ans += cnt[-x+n] + cnt[-x+1+n]
	}
	return ans
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number): number {
    const i = nums.indexOf(k);
    const n = nums.length;
    const cnt = new Array((n << 1) | 1).fill(0);
    let ans = 1;
    let x = 0;
    for (let j = i + 1; j < n; ++j) {
        x += nums[j] > k ? 1 : -1;
        ans += x >= 0 && x <= 1 ? 1 : 0;
        ++cnt[x + n];
    }
    x = 0;
    for (let j = i - 1; ~j; --j) {
        x += nums[j] > k ? 1 : -1;
        ans += x >= 0 && x <= 1 ? 1 : 0;
        ans += cnt[-x + n] + cnt[-x + 1 + n];
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
