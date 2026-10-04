---
comments: true
difficulty: Hard
rating: 2396
source: Weekly Contest 358 Q4
tags:
    - Stack
    - Greedy
    - Array
    - Math
    - Number Theory
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [2818. Apply Operations to Maximize Score](https://leetcode.com/problems/apply-operations-to-maximize-score)

[中文文档](/solution/2800-2899/2818.Apply%20Operations%20to%20Maximize%20Score/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm <code>n</code> số nguyên dương và một số nguyên <code>k</code>.</p>

<p>Ban đầu, điểm của bạn là <code>1</code>. Hãy tối đa hóa điểm bằng cách thực hiện thao tác sau nhiều nhất <code>k</code> lần:</p>

<ul>
	<li>Chọn một mảng con <strong>không rỗng</strong> bất kỳ <code>nums[l, ..., r]</code> mà trước đó bạn chưa chọn.</li>
	<li>Chọn một phần tử <code>x</code> trong <code>nums[l, ..., r]</code> có <strong>điểm nguyên tố</strong> cao nhất. Nếu có nhiều phần tử như vậy, chọn phần tử có chỉ số nhỏ nhất.</li>
	<li>Nhân điểm của bạn với <code>x</code>.</li>
</ul>

<p>Ở đây, <code>nums[l, ..., r]</code> là mảng con của <code>nums</code> bắt đầu tại chỉ số <code>l</code> và kết thúc tại chỉ số <code>r</code>, bao gồm cả hai đầu mút.</p>

<p><strong>Điểm nguyên tố</strong> của một số nguyên <code>x</code> bằng số lượng thừa số nguyên tố phân biệt của <code>x</code>. Ví dụ, điểm nguyên tố của <code>300</code> là <code>3</code> vì <code>300 = 2 * 2 * 3 * 5 * 5</code>.</p>

<p>Hãy trả về <em><strong>điểm lớn nhất có thể đạt được</strong> sau khi thực hiện nhiều nhất </em><code>k</code><em> thao tác</em>.</p>

<p>Vì đáp án có thể rất lớn, hãy trả về đáp án modulo <code>10<sup>9 </sup>+ 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [8,3,9,3,8], k = 2
<strong>Đầu ra:</strong> 81
<strong>Giải thích:</strong> Để đạt được điểm 81, ta có thể thực hiện các thao tác sau:
- Chọn mảng con nums[2, ..., 2]. nums[2] là phần tử duy nhất trong mảng con này. Vì vậy, ta nhân điểm với nums[2]. Điểm trở thành 1 * 9 = 9.
- Chọn mảng con nums[2, ..., 3]. nums[2] và nums[3] đều có điểm nguyên tố bằng 1, nhưng nums[2] có chỉ số nhỏ hơn. Vì vậy, ta nhân điểm với nums[2]. Điểm trở thành 9 * 9 = 81.
Có thể chứng minh rằng 81 là điểm cao nhất có thể đạt được.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [19,12,14,6,10,18], k = 3
<strong>Đầu ra:</strong> 4788
<strong>Giải thích:</strong> Để đạt được điểm 4788, ta có thể thực hiện các thao tác sau:
- Chọn mảng con nums[0, ..., 0]. nums[0] là phần tử duy nhất trong mảng con này. Vì vậy, ta nhân điểm với nums[0]. Điểm trở thành 1 * 19 = 19.
- Chọn mảng con nums[5, ..., 5]. nums[5] là phần tử duy nhất trong mảng con này. Vì vậy, ta nhân điểm với nums[5]. Điểm trở thành 19 * 18 = 342.
- Chọn mảng con nums[2, ..., 3]. nums[2] và nums[3] đều có điểm nguyên tố bằng 2, nhưng nums[2] có chỉ số nhỏ hơn. Vì vậy, ta nhân điểm với nums[2]. Điểm trở thành 342 * 14 = 4788.
Có thể chứng minh rằng 4788 là điểm cao nhất có thể đạt được.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length == n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= min(n * (n + 1) / 2, 10<sup>9</sup>)</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ngăn xếp đơn điệu + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Điểm nguyên tố của một mảng con là điểm của phần tử có điểm lớn nhất nằm ngoài cùng bên trái. Ngăn xếp đơn điệu cho biết đoạn mà $nums[i]$ là phần tử có điểm lớn nhất, đóng góp $(i-l)\times(r-i)$ thao tác. Với nhiều nhất $k$ thao tác, ta lần lượt nâng các giá trị lớn nhất lên các lũy thừa tương ứng cho đến khi dùng hết $k$.

<!-- thinking:end -->

Ta dễ dàng nhận thấy số mảng con mà phần tử có điểm nguyên tố cao nhất là $nums[i]$ là $cnt = (i - l) \times (r - i)$, trong đó $l$ là chỉ số nhỏ nhất sao cho $primeScore(nums[l]) \ge primeScore(nums[i])$, còn $r$ là chỉ số lớn nhất sao cho $primeScore(nums[r]) \ge primeScore(nums[i])$.

Vì ta được phép thực hiện nhiều nhất $k$ thao tác, ta có thể lần lượt xét các phần tử $nums[i]$ từ lớn đến nhỏ và tính $cnt$ của từng phần tử. Nếu $cnt \le k$, phần đóng góp của $nums[i]$ vào đáp án là $nums[i]^{cnt}$, sau đó cập nhật $k = k - cnt$. Nếu $cnt \gt k$, phần đóng góp của $nums[i]$ vào đáp án là $nums[i]^{k}$ và ta kết thúc vòng lặp.

Trả về đáp án sau vòng lặp. Lưu ý rằng số mũ có thể lớn, nên ta cần dùng phép lũy thừa nhanh.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
def primeFactors(n):
    i = 2
    ans = set()
    while i * i <= n:
        while n % i == 0:
            ans.add(i)
            n //= i
        i += 1
    if n > 1:
        ans.add(n)
    return len(ans)


class Solution:
    def maximumScore(self, nums: List[int], k: int) -> int:
        mod = 10**9 + 7
        arr = [(i, primeFactors(x), x) for i, x in enumerate(nums)]
        n = len(nums)

        left = [-1] * n
        right = [n] * n
        stk = []
        for i, f, x in arr:
            while stk and stk[-1][0] < f:
                stk.pop()
            if stk:
                left[i] = stk[-1][1]
            stk.append((f, i))

        stk = []
        for i, f, x in arr[::-1]:
            while stk and stk[-1][0] <= f:
                stk.pop()
            if stk:
                right[i] = stk[-1][1]
            stk.append((f, i))

        arr.sort(key=lambda x: -x[2])
        ans = 1
        for i, f, x in arr:
            l, r = left[i], right[i]
            cnt = (i - l) * (r - i)
            if cnt <= k:
                ans = ans * pow(x, cnt, mod) % mod
                k -= cnt
            else:
                ans = ans * pow(x, k, mod) % mod
                break
        return ans
```

#### Java

```java
class Solution {
    private final int mod = (int) 1e9 + 7;

    public int maximumScore(List<Integer> nums, int k) {
        int n = nums.size();
        int[][] arr = new int[n][0];
        for (int i = 0; i < n; ++i) {
            arr[i] = new int[] {i, primeFactors(nums.get(i)), nums.get(i)};
        }
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int[] e : arr) {
            int i = e[0], f = e[1];
            while (!stk.isEmpty() && arr[stk.peek()][1] < f) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            int f = arr[i][1];
            while (!stk.isEmpty() && arr[stk.peek()][1] <= f) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        Arrays.sort(arr, (a, b) -> b[2] - a[2]);
        long ans = 1;
        for (int[] e : arr) {
            int i = e[0], x = e[2];
            int l = left[i], r = right[i];
            long cnt = (long) (i - l) * (r - i);
            if (cnt <= k) {
                ans = ans * qpow(x, cnt) % mod;
                k -= cnt;
            } else {
                ans = ans * qpow(x, k) % mod;
                break;
            }
        }
        return (int) ans;
    }

    private int primeFactors(int n) {
        int i = 2;
        Set<Integer> ans = new HashSet<>();
        while (i <= n / i) {
            while (n % i == 0) {
                ans.add(i);
                n /= i;
            }
            ++i;
        }
        if (n > 1) {
            ans.add(n);
        }
        return ans.size();
    }

    private int qpow(long a, long n) {
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
    int maximumScore(vector<int>& nums, int k) {
        const int mod = 1e9 + 7;
        int n = nums.size();
        vector<tuple<int, int, int>> arr(n);
        for (int i = 0; i < n; ++i) {
            arr[i] = {i, primeFactors(nums[i]), nums[i]};
        }
        vector<int> left(n, -1);
        vector<int> right(n, n);
        stack<int> stk;
        for (auto [i, f, _] : arr) {
            while (!stk.empty() && get<1>(arr[stk.top()]) < f) {
                stk.pop();
            }
            if (!stk.empty()) {
                left[i] = stk.top();
            }
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; ~i; --i) {
            int f = get<1>(arr[i]);
            while (!stk.empty() && get<1>(arr[stk.top()]) <= f) {
                stk.pop();
            }
            if (!stk.empty()) {
                right[i] = stk.top();
            }
            stk.push(i);
        }
        sort(arr.begin(), arr.end(), [](const auto& lhs, const auto& rhs) {
            return get<2>(rhs) < get<2>(lhs);
        });
        long long ans = 1;
        auto qpow = [&](long long a, int n) {
            long long ans = 1;
            for (; n; n >>= 1) {
                if (n & 1) {
                    ans = ans * a % mod;
                }
                a = a * a % mod;
            }
            return ans;
        };
        for (auto [i, _, x] : arr) {
            int l = left[i], r = right[i];
            long long cnt = 1LL * (i - l) * (r - i);
            if (cnt <= k) {
                ans = ans * qpow(x, cnt) % mod;
                k -= cnt;
            } else {
                ans = ans * qpow(x, k) % mod;
                break;
            }
        }
        return ans;
    }

    int primeFactors(int n) {
        int i = 2;
        unordered_set<int> ans;
        while (i <= n / i) {
            while (n % i == 0) {
                ans.insert(i);
                n /= i;
            }
            ++i;
        }
        if (n > 1) {
            ans.insert(n);
        }
        return ans.size();
    }
};
```

#### Go

```go
func maximumScore(nums []int, k int) int {
	n := len(nums)
	const mod = 1e9 + 7
	qpow := func(a, n int) int {
		ans := 1
		for ; n > 0; n >>= 1 {
			if n&1 == 1 {
				ans = ans * a % mod
			}
			a = a * a % mod
		}
		return ans
	}
	arr := make([][3]int, n)
	left := make([]int, n)
	right := make([]int, n)
	for i, x := range nums {
		left[i] = -1
		right[i] = n
		arr[i] = [3]int{i, primeFactors(x), x}
	}
	stk := []int{}
	for _, e := range arr {
		i, f := e[0], e[1]
		for len(stk) > 0 && arr[stk[len(stk)-1]][1] < f {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			left[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	stk = []int{}
	for i := n - 1; i >= 0; i-- {
		f := arr[i][1]
		for len(stk) > 0 && arr[stk[len(stk)-1]][1] <= f {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			right[i] = stk[len(stk)-1]
		}
		stk = append(stk, i)
	}
	sort.Slice(arr, func(i, j int) bool { return arr[i][2] > arr[j][2] })
	ans := 1
	for _, e := range arr {
		i, x := e[0], e[2]
		l, r := left[i], right[i]
		cnt := (i - l) * (r - i)
		if cnt <= k {
			ans = ans * qpow(x, cnt) % mod
			k -= cnt
		} else {
			ans = ans * qpow(x, k) % mod
			break
		}
	}
	return ans
}

func primeFactors(n int) int {
	i := 2
	ans := map[int]bool{}
	for i <= n/i {
		for n%i == 0 {
			ans[i] = true
			n /= i
		}
		i++
	}
	if n > 1 {
		ans[n] = true
	}
	return len(ans)
}
```

#### TypeScript

```ts
function maximumScore(nums: number[], k: number): number {
    const mod = 10 ** 9 + 7;
    const n = nums.length;
    const arr: number[][] = Array(n)
        .fill(0)
        .map(() => Array(3).fill(0));
    const left: number[] = Array(n).fill(-1);
    const right: number[] = Array(n).fill(n);
    for (let i = 0; i < n; ++i) {
        arr[i] = [i, primeFactors(nums[i]), nums[i]];
    }
    const stk: number[] = [];
    for (const [i, f, _] of arr) {
        while (stk.length && arr[stk.at(-1)!][1] < f) {
            stk.pop();
        }
        if (stk.length) {
            left[i] = stk.at(-1)!;
        }
        stk.push(i);
    }
    stk.length = 0;
    for (let i = n - 1; i >= 0; --i) {
        const f = arr[i][1];
        while (stk.length && arr[stk.at(-1)!][1] <= f) {
            stk.pop();
        }
        if (stk.length) {
            right[i] = stk.at(-1)!;
        }
        stk.push(i);
    }
    arr.sort((a, b) => b[2] - a[2]);
    let ans = 1n;
    for (const [i, _, x] of arr) {
        const l = left[i];
        const r = right[i];
        const cnt = (i - l) * (r - i);
        if (cnt <= k) {
            ans = (ans * qpow(BigInt(x), cnt, mod)) % BigInt(mod);
            k -= cnt;
        } else {
            ans = (ans * qpow(BigInt(x), k, mod)) % BigInt(mod);
            break;
        }
    }
    return Number(ans);
}

function primeFactors(n: number): number {
    let i = 2;
    const s: Set<number> = new Set();
    while (i * i <= n) {
        while (n % i === 0) {
            s.add(i);
            n = Math.floor(n / i);
        }
        ++i;
    }
    if (n > 1) {
        s.add(n);
    }
    return s.size;
}

function qpow(a: bigint, n: number, mod: number): bigint {
    let ans = 1n;
    for (; n; n >>>= 1) {
        if (n & 1) {
            ans = (ans * a) % BigInt(mod);
        }
        a = (a * a) % BigInt(mod);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
