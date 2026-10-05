---
comments: true
difficulty: Hard
rating: 1964
source: Weekly Contest 490 Q4
tags:
    - Memoization
    - Array
    - Math
    - Dynamic Programming
    - Number Theory
---

<!-- problem:start -->

# [3850. Count Sequences to K](https://leetcode.com/problems/count-sequences-to-k)

[中文文档](/solution/3800-3899/3850.Count%20Sequences%20to%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Bắt đầu với giá trị ban đầu <code>val = 1</code> và xử lý <code>nums</code> từ trái sang phải. Tại mỗi chỉ số <code>i</code>, bạn phải chọn <strong>chính xác một</strong> trong các thao tác sau:</p>

<ul>
	<li>Nhân <code>val</code> với <code>nums[i]</code>.</li>
	<li>Chia <code>val</code> cho <code>nums[i]</code>.</li>
	<li>Giữ nguyên <code>val</code>.</li>
</ul>

<p>Sau khi xử lý tất cả phần tử, <code>val</code> được xem là <strong>bằng</strong> <code>k</code> chỉ khi giá trị hữu tỉ cuối cùng của nó <strong>chính xác</strong> bằng <code>k</code>.</p>

<p>Hãy trả về số lượng <strong>trình tự</strong> lựa chọn khác nhau sao cho <code>val == k</code>.</p>

<p><strong>Lưu ý:</strong> Phép chia là phép chia hữu tỉ (chính xác), không phải phép chia số nguyên. Ví dụ, <code>2 / 4 = 1 / 2</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,3,2], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 trình tự lựa chọn khác nhau sau đây cho kết quả <code>val == k</code>:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Trình tự</th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[0]</code></th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[1]</code></th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[2]</code></th>
			<th style="border: 1px solid black;"><code>val</code> cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Nhân: <code>val = 1 * 2 = 2</code></td>
			<td style="border: 1px solid black;">Nhân: <code>val = 2 * 3 = 6</code></td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">6</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">Nhân: <code>val = 1 * 3 = 3</code></td>
			<td style="border: 1px solid black;">Nhân: <code>val = 3 * 2 = 6</code></td>
			<td style="border: 1px solid black;">6</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,6,3], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 2 trình tự lựa chọn khác nhau sau đây cho kết quả <code>val == k</code>:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Trình tự</th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[0]</code></th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[1]</code></th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[2]</code></th>
			<th style="border: 1px solid black;"><code>val</code> cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Nhân: <code>val = 1 * 4 = 4</code></td>
			<td style="border: 1px solid black;">Chia: <code>val = 4 / 6 = 2 / 3</code></td>
			<td style="border: 1px solid black;">Nhân: <code>val = (2 / 3) * 3 = 2</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">Nhân: <code>val = 1 * 6 = 6</code></td>
			<td style="border: 1px solid black;">Chia: <code>val = 6 / 3 = 2</code></td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,5], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có 3 trình tự lựa chọn khác nhau sau đây cho kết quả <code>val == k</code>:</p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Trình tự</th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[0]</code></th>
			<th style="border: 1px solid black;">Phép toán trên <code>nums[1]</code></th>
			<th style="border: 1px solid black;"><code>val</code> cuối cùng</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Nhân: <code>val = 1 * 1 = 1</code></td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Chia: <code>val = 1 / 1 = 1</code></td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">Giữ nguyên <code>val</code></td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 19</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 6</code></li>
	<li><code>1 &lt;= k &lt;= 10<sup>15</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm với Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Bắt đầu từ $1$, với mỗi $nums[i]$ ta có thể nhân, chia hoặc giữ nguyên giá trị, và phân số cuối cùng phải bằng $k$. Mảng có độ dài nhỏ, nhưng các phân số trung gian có thể trở nên rất lớn.
>
> Một state gồm chỉ số và phân số tối giản $(p,q)$. Cây ba nhánh cần được memoization.
>
> Sau khi nhân hoặc chia, ta rút gọn bằng $\gcd$; tại một lá, kết quả là 1 khi và chỉ khi $p=k$ và $q=1$.
>
> Sau đó xóa cache để các test tiếp theo không dùng lại các state.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{dfs}(i, p, q)$ biểu thị số lượng trình tự lựa chọn khác nhau khi xử lý tại chỉ số $i$ với giá trị hữu tỉ hiện tại là $\frac{p}{q}$. Ban đầu, $\text{dfs}(0, 1, 1)$ biểu thị việc bắt đầu từ giá trị ban đầu $1$.

Tại mỗi chỉ số $i$, ta có ba lựa chọn:

1. Giữ nguyên, tức là $\text{dfs}(i + 1, p, q)$.
2. Nhân với $nums[i]$, tức là $\text{dfs}(i + 1, p \cdot nums[i], q)$.
3. Chia cho $nums[i]$, tức là $\text{dfs}(i + 1, p, q \cdot nums[i])$.

