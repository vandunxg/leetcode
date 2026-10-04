---
comments: true
difficulty: Medium
rating: 1645
source: Weekly Contest 398 Q3
tags:
    - Array
    - Hash Table
    - Math
    - Counting
---

<!-- problem:start -->

# [3153. Sum of Digit Differences of All Pairs](https://leetcode.com/problems/sum-of-digit-differences-of-all-pairs)

[中文文档](/solution/3100-3199/3153.Sum%20of%20Digit%20Differences%20of%20All%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>, trong đó tất cả các số nguyên đều có <strong>cùng</strong> số chữ số.</p>

<p><strong>Độ chênh lệch chữ số</strong> giữa hai số nguyên là <em>số lượng</em> chữ số khác nhau ở <strong>cùng</strong> một vị trí trong hai số.</p>

<p>Hãy trả về <strong>tổng</strong> <strong>độ chênh lệch chữ số</strong> giữa <strong>tất cả</strong> các cặp số nguyên trong <code>nums</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [13,23,12]</span></p>

<p><strong>Đầu ra:</strong> 4</p>

<p><strong>Giải thích:</strong><br />
Ta có:<br />
- Độ chênh lệch chữ số giữa <strong>1</strong>3 và <strong>2</strong>3 là 1.<br />
- Độ chênh lệch chữ số giữa 1<strong>3</strong> và 1<strong>2</strong> là 1.<br />
- Độ chênh lệch chữ số giữa <strong>23</strong> và <strong>12</strong> là 2.<br />
Vậy tổng độ chênh lệch chữ số giữa tất cả các cặp số nguyên là <code>1 + 1 + 2 = 4</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,10,10,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong><br />
Tất cả các số nguyên trong mảng đều giống nhau. Vì vậy, tổng độ chênh lệch chữ số giữa tất cả các cặp số nguyên sẽ bằng 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt; 10<sup>9</sup></code></li>
	<li>Tất cả các số nguyên trong <code>nums</code> có cùng số chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Tính tổng các vị trí khác nhau theo từng chữ số trên mọi cặp. So sánh từng cặp có độ phức tạp $O(n^2m)$, trong khi $n$ có thể bằng $10^5$.
>
> Các chữ số độc lập với nhau. Ở một vị trí, $v$ bản sao của một chữ số khác với $n-v$ số còn lại; chia cho hai để không đếm mỗi cặp hai lần.
>
> Tách chữ số cuối của mọi giá trị, đếm các chữ số $0..9$, rồi cộng $v(n-v)/2$. Chỉ có một vài vị trí chữ số.

<!-- thinking:end -->

Trước tiên, ta tìm số chữ số $m$ trong mảng. Sau đó, với mỗi chữ số, ta đếm số lần xuất hiện của mỗi chữ số ở vị trí đó trong mảng `nums`, ký hiệu là `cnt`. Do đó, tổng độ chênh lệch chữ số của tất cả các cặp số tại vị trí này là:

$$
\sum_{v \in \textit{cnt}} v \times (n - v)
$$

Trong đó $n$ là độ dài của mảng. Ta cộng độ chênh lệch chữ số của tất cả các vị trí rồi chia cho $2$ để nhận được đáp án.

Độ phức tạp thời gian là $O(n \times m)$, và độ phức tạp không gian là $O(C)$, trong đó $n$ và $m$ lần lượt là độ dài của mảng và số chữ số trong các số; $C$ là một hằng số, trong bài toán này $C = 10$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumDigitDifferences(self, nums: List[int]) -> int:
        n = len(nums)
        m = int(log10(nums[0])) + 1
        ans = 0
        for _ in range(m):
            cnt = Counter()
            for i, x in enumerate(nums):
                nums[i], y = divmod(x, 10)
                cnt[y] += 1
            ans += sum(v * (n - v) for v in cnt.values()) // 2
        return ans
```

#### Java

```java
class Solution {
    public long sumDigitDifferences(int[] nums) {
        int n = nums.length;
        int m = (int) Math.floor(Math.log10(nums[0])) + 1;
        int[] cnt = new int[10];
        long ans = 0;
        for (int k = 0; k < m; ++k) {
            Arrays.fill(cnt, 0);
            for (int i = 0; i < n; ++i) {
                ++cnt[nums[i] % 10];
                nums[i] /= 10;
            }
            for (int i = 0; i < 10; ++i) {
                ans += 1L * cnt[i] * (n - cnt[i]);
            }
        }
        return ans / 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sumDigitDifferences(vector<int>& nums) {
        int n = nums.size();
        int m = floor(log10(nums[0])) + 1;
        int cnt[10];
        long long ans = 0;
        for (int k = 0; k < m; ++k) {
            memset(cnt, 0, sizeof(cnt));
            for (int i = 0; i < n; ++i) {
                ++cnt[nums[i] % 10];
                nums[i] /= 10;
            }
            for (int i = 0; i < 10; ++i) {
                ans += 1LL * cnt[i] * (n - cnt[i]);
            }
        }
        return ans / 2;
    }
};
```

#### Go

```go
func sumDigitDifferences(nums []int) (ans int64) {
	n := len(nums)
	m := int(math.Floor(math.Log10(float64(nums[0])))) + 1
	for k := 0; k < m; k++ {
		cnt := [10]int{}
		for i, x := range nums {
			cnt[x%10]++
			nums[i] /= 10
		}
		for _, v := range cnt {
			ans += int64(v) * int64(n-v)
		}
	}
	ans /= 2
	return
}
```

#### TypeScript

```ts
function sumDigitDifferences(nums: number[]): number {
    const n = nums.length;
    const m = Math.floor(Math.log10(nums[0])) + 1;
    let ans: bigint = BigInt(0);
    for (let k = 0; k < m; ++k) {
        const cnt: number[] = Array(10).fill(0);
        for (let i = 0; i < n; ++i) {
            ++cnt[nums[i] % 10];
            nums[i] = Math.floor(nums[i] / 10);
        }
        for (let i = 0; i < 10; ++i) {
            ans += BigInt(cnt[i]) * BigInt(n - cnt[i]);
        }
    }
    ans /= BigInt(2);
    return Number(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
