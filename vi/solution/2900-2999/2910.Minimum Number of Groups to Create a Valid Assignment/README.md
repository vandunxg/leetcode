---
comments: true
difficulty: Medium
rating: 2132
source: Weekly Contest 368 Q3
tags:
    - Greedy
    - Array
    - Hash Table
---

<!-- problem:start -->

# [2910. Minimum Number of Groups to Create a Valid Assignment](https://leetcode.com/problems/minimum-number-of-groups-to-create-a-valid-assignment)

[中文文档](/solution/2900-2999/2910.Minimum%20Number%20of%20Groups%20to%20Create%20a%20Valid%20Assignment/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một tập hợp các <code>balls</code> được đánh số và cần sắp xếp chúng vào các hộp sao cho phân bố gần cân bằng. Có hai quy tắc cần tuân theo:</p>

<ul>
	<li>Các quả bóng trong cùng một hộp phải có cùng giá trị. Tuy nhiên, nếu có nhiều hơn một quả bóng mang cùng một số, bạn có thể đặt chúng vào các hộp khác nhau.</li>
	<li>Hộp lớn nhất chỉ được có nhiều hơn hộp nhỏ nhất một quả bóng.</li>
</ul>

<p>​Hãy trả về <em>số hộp ít nhất</em> cần dùng để sắp xếp các quả bóng theo các quy tắc trên.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> balls = [3,2,3,2,3] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 2 </span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể sắp xếp <code>balls</code> vào các hộp như sau:</p>

<ul>
	<li><code>[3,3,3]</code></li>
	<li><code>[2,2]</code></li>
</ul>

<p>Chênh lệch kích thước giữa hai hộp không vượt quá một.</p>
</div>

<p><strong class="example">Ví dụ 2: </strong></p>

<div class="example-block" style="border-color: var(--border-tertiary); border-left-width: 2px; color: var(--text-secondary); font-size: .875rem; margin-bottom: 1rem; margin-top: 1rem; overflow: visible; padding-left: 1rem;">
<p><strong>Đầu vào: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> balls = [10,10,10,3,1,1] </span></p>

<p><strong>Đầu ra: </strong> <span class="example-io" style="font-family: Menlo,sans-serif; font-size: 0.85rem;"> 4 </span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể sắp xếp <code>balls</code> vào các hộp như sau:</p>

<ul>
</ul>

<ul>
	<li><code>[10]</code></li>
	<li><code>[10,10]</code></li>
	<li><code>[3]</code></li>
	<li><code>[1,1]</code></li>
</ul>

<p>Không thể dùng ít hơn bốn hộp mà vẫn tuân theo các quy tắc. Chẳng hạn, đặt cả ba quả bóng mang số 10 vào một hộp sẽ vi phạm quy tắc về chênh lệch kích thước tối đa giữa các hộp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Các giá trị bằng nhau phải được xếp vào các nhóm có kích thước chênh lệch không quá một, nên mỗi nhóm có kích thước $k$ hoặc $k+1$. Muốn số nhóm nhỏ nhất thì ta muốn $k$ lớn nhất, và $k$ không thể lớn hơn tần suất nhỏ nhất.
>
> Ta liệt kê $k$ giảm dần từ $min(cnt.values())$. Một tần suất $v$ là không thể chia được nếu $\lfloor v/k \rfloor < v \bmod k$; nếu không, nó cần $\lceil v/(k+1) \rceil$ nhóm. Giá trị $k$ đầu tiên hoàn toàn khả thi là đáp án tối ưu.

<!-- thinking:end -->

Ta dùng một hash table $cnt$ để đếm số lần xuất hiện của mỗi số trong mảng $nums$. Gọi $k$ là số lần xuất hiện nhỏ nhất, sau đó ta có thể liệt kê kích thước nhóm trong khoảng $[k,..1]$. Vì chênh lệch kích thước giữa các nhóm không quá $1$, kích thước nhóm có thể là $k$ hoặc $k+1$.

Với kích thước nhóm $k$ đang được xét, ta duyệt qua mỗi tần suất $v$ trong hash table. Nếu $\lfloor \frac{v}{k} \rfloor < v \bmod k$, điều đó có nghĩa là ta không thể chia $v$ lần xuất hiện của cùng một giá trị thành các nhóm có kích thước $k$ hoặc $k+1$, nên có thể bỏ qua trực tiếp kích thước nhóm $k$ này. Ngược lại, ta có thể tạo các nhóm, và chỉ cần tạo nhiều nhóm kích thước $k+1$ nhất có thể để bảo đảm số nhóm nhỏ nhất. Vì vậy, ta có thể chia $v$ phần tử thành $\lceil \frac{v}{k+1} \rceil$ nhóm và cộng số nhóm đó vào đáp án đang xét. Do liệt kê $k$ từ lớn đến nhỏ, ngay khi tìm được một cách chia nhóm hợp lệ thì đó chắc chắn là tối ưu.

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minGroupsForValidAssignment(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        for k in range(min(cnt.values()), 0, -1):
            ans = 0
            for v in cnt.values():
                if v // k < v % k:
                    ans = 0
                    break
                ans += (v + k) // (k + 1)
            if ans:
                return ans
```

#### Java

```java
class Solution {
    public int minGroupsForValidAssignment(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int k = nums.length;
        for (int v : cnt.values()) {
            k = Math.min(k, v);
        }
        for (;; --k) {
            int ans = 0;
            for (int v : cnt.values()) {
                if (v / k < v % k) {
                    ans = 0;
                    break;
                }
                ans += (v + k) / (k + 1);
            }
            if (ans > 0) {
                return ans;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minGroupsForValidAssignment(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            cnt[x]++;
        }
        int k = 1e9;
        for (auto& [_, v] : cnt) {
            ans = min(ans, v);
        }
        for (;; --k) {
            int ans = 0;
            for (auto& [_, v] : cnt) {
                if (v / k < v % k) {
                    ans = 0;
                    break;
                }
                ans += (v + k) / (k + 1);
            }
            if (ans) {
                return ans;
            }
        }
    }
};
```

#### Go

```go
func minGroupsForValidAssignment(nums []int) int {
	cnt := map[int]int{}
	for _, x := range nums {
		cnt[x]++
	}
	k := len(nums)
	for _, v := range cnt {
		k = min(k, v)
	}
	for ; ; k-- {
		ans := 0
		for _, v := range cnt {
			if v/k < v%k {
				ans = 0
				break
			}
			ans += (v + k) / (k + 1)
		}
		if ans > 0 {
			return ans
		}
	}
}
```

#### TypeScript

```ts
function minGroupsForValidAssignment(nums: number[]): number {
    const cnt: Map<number, number> = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    for (let k = Math.min(...cnt.values()); ; --k) {
        let ans = 0;
        for (const [_, v] of cnt) {
            if (((v / k) | 0) < v % k) {
                ans = 0;
                break;
            }
            ans += Math.ceil(v / (k + 1));
        }
        if (ans) {
            return ans;
        }
    }
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn min_groups_for_valid_assignment(nums: Vec<i32>) -> i32 {
        let mut cnt: HashMap<i32, i32> = HashMap::new();

        for x in nums.iter() {
            let count = cnt.entry(*x).or_insert(0);
            *count += 1;
        }

        let mut k = i32::MAX;

        for &v in cnt.values() {
            k = k.min(v);
        }

        for k in (1..=k).rev() {
            let mut ans = 0;

            for &v in cnt.values() {
                if v / k < v % k {
                    ans = 0;
                    break;
                }

                ans += (v + k) / (k + 1);
            }

            if ans > 0 {
                return ans;
            }
        }

        0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
