---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Array
    - Hash Table
---

<!-- problem:start -->

# [982. Triples with Bitwise AND Equal To Zero](https://leetcode.com/problems/triples-with-bitwise-and-equal-to-zero)

[中文文档](/solution/0900-0999/0982.Triples%20with%20Bitwise%20AND%20Equal%20To%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên nums, trả về <em>số lượng <strong>bộ ba AND</strong></em>.</p>

<p><strong>Bộ ba AND</strong> là bộ ba chỉ số <code>(i, j, k)</code> thỏa mãn:</p>

<ul>
	<li><code>0 &lt;= i &lt; nums.length</code></li>
	<li><code>0 &lt;= j &lt; nums.length</code></li>
	<li><code>0 &lt;= k &lt; nums.length</code></li>
	<li><code>nums[i] &amp; nums[j] &amp; nums[k] == 0</code>, trong đó <code>&amp;</code> là toán tử AND theo bit.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,1,3]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong> Các bộ ba i, j, k sau đây thỏa mãn điều kiện:
(i=0, j=0, k=1) : 2 &amp; 2 &amp; 1
(i=0, j=1, k=0) : 2 &amp; 1 &amp; 2
(i=0, j=1, k=1) : 2 &amp; 1 &amp; 1
(i=0, j=1, k=2) : 2 &amp; 1 &amp; 3
(i=0, j=2, k=1) : 2 &amp; 3 &amp; 1
(i=1, j=0, k=0) : 1 &amp; 2 &amp; 2
(i=1, j=0, k=1) : 1 &amp; 2 &amp; 1
(i=1, j=0, k=2) : 1 &amp; 2 &amp; 3
(i=1, j=1, k=0) : 1 &amp; 1 &amp; 2
(i=1, j=2, k=0) : 1 &amp; 3 &amp; 2
(i=2, j=0, k=1) : 3 &amp; 2 &amp; 1
(i=2, j=1, k=0) : 3 &amp; 1 &amp; 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,0,0]
<strong>Đầu ra:</strong> 27
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>16</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Đếm các bộ ba thỏa $x\& y\& z=0$. Vì $n\le 1000$, ba vòng lặp lồng nhau sẽ khá tốn thời gian. Duyệt hai giá trị đầu và đếm mỗi kết quả $x\& y$, sau đó ghép mask đó với từng $z$; nếu AND bằng 0 thì cộng tần suất tương ứng. Miền giá trị nhỏ hơn $2^{16}$ nên bảng đếm vẫn đủ gọn.

<!-- thinking:end -->

Trước tiên, ta duyệt mọi cặp số $x$ và $y$, rồi dùng hash table hoặc mảng $cnt$ để đếm số lần xuất hiện của kết quả AND theo bit $x \& y$.

Tiếp theo, duyệt từng kết quả AND theo bit $xy$ và từng giá trị $z$. Nếu $xy \& z = 0$, cộng giá trị $cnt[xy]$ vào đáp án.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(n^2 + n \times M)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài mảng $nums$, còn $M$ là giá trị lớn nhất trong mảng $nums$. Trong bài này, $M \leq 2^{16}$. 

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countTriplets(self, nums: List[int]) -> int:
        cnt = Counter(x & y for x in nums for y in nums)
        return sum(v for xy, v in cnt.items() for z in nums if xy & z == 0)
```

#### Java

```java
class Solution {
    public int countTriplets(int[] nums) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int[] cnt = new int[mx + 1];
        for (int x : nums) {
            for (int y : nums) {
                cnt[x & y]++;
            }
        }
        int ans = 0;
        for (int xy = 0; xy <= mx; ++xy) {
            for (int z : nums) {
                if ((xy & z) == 0) {
                    ans += cnt[xy];
                }
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
    int countTriplets(vector<int>& nums) {
        int mx = ranges::max(nums);
        int cnt[mx + 1];
        memset(cnt, 0, sizeof cnt);
        for (int& x : nums) {
            for (int& y : nums) {
                cnt[x & y]++;
            }
        }
        int ans = 0;
        for (int xy = 0; xy <= mx; ++xy) {
            for (int& z : nums) {
                if ((xy & z) == 0) {
                    ans += cnt[xy];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func countTriplets(nums []int) (ans int) {
	mx := slices.Max(nums)
	cnt := make([]int, mx+1)
	for _, x := range nums {
		for _, y := range nums {
			cnt[x&y]++
		}
	}
	for xy := 0; xy <= mx; xy++ {
		for _, z := range nums {
			if xy&z == 0 {
				ans += cnt[xy]
			}
		}
	}
	return
}
```

#### TypeScript

```ts
function countTriplets(nums: number[]): number {
    const mx = Math.max(...nums);
    const cnt: number[] = Array(mx + 1).fill(0);
    for (const x of nums) {
        for (const y of nums) {
            cnt[x & y]++;
        }
    }
    let ans = 0;
    for (let xy = 0; xy <= mx; ++xy) {
        for (const z of nums) {
            if ((xy & z) === 0) {
                ans += cnt[xy];
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
