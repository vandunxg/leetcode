---
comments: true
difficulty: Hard
tags:
    - Stack
    - Array
    - Hash Table
    - Math
    - Sliding Window
---

<!-- problem:start -->

# [2524. Maximum Frequency Score of a Subarray 🔒](https://leetcode.com/problems/maximum-frequency-score-of-a-subarray)

[中文文档](/solution/2500-2599/2524.Maximum%20Frequency%20Score%20of%20a%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>.</p>

<p><strong>Điểm tần suất</strong> của một mảng là tổng các giá trị <strong>phân biệt</strong> trong mảng, mỗi giá trị được nâng lên lũy thừa bằng <strong>số lần xuất hiện</strong> của nó, sau đó lấy tổng <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<ul>
	<li>Ví dụ, điểm tần suất của mảng <code>[5,4,5,7,4,4]</code> là <code>(4<sup>3</sup> + 5<sup>2</sup> + 7<sup>1</sup>) modulo (10<sup>9</sup> + 7) = 96</code>.</li>
</ul>

<p>Trả về <em>điểm tần suất <strong>lớn nhất</strong> của một <strong>mảng con</strong> có kích thước </em><code>k</code><em> trong </em><code>nums</code>. Cần tối đa hóa giá trị sau khi lấy modulo, không phải giá trị thực.</p>

<p><strong>Mảng con</strong> là một phần liên tiếp của một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,2,1,2], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Mảng con [2,1,2] có điểm tần suất bằng 5. Có thể chứng minh rằng đây là điểm tần suất lớn nhất có thể đạt được.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1,1,1,1], k = 4
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Mọi mảng con có độ dài 4 đều có điểm tần suất bằng 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sliding Window + Fast Power

<!-- thinking:start -->

> **Tư duy**
>
> Một cửa sổ có độ dài $k$ có điểm số là $\sum x^{\textit{freq}(x)}\bmod (10^9+7)$; ta cần tìm giá trị lớn nhất trên tất cả các cửa sổ như vậy. Việc tính lại điểm số cho từng cửa sổ bằng lũy thừa modulo sẽ tốn nhiều thời gian khi có $n-k+1$ cửa sổ.
>
> Hai cửa sổ liên tiếp chỉ khác nhau ở một phần tử được thêm vào và một phần tử bị xóa đi. Ta dùng một hash map để lưu tần suất, rồi cập nhật điểm số theo công thức $x^{c+1}-x^c=(x-1)x^c$ (cộng $x$ khi một giá trị xuất hiện, trừ $x$ khi số lần xuất hiện của một giá trị giảm về không). Nếu phần tử thêm vào và phần tử rời đi giống nhau, điểm số không thay đổi.

<!-- thinking:end -->

Ta dùng một hash table $\textit{cnt}$ để duy trì các phần tử trong cửa sổ có kích thước $k$ và tần suất của chúng.

Trước tiên, tính điểm số của tất cả phần tử trong cửa sổ ban đầu có kích thước $k$. Sau đó, dùng cửa sổ trượt để lần lượt thêm một phần tử và xóa phần tử ngoài cùng bên trái, đồng thời cập nhật điểm số bằng lũy thừa nhanh.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxFrequencyScore(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        cnt = Counter(nums[:k])
        ans = cur = sum(pow(k, v, mod) for k, v in cnt.items()) % mod
        i = k
        while i < len(nums):
            a, b = nums[i - k], nums[i]
            if a != b:
                cur += (b - 1) * pow(b, cnt[b], mod) if cnt[b] else b
                cur -= (a - 1) * pow(a, cnt[a] - 1, mod) if cnt[a] > 1 else a
                cur %= mod
                cnt[b] += 1
                cnt[a] -= 1
                ans = max(ans, cur)
            i += 1
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int maxFrequencyScore(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int i = 0; i < k; ++i) {
            cnt.merge(nums[i], 1, Integer::sum);
        }
        long cur = 0;
        for (var e : cnt.entrySet()) {
            cur = (cur + qpow(e.getKey(), e.getValue())) % mod;
        }
        long ans = cur;
        for (int i = k; i < nums.length; ++i) {
            int a = nums[i - k];
            int b = nums[i];
            if (a != b) {
                if (cnt.getOrDefault(b, 0) > 0) {
                    cur += (b - 1) * qpow(b, cnt.get(b)) % mod;
                } else {
                    cur += b;
                }
                if (cnt.getOrDefault(a, 0) > 1) {
                    cur -= (a - 1) * qpow(a, cnt.get(a) - 1) % mod;
                } else {
                    cur -= a;
                }
                cur = (cur + mod) % mod;
                cnt.put(b, cnt.getOrDefault(b, 0) + 1);
                cnt.put(a, cnt.getOrDefault(a, 0) - 1);
                ans = Math.max(ans, cur);
            }
        }
        return (int) ans;
    }

    private long qpow(long a, long n) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxFrequencyScore(vector<int>& nums, int k) {
        using ll = long long;
        const int mod = 1e9 + 7;
        auto qpow = [&](ll a, ll n) {
            ll ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        unordered_map<int, int> cnt;
        for (int i = 0; i < k; ++i) {
            cnt[nums[i]]++;
        }
        ll cur = 0;
        for (auto& [k, v] : cnt) {
            cur = (cur + qpow(k, v)) % mod;
        }
        ll ans = cur;
        for (int i = k; i < nums.size(); ++i) {
            int a = nums[i - k], b = nums[i];
            if (a != b) {
                cur += cnt[b] ? (b - 1) * qpow(b, cnt[b]) % mod : b;
                cur -= cnt[a] > 1 ? (a - 1) * qpow(a, cnt[a] - 1) % mod : a;
                cur = (cur + mod) % mod;
                ans = max(ans, cur);
                cnt[b]++;
                cnt[a]--;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxFrequencyScore(nums []int, k int) int {
	cnt := map[int]int{}
	for _, v := range nums[:k] {
		cnt[v]++
	}
	cur := 0
	const mod int = 1e9 + 7
	qpow := func(a, n int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	for k, v := range cnt {
		cur = (cur + qpow(k, v)) % mod
	}
	ans := cur
	for i := k; i < len(nums); i++ {
		a, b := nums[i-k], nums[i]
		if a != b {
			if cnt[b] > 0 {
				cur += (b - 1) * qpow(b, cnt[b]) % mod
			} else {
				cur += b
			}
			if cnt[a] > 1 {
				cur -= (a - 1) * qpow(a, cnt[a]-1) % mod
			} else {
				cur -= a
			}
			cur = (cur + mod) % mod
			ans = max(ans, cur)
			cnt[b]++
			cnt[a]--
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
