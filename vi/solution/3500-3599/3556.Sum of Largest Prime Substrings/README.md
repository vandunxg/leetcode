---
comments: true
difficulty: Medium
rating: 1439
source: Biweekly Contest 157 Q1
tags:
    - Hash Table
    - Math
    - String
    - Number Theory
    - Sorting
---

<!-- problem:start -->

# [3556. Sum of Largest Prime Substrings](https://leetcode.com/problems/sum-of-largest-prime-substrings)

[中文文档](/solution/3500-3599/3556.Sum%20of%20Largest%20Prime%20Substrings/README.md)

## Mô tả

<!-- description:start -->
<p data-end="157" data-start="30">Cho một chuỗi <code>s</code>, hãy tìm tổng của <strong>3 <span data-keyword="prime-number">số nguyên tố</span> phân biệt lớn nhất</strong> có thể tạo thành từ bất kỳ <strong><span data-keyword="substring">chuỗi con</span></strong> nào của chuỗi.</p>

<p data-end="269" data-start="166">Trả về <strong>tổng</strong> của ba số nguyên tố phân biệt lớn nhất có thể tạo thành. Nếu có ít hơn ba số, trả về tổng của <strong>tất cả</strong> các số nguyên tố có thể tạo thành. Nếu không thể tạo thành số nguyên tố nào, trả về 0.</p>

<p data-end="370" data-is-last-node="" data-is-only-node="" data-start="271"><strong data-end="280" data-start="271">Lưu ý:</strong> Mỗi số nguyên tố chỉ được tính <strong>một lần</strong>, ngay cả khi nó xuất hiện trong <strong>nhiều</strong> chuỗi con. Ngoài ra, khi chuyển một chuỗi con thành số nguyên, mọi số 0 ở đầu đều bị bỏ qua.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;12234&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1469</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="136" data-start="16">Các số nguyên tố phân biệt tạo thành từ các chuỗi con của <code>&quot;12234&quot;</code> là 2, 3, 23, 223 và 1223.</li>
    <li data-end="226" data-start="137">3 số nguyên tố lớn nhất là 1223, 223 và 23. Tổng của chúng là 1469.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;111&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li data-end="339" data-start="244">Số nguyên tố phân biệt tạo thành từ các chuỗi con của <code>&quot;111&quot;</code> là 11.</li>
    <li data-end="412" data-is-last-node="" data-start="340">Vì chỉ có một số nguyên tố nên tổng là 11.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="39" data-start="18"><code>1 &lt;= s.length &lt;= 10</code></li>
    <li data-end="68" data-is-last-node="" data-start="40"><code>s</code> chỉ gồm các chữ số.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Với chuỗi có độ dài vừa phải, có $O(n^2)$ số nguyên được tạo từ các chuỗi con, nên ta có thể liệt kê chúng và kiểm tra tính nguyên tố. Kết quả là tổng của ba số nguyên tố phân biệt lớn nhất.
>
> Lưu các số nguyên tố vào một set, sắp xếp rồi cộng ba phần tử cuối (hoặc tất cả nếu có ít hơn ba phần tử). Thay vì phân tích lại chuỗi, ta xây dựng số từ mỗi vị trí bắt đầu theo công thức $x = 10x + \textit{digit}$.

<!-- thinking:end -->

Ta có thể liệt kê tất cả các chuỗi con và kiểm tra xem chúng có phải là số nguyên tố hay không. Vì đề bài yêu cầu trả về tổng của 3 số nguyên tố phân biệt lớn nhất, ta có thể dùng một hash table để lưu tất cả các số nguyên tố.

Sau khi duyệt qua tất cả các chuỗi con, ta sắp xếp các số nguyên tố trong hash table theo thứ tự tăng dần, sau đó lấy 3 số nguyên tố lớn nhất để tính tổng.

Nếu hash table có ít hơn 3 số nguyên tố, trả về tổng của tất cả các số nguyên tố.

