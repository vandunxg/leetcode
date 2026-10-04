---
comments: true
difficulty: Medium
rating: 1848
source: Weekly Contest 401 Q3
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Sorting
---

<!-- problem:start -->

# [3180. Maximum Total Reward Using Operations I](https://leetcode.com/problems/maximum-total-reward-using-operations-i)

[中文文档](/solution/3100-3199/3180.Maximum%20Total%20Reward%20Using%20Operations%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>rewardValues</code> có độ dài <code>n</code>, biểu diễn các giá trị phần thưởng.</p>

<p>Ban đầu, tổng phần thưởng <code>x</code> bằng 0 và tất cả các chỉ số đều <strong>chưa được đánh dấu</strong>. Bạn có thể thực hiện thao tác sau <strong>tùy ý</strong> số lần:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> <strong>chưa được đánh dấu</strong> trong đoạn <code>[0, n - 1]</code>.</li>
	<li>Nếu <code>rewardValues[i]</code> <strong>lớn hơn</strong> tổng phần thưởng hiện tại <code>x</code>, cộng <code>rewardValues[i]</code> vào <code>x</code> (tức là <code>x = x + rewardValues[i]</code>) và <strong>đánh dấu</strong> chỉ số <code>i</code>.</li>
</ul>

<p>Hãy trả về một số nguyên biểu diễn <em>tổng phần thưởng</em> <strong>lớn nhất</strong> có thể thu thập được khi thực hiện các thao tác một cách tối ưu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rewardValues = [1,1,3,3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Trong quá trình thực hiện, ta có thể lần lượt chọn để đánh dấu các chỉ số 0 và 2, khi đó tổng phần thưởng bằng 4, là giá trị lớn nhất.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">rewardValues = [1,6,4,3,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">11</span></p>

<p><strong>Giải thích:</strong></p>

<p>Lần lượt đánh dấu các chỉ số 0, 2 và 1. Khi đó tổng phần thưởng bằng 11, là giá trị lớn nhất.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= rewardValues.length &lt;= 2000</code></li>
	<li><code>1 &lt;= rewardValues[i] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Memoization + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có thể chọn một phần thưởng khi nó lớn hơn nghiêm ngặt tổng hiện tại. Liệt kê mọi tập con có độ phức tạp mũ; với các giới hạn này, việc tìm kiếm theo tổng hiện tại phù hợp hơn.
>
> Những phần thưởng $\le x$ sẽ không bao giờ được chọn. Sau khi sắp xếp, phần thưởng đầu tiên lớn hơn $x$ có thể được tìm bằng tìm kiếm nhị phân.
>
> Dùng memoization cho $dfs(x)$ trên các giá trị $v$ đó và đệ quy đến $x+v$. Các tổng luôn nhỏ hơn khoảng hai lần phần thưởng lớn nhất.

<!-- thinking:end -->

Ta có thể sắp xếp mảng `rewardValues`, sau đó dùng memoization để tìm tổng phần thưởng lớn nhất.

Ta định nghĩa hàm $\textit{dfs}(x)$ biểu diễn tổng phần thưởng lớn nhất có thể đạt được khi tổng hiện tại là $x$. Vì vậy, đáp án là $\textit{dfs}(0)$.

Quá trình thực thi hàm $\textit{dfs}(x)$ như sau:

1. Dùng tìm kiếm nhị phân trên mảng `rewardValues` để tìm chỉ số $i$ của phần tử đầu tiên lớn hơn $x$;
2. Duyệt các phần tử trong mảng `rewardValues` bắt đầu từ chỉ số $i$; với mỗi phần tử $v$, tính giá trị lớn nhất của $v + \textit{dfs}(x + v)$.
3. Trả về kết quả.

Để tránh tính toán lặp lại, ta dùng mảng memoization `f` để ghi lại các kết quả đã được tính.

Độ phức tạp thời gian là $O(n \times (\log n + M))$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng `rewardValues`, còn $M$ là hai lần giá trị lớn nhất trong mảng `rewardValues`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        @cache
        def dfs(x: int) -> int:
            i = bisect_right(rewardValues, x)
            ans = 0
            for v in rewardValues[i:]:
                ans = max(ans, v + dfs(x + v))
            return ans

        rewardValues.sort()
        return dfs(0)
```

#### Java

```java
class Solution {
    private int[] nums;
    private Integer[] f;

    public int maxTotalReward(int[] rewardValues) {
        nums = rewardValues;
        Arrays.sort(nums);
        int n = nums.length;
        f = new Integer[nums[n - 1] << 1];
        return dfs(0);
    }

    private int dfs(int x) {
        if (f[x] != null) {
            return f[x];
        }
        int i = Arrays.binarySearch(nums, x + 1);
        i = i < 0 ? -i - 1 : i;
        int ans = 0;
        for (; i < nums.length; ++i) {
            ans = Math.max(ans, nums[i] + dfs(x + nums[i]));
        }
        return f[x] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTotalReward(vector<int>& rewardValues) {
        sort(rewardValues.begin(), rewardValues.end());
        int n = rewardValues.size();
        int f[rewardValues.back() << 1];
        memset(f, -1, sizeof(f));
        function<int(int)> dfs = [&](int x) {
            if (f[x] != -1) {
                return f[x];
            }
            auto it = upper_bound(rewardValues.begin(), rewardValues.end(), x);
            int ans = 0;
            for (; it != rewardValues.end(); ++it) {
                ans = max(ans, rewardValues[it - rewardValues.begin()] + dfs(x + *it));
            }
            return f[x] = ans;
        };
        return dfs(0);
    }
};
```

#### Go

```go
func maxTotalReward(rewardValues []int) int {
	sort.Ints(rewardValues)
	n := len(rewardValues)
	f := make([]int, rewardValues[n-1]<<1)
	for i := range f {
		f[i] = -1
	}
	var dfs func(int) int
	dfs = func(x int) int {
		if f[x] != -1 {
			return f[x]
		}
		i := sort.SearchInts(rewardValues, x+1)
		f[x] = 0
		for _, v := range rewardValues[i:] {
			f[x] = max(f[x], v+dfs(x+v))
		}
		return f[x]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function maxTotalReward(rewardValues: number[]): number {
    rewardValues.sort((a, b) => a - b);
    const search = (x: number): number => {
        let [l, r] = [0, rewardValues.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (rewardValues[mid] > x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const f: number[] = Array(rewardValues.at(-1)! << 1).fill(-1);
    const dfs = (x: number): number => {
        if (f[x] !== -1) {
            return f[x];
        }
        let ans = 0;
        for (let i = search(x); i < rewardValues.length; ++i) {
            ans = Math.max(ans, rewardValues[i] + dfs(x + rewardValues[i]));
        }
        return (f[x] = ans);
    };
    return dfs(0);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Quy hoạch động

<!-- thinking:start -->

> **Tư duy**
>
> Việc tìm kiếm vẫn phải trả giá cho đệ quy. Khả năng đạt được tổng $j$ chỉ phụ thuộc vào những phần thưởng đã sử dụng.
>
> Để chọn $v$, tổng trước đó phải thỏa mãn điều kiện $<v$, nên $f[j]$ trở thành true từ $f[j-v]$ khi $0\le j-v<v$.
>
> Sau khi sắp xếp và loại bỏ các giá trị trùng nhau, dùng một mảng Boolean bắt đầu với $f[0]=\mathrm{True}$. Chỉ số lớn nhất có giá trị true là đáp án.

<!-- thinking:end -->

Ta định nghĩa $f[i][j]$ cho biết có thể đạt được tổng phần thưởng bằng $j$ khi sử dụng $i$ giá trị phần thưởng đầu tiên hay không. Ban đầu, $f[0][0] = \textit{True}$ và tất cả các giá trị khác là $\textit{False}$.

Xét giá trị phần thưởng thứ $i$ là $v$. Nếu không chọn nó, thì $f[i][j] = f[i - 1][j]$. Nếu chọn nó, thì $f[i][j] = f[i - 1][j - v]$, trong đó $0 \leq j - v < v$. Do đó, công thức chuyển trạng thái là:

$$
f[i][j] = f[i - 1][j] \vee f[i - 1][j - v]
$$

Đáp án cuối cùng là $\max\{j \mid f[n][j] = \textit{True}\}$.

Vì $f[i][j]$ chỉ phụ thuộc vào $f[i - 1][j]$ và $f[i - 1][j - v]$, ta có thể loại bỏ chiều đầu tiên và chỉ dùng mảng một chiều để chuyển trạng thái.

Độ phức tạp thời gian là $O(n \times M)$, và độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng `rewardValues`, còn $M$ là hai lần giá trị lớn nhất trong mảng `rewardValues`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        nums = sorted(set(rewardValues))
        m = nums[-1] << 1
        f = [False] * m
        f[0] = True
        for v in nums:
            for j in range(m):
                if 0 <= j - v < v:
                    f[j] |= f[j - v]
        ans = m - 1
        while not f[ans]:
            ans -= 1
        return ans
```

#### Java

```java
class Solution {
    public int maxTotalReward(int[] rewardValues) {
        int[] nums = Arrays.stream(rewardValues).distinct().sorted().toArray();
        int n = nums.length;
        int m = nums[n - 1] << 1;
        boolean[] f = new boolean[m];
        f[0] = true;
        for (int v : nums) {
            for (int j = 0; j < m; ++j) {
                if (0 <= j - v && j - v < v) {
                    f[j] |= f[j - v];
                }
            }
        }
        int ans = m - 1;
        while (!f[ans]) {
            --ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTotalReward(vector<int>& rewardValues) {
        sort(rewardValues.begin(), rewardValues.end());
        rewardValues.erase(unique(rewardValues.begin(), rewardValues.end()), rewardValues.end());
        int n = rewardValues.size();
        int m = rewardValues.back() << 1;
        bool f[m];
        memset(f, false, sizeof(f));
        f[0] = true;
        for (int v : rewardValues) {
            for (int j = 1; j < m; ++j) {
                if (0 <= j - v && j - v < v) {
                    f[j] = f[j] || f[j - v];
                }
            }
        }
        int ans = m - 1;
        while (!f[ans]) {
            --ans;
        }
        return ans;
    }
};
```

#### Go

```go
func maxTotalReward(rewardValues []int) int {
	slices.Sort(rewardValues)
	nums := slices.Compact(rewardValues)
	n := len(nums)
	m := nums[n-1] << 1
	f := make([]bool, m)
	f[0] = true
	for _, v := range nums {
		for j := 1; j < m; j++ {
			if 0 <= j-v && j-v < v {
				f[j] = f[j] || f[j-v]
			}
		}
	}
	ans := m - 1
	for !f[ans] {
		ans--
	}
	return ans
}
```

#### TypeScript

```ts
function maxTotalReward(rewardValues: number[]): number {
    const nums = Array.from(new Set(rewardValues)).sort((a, b) => a - b);
    const n = nums.length;
    const m = nums[n - 1] << 1;
    const f: boolean[] = Array(m).fill(false);
    f[0] = true;
    for (const v of nums) {
        for (let j = 1; j < m; ++j) {
            if (0 <= j - v && j - v < v) {
                f[j] = f[j] || f[j - v];
            }
        }
    }
    let ans = m - 1;
    while (!f[ans]) {
        --ans;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Quy hoạch động + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 2 duyệt mọi $j$ trong $O(nM)$. Cùng một cập nhật có thể viết là “OR $v$ bit thấp của $f$ sau khi dịch trái $v$ bit”.
>
> Lưu khả năng đạt được dưới dạng các bit của một số nguyên.
>
> Với mỗi $v$, thực hiện $f\mathrel{|}=(f\bmod 2^v)\ll v$. Bit 1 cao nhất là tổng lớn nhất, còn xử lý song song theo word giúp giảm hệ số $w$.

<!-- thinking:end -->

Ta có thể tối ưu Lời giải 2 bằng cách định nghĩa một số nhị phân $f$ để lưu trạng thái hiện tại, trong đó bit thứ $i$ của $f$ bằng $1$ cho biết có thể đạt được tổng phần thưởng bằng $i$.

Dựa trên công thức chuyển trạng thái của Lời giải 2, $f[j] = f[j] \vee f[j - v]$ tương đương với việc lấy $v$ bit thấp của $f$, dịch trái chúng $v$ bit, sau đó thực hiện phép OR với $f$ ban đầu.

Vì vậy, đáp án là vị trí của bit cao nhất trong $f$.

Độ phức tạp thời gian là $O(n \times M / w)$, và độ phức tạp không gian là $O(n + M / w)$. Trong đó, $n$ là độ dài của mảng `rewardValues`, $M$ là hai lần giá trị lớn nhất trong mảng `rewardValues`, và số nguyên $w = 32$ hoặc $64$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxTotalReward(self, rewardValues: List[int]) -> int:
        nums = sorted(set(rewardValues))
        f = 1
        for v in nums:
            f |= (f & ((1 << v) - 1)) << v
        return f.bit_length() - 1
```

#### Java

```java
import java.math.BigInteger;

class Solution {
    public int maxTotalReward(int[] rewardValues) {
        int[] nums = Arrays.stream(rewardValues).distinct().sorted().toArray();
        BigInteger f = BigInteger.ONE;
        for (int v : nums) {
            BigInteger mask = BigInteger.ONE.shiftLeft(v).subtract(BigInteger.ONE);
            BigInteger shifted = f.and(mask).shiftLeft(v);
            f = f.or(shifted);
        }
        return f.bitLength() - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxTotalReward(vector<int>& rewardValues) {
        sort(rewardValues.begin(), rewardValues.end());
        rewardValues.erase(unique(rewardValues.begin(), rewardValues.end()), rewardValues.end());
        bitset<100000> f{1};
        for (int v : rewardValues) {
            int shift = f.size() - v;
            f |= f << shift >> (shift - v);
        }
        for (int i = rewardValues.back() * 2 - 1;; i--) {
            if (f.test(i)) {
                return i;
            }
        }
    }
};
```

#### Go

```go
func maxTotalReward(rewardValues []int) int {
	slices.Sort(rewardValues)
	rewardValues = slices.Compact(rewardValues)
	one := big.NewInt(1)
	f := big.NewInt(1)
	p := new(big.Int)
	for _, v := range rewardValues {
		mask := p.Sub(p.Lsh(one, uint(v)), one)
		f.Or(f, p.Lsh(p.And(f, mask), uint(v)))
	}
	return f.BitLen() - 1
}
```

#### TypeScript

```ts
function maxTotalReward(rewardValues: number[]): number {
    rewardValues.sort((a, b) => a - b);
    rewardValues = [...new Set(rewardValues)];
    let f = 1n;
    for (const x of rewardValues) {
        const mask = (1n << BigInt(x)) - 1n;
        f = f | ((f & mask) << BigInt(x));
    }
    return f.toString(2).length - 1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
