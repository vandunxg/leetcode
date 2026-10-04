---
comments: true
difficulty: Medium
rating: 1227
source: Biweekly Contest 161 Q1
tags:
    - Array
    - Math
    - Number Theory
---

<!-- problem:start -->

# [3618. Split Array by Prime Indices](https://leetcode.com/problems/split-array-by-prime-indices)

[中文文档](/solution/3600-3699/3618.Split%20Array%20by%20Prime%20Indices/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Chia <code>nums</code> thành hai mảng <code>A</code> và <code>B</code> theo quy tắc sau:</p>

<ul>
    <li>Các phần tử ở <strong><span data-keyword="prime-number">chỉ số nguyên tố</span></strong> trong <code>nums</code> phải được đưa vào mảng <code>A</code>.</li>
    <li>Tất cả các phần tử còn lại phải được đưa vào mảng <code>B</code>.</li>
</ul>

<p>Trả về hiệu <strong>tuyệt đối</strong> giữa tổng của hai mảng: <code>|sum(A) - sum(B)|</code>.</p>

<p><strong>Lưu ý:</strong> Tổng của một mảng rỗng bằng 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Chỉ số nguyên tố duy nhất trong mảng là 2, vì vậy <code>nums[2] = 4</code> được đặt vào mảng <code>A</code>.</li>
    <li>Các phần tử còn lại, <code>nums[0] = 2</code> và <code>nums[1] = 3</code>, được đặt vào mảng <code>B</code>.</li>
    <li><code>sum(A) = 4</code>, <code>sum(B) = 2 + 3 = 5</code>.</li>
    <li>Hiệu tuyệt đối là <code>|4 - 5| = 1</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [-1,5,7,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Các chỉ số nguyên tố trong mảng là 2 và 3, vì vậy <code>nums[2] = 7</code> và <code>nums[3] = 0</code> được đặt vào mảng <code>A</code>.</li>
    <li>Các phần tử còn lại, <code>nums[0] = -1</code> và <code>nums[1] = 5</code>, được đặt vào mảng <code>B</code>.</li>
    <li><code>sum(A) = 7 + 0 = 7</code>, <code>sum(B) = -1 + 5 = 4</code>.</li>
    <li>Hiệu tuyệt đối là <code>|7 - 4| = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sàng Eratosthenes + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Đáp án là hiệu tuyệt đối giữa tổng tại các chỉ số nguyên tố và tổng tại các chỉ số hợp số, tức là một tổng có dấu theo $i$. Các chỉ số có thể lên đến $10^5$, nên việc thử chia cho từng chỉ số là không cần thiết.
>
> Sàng Eratosthenes đánh dấu tính nguyên tố trên đoạn $[0,10^5]$. Duyệt qua mảng, cộng $x$ tại chỉ số nguyên tố và cộng $-x$ ở các vị trí còn lại, sau đó lấy giá trị tuyệt đối.
>
> Có thể tái sử dụng sàng; mỗi truy vấn chỉ cần một lần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể sử dụng Sàng Eratosthenes để tiền xử lý tất cả số nguyên tố trong khoảng $[0, 10^5]$. Sau đó, ta duyệt qua mảng $\textit{nums}$. Với $\textit{nums}[i]$, nếu $i$ là số nguyên tố, ta cộng $\textit{nums}[i]$ vào đáp án; ngược lại, ta cộng $-\textit{nums}[i]$ vào đáp án. Cuối cùng, ta trả về giá trị tuyệt đối của đáp án.

Bỏ qua thời gian và không gian tiền xử lý, độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
m = 10**5 + 10
primes = [True] * m
primes[0] = primes[1] = False
for i in range(2, m):
    if primes[i]:
        for j in range(i + i, m, i):
            primes[j] = False


class Solution:
    def splitArray(self, nums: List[int]) -> int:
        return abs(sum(x if primes[i] else -x for i, x in enumerate(nums)))
```

#### Java

```java
class Solution {
    private static final int M = 100000 + 10;
    private static boolean[] primes = new boolean[M];

    static {
        for (int i = 0; i < M; i++) {
            primes[i] = true;
        }
        primes[0] = primes[1] = false;

        for (int i = 2; i < M; i++) {
            if (primes[i]) {
                for (int j = i + i; j < M; j += i) {
                    primes[j] = false;
                }
            }
        }
    }

    public long splitArray(int[] nums) {
        long ans = 0;
        for (int i = 0; i < nums.length; ++i) {
            ans += primes[i] ? nums[i] : -nums[i];
        }
        return Math.abs(ans);
    }
}
```

#### C++

```cpp
const int M = 1e5 + 10;
bool primes[M];
auto init = [] {
    memset(primes, true, sizeof(primes));
    primes[0] = primes[1] = false;
    for (int i = 2; i < M; ++i) {
        if (primes[i]) {
            for (int j = i + i; j < M; j += i) {
                primes[j] = false;
            }
        }
    }
    return 0;
}();

class Solution {
public:
    long long splitArray(vector<int>& nums) {
        long long ans = 0;
        for (int i = 0; i < nums.size(); ++i) {
            ans += primes[i] ? nums[i] : -nums[i];
        }
        return abs(ans);
    }
};
```

#### Go

```go
const M = 100000 + 10

var primes [M]bool

func init() {
    for i := 0; i < M; i++ {
        primes[i] = true
    }
    primes[0], primes[1] = false, false

    for i := 2; i < M; i++ {
        if primes[i] {
            for j := i + i; j < M; j += i {
                primes[j] = false
            }
        }
    }
}

func splitArray(nums []int) (ans int64) {
    for i, num := range nums {
        if primes[i] {
            ans += int64(num)
        } else {
            ans -= int64(num)
        }
    }
    return max(ans, -ans)
}
```

#### TypeScript

```ts
const M = 100000 + 10;
const primes: boolean[] = Array(M).fill(true);

const init = (() => {
    primes[0] = primes[1] = false;

    for (let i = 2; i < M; i++) {
        if (primes[i]) {
            for (let j = i + i; j < M; j += i) {
                primes[j] = false;
            }
        }
    }
})();

function splitArray(nums: number[]): number {
    let ans = 0;
    for (let i = 0; i < nums.length; i++) {
        ans += primes[i] ? nums[i] : -nums[i];
    }
    return Math.abs(ans);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
