---
comments: true
difficulty: Medium
rating: 1506
source: Weekly Contest 450 Q2
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [3551. Minimum Swaps to Sort by Digit Sum](https://leetcode.com/problems/minimum-swaps-to-sort-by-digit-sum)

[中文文档](/solution/3500-3599/3551.Minimum%20Swaps%20to%20Sort%20by%20Digit%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên dương <strong>phân biệt</strong>. Bạn cần sắp xếp mảng theo thứ tự <strong>tăng dần</strong> dựa trên tổng các chữ số của mỗi số. Nếu hai số có cùng tổng chữ số, số <strong>nhỏ hơn</strong> đứng trước trong mảng đã sắp xếp.</p>

<p>Trả về số lần hoán đổi <strong>ít nhất</strong> cần thực hiện để đưa <code>nums</code> về thứ tự này.</p>

<p>Một <strong>phép hoán đổi</strong> được định nghĩa là việc đổi chỗ các giá trị tại hai vị trí khác nhau trong mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [37,100]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tính tổng các chữ số của mỗi số nguyên: <code>[3 + 7 = 10, 1 + 0 + 0 = 1] &rarr; [10, 1]</code></li>
    <li>Sắp xếp các số nguyên dựa trên tổng chữ số: <code>[100, 37]</code>. Hoán đổi <code>37</code> với <code>100</code> để đạt được thứ tự đã sắp xếp.</li>
    <li>Vì vậy, số lần hoán đổi ít nhất cần thực hiện để sắp xếp lại <code>nums</code> là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [22,14,33,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tính tổng các chữ số của mỗi số nguyên: <code>[2 + 2 = 4, 1 + 4 = 5, 3 + 3 = 6, 7 = 7] &rarr; [4, 5, 6, 7]</code></li>
    <li>Sắp xếp các số nguyên dựa trên tổng chữ số: <code>[22, 14, 33, 7]</code>. Mảng đã được sắp xếp.</li>
    <li>Vì vậy, số lần hoán đổi ít nhất cần thực hiện để sắp xếp lại <code>nums</code> là 0.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [18,43,34,16]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tính tổng các chữ số của mỗi số nguyên: <code>[1 + 8 = 9, 4 + 3 = 7, 3 + 4 = 7, 1 + 6 = 7] &rarr; [9, 7, 7, 7]</code></li>
    <li>Sắp xếp các số nguyên dựa trên tổng chữ số: <code>[16, 34, 43, 18]</code>. Hoán đổi <code>18</code> với <code>16</code>, rồi hoán đổi <code>43</code> với <code>34</code> để đạt được thứ tự đã sắp xếp.</li>
    <li>Vì vậy, số lần hoán đổi ít nhất cần thực hiện để sắp xếp lại <code>nums</code> là 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>nums</code> gồm các số nguyên dương <strong>phân biệt</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Thứ tự đích được xác định duy nhất bởi $(\textit{digitSum}(x), x)$. Số lần hoán đổi ít nhất trong một hoán vị bằng $n$ trừ đi số chu trình.
>
> Ánh xạ mỗi giá trị tới chỉ số của nó trong mảng đã sắp xếp rồi lần theo các con trỏ đó, đồng thời đánh dấu các vị trí đã thăm. Mỗi chu trình có độ dài $\ell$ cần $\ell-1$ lần hoán đổi, nên tổng số lần hoán đổi là $n$ trừ đi số chu trình.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minSwaps(self, nums: List[int]) -> int:
        def f(x: int) -> int:
            s = 0
            while x:
                s += x % 10
                x //= 10
            return s

        n = len(nums)
        arr = sorted((f(x), x) for x in nums)
        d = {a[1]: i for i, a in enumerate(arr)}
        ans = n
        vis = [False] * n
        for i in range(n):
            if not vis[i]:
                ans -= 1
                j = i
                while not vis[j]:
                    vis[j] = True
                    j = d[nums[j]]
        return ans
```

#### Java

```java
class Solution {
    public int minSwaps(int[] nums) {
        int n = nums.length;
        int[][] arr = new int[n][2];
        for (int i = 0; i < n; i++) {
            arr[i][0] = f(nums[i]);
            arr[i][1] = nums[i];
        }
        Arrays.sort(arr, (a, b) -> {
            if (a[0] != b[0]) return Integer.compare(a[0], b[0]);
            return Integer.compare(a[1], b[1]);
        });
        Map<Integer, Integer> d = new HashMap<>();
        for (int i = 0; i < n; i++) {
            d.put(arr[i][1], i);
        }
        boolean[] vis = new boolean[n];
        int ans = n;
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                ans--;
                int j = i;
                while (!vis[j]) {
                    vis[j] = true;
                    j = d.get(nums[j]);
                }
            }
        }
        return ans;
    }

    private int f(int x) {
        int s = 0;
        while (x != 0) {
            s += x % 10;
            x /= 10;
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int f(int x) {
        int s = 0;
        while (x) {
            s += x % 10;
            x /= 10;
        }
        return s;
    }

    int minSwaps(vector<int>& nums) {
        int n = nums.size();
        vector<pair<int, int>> arr(n);
        for (int i = 0; i < n; ++i) arr[i] = {f(nums[i]), nums[i]};
        sort(arr.begin(), arr.end());
        unordered_map<int, int> d;
        for (int i = 0; i < n; ++i) d[arr[i].second] = i;
        vector<char> vis(n, 0);
        int ans = n;
        for (int i = 0; i < n; ++i) {
            if (!vis[i]) {
                --ans;
                int j = i;
                while (!vis[j]) {
                    vis[j] = 1;
                    j = d[nums[j]];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minSwaps(nums []int) int {
    n := len(nums)
    arr := make([][2]int, n)
    for i := 0; i < n; i++ {
        arr[i][0] = f(nums[i])
        arr[i][1] = nums[i]
    }
    sort.Slice(arr, func(i, j int) bool {
        if arr[i][0] != arr[j][0] {
            return arr[i][0] < arr[j][0]
        }
        return arr[i][1] < arr[j][1]
    })
    d := make(map[int]int, n)
    for i := 0; i < n; i++ {
        d[arr[i][1]] = i
    }
    vis := make([]bool, n)
    ans := n
    for i := 0; i < n; i++ {
        if !vis[i] {
            ans--
            j := i
            for !vis[j] {
                vis[j] = true
                j = d[nums[j]]
            }
        }
    }
    return ans
}

func f(x int) int {
    s := 0
    for x != 0 {
        s += x % 10
        x /= 10
    }
    return s
}
```

#### TypeScript

```ts
function f(x: number): number {
    let s = 0;
    while (x !== 0) {
        s += x % 10;
        x = Math.floor(x / 10);
    }
    return s;
}

function minSwaps(nums: number[]): number {
    const n = nums.length;
    const arr: [number, number][] = new Array(n);
    for (let i = 0; i < n; i++) {
        arr[i] = [f(nums[i]), nums[i]];
    }
    arr.sort((a, b) => (a[0] !== b[0] ? a[0] - b[0] : a[1] - b[1]));
    const d = new Map<number, number>();
    for (let i = 0; i < n; i++) {
        d.set(arr[i][1], i);
    }
    const vis: boolean[] = new Array(n).fill(false);
    let ans = n;
    for (let i = 0; i < n; i++) {
        if (!vis[i]) {
            ans--;
            let j = i;
            while (!vis[j]) {
                vis[j] = true;
                j = d.get(nums[j])!;
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
