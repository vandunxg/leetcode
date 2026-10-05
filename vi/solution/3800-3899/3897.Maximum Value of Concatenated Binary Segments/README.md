---
comments: true
difficulty: Hard
rating: 1998
source: Biweekly Contest 180 Q4
tags:
    - Greedy
    - Array
    - Sorting
---

<!-- problem:start -->

# [3897. Maximum Value of Concatenated Binary Segments](https://leetcode.com/problems/maximum-value-of-concatenated-binary-segments)

[中文文档](/solution/3800-3899/3897.Maximum%20Value%20of%20Concatenated%20Binary%20Segments/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng số nguyên <code>nums1</code> và <code>nums0</code>, mỗi mảng có kích thước <code>n</code>.</p>

<ul>
	<li><code>nums1[i]</code> biểu thị số lượng ký tự <code>&#39;1&#39;</code> trong đoạn thứ <code>i<sup>th</sup></code>.</li>
	<li><code>nums0[i]</code> biểu thị số lượng ký tự <code>&#39;0&#39;</code> trong đoạn thứ <code>i<sup>th</sup></code>.</li>
</ul>

<p>Với mỗi chỉ số <code>i</code>, hãy tạo một đoạn nhị phân gồm:</p>

<ul>
	<li><code>nums1[i]</code> lần xuất hiện của <code>&#39;1&#39;</code>, theo sau bởi</li>
	<li><code>nums0[i]</code> lần xuất hiện của <code>&#39;0&#39;</code>.</li>
</ul>

<p>Bạn có thể <strong>sắp xếp lại</strong> thứ tự của các <strong>đoạn</strong> này theo bất kỳ cách nào. Sau đó, <strong>nối</strong> tất cả các đoạn để tạo thành một chuỗi nhị phân duy nhất.</p>

<p>Trả về giá trị số nguyên <strong>lớn nhất</strong> có thể có của chuỗi nhị phân sau khi nối.</p>

<p>Vì kết quả có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [1,2], nums0 = [1,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số 0, <code>nums1[0] = 1</code> và <code>nums0[0] = 1</code>, nên đoạn được tạo là <code>&quot;10&quot;</code>.</li>
	<li>Tại chỉ số 1, <code>nums1[1] = 2</code> và <code>nums0[1] = 0</code>, nên đoạn được tạo là <code>&quot;11&quot;</code>.</li>
	<li>Sắp xếp lại các đoạn thành <code>&quot;11&quot;</code> đứng trước <code>&quot;10&quot;</code> sẽ tạo ra chuỗi nhị phân <code>&quot;1110&quot;</code>.</li>
	<li>Số nhị phân <code>&quot;1110&quot;</code> có giá trị 14, là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums1 = [3,1], nums0 = [0,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">120</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tại chỉ số 0, <code>nums1[0] = 3</code> và <code>nums0[0] = 0</code>, nên đoạn được tạo là <code>&quot;111&quot;</code>.</li>
	<li>Tại chỉ số 1, <code>nums1[1] = 1</code> và <code>nums0[1] = 3</code>, nên đoạn được tạo là <code>&quot;1000&quot;</code>.</li>
	<li>Sắp xếp lại các đoạn thành <code>&quot;111&quot;</code> đứng trước <code>&quot;1000&quot;</code> sẽ tạo ra chuỗi nhị phân <code>&quot;1111000&quot;</code>.</li>
	<li>Số nhị phân <code>&quot;1111000&quot;</code> có giá trị 120, là giá trị lớn nhất có thể.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n == nums1.length == nums0.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums0[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>nums1[i] + nums0[i] &gt; 0</code></li>
	<li>Tổng tất cả phần tử trong <code>nums1</code> và <code>nums0</code> không vượt quá 2 * 10<sup>5</sup>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi đoạn gồm một số lượng $1$s đứng trước các $0$s. Ta có thể sắp xếp lại các đoạn để tối đa hóa số nhị phân được nối. Tổng độ dài $\le 2 \times 10^5$.
>
> So sánh nhị phân là so sánh từ điển. Thứ tự của $A$ và $B$ được chọn dựa trên chuỗi nào giữa $AB$ và $BA$ tốt hơn.
>
> Các đoạn chỉ gồm $1$ đứng trước (để có nhiều $1$ hơn ở vị trí sớm hơn); các đoạn hỗn hợp được sắp xếp theo số lượng $1$ giảm dần rồi đến số lượng $0$ tăng dần; các đoạn chỉ gồm $0$ đứng cuối.
>
> Sau khi sắp xếp, cộng các lũy thừa của hai đã tính trước cho mỗi $1$; không cần tạo chuỗi bit.

<!-- thinking:end -->

Gọi chuỗi nhị phân tương ứng với đoạn thứ $i$ là $1^{x_i}0^{y_i}$, trong đó $x_i = \textit{nums1}[i]$ và $y_i = \textit{nums0}[i]$.

Đề bài cho phép chúng ta sắp xếp lại các đoạn này theo bất kỳ thứ tự nào, với mục tiêu tối đa hóa giá trị số nguyên được biểu diễn bởi chuỗi nhị phân cuối cùng. Vì so sánh các chuỗi nhị phân theo giá trị về cơ bản tương đương với so sánh từ điển, chúng ta muốn càng nhiều `1` càng xuất hiện sớm càng tốt.

Xét thứ tự tương đối của hai đoạn $A = 1^a0^b$ và $B = 1^c0^d$. Nếu nối chúng theo thứ tự $AB$ hoặc $BA$, rõ ràng ta nên chọn thứ tự có kết quả lớn hơn theo thứ tự từ điển.

Dựa trên quy tắc này, ta có thể suy ra chiến lược sắp xếp sau:

- Nếu một đoạn thỏa mãn $y = 0$, đoạn đó chỉ gồm một số lượng `1`. Các đoạn như vậy nên được đặt càng sớm càng tốt vì chúng không đưa `0` vào quá sớm. Trong số các đoạn này, đoạn có nhiều `1` hơn nên đứng trước.
- Nếu hai đoạn đều thỏa mãn $x > 0$ và $y > 0$, đoạn có nhiều `1` ở đầu hơn nên đứng trước, vì vậy ta sắp xếp theo $x$ giảm dần. Nếu $x$ bằng nhau, đoạn có ít `0` hơn nên đứng trước, vì vậy ta sắp xếp theo $y$ tăng dần.
- Nếu một đoạn thỏa mãn $x = 0$, đoạn đó chỉ gồm một số lượng `0`. Các đoạn như vậy nên được đặt ở cuối.

Sau khi sắp xếp theo cách này, chuỗi nhị phân được nối sẽ đạt giá trị lớn nhất.

Tiếp theo, ta không cần thực sự tạo toàn bộ chuỗi nhị phân. Gọi tổng độ dài của tất cả các đoạn sau khi nối là $m$. Ta tính trước $2^0, 2^1, \dots, 2^{m-1}$ theo modulo $10^9 + 7$. Sau đó, ta duyệt các đoạn theo thứ tự đã sắp xếp:

- Khi gặp một `1`, ta cộng trọng số của bit cao nhất hiện tại vào đáp án.
- Khi gặp một `0`, ta chỉ cần lùi vị trí hiện tại lại.

Cuối cùng, ta thu được đáp án.

Độ phức tạp thời gian là $O(n \log n + m)$, còn độ phức tạp không gian là $O(n + m)$. Ở đây, $n$ là số đoạn và $m = \sum \textit{nums1}[i] + \sum \textit{nums0}[i]$. Đề bài đảm bảo rằng $m \le 2 \times 10^5$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValue(self, nums1: list[int], nums0: list[int]) -> int:
        MOD = 10**9 + 7
        pairs = list(zip(nums1, nums0))
        b = sum(x + y for x, y in pairs)

        def key(p: tuple[int, int]) -> tuple[int, int, int]:
            x, y = p
            if y == 0:
                return (0, -x, 0)
            if x > 0:
                return (1, -x, y)
            return (2, y, 0)

        pairs.sort(key=key)

        ans = 0
        p = [1] * b
        for i in range(1, b):
            p[i] = p[i - 1] * 2 % MOD

        b -= 1
        for cnt1, cnt0 in pairs:
            while cnt1:
                ans = (ans + p[b]) % MOD
                b -= 1
                cnt1 -= 1
            b -= cnt0
        return ans
```

#### Java

```java
class Solution {
    private static final int MOD = 1_000_000_007;

    public int maxValue(int[] nums1, int[] nums0) {
        int n = nums1.length;
        int[][] pairs = new int[n][2];
        int b = 0;
        for (int i = 0; i < n; ++i) {
            pairs[i][0] = nums1[i];
            pairs[i][1] = nums0[i];
            b += nums1[i] + nums0[i];
        }

        Arrays.sort(pairs, (a, c) -> {
            int x1 = a[0], y1 = a[1];
            int x2 = c[0], y2 = c[1];
            int g1 = y1 == 0 ? 0 : x1 > 0 ? 1 : 2;
            int g2 = y2 == 0 ? 0 : x2 > 0 ? 1 : 2;
            if (g1 != g2) {
                return Integer.compare(g1, g2);
            }
            if (g1 == 0) {
                return Integer.compare(x2, x1);
            }
            if (g1 == 1) {
                if (x1 != x2) {
                    return Integer.compare(x2, x1);
                }
                return Integer.compare(y1, y2);
            }
            return Integer.compare(y1, y2);
        });

        long[] p = new long[b];
        p[0] = 1;
        for (int i = 1; i < b; ++i) {
            p[i] = p[i - 1] * 2 % MOD;
        }

        long ans = 0;
        --b;
        for (int[] pair : pairs) {
            int cnt1 = pair[0], cnt0 = pair[1];
            while (cnt1-- > 0) {
                ans = (ans + p[b--]) % MOD;
            }
            b -= cnt0;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    static constexpr int MOD = 1'000'000'007;

    int maxValue(vector<int>& nums1, vector<int>& nums0) {
        vector<pair<int, int>> pairs;
        int b = 0;
        for (int i = 0; i < nums1.size(); ++i) {
            pairs.emplace_back(nums1[i], nums0[i]);
            b += nums1[i] + nums0[i];
        }

        sort(pairs.begin(), pairs.end(), [](const auto& a, const auto& b) {
            auto group = [](const pair<int, int>& p) {
                if (p.second == 0) {
                    return 0;
                }
                if (p.first > 0) {
                    return 1;
                }
                return 2;
            };

            int g1 = group(a), g2 = group(b);
            if (g1 != g2) {
                return g1 < g2;
            }
            if (g1 == 0) {
                return a.first > b.first;
            }
            if (g1 == 1) {
                if (a.first != b.first) {
                    return a.first > b.first;
                }
                return a.second < b.second;
            }
            return a.second < b.second;
        });

        vector<long long> p(b, 1);
        for (int i = 1; i < b; ++i) {
            p[i] = p[i - 1] * 2 % MOD;
        }

        long long ans = 0;
        --b;
        for (auto& [cnt1, cnt0] : pairs) {
            while (cnt1--) {
                ans = (ans + p[b--]) % MOD;
            }
            b -= cnt0;
        }
        return (int) ans;
    }
};
```

#### Go

```go
const MOD int = 1_000_000_007

func maxValue(nums1 []int, nums0 []int) int {
	type pair struct{ x, y int }

	pairs := make([]pair, len(nums1))
	b := 0
	for i := range nums1 {
		pairs[i] = pair{nums1[i], nums0[i]}
		b += nums1[i] + nums0[i]
	}

	group := func(p pair) int {
		if p.y == 0 {
			return 0
		}
		if p.x > 0 {
			return 1
		}
		return 2
	}

	sort.Slice(pairs, func(i, j int) bool {
		a, b := pairs[i], pairs[j]
		g1, g2 := group(a), group(b)
		if g1 != g2 {
			return g1 < g2
		}
		if g1 == 0 {
			return a.x > b.x
		}
		if g1 == 1 {
			if a.x != b.x {
				return a.x > b.x
			}
			return a.y < b.y
		}
		return a.y < b.y
	})

	p := make([]int, b)
	p[0] = 1
	for i := 1; i < b; i++ {
		p[i] = p[i-1] * 2 % MOD
	}

	ans := 0
	b--
	for _, pr := range pairs {
		cnt1, cnt0 := pr.x, pr.y
		for cnt1 > 0 {
			ans = (ans + p[b]) % MOD
			b--
			cnt1--
		}
		b -= cnt0
	}
	return ans
}
```

#### TypeScript

```ts
function maxValue(nums1: number[], nums0: number[]): number {
    const MOD = 1_000_000_007;
    const pairs: [number, number][] = [];
    let b = 0;

    for (let i = 0; i < nums1.length; ++i) {
        pairs.push([nums1[i], nums0[i]]);
        b += nums1[i] + nums0[i];
    }

    const group = ([x, y]: [number, number]): number => {
        if (y === 0) {
            return 0;
        }
        if (x > 0) {
            return 1;
        }
        return 2;
    };

    pairs.sort((a, c) => {
        const g1 = group(a);
        const g2 = group(c);
        if (g1 !== g2) {
            return g1 - g2;
        }
        if (g1 === 0) {
            return c[0] - a[0];
        }
        if (g1 === 1) {
            if (a[0] !== c[0]) {
                return c[0] - a[0];
            }
            return a[1] - c[1];
        }
        return a[1] - c[1];
    });

    const p = Array<number>(b).fill(1);
    for (let i = 1; i < b; ++i) {
        p[i] = (p[i - 1] * 2) % MOD;
    }

    let ans = 0;
    --b;
    for (let [cnt1, cnt0] of pairs) {
        while (cnt1 > 0) {
            ans = (ans + p[b]) % MOD;
            --b;
            --cnt1;
        }
        b -= cnt0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
