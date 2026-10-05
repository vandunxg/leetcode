---
comments: true
difficulty: Hard
rating: 2302
source: Weekly Contest 491 Q4
tags:
    - Array
    - Hash Table
    - Counting
    - Sliding Window
---

<!-- problem:start -->

# [3859. Count Subarrays With K Distinct Integers](https://leetcode.com/problems/count-subarrays-with-k-distinct-integers)

[中文文档](/solution/3800-3899/3859.Count%20Subarrays%20With%20K%20Distinct%20Integers/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và hai số nguyên <code>k</code> và <code>m</code>.</p>

<p>Hãy trả về một số nguyên biểu thị số lượng <strong><span data-keyword="subarray-nonempty">mảng con</span></strong> của <code>nums</code> thỏa mãn:</p>

<ul>
	<li>Mảng con chứa <strong>đúng</strong> <code>k</code> số nguyên <strong>phân biệt</strong>.</li>
	<li>Trong mảng con, mỗi số nguyên <strong>phân biệt</strong> xuất hiện <strong>ít nhất</strong> <code>m</code> lần.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,2], k = 2, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có <code>k = 2</code> số nguyên phân biệt, mỗi số xuất hiện ít nhất <code>m = 2</code> lần là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Các số<br />
			phân biệt</th>
			<th style="border: 1px solid black;">Tần suất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[1, 2, 1, 2]</td>
			<td style="border: 1px solid black;">{1, 2} &rarr; 2</td>
			<td style="border: 1px solid black;">{1: 2, 2: 2}</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1, 2, 1, 2, 2]</td>
			<td style="border: 1px solid black;">{1, 2} &rarr; 2</td>
			<td style="border: 1px solid black;">{1: 2, 2: 3}</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,1,2,4], k = 2, m = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các mảng con có <code>k = 2</code> số nguyên phân biệt, mỗi số xuất hiện ít nhất <code>m = 1</code> lần là:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Mảng con</th>
			<th style="border: 1px solid black;">Các số<br />
			phân biệt</th>
			<th style="border: 1px solid black;">Tần suất</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">[3, 1]</td>
			<td style="border: 1px solid black;">{3, 1} &rarr; 2</td>
			<td style="border: 1px solid black;">{3: 1, 1: 1}</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[1, 2]</td>
			<td style="border: 1px solid black;">{1, 2} &rarr; 2</td>
			<td style="border: 1px solid black;">{1: 1, 2: 1}</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">[2, 4]</td>
			<td style="border: 1px solid black;">{2, 4} &rarr; 2</td>
			<td style="border: 1px solid black;">{2: 1, 4: 1}</td>
		</tr>
	</tbody>
</table>

<p>Vì vậy, đáp án là 3.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k, m &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng con phải chứa đúng $k$ giá trị phân biệt, mỗi giá trị xuất hiện ít nhất $m$ lần. Vì $n \le 10^5$, ta không thể liệt kê tất cả các đoạn.
>
> Điều kiện có đúng $k$ loại tương đương với có ít nhất $k$ loại trừ đi có ít nhất $k+1$ loại, kết hợp với ràng buộc cửa sổ rằng có ít nhất $k$ giá trị có tần suất đạt $m$.
>
> Dùng two pointers để theo dõi số lượng phần tử phân biệt và số giá trị đã đạt $m$. Khi cả số loại đạt giới hạn $\textit{lim}$ và $t \ge k$ đều thỏa mãn, ta dịch đầu trái. Mọi vị trí bắt đầu trước đầu trái đó đều hợp lệ.
>
> $f(k)-f(k+1)$ là số lượng mảng con có đúng $k$ loại.

<!-- thinking:end -->
<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSubarrays(self, nums: list[int], k: int, m: int) -> int:
        def f(lim: int) -> int:
            cnt = Counter()
            t = 0
            ans = l = 0
            for x in nums:
                cnt[x] += 1
                if cnt[x] == m:
                    t += 1
                while len(cnt) >= lim and t >= k:
                    y = nums[l]
                    cnt[y] -= 1
                    if cnt[y] == m - 1:
                        t -= 1
                    if cnt[y] == 0:
                        cnt.pop(y)
                    l += 1
                ans += l
            return ans

        return f(k) - f(k + 1)
```

#### Java

```java
class Solution {
    private int[] nums;
    private int k;
    private int m;

    public long countSubarrays(int[] nums, int k, int m) {
        this.nums = nums;
        this.k = k;
        this.m = m;
        return f(k) - f(k + 1);
    }

    private long f(int lim) {
        Map<Integer, Integer> cnt = new HashMap<>();
        long ans = 0;
        int l = 0;
        int t = 0;

        for (int x : nums) {
            if (cnt.merge(x, 1, Integer::sum) == m) {
                t++;
            }

            while (cnt.size() >= lim && t >= k) {
                int y = nums[l++];
                int cur = cnt.merge(y, -1, Integer::sum);
                if (cur == m - 1) {
                    --t;
                }
                if (cur == 0) {
                    cnt.remove(y);
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
    long long countSubarrays(vector<int>& nums, int k, int m) {
        auto f = [&](int lim) -> long long {
            unordered_map<int, int> cnt;
            long long ans = 0;
            int l = 0;
            int t = 0;

            for (int x : nums) {
                if (++cnt[x] == m) {
                    ++t;
                }

                while (cnt.size() >= lim && t >= k) {
                    int y = nums[l++];
                    if (--cnt[y] == m - 1) {
                        --t;
                    }
                    if (cnt[y] == 0) {
                        cnt.erase(y);
                    }
                }

                ans += l;
            }

            return ans;
        };

        return f(k) - f(k + 1);
    }
};
```

#### Go

```go
func countSubarrays(nums []int, k int, m int) int64 {
	f := func(lim int) int64 {
		cnt := make(map[int]int)
		var ans int64
		l := 0
		t := 0

		for _, x := range nums {
			cnt[x]++
			if cnt[x] == m {
				t++
			}

			for len(cnt) >= lim && t >= k {
				y := nums[l]
				l++
				cnt[y]--
				if cnt[y] == m-1 {
					t--
				}
				if cnt[y] == 0 {
					delete(cnt, y)
				}
			}

			ans += int64(l)
		}

		return ans
	}

	return f(k) - f(k+1)
}
```

#### TypeScript

```ts
function countSubarrays(nums: number[], k: number, m: number): number {
    const f = (lim: number): number => {
        const cnt = new Map<number, number>();
        let ans = 0;
        let l = 0;
        let t = 0;

        for (const x of nums) {
            cnt.set(x, (cnt.get(x) ?? 0) + 1);
            if (cnt.get(x) === m) {
                t++;
            }

            while (cnt.size >= lim && t >= k) {
                const y = nums[l++];
                cnt.set(y, cnt.get(y)! - 1);

                if (cnt.get(y) === m - 1) {
                    t--;
                }

                if (cnt.get(y) === 0) {
                    cnt.delete(y);
                }
            }

            ans += l;
        }

        return ans;
    };

    return f(k) - f(k + 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
