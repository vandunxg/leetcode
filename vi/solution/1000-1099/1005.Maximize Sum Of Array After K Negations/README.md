---
comments: true
difficulty: Easy
rating: 1274
source: Weekly Contest 127 Q1
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [1005. Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations)

[中文文档](/solution/1000-1099/1005.Maximize%20Sum%20Of%20Array%20After%20K%20Negations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> và số nguyên <code>k</code>. Hãy thay đổi mảng theo cách sau:</p>

<ul>
	<li>Chọn chỉ số <code>i</code> và thay <code>nums[i]</code> bằng <code>-nums[i]</code>.</li>
</ul>

<p>Thực hiện thao tác này đúng <code>k</code> lần. Bạn có thể chọn cùng một chỉ số <code>i</code> nhiều lần.</p>

<p>Trả về <em>tổng lớn nhất có thể của mảng sau khi thay đổi theo cách này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,2,3], k = 1
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Chọn chỉ số 1, khi đó nums trở thành [4,-2,3].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,-1,0,2], k = 3
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Chọn các chỉ số (1, 2, 2), khi đó nums trở thành [3,1,0,2].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,-3,-1,5,-4], k = 2
<strong>Đầu ra:</strong> 13
<strong>Giải thích:</strong> Chọn các chỉ số (1, 4), khi đó nums trở thành [2,3,-1,5,4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Lật dấu giá trị nhỏ nhất hiện tại nhiều lần là cách tối ưu. $n$ và $k$ đều không quá $10^4$, nhưng tìm giá trị nhỏ nhất ở mỗi lượt cần thêm cấu trúc hỗ trợ. Các giá trị nằm trong $[-100,100]$, nên không cần sắp xếp mảng ban đầu.
>
> Để tối đa tổng, ta nên đổi dấu các số âm nhỏ nhất trước. Nếu còn số lần đổi dấu lẻ và không có số 0, cần đổi dấu số dương nhỏ nhất thêm một lần.
>
> Dùng frequency map để xử lý số lần đổi dấu từ $-100$ đến $-1$, có thể đổi dấu số dương nhỏ nhất nếu cần, rồi tính tổng giá trị nhân với tần suất.

<!-- thinking:end -->

Để tối đa tổng mảng, ta nên đổi các số âm nhỏ nhất thành số dương.

Vì các phần tử nằm trong $[-100, 100]$, ta dùng hash table $\textit{cnt}$ để đếm số lần xuất hiện của từng giá trị trong mảng $\textit{nums}$. Sau đó, duyệt $x$ từ $-100$ trở lên. Nếu $x$ có trong hash table, đặt $m = \min(\textit{cnt}[x], k)$ là số lần đổi dấu giá trị $x$. Ta giảm $\textit{cnt}[x]$ đi $m$, tăng $\textit{cnt}[-x]$ thêm $m$ và giảm $k$ đi $m$. Nếu $k$ bằng $0$, thao tác hoàn tất và ta thoát vòng lặp.

Nếu $k$ vẫn lẻ và $\textit{cnt}[0] = 0$, ta lấy số dương nhỏ nhất $x$ trong $\textit{cnt}$, giảm $\textit{cnt}[x]$ đi $1$ và tăng $\textit{cnt}[-x]$ thêm $1$.

Cuối cùng, duyệt hash table $\textit{cnt}$ và cộng các tích $x$ với $\textit{cnt}[x]$ để tính đáp án.

Độ phức tạp thời gian là $O(n + M)$ và độ phức tạp không gian là $O(M)$, trong đó $n$ là độ dài mảng $\textit{nums}$ còn $M$ là kích thước miền giá trị của $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestSumAfterKNegations(self, nums: List[int], k: int) -> int:
        cnt = Counter(nums)
        for x in range(-100, 0):
            if cnt[x]:
                m = min(cnt[x], k)
                cnt[x] -= m
                cnt[-x] += m
                k -= m
                if k == 0:
                    break
        if k & 1 and cnt[0] == 0:
            for x in range(1, 101):
                if cnt[x]:
                    cnt[x] -= 1
                    cnt[-x] += 1
                    break
        return sum(x * v for x, v in cnt.items())
```

#### Java

```java
class Solution {
    public int largestSumAfterKNegations(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        for (int x = -100; x < 0 && k > 0; ++x) {
            if (cnt.getOrDefault(x, 0) > 0) {
                int m = Math.min(cnt.get(x), k);
                cnt.merge(x, -m, Integer::sum);
                cnt.merge(-x, m, Integer::sum);
                k -= m;
            }
        }
        if ((k & 1) == 1 && cnt.getOrDefault(0, 0) == 0) {
            for (int x = 1; x <= 100; ++x) {
                if (cnt.getOrDefault(x, 0) > 0) {
                    cnt.merge(x, -1, Integer::sum);
                    cnt.merge(-x, 1, Integer::sum);
                    break;
                }
            }
        }
        int ans = 0;
        for (var e : cnt.entrySet()) {
            ans += e.getKey() * e.getValue();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        for (int& x : nums) {
            ++cnt[x];
        }
        for (int x = -100; x < 0 && k > 0; ++x) {
            if (cnt[x]) {
                int m = min(cnt[x], k);
                cnt[x] -= m;
                cnt[-x] += m;
                k -= m;
            }
        }
        if ((k & 1) && !cnt[0]) {
            for (int x = 1; x <= 100; ++x) {
                if (cnt[x]) {
                    --cnt[x];
                    ++cnt[-x];
                    break;
                }
            }
        }
        int ans = 0;
        for (auto& [x, v] : cnt) {
            ans += x * v;
        }
        return ans;
    }
};
```

#### Go

```go
func largestSumAfterKNegations(nums []int, k int) (ans int) {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	for x := -100; x < 0 && k > 0; x++ {
		if cnt[x] > 0 {
			m := min(k, cnt[x])
			cnt[x] -= m
			cnt[-x] += m
			k -= m
		}
	}
	if k&1 == 1 && cnt[0] == 0 {
		for x := 1; x <= 100; x++ {
			if cnt[x] > 0 {
				cnt[x]--
				cnt[-x]++
				break
			}
		}
	}
	for x, v := range cnt {
		ans += x * v
	}
	return
}
```

#### TypeScript

```ts
function largestSumAfterKNegations(nums: number[], k: number): number {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    for (let x = -100; x < 0 && k > 0; ++x) {
        if (cnt.get(x)! > 0) {
            const m = Math.min(cnt.get(x) || 0, k);
            cnt.set(x, (cnt.get(x) || 0) - m);
            cnt.set(-x, (cnt.get(-x) || 0) + m);
            k -= m;
        }
    }
    if ((k & 1) === 1 && (cnt.get(0) || 0) === 0) {
        for (let x = 1; x <= 100; ++x) {
            if (cnt.get(x)! > 0) {
                cnt.set(x, (cnt.get(x) || 0) - 1);
                cnt.set(-x, (cnt.get(-x) || 0) + 1);
                break;
            }
        }
    }
    return Array.from(cnt.entries()).reduce((acc, [k, v]) => acc + k * v, 0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
