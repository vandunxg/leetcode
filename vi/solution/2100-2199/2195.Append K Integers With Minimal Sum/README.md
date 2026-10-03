---
comments: true
difficulty: Medium
rating: 1658
source: Weekly Contest 283 Q2
tags:
    - Greedy
    - Array
    - Math
    - Sorting
---

<!-- problem:start -->

# [2195. Append K Integers With Minimal Sum](https://leetcode.com/problems/append-k-integers-with-minimal-sum)

[中文文档](/solution/2100-2199/2195.Append%20K%20Integers%20With%20Minimal%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>. Hãy nối thêm <code>k</code> số nguyên dương <strong>khác nhau</strong> <strong>không</strong> xuất hiện trong <code>nums</code> vào <code>nums</code> sao cho tổng của mảng sau cùng là <strong>nhỏ nhất</strong>.</p>

<p>Trả về <em>tổng của</em> <code>k</code> <em>số nguyên được nối thêm vào</em> <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,4,25,10,25], k = 2
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Hai số nguyên dương khác nhau không xuất hiện trong nums được nối thêm là 2 và 3.
Tổng của nums sau khi nối thêm là 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70, đây là tổng nhỏ nhất.
Tổng của hai số nguyên được nối thêm là 2 + 3 = 5, nên ta trả về 5.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5,6], k = 6
<strong>Đầu ra:</strong> 25
<strong>Giải thích:</strong> Sáu số nguyên dương khác nhau không xuất hiện trong nums được nối thêm là 1, 2, 3, 4, 7 và 8.
Tổng của nums sau khi nối thêm là 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36, đây là tổng nhỏ nhất.
Tổng của sáu số nguyên được nối thêm là 1 + 2 + 3 + 4 + 7 + 8 = 25, nên ta trả về 25.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam + Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Cần nối thêm $k$ số nguyên dương bị thiếu với tổng nhỏ nhất, tức là $k$ số nguyên dương nhỏ nhất không xuất hiện trong $\textit{nums}$. Nếu đi từ $1$ và kiểm tra sự xuất hiện của từng số, vòng lặp có thể quá lâu khi $k$ và các khoảng trống lớn.
>
> Sắp xếp cùng hai phần tử mốc $0$ và $2\times 10^9$. Mỗi cặp kề nhau $(a,b)$ tạo ra một khoảng trống liên tiếp; ta lấy $\min(k,b-a-1)$ số đầu tiên, có tổng là một cấp số cộng.
>
> Trừ dần $k$ từ trái sang phải.

<!-- thinking:end -->

Ta có thể thêm hai phần tử mốc vào mảng là $0$ và $2 \times 10^9$.

Sau đó, ta sắp xếp mảng. Với bất kỳ hai phần tử kề nhau $a$ và $b$ trong mảng, các số nguyên trong khoảng $[a+1, b-1]$ không xuất hiện trong mảng, nên ta có thể thêm các số này vào mảng.

Do đó, ta duyệt các cặp phần tử kề nhau $(a, b)$ trong mảng theo thứ tự từ nhỏ đến lớn. Với mỗi cặp phần tử kề nhau, ta tính số lượng số nguyên $m$ trong khoảng $[a+1, b-1]$. Tổng của $m$ số nguyên này là $\frac{m \times (a+1 + a+m)}{2}$. Ta cộng tổng này vào đáp án và trừ $m$ khỏi $k$. Nếu $k$ giảm về $0$, ta có thể dừng việc duyệt và trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimalKSum(self, nums: List[int], k: int) -> int:
        nums.extend([0, 2 * 10**9])
        nums.sort()
        ans = 0
        for a, b in pairwise(nums):
            m = max(0, min(k, b - a - 1))
            ans += (a + 1 + a + m) * m // 2
            k -= m
        return ans
```

#### Java

```java
class Solution {
    public long minimalKSum(int[] nums, int k) {
        int n = nums.length;
        int[] arr = new int[n + 2];
        arr[1] = 2 * 1000000000;
        System.arraycopy(nums, 0, arr, 2, n);
        Arrays.sort(arr);
        long ans = 0;
        for (int i = 0; i < n + 1 && k > 0; ++i) {
            int m = Math.max(0, Math.min(k, arr[i + 1] - arr[i] - 1));
            ans += (arr[i] + 1L + arr[i] + m) * m / 2;
            k -= m;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long minimalKSum(vector<int>& nums, int k) {
        nums.push_back(0);
        nums.push_back(2e9);
        sort(nums.begin(), nums.end());
        long long ans = 0;
        for (int i = 0; i < nums.size() - 1 && k > 0; ++i) {
            int m = max(0, min(k, nums[i + 1] - nums[i] - 1));
            ans += 1LL * (nums[i] + 1 + nums[i] + m) * m / 2;
            k -= m;
        }
        return ans;
    }
};
```

#### Go

```go
func minimalKSum(nums []int, k int) (ans int64) {
	nums = append(nums, []int{0, 2e9}...)
	sort.Ints(nums)
	for i, b := range nums[1:] {
		a := nums[i]
		m := max(0, min(k, b-a-1))
		ans += int64(a+1+a+m) * int64(m) / 2
		k -= m
	}
	return ans
}
```

#### TypeScript

```ts
function minimalKSum(nums: number[], k: number): number {
    nums.push(...[0, 2 * 10 ** 9]);
    nums.sort((a, b) => a - b);
    let ans = 0;
    for (let i = 0; i < nums.length - 1; ++i) {
        const m = Math.max(0, Math.min(k, nums[i + 1] - nums[i] - 1));
        ans += Number((BigInt(nums[i] + 1 + nums[i] + m) * BigInt(m)) / BigInt(2));
        k -= m;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
