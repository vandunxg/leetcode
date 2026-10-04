---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
---

<!-- problem:start -->

# [3431. Minimum Unlocked Indices to Sort Nums 🔒](https://leetcode.com/problems/minimum-unlocked-indices-to-sort-nums)

[中文文档](/solution/3400-3499/3431.Minimum%20Unlocked%20Indices%20to%20Sort%20Nums/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên từ 1 đến 3 và một mảng <strong>nhị phân</strong> <code>locked</code> có cùng kích thước.</p>

<p>Ta gọi <code>nums</code> là <strong>có thể sắp xếp</strong> nếu có thể sắp xếp nó bằng các phép hoán đổi hai phần tử kề nhau, trong đó phép hoán đổi giữa hai chỉ số <code>i</code> và <code>i + 1</code> được phép khi <code>nums[i] - nums[i + 1] == 1</code> và <code>locked[i] == 0</code>.</p>

<p>Trong một thao tác, bạn có thể mở khóa chỉ số <code>i</code> bất kỳ bằng cách đặt <code>locked[i]</code> thành 0.</p>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để <code>nums</code> <strong>có thể sắp xếp</strong>. Nếu không thể làm cho <code>nums</code> có thể sắp xếp, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,3,2], locked = [1,0,1,1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể sắp xếp <code>nums</code> bằng các phép hoán đổi sau:</p>

<ul>
    <li>hoán đổi chỉ số 1 với 2</li>
    <li>hoán đổi chỉ số 4 với 5</li>
</ul>

<p>Vì vậy, không cần mở khóa chỉ số nào.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,1,3,2,2], locked = [1,0,1,1,0,1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Nếu mở khóa các chỉ số 2 và 5, ta có thể sắp xếp <code>nums</code> bằng các phép hoán đổi sau:</p>

<ul>
    <li>hoán đổi chỉ số 1 với 2</li>
    <li>hoán đổi chỉ số 2 với 3</li>
    <li>hoán đổi chỉ số 4 với 5</li>
    <li>hoán đổi chỉ số 5 với 6</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,1,2,3,2,1], locked = [0,0,0,0,0,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ngay cả khi tất cả các chỉ số đều đã được mở khóa, có thể chứng minh rằng <code>nums</code> vẫn không thể sắp xếp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 3</code></li>
    <li><code>locked.length == nums.length</code></li>
    <li><code>0 &lt;= locked[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố suy luận

<!-- thinking:start -->

> **Tư duy**
>
> Mảng chỉ chứa $1,2,3$, và các phép hoán đổi kề nhau bị giới hạn bởi $\textit{locked}$. Không thể mô phỏng từng nghịch thế với $n\le 10^5$.
>
> Chỉ có $1$ với $2$ và $2$ với $3$ mới có thể đi qua nhau. $3$ không bao giờ có thể đi qua $1$, nên nếu $3$ xuất hiện trước một $1$ thì điều đó là không thể.
>
> Các chỉ số cần mở khóa là những vị trí vẫn bị khóa trong $[\textit{first2},\textit{last1})$ và $[\textit{first3},\textit{last2})$. Một lần duyệt sẽ ghi lại bốn đầu mút và đếm các vị trí đó.

<!-- thinking:end -->

Theo mô tả bài toán, để $\textit{nums}$ có thể sắp xếp, vị trí của số $3$ phải nằm sau vị trí của số $1$. Nếu vị trí của số $3$ nằm trước vị trí của số $1$, thì dù hoán đổi thế nào, số $3$ cũng không thể đi đến vị trí của số $1$, nên không thể làm cho $\textit{nums}$ có thể sắp xếp.

Ta dùng $\textit{first2}$ và $\textit{first3}$ để biểu diễn vị trí xuất hiện đầu tiên của các số $2$ và $3$, đồng thời dùng $\textit{last1}$ và $\textit{last2}$ để biểu diễn vị trí xuất hiện cuối cùng của các số $1$ và $2$.

Khi chỉ số $i$ nằm trong đoạn $[\textit{first2}, \textit{last1})$ hoặc $[\textit{first3}, \textit{last2})]$, giá trị tương ứng của $\textit{locked}[i]$ phải là $0$; nếu không, ta cần thực hiện một thao tác. Vì vậy, ta chỉ cần duyệt qua mảng $\textit{locked}$ và đếm các chỉ số không thỏa mãn điều kiện.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minUnlockedIndices(self, nums: List[int], locked: List[int]) -> int:
        n = len(nums)
        first2 = first3 = n
        last1 = last2 = -1
        for i, x in enumerate(nums):
            if x == 1:
                last1 = i
            elif x == 2:
                first2 = min(first2, i)
                last2 = i
            else:
                first3 = min(first3, i)
        if first3 < last1:
            return -1
        return sum(
            st and (first2 <= i < last1 or first3 <= i < last2)
            for i, st in enumerate(locked)
        )
```

#### Java

```java
class Solution {
    public int minUnlockedIndices(int[] nums, int[] locked) {
        int n = nums.length;
        int first2 = n, first3 = n;
        int last1 = -1, last2 = -1;
        for (int i = 0; i < n; ++i) {
            if (nums[i] == 1) {
                last1 = i;
            } else if (nums[i] == 2) {
                first2 = Math.min(first2, i);
                last2 = i;
            } else {
                first3 = Math.min(first3, i);
            }
        }
        if (first3 < last1) {
            return -1;
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (locked[i] == 1 && ((first2 <= i && i < last1) || (first3 <= i && i < last2))) {
                ++ans;
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
    int minUnlockedIndices(vector<int>& nums, vector<int>& locked) {
        int n = nums.size();
        int first2 = n, first3 = n;
        int last1 = -1, last2 = -1;

        for (int i = 0; i < n; ++i) {
            if (nums[i] == 1) {
                last1 = i;
            } else if (nums[i] == 2) {
                first2 = min(first2, i);
                last2 = i;
            } else {
                first3 = min(first3, i);
            }
        }

        if (first3 < last1) {
            return -1;
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (locked[i] == 1 && ((first2 <= i && i < last1) || (first3 <= i && i < last2))) {
                ++ans;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minUnlockedIndices(nums []int, locked []int) (ans int) {
    n := len(nums)
    first2, first3 := n, n
    last1, last2 := -1, -1
    for i, x := range nums {
        if x == 1 {
            last1 = i
        } else if x == 2 {
            if i < first2 {
                first2 = i
            }
            last2 = i
        } else {
            if i < first3 {
                first3 = i
            }
        }
    }
    if first3 < last1 {
        return -1
    }
    for i, st := range locked {
        if st == 1 && ((first2 <= i && i < last1) || (first3 <= i && i < last2)) {
            ans++
        }
    }
    return ans
}
```

#### TypeScript

```ts
function minUnlockedIndices(nums: number[], locked: number[]): number {
    const n = nums.length;
    let [first2, first3] = [n, n];
    let [last1, last2] = [-1, -1];

    for (let i = 0; i < n; i++) {
        if (nums[i] === 1) {
            last1 = i;
        } else if (nums[i] === 2) {
            first2 = Math.min(first2, i);
            last2 = i;
        } else {
            first3 = Math.min(first3, i);
        }
    }

    if (first3 < last1) {
        return -1;
    }

    let ans = 0;
    for (let i = 0; i < n; i++) {
        if (locked[i] === 1 && ((first2 <= i && i < last1) || (first3 <= i && i < last2))) {
            ans++;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
