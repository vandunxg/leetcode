---
comments: true
difficulty: Easy
rating: 1227
source: Weekly Contest 472 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3718. Smallest Missing Multiple of K](https://leetcode.com/problems/smallest-missing-multiple-of-k)

[中文文档](/solution/3700-3799/3718.Smallest%20Missing%20Multiple%20of%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>, hãy trả về <strong>bội dương nhỏ nhất</strong> của <code>k</code> <strong>không xuất hiện</strong> trong <code>nums</code>.</p>

<p>Một <strong>bội</strong> của <code>k</code> là bất kỳ số nguyên dương nào chia hết cho <code>k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [8,2,3,4,6], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">10</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các bội của <code>k = 2</code> là 2, 4, 6, 8, 10, 12... và bội nhỏ nhất không xuất hiện trong <code>nums</code> là 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,4,7,10,15], k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các bội của <code>k = 5</code> là 5, 10, 15, 20... và bội nhỏ nhất không xuất hiện trong <code>nums</code> là 5.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 100</code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm bội dương nhỏ nhất $k,2k,\ldots$ không xuất hiện. Vì cả $n$ và $k$ đều không vượt quá $100$, chỉ cần lưu các phần tử của mảng vào một set rồi lần lượt kiểm tra các bội là đủ.

<!-- thinking:end -->

Trước tiên, ta dùng một bảng băm $\textit{s}$ để lưu các số xuất hiện trong mảng $\textit{nums}$. Sau đó, bắt đầu từ bội dương đầu tiên của $k$, là $k \times 1$, ta lần lượt liệt kê từng bội dương cho đến khi tìm thấy bội đầu tiên không xuất hiện trong bảng băm $\textit{s}$, đó chính là đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def missingMultiple(self, nums: List[int], k: int) -> int:
        s = set(nums)
        for i in count(1):
            x = k * i
            if x not in s:
                return x
```

#### Java

```java
class Solution {
    public int missingMultiple(int[] nums, int k) {
        boolean[] s = new boolean[101];
        for (int x : nums) {
            s[x] = true;
        }
        for (int i = 1;; ++i) {
            int x = k * i;
            if (x >= s.length || !s[x]) {
                return x;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int missingMultiple(vector<int>& nums, int k) {
        unordered_set<int> s;
        for (int x : nums) {
            s.insert(x);
        }
        for (int i = 1;; ++i) {
            int x = k * i;
            if (!s.contains(x)) {
                return x;
            }
        }
    }
};
```

#### Go

```go
func missingMultiple(nums []int, k int) int {
	s := map[int]bool{}
	for _, x := range nums {
		s[x] = true
	}
	for i := 1; ; i++ {
		if x := k * i; !s[x] {
			return x
		}
	}
}
```

#### TypeScript

```ts
function missingMultiple(nums: number[], k: number): number {
    const s = new Set<number>(nums);
    for (let i = 1; ; ++i) {
        const x = k * i;
        if (!s.has(x)) {
            return x;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
