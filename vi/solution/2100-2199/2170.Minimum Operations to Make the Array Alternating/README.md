---
comments: true
difficulty: Medium
rating: 1662
source: Weekly Contest 280 Q2
tags:
    - Greedy
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [2170. Minimum Operations to Make the Array Alternating](https://leetcode.com/problems/minimum-operations-to-make-the-array-alternating)

[中文文档](/solution/2100-2199/2170.Minimum%20Operations%20to%20Make%20the%20Array%20Alternating/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <strong>0-indexed</strong> <code>nums</code> gồm <code>n</code> số nguyên dương.</p>

<p>Mảng <code>nums</code> được gọi là <strong>xen kẽ</strong> nếu:</p>

<ul>
	<li><code>nums[i - 2] == nums[i]</code>, với <code>2 &lt;= i &lt;= n - 1</code>.</li>
	<li><code>nums[i - 1] != nums[i]</code>, với <code>1 &lt;= i &lt;= n - 1</code>.</li>
</ul>

<p>Trong một <strong>thao tác</strong>, bạn có thể chọn một chỉ số <code>i</code> và <strong>đổi</strong> <code>nums[i]</code> thành <strong>bất kỳ</strong> số nguyên dương nào.</p>

<p>Trả về <em><strong>số thao tác ít nhất</strong> cần thực hiện để biến mảng thành mảng xen kẽ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,1,3,2,4,3]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong>
Một cách để biến mảng thành mảng xen kẽ là đổi nó thành [3,1,3,<u><strong>1</strong></u>,<u><strong>3</strong></u>,<u><strong>1</strong></u>].
Số thao tác cần thực hiện trong trường hợp này là 3.
Có thể chứng minh rằng không thể biến mảng thành mảng xen kẽ với ít hơn 3 thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,2,2,2]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Một cách để biến mảng thành mảng xen kẽ là đổi nó thành [1,2,<u><strong>1</strong></u>,2,<u><strong>1</strong></u>].
Số thao tác cần thực hiện trong trường hợp này là 2.
Lưu ý rằng không thể đổi mảng thành [<u><strong>2</strong></u>,2,2,2,2] vì khi đó nums[0] == nums[1], vi phạm điều kiện của mảng xen kẽ.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duy trì số lượng ở các chỉ số lẻ và chẵn

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng xen kẽ có cùng giá trị tại mọi chỉ số chẵn, cùng giá trị tại mọi chỉ số lẻ, và hai giá trị này khác nhau. Mỗi thao tác ghi đè một ô, nên số lần chỉnh sửa tối thiểu bằng số vị trí khác với các giá trị đích đã chọn. Ta cần chọn một giá trị xuất hiện nhiều ở chỉ số chẵn và một giá trị xuất hiện nhiều ở chỉ số lẻ sao cho chúng khác nhau.
>
> Đếm hai tần suất lớn nhất ở các chỉ số chẵn và lẻ. Nếu các giá trị xuất hiện nhiều nhất khác nhau, giữ lại cả hai; nếu trùng nhau, chọn phương án tốt hơn giữa “giá trị xuất hiện nhiều nhất ở chỉ số chẵn + giá trị đứng thứ hai ở chỉ số lẻ” và cặp ngược lại.
>
> $f$ tìm hai key đó; đáp án là $n$ trừ đi tổng các tần suất được giữ lại.

<!-- thinking:end -->

Theo mô tả đề bài, nếu một mảng $\textit{nums}$ là mảng xen kẽ, các phần tử ở chỉ số lẻ và chẵn phải khác nhau, đồng thời các phần tử ở chỉ số lẻ giống nhau và các phần tử ở chỉ số chẵn cũng giống nhau.

Để tối thiểu hóa số thao tác cần thực hiện để biến mảng $\textit{nums}$ thành mảng xen kẽ, ta có thể đếm số lần xuất hiện của các phần tử ở chỉ số lẻ và chẵn. Ta tìm hai phần tử xuất hiện nhiều nhất ở các chỉ số chẵn là $a_0$ và $a_2$, cùng số lần xuất hiện tương ứng là $a_1$ và $a_3$; tương tự, ta tìm hai phần tử xuất hiện nhiều nhất ở các chỉ số lẻ là $b_0$ và $b_2$, cùng số lần xuất hiện tương ứng là $b_1$ và $b_3$.

