---
comments: true
difficulty: Easy
rating: 1382
source: Biweekly Contest 145 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3375. Minimum Operations to Make Array Values Equal to K](https://leetcode.com/problems/minimum-operations-to-make-array-values-equal-to-k)

[Tài liệu tiếng Trung](/solution/3300-3399/3375.Minimum%20Operations%20to%20Make%20Array%20Values%20Equal%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một số nguyên <code>h</code> được gọi là <strong>hợp lệ</strong> nếu tất cả các giá trị trong mảng <strong>lớn hơn nghiêm ngặt</strong> <code>h</code> đều <em>giống hệt nhau</em>.</p>

<p>Ví dụ, nếu <code>nums = [10, 8, 10, 8]</code>, <code>h = 9</code> là một số nguyên <strong>hợp lệ</strong> vì mọi <code>nums[i] &gt; 9</code>&nbsp;đều bằng 10, còn 5 không phải là một số nguyên <strong>hợp lệ</strong>.</p>

<p>Bạn được phép thực hiện thao tác sau trên <code>nums</code>:</p>

<ul>
	<li>Chọn một số nguyên <code>h</code> <em>hợp lệ</em> với các giá trị <strong>hiện tại</strong> trong <code>nums</code>.</li>
	<li>Với mỗi chỉ số <code>i</code> sao cho <code>nums[i] &gt; h</code>, đặt <code>nums[i]</code> thành <code>h</code>.</li>
</ul>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để mọi phần tử trong <code>nums</code> <strong>bằng nhau</strong> và bằng <code>k</code>. Nếu không thể làm cho tất cả phần tử bằng <code>k</code>, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,5,4,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác có thể được thực hiện theo thứ tự, lần lượt sử dụng các số nguyên hợp lệ 4 và 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể làm cho tất cả giá trị bằng 2.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,7,5,3], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các thao tác có thể được thực hiện bằng các số nguyên hợp lệ theo thứ tự 7, 5, 3 và 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100 </code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phép toán đưa mọi giá trị lớn hơn giá trị lớn thứ hai hiện tại $h$ về $h$, và mục tiêu là đưa tất cả phần tử về $k$. Các giá trị chỉ giảm, nên nếu có phần tử nhỏ hơn $k$ thì không thể thực hiện được.
>
> Mỗi phép toán loại bỏ một giá trị phân biệt lớn hơn $k$. Nếu giá trị nhỏ nhất đã là $k$, nhóm giá trị đó không cần phép toán.
>
> Sau khi đưa các giá trị vào set, đáp án là số phần tử của set trừ đi $[\textit{mi}=k]$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể chọn giá trị lớn thứ hai trong mảng hiện tại làm số nguyên hợp lệ $h$ ở mỗi lần, rồi đổi tất cả các số lớn hơn $h$ thành $h$. Cách này giúp tối thiểu hóa số thao tác. Ngoài ra, vì thao tác chỉ làm giảm các số, nếu có số nhỏ hơn $k$ trong mảng hiện tại thì ta không thể làm cho tất cả phần tử bằng $k$, nên trả về -1 ngay.

Ta duyệt qua mảng $\textit{nums}$. Với số hiện tại $x$, nếu $x < k$, ta trả về -1 ngay. Nếu không, ta thêm $x$ vào hash table và cập nhật giá trị nhỏ nhất $\textit{mi}$ trong mảng hiện tại. Cuối cùng, ta trả về kích thước của hash table trừ đi 1 (nếu $\textit{mi} = k$, ta cần trừ 1).

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], k: int) -> int:
        s = set()
        mi = inf
        for x in nums:
            if x < k:
                return -1
            mi = min(mi, x)
            s.add(x)
        return len(s) - int(k == mi)
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int k) {
        Set<Integer> s = new HashSet<>();
        int mi = 1 << 30;
        for (int x : nums) {
            if (x < k) {
                return -1;
            }
            mi = Math.min(mi, x);
            s.add(x);
        }
        return s.size() - (mi == k ? 1 : 0);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int k) {
        unordered_set<int> s;
        int mi = INT_MAX;
        for (int x : nums) {
            if (x < k) {
                return -1;
            }
            mi = min(mi, x);
            s.insert(x);
        }
        return s.size() - (mi == k);
    }
};
```

#### Go

```go
func minOperations(nums []int, k int) int {
	mi := 1 << 30
	s := map[int]bool{}
	for _, x := range nums {
		if x < k {
			return -1
		}
		s[x] = true
		mi = min(mi, x)
	}
	if mi == k {
		return len(s) - 1
	}
	return len(s)
}
```

#### TypeScript

```ts
function minOperations(nums: number[], k: number): number {
    const s = new Set<number>([k]);
    for (const x of nums) {
        if (x < k) return -1;
        s.add(x);
    }
    return s.size - 1;
}
```

#### JavaScript

```js
function minOperations(nums, k) {
    const s = new Set([k]);
    for (const x of nums) {
        if (x < k) return -1;
        s.add(x);
    }
    return s.size - 1;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(nums: Vec<i32>, k: i32) -> i32 {
        use std::collections::HashSet;

        let mut s = HashSet::new();
        let mut mi = i32::MAX;

        for &x in &nums {
            if x < k {
                return -1;
            }
            s.insert(x);
            mi = mi.min(x);
        }

        (s.len() as i32) - if mi == k { 1 } else { 0 }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
