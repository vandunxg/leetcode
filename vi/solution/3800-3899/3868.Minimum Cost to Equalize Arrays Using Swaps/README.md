---
comments: true
difficulty: Medium
rating: 1578
source: Biweekly Contest 178 Q3
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3868. Minimum Cost to Equalize Arrays Using Swaps](https://leetcode.com/problems/minimum-cost-to-equalize-arrays-using-swaps)

[中文文档](/solution/3800-3899/3868.Minimum%20Cost%20to%20Equalize%20Arrays%20Using%20Swaps/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>, cả hai đều có kích thước <code>n</code>.</p>

<p>Bạn có thể thực hiện hai thao tác sau trên hai mảng này nhiều lần tùy ý:</p>

<ul>
	<li><strong>Đổi chỗ trong cùng một mảng</strong>: Chọn hai chỉ số <code>i</code> và <code>j</code>. Sau đó, chọn đổi chỗ <code>nums1[i]</code> với <code>nums1[j]</code>, hoặc đổi chỗ <code>nums2[i]</code> với <code>nums2[j]</code>. Thao tác này <strong>không mất phí</strong>.</li>
	<li><strong>Đổi chỗ giữa hai mảng</strong>: Chọn một chỉ số <code>i</code>. Sau đó, đổi chỗ <code>nums1[i]</code> với <code>nums2[i]</code>. Thao tác này <strong>tốn chi phí 1</strong>.</li>
</ul>

<p>Hãy trả về một số nguyên biểu thị <strong>chi phí nhỏ nhất</strong> để biến <code>nums1</code> và <code>nums2</code> thành <strong>giống hệt nhau</strong>. Nếu không thể thực hiện, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [10,20], nums2 = [20,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi chỗ <code>nums2[0] = 20</code> với <code>nums2[1] = 10</code>.

    <ul>
    	<li><code>nums2</code> trở thành <code>[10, 20]</code>.</li>
    	<li>Thao tác này không mất phí.</li>
    </ul>
    </li>
    <li><code>nums1</code> và <code>nums2</code> lúc này đã giống hệt nhau. Chi phí là 0.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [10,10], nums2 = [20,20]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Đổi chỗ <code>nums1[0] = 10</code> với <code>nums2[0] = 20</code>.

    <ul>
    	<li><code>nums1</code> trở thành <code>[20, 10]</code>.</li>
    	<li><code>nums2</code> trở thành <code>[10, 20]</code>.</li>
    	<li>Thao tác này tốn chi phí 1.</li>
    </ul>
    </li>
    <li>Đổi chỗ <code>nums2[0] = 10</code> với <code>nums2[1] = 20</code>.
    <ul>
    	<li><code>nums2</code> trở thành <code>[20, 10]</code>.</li>
    	<li>Thao tác này không mất phí.</li>
    </ul>
    </li>
    <li><code>nums1</code> và <code>nums2</code> lúc này đã giống hệt nhau. Chi phí là 1.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [10,20], nums2 = [30,40]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể biến hai mảng thành giống hệt nhau. Vì vậy, đáp án là -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n == nums1.length == nums2.length &lt;= 8 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= nums1[i], nums2[i] &lt;= 8 * 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Đổi chỗ trong cùng một mảng là miễn phí, còn đổi chỗ giữa hai mảng tại cùng một chỉ số tốn $1$. Ta cần biến hai mảng thành cùng một dãy sau các thao tác đó. $n \le 8 \times 10^4$.
>
> Các phép đổi chỗ miễn phí có thể tùy ý hoán vị từng mảng, nên hai mảng chỉ cần có cùng multiset. Mỗi phép đổi chỗ giữa hai mảng sẽ đổi một giá trị còn thừa ở mỗi bên.
>
> Trước tiên, loại bỏ các giá trị giống nhau. Mọi số lượng còn lại phải là số chẵn (cần hai phép đổi chỗ giữa hai mảng để ghép một giá trị với chính nó); chi phí là một nửa tổng số phần tử còn lại.
>
> Nếu số lượng còn lại của bất kỳ giá trị nào là lẻ thì không thể thực hiện.

<!-- thinking:end -->

Ta có thể dùng hai hash table $\textit{cnt1}$ và $\textit{cnt2}$ để đếm số lần xuất hiện của mỗi số nguyên trong hai mảng. Trong quá trình đếm, ta có thể trực tiếp loại bỏ các lần xuất hiện của những số nguyên xuất hiện trong cả hai mảng. Cuối cùng, ta kiểm tra xem số lần xuất hiện của mọi số nguyên trong cả hai hash table có phải là số chẵn hay không. Nếu có số nguyên nào có số lần xuất hiện lẻ, ta trả về -1. Nếu không, ta tính tổng một nửa số lần xuất hiện của mọi số nguyên trong $\textit{cnt1}$, chính là chi phí nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của các mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, nums1: list[int], nums2: list[int]) -> int:
        cnt2 = Counter(nums2)
        cnt1 = Counter()
        for x in nums1:
            if cnt2[x]:
                cnt2[x] -= 1
            else:
                cnt1[x] += 1
        ans = 0
        for v in cnt1.values():
            if v % 2 == 1:
                return -1
            ans += v // 2
        for v in cnt2.values():
            if v % 2 == 1:
                return -1
        return ans
```

#### Java

```java
class Solution {
    public int minCost(int[] nums1, int[] nums2) {
        Map<Integer, Integer> cnt2 = new HashMap<>();
        for (int x : nums2) {
            cnt2.merge(x, 1, Integer::sum);
        }

        Map<Integer, Integer> cnt1 = new HashMap<>();
        for (int x : nums1) {
            int c = cnt2.getOrDefault(x, 0);
            if (c > 0) {
                cnt2.put(x, c - 1);
            } else {
                cnt1.merge(x, 1, Integer::sum);
            }
        }

        int ans = 0;

        for (int v : cnt1.values()) {
            if ((v & 1) == 1) {
                return -1;
            }
            ans += v / 2;
        }

        for (int v : cnt2.values()) {
            if ((v & 1) == 1) {
                return -1;
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
    int minCost(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> cnt2;
        for (int x : nums2) {
            ++cnt2[x];
        }

        unordered_map<int, int> cnt1;
        for (int x : nums1) {
            if (cnt2[x] > 0) {
                --cnt2[x];
            } else {
                ++cnt1[x];
            }
        }

        int ans = 0;

        for (auto& [_, v] : cnt1) {
            if (v & 1) {
                return -1;
            }
            ans += v / 2;
        }

        for (auto& [_, v] : cnt2) {
            if (v & 1) {
                return -1;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minCost(nums1 []int, nums2 []int) int {
	cnt2 := map[int]int{}
	for _, x := range nums2 {
		cnt2[x]++
	}

	cnt1 := map[int]int{}
	for _, x := range nums1 {
		if cnt2[x] > 0 {
			cnt2[x]--
		} else {
			cnt1[x]++
		}
	}

	ans := 0

	for _, v := range cnt1 {
		if v%2 == 1 {
			return -1
		}
		ans += v / 2
	}

	for _, v := range cnt2 {
		if v%2 == 1 {
			return -1
		}
	}

	return ans
}
```

#### TypeScript

```ts
function minCost(nums1: number[], nums2: number[]): number {
    const cnt2 = new Map<number, number>();

    for (const x of nums2) {
        cnt2.set(x, (cnt2.get(x) ?? 0) + 1);
    }

    const cnt1 = new Map<number, number>();

    for (const x of nums1) {
        const c = cnt2.get(x) ?? 0;
        if (c > 0) {
            cnt2.set(x, c - 1);
        } else {
            cnt1.set(x, (cnt1.get(x) ?? 0) + 1);
        }
    }

    let ans = 0;

    for (const v of cnt1.values()) {
        if (v % 2 === 1) {
            return -1;
        }
        ans += Math.floor(v / 2);
    }

    for (const v of cnt2.values()) {
        if (v % 2 === 1) {
            return -1;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
