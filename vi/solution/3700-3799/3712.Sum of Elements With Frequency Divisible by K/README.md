---
comments: true
difficulty: Easy
rating: 1198
source: Weekly Contest 471 Q1
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3712. Sum of Elements With Frequency Divisible by K](https://leetcode.com/problems/sum-of-elements-with-frequency-divisible-by-k)

[Tài liệu tiếng Trung](/solution/3700-3799/3712.Sum%20of%20Elements%20With%20Frequency%20Divisible%20by%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Trả về một số nguyên biểu thị <strong>tổng</strong> của tất cả các phần tử trong <code>nums</code> có <strong><span data-keyword="frequency-array">tần suất</span></strong> chia hết cho <code>k</code>, hoặc 0 nếu không có phần tử nào như vậy.</p>

<p><strong>Lưu ý:</strong> Nếu tổng tần suất của một phần tử chia hết cho <code>k</code>, phần tử đó được tính vào tổng <strong>đúng</strong> số lần nó xuất hiện trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2,3,3,3,3,4], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">16</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số 1 xuất hiện một lần (tần suất lẻ).</li>
	<li>Số 2 xuất hiện hai lần (tần suất chẵn).</li>
	<li>Số 3 xuất hiện bốn lần (tần suất chẵn).</li>
	<li>Số 4 xuất hiện một lần (tần suất lẻ).</li>
</ul>

<p>Vì vậy, tổng cần tìm là <code>2 + 2 + 3 + 3 + 3 + 3 = 16</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có phần tử nào xuất hiện một số lần chẵn, nên tổng cần tìm là 0.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,4,4,1,2,3], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">12</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Số 1 xuất hiện một lần.</li>
	<li>Số 2 xuất hiện một lần.</li>
	<li>Số 3 xuất hiện một lần.</li>
	<li>Số 4 xuất hiện ba lần.</li>
</ul>

<p>Vì vậy, tổng cần tìm là <code>4 + 4 + 4 = 12</code>.</p>
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

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Điều kiện chỉ phụ thuộc vào việc tần suất của mỗi giá trị có chia hết cho $k$ hay không, chứ không phụ thuộc vào vị trí. Với $n\le 100$, một bảng tần suất là đủ; mỗi giá trị thỏa mãn điều kiện được cộng vào tổng đúng số lần nó xuất hiện.

<!-- thinking:end -->

Ta sử dụng một bảng băm $\textit{cnt}$ để ghi nhận tần suất của mỗi số. Duyệt qua mảng $\textit{nums}$, với mỗi số $x$, ta tăng $\textit{cnt}[x]$ thêm $1$.

Sau đó, ta duyệt qua bảng băm $\textit{cnt}$. Với mỗi phần tử $x$, nếu tần suất $\textit{cnt}[x]$ chia hết cho $k$, ta cộng $x$ nhân với tần suất của nó vào kết quả.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(m)$, trong đó $m$ là số phần tử phân biệt trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumDivisibleByK(self, nums: List[int], k: int) -> int:
        cnt = Counter(nums)
        return sum(x * v for x, v in cnt.items() if v % k == 0)
```

#### Java

```java
class Solution {
    public int sumDivisibleByK(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        int ans = 0;
        for (var e : cnt.entrySet()) {
            int x = e.getKey(), v = e.getValue();
            if (v % k == 0) {
                ans += x * v;
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
    int sumDivisibleByK(vector<int>& nums, int k) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }
        int ans = 0;
        for (auto& [x, v] : cnt) {
            if (v % k == 0) {
                ans += x * v;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func sumDivisibleByK(nums []int, k int) (ans int) {
    cnt := map[int]int{}
    for _, x := range nums {
        cnt[x]++
    }
    for x, v := range cnt {
        if v%k == 0 {
            ans += x * v
        }
    }
    return
}
```

#### TypeScript

```ts
function sumDivisibleByK(nums: number[], k: number): number {
    const cnt = new Map();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
    }
    let ans = 0;
    for (const [x, v] of cnt.entries()) {
        if (v % k === 0) {
            ans += x * v;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
