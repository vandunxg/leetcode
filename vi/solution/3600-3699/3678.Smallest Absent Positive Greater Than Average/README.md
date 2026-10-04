---
comments: true
difficulty: Easy
rating: 1306
source: Biweekly Contest 165 Q1
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3678. Smallest Absent Positive Greater Than Average](https://leetcode.com/problems/smallest-absent-positive-greater-than-average)

[中文文档](/solution/3600-3699/3678.Smallest%20Absent%20Positive%20Greater%20Than%20Average/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về số nguyên <strong>dương nhỏ nhất chưa xuất hiện</strong> trong <code>nums</code> và <strong>lớn hơn nghiêm ngặt</strong> <strong>giá trị trung bình</strong> của tất cả phần tử trong <code>nums</code>.</p>
Giá trị <strong>trung bình</strong> của một mảng được định nghĩa là tổng tất cả phần tử chia cho số lượng phần tử.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị trung bình của <code>nums</code> là <code>(3 + 5) / 2 = 8 / 2 = 4</code>.</li>
	<li>Số nguyên dương nhỏ nhất chưa xuất hiện và lớn hơn 4 là 6.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,1,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>​​​​​​​Giá trị trung bình của <code>nums</code> là <code>(-1 + 1 + 2) / 3 = 2 / 3 = 0.667</code>.</li>
	<li>Số nguyên dương nhỏ nhất chưa xuất hiện và lớn hơn 0.667 là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,-1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Giá trị trung bình của <code>nums</code> là <code>(4 + (-1)) / 2 = 3 / 2 = 1.50</code>.</li>
	<li>Số nguyên dương nhỏ nhất chưa xuất hiện và lớn hơn 1.50 là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-100 &lt;= nums[i] &lt;= 100</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm số nguyên dương nhỏ nhất chưa xuất hiện trong mảng và lớn hơn nghiêm ngặt giá trị trung bình. Chỉ cần bắt đầu từ $\max(1,\lfloor\textit{avg}\rfloor+1)$ là đủ: một số dương bị thiếu không thể nằm quá xa.
>
> Lưu các phần tử của mảng vào một set và lấy giá trị trung bình nguyên làm cận dưới. Tăng candidate khi nó vẫn còn nằm trong set.
>
> Việc duyệt theo trục giá trị là ngắn và vẫn có độ phức tạp tuyến tính theo $n$.

<!-- thinking:end -->

Ta dùng một hash map $\textit{s}$ để ghi lại các phần tử xuất hiện trong mảng $\textit{nums}$.

Sau đó, ta tính giá trị trung bình $\textit{avg}$ của mảng $\textit{nums}$ và khởi tạo đáp án $\textit{ans}$ là $\max(1, \lfloor \textit{avg} \rfloor + 1)$.

Nếu $\textit{ans}$ xuất hiện trong $\textit{s}$, ta tăng $\textit{ans}$ cho đến khi nó không còn xuất hiện trong $\textit{s}$.

Cuối cùng, ta trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestAbsent(self, nums: List[int]) -> int:
        s = set(nums)
        ans = max(1, sum(nums) // len(nums) + 1)
        while ans in s:
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int smallestAbsent(int[] nums) {
        Set<Integer> s = new HashSet<>();
        int sum = 0;
        for (int x : nums) {
            s.add(x);
            sum += x;
        }
        int ans = Math.max(1, sum / nums.length + 1);
        while (s.contains(ans)) {
            ++ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestAbsent(vector<int>& nums) {
        unordered_set<int> s;
        int sum = 0;
        for (int x : nums) {
            s.insert(x);
            sum += x;
        }
        int ans = max(1, sum / (int) nums.size() + 1);
        while (s.contains(ans)) {
            ++ans;
        }
        return ans;
    }
};
```

#### Go

```go
func smallestAbsent(nums []int) int {
	s := map[int]bool{}
	sum := 0
	for _, x := range nums {
		s[x] = true
		sum += x
	}
	ans := max(1, sum/len(nums)+1)
	for s[ans] {
		ans++
	}
	return ans
}
```

#### TypeScript

```ts
function smallestAbsent(nums: number[]): number {
    const s = new Set<number>(nums);
    const sum = nums.reduce((a, b) => a + b, 0);
    let ans = Math.max(1, Math.floor(sum / nums.length) + 1);
    while (s.has(ans)) {
        ans++;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
