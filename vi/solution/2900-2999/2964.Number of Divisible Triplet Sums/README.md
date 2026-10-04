---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2964. Number of Divisible Triplet Sums 🔒](https://leetcode.com/problems/number-of-divisible-triplet-sums)

[中文文档](/solution/2900-2999/2964.Number%20of%20Divisible%20Triplet%20Sums/README.md)

## Mô tả

<!-- description:start -->

Cho một mảng số nguyên <strong>đánh chỉ số từ 0</strong> <code>nums</code> và một số nguyên <code>d</code>, hãy trả về <em>số bộ ba</em> <code>(i, j, k)</code> <em>sao cho</em> <code>i &lt; j &lt; k</code> <em>và</em> <code>(nums[i] + nums[j] + nums[k]) % d == 0</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,4,7,8], d = 5
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Các bộ ba chia hết cho 5 là: (0, 1, 2), (0, 2, 4), (1, 2, 4).
Có thể chứng minh rằng không còn bộ ba nào khác chia hết cho 5. Vì vậy, đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3], d = 3
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Mọi bộ ba được chọn ở đây đều có tổng bằng 9, chia hết cho 3. Vì vậy, đáp án là tổng số bộ ba có thể chọn, bằng 4.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,3,3,3], d = 6
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Mọi bộ ba được chọn ở đây đều có tổng bằng 9, không chia hết cho 6. Vì vậy, đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= d &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba $i<j<k$ có tổng chia hết cho $d$. Vòng lặp ba chiều quá chậm khi $n$ ở mức trung bình. Duyệt hai chỉ số cuối và tra cứu trong bảng băm phần dư $nums[i] \bmod d$ đã gặp.
>
> Thêm vào đáp án trước khi chèn $nums[j] \bmod d$, để duy trì điều kiện $i<j<k$.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $cnt$ để ghi lại số lần xuất hiện của $nums[i] \bmod d$, sau đó duyệt $j$ và $k$, tính giá trị của $nums[i] \bmod d$ để phương trình $(nums[i] + nums[j] + nums[k]) \bmod d = 0$ đúng, đó là $(d - (nums[j] + nums[k]) \bmod d) \bmod d$, rồi cộng số lần xuất hiện của giá trị này vào đáp án. Sau đó, ta tăng số lần xuất hiện của $nums[j] \bmod d$ lên một. Tiếp tục duyệt $j$ và $k$ cho đến khi $j$ chạm cuối mảng.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divisibleTripletCount(self, nums: List[int], d: int) -> int:
        cnt = defaultdict(int)
        ans, n = 0, len(nums)
        for j in range(n):
            for k in range(j + 1, n):
                x = (d - (nums[j] + nums[k]) % d) % d
                ans += cnt[x]
            cnt[nums[j] % d] += 1
        return ans
```

#### Java

```java
class Solution {
    public int divisibleTripletCount(int[] nums, int d) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0, n = nums.length;
        for (int j = 0; j < n; ++j) {
            for (int k = j + 1; k < n; ++k) {
                int x = (d - (nums[j] + nums[k]) % d) % d;
                ans += cnt.getOrDefault(x, 0);
            }
            cnt.merge(nums[j] % d, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int divisibleTripletCount(vector<int>& nums, int d) {
        unordered_map<int, int> cnt;
        int ans = 0, n = nums.size();
        for (int j = 0; j < n; ++j) {
            for (int k = j + 1; k < n; ++k) {
                int x = (d - (nums[j] + nums[k]) % d) % d;
                ans += cnt[x];
            }
            cnt[nums[j] % d]++;
        }
        return ans;
    }
};
```

#### Go

```go
func divisibleTripletCount(nums []int, d int) (ans int) {
	n := len(nums)
	cnt := map[int]int{}
	for j := 0; j < n; j++ {
		for k := j + 1; k < n; k++ {
			x := (d - (nums[j]+nums[k])%d) % d
			ans += cnt[x]
		}
		cnt[nums[j]%d]++
	}
	return
}
```

#### TypeScript

```ts
function divisibleTripletCount(nums: number[], d: number): number {
    const n = nums.length;
    const cnt: Map<number, number> = new Map();
    let ans = 0;
    for (let j = 0; j < n; ++j) {
        for (let k = j + 1; k < n; ++k) {
            const x = (d - ((nums[j] + nums[k]) % d)) % d;
            ans += cnt.get(x) || 0;
        }
        cnt.set(nums[j] % d, (cnt.get(nums[j] % d) || 0) + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
