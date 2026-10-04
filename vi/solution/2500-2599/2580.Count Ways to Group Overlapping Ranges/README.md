---
comments: true
difficulty: Medium
rating: 1631
source: Biweekly Contest 99 Q3
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [2580. Count Ways to Group Overlapping Ranges](https://leetcode.com/problems/count-ways-to-group-overlapping-ranges)

[Tài liệu tiếng Trung](/solution/2500-2599/2580.Count%20Ways%20to%20Group%20Overlapping%20Ranges/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên 2D <code>ranges</code>, trong đó <code>ranges[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> biểu thị rằng mọi số nguyên giữa <code>start<sub>i</sub></code> và <code>end<sub>i</sub></code> (cả hai đầu đều <strong>được tính</strong>) đều thuộc range thứ <code>i<sup>th</sup></code>.</p>

<p>Hãy chia <code>ranges</code> thành <strong>hai</strong> nhóm (có thể rỗng) sao cho:</p>

<ul>
	<li>Mỗi range thuộc đúng một nhóm.</li>
	<li>Mọi hai range <strong>chồng lấp</strong> phải thuộc <strong>cùng</strong> một nhóm.</li>
</ul>

<p>Hai range được gọi là <strong>chồng lấp</strong>&nbsp;nếu tồn tại ít nhất <strong>một</strong> số nguyên xuất hiện trong cả hai range.</p>

<ul>
	<li>Ví dụ, <code>[1, 3]</code> và <code>[2, 5]</code> chồng lấp vì <code>2</code> và <code>3</code> xuất hiện trong cả hai range.</li>
</ul>

<p>Trả về <em><strong>tổng số</strong> cách chia</em> <code>ranges</code> <em>thành hai nhóm</em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranges = [[6,10],[5,15]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong>
Hai range chồng lấp, nên chúng phải nằm trong cùng một nhóm.
Do đó, có hai cách:
- Đặt cả hai range vào nhóm 1.
- Đặt cả hai range vào nhóm 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> ranges = [[1,3],[10,20],[2,5],[4,8]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Các range [1,3] và [2,5] chồng lấp. Vì vậy, chúng phải nằm trong cùng một nhóm.
Tương tự, các range [2,5] và [4,8] cũng chồng lấp. Vì vậy, chúng cũng phải nằm trong cùng một nhóm.
Do đó, có bốn cách nhóm chúng:
- Tất cả các range nằm trong nhóm 1.
- Tất cả các range nằm trong nhóm 2.
- Các range [1,3], [2,5] và [4,8] nằm trong nhóm 1, còn [10,20] nằm trong nhóm 2.
- Các range [1,3], [2,5] và [4,8] nằm trong nhóm 2, còn [10,20] nằm trong nhóm 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= ranges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>ranges[i].length == 2</code></li>
	<li><code>0 &lt;= start<sub>i</sub> &lt;= end<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Đếm + Lũy thừa nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Các range chồng lấp phải thuộc cùng một nhóm; ta cần đếm số cách chia chúng thành hai nhóm. Có $2^n$ cách gán là không khả thi khi $n\le 10^5$.
>
> Tính chồng lấp có tính bắc cầu: sau khi sắp xếp và gộp, mỗi thành phần liên thông phải được đưa hoàn toàn vào một nhóm. Các thành phần độc lập với nhau, nên số cách là $2^{\textit{cnt}}$ modulo $10^9+7$.

<!-- thinking:end -->

Trước tiên, ta có thể sắp xếp các đoạn trong range, gộp các đoạn chồng lấp và đếm số đoạn không chồng lấp, ký hiệu là $cnt$.

Mỗi đoạn không chồng lấp có thể được chọn để đưa vào nhóm thứ nhất hoặc nhóm thứ hai, nên số phương án là $2^{cnt}$. Lưu ý rằng $2^{cnt}$ có thể rất lớn, vì vậy ta cần lấy modulo $10^9 + 7$. Ở đây, ta có thể dùng lũy thừa nhanh để giải bài toán này.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là số đoạn.

Ngoài ra, ta cũng có thể không dùng lũy thừa nhanh. Khi tìm thấy một đoạn mới không chồng lấp, ta nhân số phương án với 2 và lấy modulo $10^9 + 7$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWays(self, ranges: List[List[int]]) -> int:
        ranges.sort()
        cnt, mx = 0, -1
        for start, end in ranges:
            if start > mx:
                cnt += 1
            mx = max(mx, end)
        mod = 10**9 + 7
        return pow(2, cnt, mod)
```

#### Java

```java
class Solution {
    public int countWays(int[][] ranges) {
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int cnt = 0, mx = -1;
        for (int[] e : ranges) {
            if (e[0] > mx) {
                ++cnt;
            }
            mx = Math.max(mx, e[1]);
        }
        return qpow(2, cnt, (int) 1e9 + 7);
    }

    private int qpow(long a, int n, int mod) {
        long ans = 1;
        for (; n > 0; n >>= 1) {
            if ((n & 1) == 1) {
                ans = ans * a % mod;
            }
            a = a * a % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countWays(vector<vector<int>>& ranges) {
        sort(ranges.begin(), ranges.end());
        int cnt = 0, mx = -1;
        for (auto& e : ranges) {
            cnt += e[0] > mx;
            mx = max(mx, e[1]);
        }
        using ll = long long;
        auto qpow = [&](ll a, int n, int mod) {
            ll ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        return qpow(2, cnt, 1e9 + 7);
    }
};
```

#### Go

```go
func countWays(ranges [][]int) int {
	sort.Slice(ranges, func(i, j int) bool { return ranges[i][0] < ranges[j][0] })
	cnt, mx := 0, -1
	for _, e := range ranges {
		if e[0] > mx {
			cnt++
		}
		if mx < e[1] {
			mx = e[1]
		}
	}
	qpow := func(a, n, mod int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	return qpow(2, cnt, 1e9+7)
}
```

#### TypeScript

```ts
function countWays(ranges: number[][]): number {
    ranges.sort((a, b) => a[0] - b[0]);
    let mx = -1;
    let ans = 1;
    const mod = 10 ** 9 + 7;
    for (const [start, end] of ranges) {
        if (start > mx) {
            ans = (ans * 2) % mod;
        }
        mx = Math.max(mx, end);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 đếm số thành phần rồi tính lũy thừa. Nhân đáp án với $2$ mỗi khi một thành phần mới bắt đầu giúp bỏ qua bước lũy thừa riêng biệt, trong khi vẫn dùng cách gộp tương tự.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countWays(self, ranges: List[List[int]]) -> int:
        ranges.sort()
        mx = -1
        mod = 10**9 + 7
        ans = 1
        for start, end in ranges:
            if start > mx:
                ans = ans * 2 % mod
            mx = max(mx, end)
        return ans
```

#### Java

```java
class Solution {
    public int countWays(int[][] ranges) {
        Arrays.sort(ranges, (a, b) -> a[0] - b[0]);
        int mx = -1;
        int ans = 1;
        final int mod = (int) 1e9 + 7;
        for (int[] e : ranges) {
            if (e[0] > mx) {
                ans = ans * 2 % mod;
            }
            mx = Math.max(mx, e[1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countWays(vector<vector<int>>& ranges) {
        sort(ranges.begin(), ranges.end());
        int ans = 1, mx = -1;
        const int mod = 1e9 + 7;
        for (auto& e : ranges) {
            if (e[0] > mx) {
                ans = ans * 2 % mod;
            }
            mx = max(mx, e[1]);
        }
        return ans;
    }
};
```

#### Go

```go
func countWays(ranges [][]int) int {
	sort.Slice(ranges, func(i, j int) bool { return ranges[i][0] < ranges[j][0] })
	ans, mx := 1, -1
	const mod = 1e9 + 7
	for _, e := range ranges {
		if e[0] > mx {
			ans = ans * 2 % mod
		}
		if mx < e[1] {
			mx = e[1]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
