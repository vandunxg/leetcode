---
comments: true
difficulty: Medium
rating: 1439
source: Weekly Contest 464 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3659. Partition Array Into K-Distinct Groups](https://leetcode.com/problems/partition-array-into-k-distinct-groups)

[中文文档](/solution/3600-3699/3659.Partition%20Array%20Into%20K-Distinct%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Nhiệm vụ của bạn là xác định xem có thể chia tất cả phần tử của <code>nums</code> thành một hoặc nhiều nhóm sao cho:</p>

<ul>
	<li>Mỗi nhóm chứa <strong>chính xác</strong> <code>k</code> phần tử.</li>
	<li>Tất cả phần tử trong mỗi nhóm đều <strong>phân biệt</strong>.</li>
	<li>Mỗi phần tử trong <code>nums</code> phải được gán vào <strong>chính xác</strong> một nhóm.</li>
</ul>

<p>Trả về <code>true</code> nếu có thể thực hiện việc chia như vậy, ngược lại trả về <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chia khả thi là tạo 2 nhóm:</p>

<ul>
	<li>Nhóm 1: <code>[1, 2]</code></li>
	<li>Nhóm 2: <code>[3, 4]</code></li>
</ul>

<p>Mỗi nhóm chứa <code>k = 2</code> phần tử phân biệt và tất cả phần tử đều được sử dụng đúng một lần.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,2,2], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">true</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một cách chia khả thi là tạo 2 nhóm:</p>

<ul>
	<li>Nhóm 1: <code>[2, 3]</code></li>
	<li>Nhóm 2: <code>[2, 5]</code></li>
</ul>

<p>Mỗi nhóm chứa <code>k = 2</code> phần tử phân biệt và tất cả phần tử đều được sử dụng đúng một lần.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">false</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không thể tạo các nhóm gồm <code>k = 3</code> phần tử phân biệt bằng cách sử dụng mỗi giá trị đúng một lần.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code><sup>​​​​​​​</sup>1 &lt;= k &lt;= nums.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải chia các giá trị phân biệt thành những nhóm có $k$ phần tử. Nếu $k$ không chia hết $n$, số nhóm sẽ không phải là số nguyên.
>
> Có $m=n/k$ nhóm. Một giá trị không thể xuất hiện hai lần trong cùng một nhóm, nên tần suất của mỗi giá trị không được vượt quá $m$.
>
> Giới hạn này cũng đủ: các tần suất không vượt quá $m$ có thể được xoay vòng giữa $m$ nhóm. Vì vậy, chỉ cần so sánh số lần xuất hiện lớn nhất với $m$.

<!-- thinking:end -->

Ta ký hiệu độ dài của mảng là $n$. Nếu $n$ không chia hết cho $k$, ta không thể chia mảng thành các nhóm mà mỗi nhóm chứa $k$ phần tử, nên trả về trực tiếp $\text{false}$.

Tiếp theo, ta tính kích thước của mỗi nhóm $m = n / k$ và đếm số lần xuất hiện của từng phần tử trong mảng. Nếu số lần xuất hiện của bất kỳ phần tử nào vượt quá $m$, phần tử đó không thể được phân phối vào các nhóm, nên ta trả về trực tiếp $\text{false}$.

Cuối cùng, nếu số lần xuất hiện của mọi phần tử không vượt quá $m$, ta có thể chia mảng thành các nhóm mà mỗi nhóm chứa $k$ phần tử, và trả về $\text{true}$.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$ hoặc $O(M)$. Trong đó $n$ là độ dài của mảng và $M$ là giá trị lớn nhất của các phần tử trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def partitionArray(self, nums: List[int], k: int) -> bool:
        m, mod = divmod(len(nums), k)
        if mod:
            return False
        return max(Counter(nums).values()) <= m
```

#### Java

```java
class Solution {
    public boolean partitionArray(int[] nums, int k) {
        int n = nums.length;
        if (n % k != 0) {
            return false;
        }
        int m = n / k;
        int mx = Arrays.stream(nums).max().getAsInt();
        int[] cnt = new int[mx + 1];
        for (int x : nums) {
            if (++cnt[x] > m) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool partitionArray(vector<int>& nums, int k) {
        int n = nums.size();
        if (n % k) {
            return false;
        }
        int m = n / k;
        int mx = *ranges::max_element(nums);
        vector<int> cnt(mx + 1);
        for (int x : nums) {
            if (++cnt[x] > m) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func partitionArray(nums []int, k int) bool {
	n := len(nums)
	if n%k != 0 {
		return false
	}
	m := n / k
	mx := slices.Max(nums)
	cnt := make([]int, mx+1)
	for _, x := range nums {
		if cnt[x]++; cnt[x] > m {
			return false
		}
	}
	return true
}
```

#### TypeScript

```ts
function partitionArray(nums: number[], k: number): boolean {
    const n = nums.length;
    if (n % k) {
        return false;
    }
    const m = n / k;
    const mx = Math.max(...nums);
    const cnt: number[] = Array(mx + 1).fill(0);
    for (const x of nums) {
        if (++cnt[x] > m) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
