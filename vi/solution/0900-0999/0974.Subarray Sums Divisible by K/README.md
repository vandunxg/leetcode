---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k)

[中文文档](/solution/0900-0999/0974.Subarray%20Sums%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>, hãy trả về <em>số lượng </em><strong>mảng con</strong><em> không rỗng có tổng chia hết cho </em><code>k</code>.</p>

<p><strong>Mảng con</strong> là một đoạn <strong>liên tiếp</strong> trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,5,0,-2,-3,1], k = 5
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Có 7 mảng con có tổng chia hết cho k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [5], k = 9
<strong>Đầu ra:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>2 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Prefix Sum

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của một mảng con chia hết cho $k$ khi và chỉ khi hai tổng tiền tố tương đương modulo $k$. Liệt kê mọi đoạn có độ phức tạp bậc hai. Khi duyệt mảng, bộ đếm số dư cho biết có bao nhiêu tổng tiền tố trước đó có cùng số dư với $s$ hiện tại; đó chính là số mảng con hợp lệ kết thúc tại đây. Khởi tạo $cnt[0]=1$ cho tiền tố rỗng.

<!-- thinking:end -->

Giả sử tồn tại $i \leq j$ sao cho tổng của $\textit{nums}[i,..j]$ chia hết cho $k$. Gọi $s_i$ là tổng của $\textit{nums}[0,..i]$ và $s_j$ là tổng của $\textit{nums}[0,..j]$. Khi đó, $s_j - s_i$ chia hết cho $k$, tức $(s_j - s_i) \bmod k = 0$, suy ra $s_j \bmod k = s_i \bmod k$. Vì vậy, ta có thể dùng hash table để đếm số tổng tiền tố theo modulo $k$, qua đó nhanh chóng xác định có mảng con nào thỏa điều kiện hay không.

Ta dùng hash table $\textit{cnt}$ để đếm số tổng tiền tố theo modulo $k$, trong đó $\textit{cnt}[i]$ là số tổng tiền tố có số dư $i$ khi chia cho $k$. Ban đầu, $\textit{cnt}[0] = 1$. Biến $s$ lưu tổng tiền tố và ban đầu $s = 0$.

Tiếp theo, duyệt mảng $\textit{nums}$ từ trái sang phải. Với mỗi phần tử $x$, tính $s = (s + x) \bmod k$, rồi cập nhật đáp án $\textit{ans} = \textit{ans} + \textit{cnt}[s]$. Ở đây, $\textit{cnt}[s]$ là số tổng tiền tố có số dư $s$ khi chia cho $k$. Cuối cùng, tăng $\textit{cnt}[s]$ thêm $1$ rồi chuyển sang phần tử tiếp theo.

Cuối cùng, trả về đáp án $\textit{ans}$.

> Lưu ý: Vì $s$ có thể âm, ta có thể cộng $k$ vào kết quả của $s \bmod k$, sau đó lấy modulo $k$ một lần nữa để đảm bảo $s$ không âm.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraysDivByK(self, nums: List[int], k: int) -> int:
        cnt = Counter({0: 1})
        ans = s = 0
        for x in nums:
            s = (s + x) % k
            ans += cnt[s]
            cnt[s] += 1
        return ans
```

#### Java

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        int ans = 0, s = 0;
        for (int x : nums) {
            s = ((s + x) % k + k) % k;
            ans += cnt.getOrDefault(s, 0);
            cnt.merge(s, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        unordered_map<int, int> cnt{{0, 1}};
        int ans = 0, s = 0;
        for (int& x : nums) {
            s = ((s + x) % k + k) % k;
            ans += cnt[s]++;
        }
        return ans;
    }
};
```

#### Go

```go
func subarraysDivByK(nums []int, k int) (ans int) {
	cnt := map[int]int{0: 1}
	s := 0
	for _, x := range nums {
		s = ((s+x)%k + k) % k
		ans += cnt[s]
		cnt[s]++
	}
	return
}
```

#### TypeScript

```ts
function subarraysDivByK(nums: number[], k: number): number {
    const cnt: { [key: number]: number } = { 0: 1 };
    let s = 0;
    let ans = 0;
    for (const x of nums) {
        s = (((s + x) % k) + k) % k;
        ans += cnt[s] || 0;
        cnt[s] = (cnt[s] || 0) + 1;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