Độ phức tạp thời gian là $O(n^2 \times \sqrt{M})$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là độ dài của chuỗi và $M$ là giá trị của chuỗi con lớn nhất.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumOfLargestPrimes(self, s: str) -> int:
        def is_prime(x: int) -> bool:
            if x < 2:
                return False
            return all(x % i for i in range(2, int(sqrt(x)) + 1))

        st = set()
        n = len(s)
        for i in range(n):
            x = 0
            for j in range(i, n):
                x = x * 10 + int(s[j])
                if is_prime(x):
                    st.add(x)
        return sum(sorted(st)[-3:])
```

#### Java

```java
class Solution {
    public long sumOfLargestPrimes(String s) {
        Set<Long> st = new HashSet<>();
        int n = s.length();

        for (int i = 0; i < n; i++) {
            long x = 0;
            for (int j = i; j < n; j++) {
                x = x * 10 + (s.charAt(j) - '0');
                if (is_prime(x)) {
                    st.add(x);
                }
            }
        }

        List<Long> sorted = new ArrayList<>(st);
        Collections.sort(sorted);

        long ans = 0;
        int start = Math.max(0, sorted.size() - 3);
        for (int idx = start; idx < sorted.size(); idx++) {
            ans += sorted.get(idx);
        }
        return ans;
    }

    private boolean is_prime(long x) {
        if (x < 2) return false;
        for (long i = 2; i * i <= x; i++) {
            if (x % i == 0) return false;
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long sumOfLargestPrimes(string s) {
        unordered_set<long long> st;
        int n = s.size();

        for (int i = 0; i < n; ++i) {
            long long x = 0;
            for (int j = i; j < n; ++j) {
                x = x * 10 + (s[j] - '0');
                if (is_prime(x)) {
                    st.insert(x);
                }
            }
        }

        vector<long long> sorted(st.begin(), st.end());
        sort(sorted.begin(), sorted.end());

        long long ans = 0;
        int cnt = 0;
        for (int i = (int) sorted.size() - 1; i >= 0 && cnt < 3; --i, ++cnt) {
            ans += sorted[i];
        }
        return ans;
    }

private:
    bool is_prime(long long x) {
        if (x < 2) return false;
        for (long long i = 2; i * i <= x; ++i) {
            if (x % i == 0) return false;
        }
        return true;
    }
};
```

#### Go

```go
func sumOfLargestPrimes(s string) (ans int64) {
    st := make(map[int64]struct{})
    n := len(s)

    for i := 0; i < n; i++ {
        var x int64 = 0
        for j := i; j < n; j++ {
            x = x*10 + int64(s[j]-'0')
            if isPrime(x) {
                st[x] = struct{}{}
            }
        }
    }

    nums := make([]int64, 0, len(st))
    for num := range st {
        nums = append(nums, num)
    }
    sort.Slice(nums, func(i, j int) bool { return nums[i] < nums[j] })
    for i := len(nums) - 1; i >= 0 && len(nums)-i <= 3; i-- {
        ans += nums[i]
    }
    return
}

func isPrime(x int64) bool {
    if x < 2 {
        return false
    }
    sqrtX := int64(math.Sqrt(float64(x)))
    for i := int64(2); i <= sqrtX; i++ {
        if x%i == 0 {
            return false
        }
    }
    return true
}
```

#### TypeScript

```ts
function sumOfLargestPrimes(s: string): number {
    const st = new Set<number>();
    const n = s.length;

    for (let i = 0; i < n; i++) {
        let x = 0;
        for (let j = i; j < n; j++) {
            x = x * 10 + Number(s[j]);
            if (isPrime(x)) {
                st.add(x);
            }
        }
    }

    const sorted = Array.from(st).sort((a, b) => a - b);
    const topThree = sorted.slice(-3);
    return topThree.reduce((sum, val) => sum + val, 0);
}

function isPrime(x: number): boolean {
    if (x < 2) return false;
    for (let i = 2; i * i <= x; i++) {
        if (x % i === 0) return false;
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