Nếu $a_0 \neq b_0$, ta có thể đổi tất cả phần tử ở chỉ số chẵn trong mảng $\textit{nums}$ thành $a_0$ và tất cả phần tử ở chỉ số lẻ thành $b_0$, khi đó số thao tác là $n - (a_1 + b_1)$; nếu $a_0 = b_0$, ta có thể đổi tất cả phần tử ở chỉ số chẵn trong mảng $\textit{nums}$ thành $a_0$ và tất cả phần tử ở chỉ số lẻ thành $b_2$, hoặc đổi tất cả phần tử ở chỉ số chẵn thành $a_2$ và tất cả phần tử ở chỉ số lẻ thành $b_0$, khi đó số thao tác là $n - \max(a_1 + b_3, a_3 + b_1)$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        def f(i: int) -> Tuple[int, int, int, int]:
            k1 = k2 = 0
            cnt = Counter(nums[i::2])
            for k, v in cnt.items():
                if cnt[k1] < v:
                    k2, k1 = k1, k
                elif cnt[k2] < v:
                    k2 = k
            return k1, cnt[k1], k2, cnt[k2]

        a, b = f(0), f(1)
        n = len(nums)
        if a[0] != b[0]:
            return n - (a[1] + b[1])
        return n - max(a[1] + b[3], a[3] + b[1])
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums) {
        int[] a = f(nums, 0);
        int[] b = f(nums, 1);
        int n = nums.length;
        if (a[0] != b[0]) {
            return n - (a[1] + b[1]);
        }
        return n - Math.max(a[1] + b[3], a[3] + b[1]);
    }

    private int[] f(int[] nums, int i) {
        int k1 = 0, k2 = 0;
        Map<Integer, Integer> cnt = new HashMap<>();
        for (; i < nums.length; i += 2) {
            cnt.merge(nums[i], 1, Integer::sum);
        }
        for (var e : cnt.entrySet()) {
            int k = e.getKey(), v = e.getValue();
            if (cnt.getOrDefault(k1, 0) < v) {
                k2 = k1;
                k1 = k;
            } else if (cnt.getOrDefault(k2, 0) < v) {
                k2 = k;
            }
        }
        return new int[] {k1, cnt.getOrDefault(k1, 0), k2, cnt.getOrDefault(k2, 0)};
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumOperations(vector<int>& nums) {
        auto f = [&](int i) -> vector<int> {
            int k1 = 0, k2 = 0;
            unordered_map<int, int> cnt;
            for (; i < nums.size(); i += 2) {
                cnt[nums[i]]++;
            }
            for (auto& [k, v] : cnt) {
                if (!k1 || cnt[k1] < v) {
                    k2 = k1;
                    k1 = k;
                } else if (!k2 || cnt[k2] < v) {
                    k2 = k;
                }
            }
            return {k1, !k1 ? 0 : cnt[k1], k2, !k2 ? 0 : cnt[k2]};
        };
        vector<int> a = f(0);
        vector<int> b = f(1);
        int n = nums.size();
        if (a[0] != b[0]) {
            return n - (a[1] + b[1]);
        }
        return n - max(a[1] + b[3], a[3] + b[1]);
    }
};
```

#### Go

```go
func minimumOperations(nums []int) int {
	f := func(i int) [4]int {
		cnt := make(map[int]int)
		for ; i < len(nums); i += 2 {
			cnt[nums[i]]++
		}

		k1, k2 := 0, 0
		for k, v := range cnt {
			if cnt[k1] < v {
				k2, k1 = k1, k
			} else if cnt[k2] < v {
				k2 = k
			}
		}
		return [4]int{k1, cnt[k1], k2, cnt[k2]}
	}

	a := f(0)
	b := f(1)
	n := len(nums)
	if a[0] != b[0] {
		return n - (a[1] + b[1])
	}
	return n - max(a[1]+b[3], a[3]+b[1])
}
```

#### TypeScript

```ts
function minimumOperations(nums: number[]): number {
    const f = (i: number): [number, number, number, number] => {
        const cnt: Map<number, number> = new Map();
        for (; i < nums.length; i += 2) {
            cnt.set(nums[i], (cnt.get(nums[i]) || 0) + 1);
        }

        let [k1, k2] = [0, 0];
        for (const [k, v] of cnt) {
            if ((cnt.get(k1) || 0) < v) {
                k2 = k1;
                k1 = k;
            } else if ((cnt.get(k2) || 0) < v) {
                k2 = k;
            }
        }
        return [k1, cnt.get(k1) || 0, k2, cnt.get(k2) || 0];
    };

    const a = f(0);
    const b = f(1);
    const n = nums.length;
    if (a[0] !== b[0]) {
        return n - (a[1] + b[1]);
    }
    return n - Math.max(a[1] + b[3], a[3] + b[1]);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
