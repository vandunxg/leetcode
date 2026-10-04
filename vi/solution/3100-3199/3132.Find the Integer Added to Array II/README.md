---
comments: true
difficulty: Medium
rating: 1620
source: Weekly Contest 395 Q2
tags:
    - Array
    - Two Pointers
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [3132. Find the Integer Added to Array II](https://leetcode.com/problems/find-the-integer-added-to-array-ii)

[中文文档](/solution/3100-3199/3132.Find%20the%20Integer%20Added%20to%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums1</code> và <code>nums2</code>.</p>

<p>Ta nói <code>nums2</code> có thể đạt được từ <code>nums1</code> với một số nguyên <code>x</code> nếu có thể xóa hai phần tử trong <code>nums1</code>, sau đó cộng <code>x</code> vào tất cả các phần tử còn lại của <code>nums1</code> (hoặc trừ đi trong trường hợp <code>x</code> âm), để mảng kết quả trở thành <code>nums2</code>. Hai mảng được xem là <strong>bằng nhau</strong> khi chúng chứa cùng các số nguyên với cùng số lần xuất hiện.</p>

<p>Trả về số nguyên <strong>nhỏ nhất</strong> <code>x</code> có thể khiến <code>nums2</code> đạt được từ <code>nums1</code>.</p>

<p>Đảm bảo rằng <code>nums2</code> có thể đạt được từ <code>nums1</code> với ít nhất một giá trị <code>x</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">nums1 = [4,20,16,12,8], nums2 = [14,18,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">-2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa các phần tử tại các chỉ số <code>[0,4]</code> và cộng -2, <code>nums1</code> trở thành <code>[18,14,10]</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">nums1 = [3,5,5,3], nums2 = [7,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io" style="
    font-family: Menlo,sans-serif;
    font-size: 0.85rem;
">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi xóa các phần tử tại các chỉ số <code>[0,3]</code> và cộng 2, <code>nums1</code> trở thành <code>[7,7]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums1.length &lt;= 200</code></li>
	<li><code>nums2.length == nums1.length - 2</code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= 1000</code></li>
	<li>
	<p>Đảm bảo rằng <code>nums2</code> có thể đạt được từ <code>nums1</code> với ít nhất một giá trị <code>x</code>.</p>
	</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Liệt kê + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Hai phần tử bị loại khỏi $nums1$, sau đó mọi giá trị còn lại được dịch chuyển một lượng $x$ để khớp với $nums2$. Thử mọi cặp phần tử bị xóa có độ phức tạp $O(n^2)$.
>
> Sau khi sắp xếp, $x$ phải bằng $nums2[0]$ trừ đi một trong ba giá trị đầu tiên của $nums1$, vì nhiều nhất chỉ có thể xóa hai phần tử đứng đầu.
>
> Với mỗi ứng viên, dùng hai con trỏ để đếm số phần tử không khớp; số phần tử này nhiều nhất là hai. Giá trị $x$ nhỏ nhất thỏa mãn là đáp án.

<!-- thinking:end -->

Trước tiên, ta sắp xếp hai mảng $nums1$ và $nums2$. Vì cần xóa hai phần tử khỏi $nums1$, ta chỉ cần xét ba phần tử đầu tiên của $nums1$, lần lượt ký hiệu là $a_1, a_2, a_3$. Ta có thể liệt kê phần tử đầu tiên $b_1$ của $nums2$, khi đó tính được $x = b_1 - a_i$, với $i \in \{1, 2, 3\}$. Sau đó, ta dùng phương pháp hai con trỏ để xác định liệu có tồn tại một số nguyên $x$ khiến $nums1$ và $nums2$ bằng nhau hay không, rồi chọn giá trị $x$ nhỏ nhất thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumAddedInteger(self, nums1: List[int], nums2: List[int]) -> int:
        def f(x: int) -> bool:
            i = j = cnt = 0
            while i < len(nums1) and j < len(nums2):
                if nums2[j] - nums1[i] != x:
                    cnt += 1
                else:
                    j += 1
                i += 1
            return cnt <= 2

        nums1.sort()
        nums2.sort()
        ans = inf
        for i in range(3):
            x = nums2[0] - nums1[i]
            if f(x):
                ans = min(ans, x)
        return ans
```

#### Java

```java
class Solution {
    public int minimumAddedInteger(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int ans = 1 << 30;
        for (int i = 0; i < 3; ++i) {
            int x = nums2[0] - nums1[i];
            if (f(nums1, nums2, x)) {
                ans = Math.min(ans, x);
            }
        }
        return ans;
    }

    private boolean f(int[] nums1, int[] nums2, int x) {
        int i = 0, j = 0, cnt = 0;
        while (i < nums1.length && j < nums2.length) {
            if (nums2[j] - nums1[i] != x) {
                ++cnt;
            } else {
                ++j;
            }
            ++i;
        }
        return cnt <= 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumAddedInteger(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        int ans = 1 << 30;
        auto f = [&](int x) {
            int i = 0, j = 0, cnt = 0;
            while (i < nums1.size() && j < nums2.size()) {
                if (nums2[j] - nums1[i] != x) {
                    ++cnt;
                } else {
                    ++j;
                }
                ++i;
            }
            return cnt <= 2;
        };
        for (int i = 0; i < 3; ++i) {
            int x = nums2[0] - nums1[i];
            if (f(x)) {
                ans = min(ans, x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumAddedInteger(nums1 []int, nums2 []int) int {
	sort.Ints(nums1)
	sort.Ints(nums2)
	ans := 1 << 30
	f := func(x int) bool {
		i, j, cnt := 0, 0, 0
		for i < len(nums1) && j < len(nums2) {
			if nums2[j]-nums1[i] != x {
				cnt++
			} else {
				j++
			}
			i++
		}
		return cnt <= 2
	}
	for _, a := range nums1[:3] {
		x := nums2[0] - a
		if f(x) {
			ans = min(ans, x)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function minimumAddedInteger(nums1: number[], nums2: number[]): number {
    nums1.sort((a, b) => a - b);
    nums2.sort((a, b) => a - b);
    const f = (x: number): boolean => {
        let [i, j, cnt] = [0, 0, 0];
        while (i < nums1.length && j < nums2.length) {
            if (nums2[j] - nums1[i] !== x) {
                ++cnt;
            } else {
                ++j;
            }
            ++i;
        }
        return cnt <= 2;
    };
    let ans = Infinity;
    for (let i = 0; i < 3; ++i) {
        const x = nums2[0] - nums1[i];
        if (f(x)) {
            ans = Math.min(ans, x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
