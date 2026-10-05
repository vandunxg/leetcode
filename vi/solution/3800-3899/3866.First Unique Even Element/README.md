---
comments: true
difficulty: Easy
rating: 1209
source: Biweekly Contest 178 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3866. First Unique Even Element](https://leetcode.com/problems/first-unique-even-element)

[中文文档](/solution/3800-3899/3866.First%20Unique%20Even%20Element/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về một số nguyên biểu thị số nguyên <strong>chẵn</strong> đầu tiên (có chỉ số nhỏ nhất trong mảng) xuất hiện <strong>đúng</strong> một lần trong <code>nums</code>. Nếu không có số nguyên nào như vậy, hãy trả về -1.</p>

<p>Một số nguyên <code>x</code> được xem là <strong>chẵn</strong> nếu chia hết cho 2.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,2,5,4,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cả 2 và 6 đều là số chẵn và mỗi số xuất hiện đúng một lần. Vì 2 xuất hiện trước trong mảng nên đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có số nguyên chẵn nào xuất hiện đúng một lần, nên trả về -1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Counting

<!-- thinking:start -->

> **Tư duy**
>
> Tìm giá trị chẵn đầu tiên xuất hiện đúng một lần. Vì độ dài mảng $\le 100$, ta có thể duyệt hai lần.
>
> Trước tiên đếm số lần xuất hiện, sau đó duyệt theo thứ tự ban đầu để chọn phần tử có chỉ số nhỏ nhất.
>
> Điều kiện cần kiểm tra là số chẵn và có tần suất bằng $1$.
>
> Nếu không có phần tử nào thỏa mãn, trả về $-1$.

<!-- thinking:end -->

Ta có thể dùng một hash table hoặc mảng $\textit{cnt}$ để đếm số lần xuất hiện của mỗi số nguyên trong mảng. Sau đó, ta duyệt lại mảng để tìm và trả về số chẵn đầu tiên thỏa mãn điều kiện. Nếu không có số chẵn nào như vậy, ta trả về -1.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(M)$, trong đó $M$ là miền giá trị của các số nguyên trong mảng (100 trong bài này).

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstUniqueEven(self, nums: list[int]) -> int:
        cnt = Counter(nums)
        for x in nums:
            if x % 2 == 0 and cnt[x] == 1:
                return x
        return -1
```

#### Java

```java
class Solution {
    public int firstUniqueEven(int[] nums) {
        int[] cnt = new int[101];
        for (int x : nums) {
            ++cnt[x];
        }
        for (int x : nums) {
            if (x % 2 == 0 && cnt[x] == 1) {
                return x;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstUniqueEven(vector<int>& nums) {
        int cnt[101]{};
        for (int x : nums) {
            ++cnt[x];
        }
        for (int x : nums) {
            if (x % 2 == 0 && cnt[x] == 1) {
                return x;
            }
        }
        return -1;
    }
};
```

#### Go

```go
func firstUniqueEven(nums []int) int {
    cnt := make([]int, 101)
    for _, x := range nums {
        cnt[x]++
    }
    for _, x := range nums {
        if x%2 == 0 && cnt[x] == 1 {
            return x
        }
    }
    return -1
}
```

#### TypeScript

```ts
function firstUniqueEven(nums: number[]): number {
    const cnt: number[] = new Array(101).fill(0);

    for (const x of nums) {
        cnt[x]++;
    }

    for (const x of nums) {
        if (x % 2 === 0 && cnt[x] === 1) {
            return x;
        }
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
