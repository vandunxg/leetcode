---
comments: true
difficulty: Medium
rating: 1463
source: Weekly Contest 517 Q2
---

<!-- problem:start -->

# [4039. Sum of Decoded Numbers](https://leetcode.com/problems/sum-of-decoded-numbers)

[中文文档](/solution/4000-4099/4039.Sum%20of%20Decoded%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Mỗi <code>nums[i]</code> là một số nguyên <strong>được mã hóa</strong>, biểu diễn hai số nguyên dương <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code>. Để giải mã <code>nums[i]</code>, ta định nghĩa:</p>

<ul>
	<li><code>width<sub>i</sub> = nums[i] % 10</code>.</li>
	<li><code>d<sub>i</sub> = floor(nums[i] / 10)</code>.</li>
	<li><code>x<sub>i</sub></code> là số nguyên được tạo bởi <code>width<sub>i</sub></code> chữ số đầu tiên trong biểu diễn thập phân của <code>d<sub>i</sub></code>.</li>
	<li><code>y<sub>i</sub></code> là số nguyên được tạo bởi tất cả các chữ số còn lại trong biểu diễn thập phân của <code>d<sub>i</sub></code>.</li>
</ul>

<p>Đảm bảo rằng biểu diễn thập phân của <code>d<sub>i</sub></code> có nhiều hơn <code>width<sub>i</sub></code> chữ số. Do đó, cả <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> đều có ít nhất một chữ số.</p>

<p><strong>Giá trị sau khi giải mã</strong> của <code>nums[i]</code> là <code>x<sub>i</sub><sup>y<sub>i</sub></sup></code>.</p>

<p>Trả về tổng các giá trị sau khi giải mã của tất cả phần tử trong <code>nums</code>, lấy modulo <code>10<sup>9</sup> + 7</code>.</p>

<p>Hàm <code>floor()</code> trả về phần nguyên của phép chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [231]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với 231, ta có <code>width = 1</code>, <code>d = 23</code>, <code>x = 2</code> và <code>y = 3</code>.</li>
	<li>Giá trị sau khi giải mã của 231 là <code>2<sup>3</sup> = 8</code>.</li>
	<li>Vì <code>nums</code> chỉ có một phần tử nên tổng các giá trị sau khi giải mã là 8.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2522,2101]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1649</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với 2522, ta có <code>width = 2</code>, <code>d = 252</code>, <code>x = 25</code> và <code>y = 2</code>.</li>
	<li>Giá trị sau khi giải mã của 2522 là <code>25<sup>2</sup> = 625</code>.</li>
	<li>Với 2101, ta có <code>width = 1</code>, <code>d = 210</code>, <code>x = 2</code> và <code>y = 10</code>.</li>
	<li>Giá trị sau khi giải mã của 2101 là <code>2<sup>10</sup> = 1024</code>.</li>
	<li>Tổng các giá trị sau khi giải mã là <code>625 + 1024 = 1649</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2301]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">73741817</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Với 2301, ta có <code>width = 1</code>, <code>d = 230</code>, <code>x = 2</code> và <code>y = 30</code>.</li>
	<li>Giá trị sau khi giải mã là <code>2<sup>30</sup> = 1073741824</code>.</li>
	<li>Do đó, đáp án là <code>1073741824 modulo (10<sup>9</sup> + 7) = 73741817</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>100 &lt; nums[i] &lt; 10<sup>15</sup></code></li>
	<li><code>1 &lt;= width<sub>i</sub> &lt;= 9</code></li>
	<li><code>1 &lt;= x<sub>i</sub>, y<sub>i</sub> &lt; 10<sup>9</sup></code></li>
	<li>Các chuỗi chữ số dùng để tạo <code>x<sub>i</sub></code> và <code>y<sub>i</sub></code> không có số 0 ở đầu.</li>
	<li>Đảm bảo mọi phần tử trong <code>nums</code> đều là số nguyên được mã hóa hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi phần tử được giải mã độc lập: chữ số cuối là độ rộng, các chữ số còn lại được tách thành $x$ và $y$, sau đó ta tính $x^y$. Các phần tử không dùng chung trạng thái.