Để tránh các số tăng quá lớn, ta rút gọn tử số và mẫu số sau mỗi phép nhân hoặc chia. Cuối cùng, khi $i$ bằng $n$, nếu $\frac{p}{q}$ chính xác bằng $k$, ta trả về $1$; nếu không, ta trả về $0$.

Độ phức tạp thời gian là $O(n^4 + \log k)$, và độ phức tạp không gian là $O(n^4)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countSequences(self, nums: List[int], k: int) -> int:
        @cache
        def dfs(i: int, p: int, q: int) -> int:
            if i == n:
                return 1 if p == k and q == 1 else 0
            x = nums[i]
            res = dfs(i + 1, p, q)
            g = gcd(p * x, q)
            res += dfs(i + 1, p * x // g, q // g)
            g = gcd(p, q * x)
            res += dfs(i + 1, p // g, q * x // g)
            return res

        n = len(nums)
        ans = dfs(0, 1, 1)
        dfs.cache_clear()
        return ans
```

#### Java

```java
class Solution {

    record State(int i, long p, long q) {
    }

    private Map<State, Integer> f;
    private int[] nums;
    private int n;
    private long k;

    public int countSequences(int[] nums, long k) {
        this.nums = nums;
        this.n = nums.length;
        this.k = k;
        this.f = new HashMap<>();
        return dfs(0, 1L, 1L);
    }

    private int dfs(int i, long p, long q) {
        if (i == n) {
            return (p == k && q == 1L) ? 1 : 0;
        }

        State key = new State(i, p, q);
        if (f.containsKey(key)) {
            return f.get(key);
        }

        int res = dfs(i + 1, p, q);

        long x = nums[i];

        long g1 = gcd(p * x, q);
        res += dfs(i + 1, (p * x) / g1, q / g1);

        long g2 = gcd(p, q * x);
        res += dfs(i + 1, p / g2, (q * x) / g2);

        f.put(key, res);
        return res;
    }

    private long gcd(long a, long b) {
        while (b != 0) {
            long t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int n;
    long long k;
    vector<int>* nums;
    map<tuple<int, long long, long long>, int> f;

    int countSequences(vector<int>& nums, long long k) {
        this->nums = &nums;
        this->n = nums.size();
        this->k = k;
        f.clear();
        return dfs(0, 1LL, 1LL);
    }

    int dfs(int i, long long p, long long q) {
        if (i == n) {
            return (p == k && q == 1LL) ? 1 : 0;
        }

        auto key = make_tuple(i, p, q);
        if (f.count(key)) return f[key];

        int res = dfs(i + 1, p, q);

        long long x = (*nums)[i];

        long long g1 = gcd(p * x, q);
        res += dfs(i + 1, (p * x) / g1, q / g1);

        long long g2 = gcd(p, q * x);
        res += dfs(i + 1, p / g2, (q * x) / g2);

        f[key] = res;
        return res;
    }
};
```

#### Go

```go
func countSequences(nums []int, k int64) int {
	n := len(nums)
	type state struct {
		i int
		p int64
		q int64
	}
	f := make(map[state]int)

	var gcd func(int64, int64) int64
	gcd = func(a, b int64) int64 {
		for b != 0 {
			a, b = b, a%b
		}
		return a
	}

	var dfs func(int, int64, int64) int
	dfs = func(i int, p int64, q int64) int {
		if i == n {
			if p == k && q == 1 {
				return 1
			}
			return 0
		}

		key := state{i, p, q}
		if v, ok := f[key]; ok {
			return v
		}

		res := dfs(i+1, p, q)

		x := int64(nums[i])

		g1 := gcd(p*x, q)
		res += dfs(i+1, (p*x)/g1, q/g1)

		g2 := gcd(p, q*x)
		res += dfs(i+1, p/g2, (q*x)/g2)

		f[key] = res
		return res
	}

	return dfs(0, 1, 1)
}
```

#### TypeScript

```ts
function countSequences(nums: number[], k: number): number {
    const n = nums.length;
    const f = new Map<string, number>();

    function gcd(a: number, b: number): number {
        while (b !== 0) {
            const t = a % b;
            a = b;
            b = t;
        }
        return a;
    }

    function dfs(i: number, p: number, q: number): number {
        if (i === n) {
            return p === k && q === 1 ? 1 : 0;
        }

        const key = `${i},${p},${q}`;
        if (f.has(key)) return f.get(key)!;

        let res = dfs(i + 1, p, q);

        const x = nums[i];

        const g1 = gcd(p * x, q);
        res += dfs(i + 1, (p * x) / g1, q / g1);

        const g2 = gcd(p, q * x);
        res += dfs(i + 1, p / g2, (q * x) / g2);

        f.set(key, res);
        return res;
    }

    return dfs(0, 1, 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