>
> $y$ có thể đạt tới $10^9$, nên không thể nhân trong một vòng lặp. Lũy thừa nhanh tính $x^y\bmod(10^9+7)$ trong $O(\log y)$, rồi ta cộng các kết quả theo cùng modulo nguyên tố.

<!-- thinking:end -->

Ta giải mã từng phần tử đúng như mô tả trong đề bài. Với mỗi phần tử $v$ trong $\textit{nums}$, độ rộng của nó là $w = v \bmod 10$, còn số nhận được sau khi bỏ chữ số cuối là $d = \lfloor v / 10 \rfloor$. Chuyển $d$ thành chuỗi thập phân $s$, giá trị $x$ là số nguyên được tạo bởi $w$ ký tự đầu tiên của $s$, còn $y$ là số nguyên được tạo bởi các ký tự còn lại.

Vì $y$ có thể lớn tới $10^9$, việc nhân lặp lại sẽ quá chậm, nên ta dùng lũy thừa nhanh để tính $x^y \bmod (10^9 + 7)$ trong thời gian $O(\log y)$, sau đó cộng dồn các giá trị sau khi giải mã theo modulo $10^9 + 7$.

Độ phức tạp thời gian là $O(n \times \log M)$, và độ phức tạp không gian là $O(\log M)$. Trong đó, $n$ là độ dài mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sumDecoded(self, nums: List[int]) -> int:
        mod = 10**9 + 7
        ans = 0
        for v in nums:
            d, w = divmod(v, 10)
            s = str(d)
            x = int(s[:w])
            y = int(s[w:])
            ans = (ans + pow(x, y, mod)) % mod
        return ans
```

#### Java

```java
class Solution {
    public int sumDecoded(long[] nums) {
        final long mod = 1000000007L;
        long ans = 0;

        for (long v : nums) {
            long d = v / 10;
            int w = (int) (v % 10);

            String s = Long.toString(d);
            long x = Long.parseLong(s.substring(0, w));
            long y = Long.parseLong(s.substring(w));

            ans = (ans + pow(x, y, mod)) % mod;
        }

        return (int) ans;
    }

    private long pow(long x, long y, long mod) {
        long res = 1;
        while (y > 0) {
            if ((y & 1) != 0) {
                res = res * x % mod;
            }
            x = x * x % mod;
            y >>= 1;
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int sumDecoded(vector<long long>& nums) {
        const long long mod = 1000000007;
        long long ans = 0;

        for (long long v : nums) {
            long long d = v / 10;
            int w = v % 10;

            string s = to_string(d);
            long long x = stoll(s.substr(0, w));
            long long y = stoll(s.substr(w));

            ans = (ans + qpow(x, y, mod)) % mod;
        }

        return ans;
    }

private:
    long long qpow(long long x, long long y, long long mod) {
        long long res = 1;
        while (y) {
            if (y & 1) {
                res = res * x % mod;
            }
            x = x * x % mod;
            y >>= 1;
        }
        return res;
    }
};
```

#### Go

```go
func sumDecoded(nums []int64) int {
	const mod int64 = 1000000007
	var ans int64

	for _, v := range nums {
		d, w := v/10, int(v%10)
		s := strconv.FormatInt(d, 10)

		x, _ := strconv.ParseInt(s[:w], 10, 64)
		y, _ := strconv.ParseInt(s[w:], 10, 64)

		ans = (ans + pow(x, y, mod)) % mod
	}

	return int(ans)
}

func pow(x, y, mod int64) int64 {
	res := int64(1)
	for y > 0 {
		if y&1 != 0 {
			res = res * x % mod
		}
		x = x * x % mod
		y >>= 1
	}
	return res
}
```

#### TypeScript

```ts
function sumDecoded(nums: number[]): number {
    const mod = 1000000007n;
    let ans = 0n;

    for (const v of nums) {
        const d = Math.floor(v / 10);
        const w = v % 10;

        const s = String(d);
        const x = BigInt(s.slice(0, w));
        const y = BigInt(s.slice(w));

        ans = (ans + pow(x, y, mod)) % mod;
    }

    return Number(ans);
}

function pow(x: bigint, y: bigint, mod: bigint): bigint {
    let res = 1n;

    while (y > 0n) {
        if (y & 1n) {
            res = (res * x) % mod;
        }
        x = (x * x) % mod;
        y >>= 1n;
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
