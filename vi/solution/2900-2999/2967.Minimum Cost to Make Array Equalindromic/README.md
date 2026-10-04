---
comments: true
difficulty: Medium
rating: 2116
source: Weekly Contest 376 Q3
tags:
    - Greedy
    - Array
    - Math
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [2967. Minimum Cost to Make Array Equalindromic](https://leetcode.com/problems/minimum-cost-to-make-array-equalindromic)

[中文文档](/solution/2900-2999/2967.Minimum%20Cost%20to%20Make%20Array%20Equalindromic/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>0-indexed</strong> <code>nums</code> có độ dài <code>n</code>.</p>

<p>Bạn được phép thực hiện một <strong>nước đi đặc biệt</strong> bất kỳ số lần nào (<strong>kể cả không thực hiện</strong>) trên <code>nums</code>. Trong một <strong>nước đi</strong> <strong>đặc biệt</strong>, bạn thực hiện các bước sau <strong>theo thứ tự</strong>:</p>

<ul>
	<li>Chọn một chỉ số <code>i</code> trong đoạn <code>[0, n - 1]</code> và một số nguyên <strong>dương</strong> <code>x</code>.</li>
	<li>Cộng <code>|nums[i] - x|</code> vào tổng chi phí.</li>
	<li>Thay đổi giá trị của <code>nums[i]</code> thành <code>x</code>.</li>
</ul>

<p><strong>Số đối xứng</strong> là số nguyên dương không thay đổi khi đảo ngược các chữ số. Ví dụ, <code>121</code>, <code>2552</code> và <code>65756</code> là các số đối xứng, còn <code>24</code>, <code>46</code>, <code>235</code> thì không.</p>

<p>Một mảng được gọi là <strong>equalindromic</strong> nếu tất cả phần tử trong mảng đều bằng một số nguyên <code>y</code>, trong đó <code>y</code> là một <strong>số đối xứng</strong> nhỏ hơn <code>10<sup>9</sup></code>.</p>

<p>Trả về <em>một số nguyên biểu thị <strong>tổng chi phí nhỏ nhất</strong> để biến </em><code>nums</code><em> thành <strong>equalindromic</strong> bằng cách thực hiện một số nước đi đặc biệt bất kỳ.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,5]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Ta có thể biến mảng thành equalindromic bằng cách đổi tất cả phần tử thành 3, là một số đối xứng. Chi phí để biến mảng thành [3,3,3,3,3] bằng 4 nước đi đặc biệt là |1 - 3| + |2 - 3| + |4 - 3| + |5 - 3| = 6.
Có thể chứng minh rằng việc đổi tất cả phần tử thành bất kỳ số đối xứng nào khác 3 đều không thể đạt được chi phí thấp hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [10,12,13,14,15]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> Ta có thể biến mảng thành equalindromic bằng cách đổi tất cả phần tử thành 11, là một số đối xứng. Chi phí để biến mảng thành [11,11,11,11,11] bằng 5 nước đi đặc biệt là |10 - 11| + |12 - 11| + |13 - 11| + |14 - 11| + |15 - 11| = 11.
Có thể chứng minh rằng việc đổi tất cả phần tử thành bất kỳ số đối xứng nào khác 11 đều không thể đạt được chi phí thấp hơn.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [22,33,22,33,22]
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong> Ta có thể biến mảng thành equalindromic bằng cách đổi tất cả phần tử thành 22, là một số đối xứng. Chi phí để biến mảng thành [22,22,22,22,22] bằng 2 nước đi đặc biệt là |33 - 22| + |33 - 22| = 22.
Có thể chứng minh rằng việc đổi tất cả phần tử thành bất kỳ số đối xứng nào khác 22 đều không thể đạt được chi phí thấp hơn.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tiền xử lý + Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Mọi giá trị đều trở thành một số đối xứng; chi phí $L_1$ đạt nhỏ nhất gần trung vị. Ta tạo các số đối xứng bằng cách đối xứng các nửa trong $[1,10^5]$ và lưu chúng đã sắp xếp trong $ps$.
>
> Sắp xếp $nums$, tìm kiếm nhị phân trong $ps$ quanh trung vị và tính chi phí với một vài phần tử lân cận. Vì $n \le 10^5$, ta không cần liệt kê toàn bộ các đích đến.

<!-- thinking:end -->

Miền giá trị của các số đối xứng trong bài toán là $[1, 10^9]$. Do tính đối xứng của các số đối xứng, ta có thể liệt kê các số trong miền $[1, 10^5]$, sau đó đảo ngược và nối chúng để thu được tất cả các số đối xứng. Lưu ý rằng nếu đó là số đối xứng có số chữ số lẻ, ta cần bỏ chữ số cuối trước khi đảo ngược. Mảng các số đối xứng thu được sau khi tiền xử lý được ký hiệu là $ps$. Ta sắp xếp mảng $ps$.

Tiếp theo, ta sắp xếp mảng $nums$ và lấy trung vị $x$ của $nums$. Ta chỉ cần dùng tìm kiếm nhị phân để tìm trong mảng số đối xứng $ps$ một số gần với $x$ nhất, sau đó tính chi phí để biến $nums$ thành số này và thu được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng $nums$, còn $M$ là độ dài của mảng số đối xứng $ps$.

Các bài toán tương tự:

- [906. Super Palindromes](https://github.com/doocs/leetcode/blob/main/solution/0900-0999/0906.Super%20Palindromes/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
ps = []
for i in range(1, 10**5 + 1):
    s = str(i)
    t1 = s[::-1]
    t2 = s[:-1][::-1]
    ps.append(int(s + t1))
    ps.append(int(s + t2))
ps.sort()


class Solution:
    def minimumCost(self, nums: List[int]) -> int:
        def f(x: int) -> int:
            return sum(abs(v - x) for v in nums)

        nums.sort()
        i = bisect_left(ps, nums[len(nums) // 2])
        return min(f(ps[j]) for j in range(i - 1, i + 2) if 0 <= j < len(ps))
```

#### Java

```java
public class Solution {
    private static long[] ps;
    private int[] nums;

    static {
        ps = new long[2 * (int) 1e5];
        for (int i = 1; i <= 1e5; i++) {
            String s = Integer.toString(i);
            String t1 = new StringBuilder(s).reverse().toString();
            String t2 = new StringBuilder(s.substring(0, s.length() - 1)).reverse().toString();
            ps[2 * i - 2] = Long.parseLong(s + t1);
            ps[2 * i - 1] = Long.parseLong(s + t2);
        }
        Arrays.sort(ps);
    }

    public long minimumCost(int[] nums) {
        this.nums = nums;
        Arrays.sort(nums);
        int i = Arrays.binarySearch(ps, nums[nums.length / 2]);
        i = i < 0 ? -i - 1 : i;
        long ans = 1L << 60;
        for (int j = i - 1; j <= i + 1; j++) {
            if (0 <= j && j < ps.length) {
                ans = Math.min(ans, f(ps[j]));
            }
        }
        return ans;
    }

    private long f(long x) {
        long ans = 0;
        for (int v : nums) {
            ans += Math.abs(v - x);
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;

ll ps[2 * 100000];

int init = [] {
    for (int i = 1; i <= 100000; i++) {
        string s = to_string(i);
        string t1 = s;
        reverse(t1.begin(), t1.end());
        string t2 = s.substr(0, s.length() - 1);
        reverse(t2.begin(), t2.end());
        ps[2 * i - 2] = stoll(s + t1);
        ps[2 * i - 1] = stoll(s + t2);
    }
    sort(ps, ps + 2 * 100000);
    return 0;
}();

class Solution {
public:
    long long minimumCost(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int i = lower_bound(ps, ps + 2 * 100000, nums[nums.size() / 2]) - ps;
        auto f = [&](ll x) {
            ll ans = 0;
            for (int& v : nums) {
                ans += abs(v - x);
            }
            return ans;
        };
        ll ans = LLONG_MAX;
        for (int j = i - 1; j <= i + 1; j++) {
            if (0 <= j && j < 2 * 100000) {
                ans = min(ans, f(ps[j]));
            }
        }
        return ans;
    }
};
```

#### Go

```go
var ps [2 * 100000]int64

func init() {
	for i := 1; i <= 100000; i++ {
		s := strconv.Itoa(i)
		t1 := reverseString(s)
		t2 := reverseString(s[:len(s)-1])
		ps[2*i-2], _ = strconv.ParseInt(s+t1, 10, 64)
		ps[2*i-1], _ = strconv.ParseInt(s+t2, 10, 64)
	}
	sort.Slice(ps[:], func(i, j int) bool {
		return ps[i] < ps[j]
	})
}

func reverseString(s string) string {
	cs := []rune(s)
	for i, j := 0, len(cs)-1; i < j; i, j = i+1, j-1 {
		cs[i], cs[j] = cs[j], cs[i]
	}
	return string(cs)
}

func minimumCost(nums []int) int64 {
	sort.Ints(nums)
	i := sort.Search(len(ps), func(i int) bool {
		return ps[i] >= int64(nums[len(nums)/2])
	})

	f := func(x int64) int64 {
		var ans int64
		for _, v := range nums {
			ans += int64(abs(int(x - int64(v))))
		}
		return ans
	}

	ans := int64(math.MaxInt64)
	for j := i - 1; j <= i+1; j++ {
		if 0 <= j && j < len(ps) {
			ans = min(ans, f(ps[j]))
		}
	}
	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
const ps = Array(2e5).fill(0);

const init = (() => {
    for (let i = 1; i <= 1e5; ++i) {
        const s: string = i.toString();
        const t1: string = s.split('').reverse().join('');
        const t2: string = s.slice(0, -1).split('').reverse().join('');
        ps[2 * i - 2] = parseInt(s + t1, 10);
        ps[2 * i - 1] = parseInt(s + t2, 10);
    }
    ps.sort((a, b) => a - b);
})();

function minimumCost(nums: number[]): number {
    const search = (x: number): number => {
        let [l, r] = [0, ps.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (ps[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    const f = (x: number): number => {
        return nums.reduce((acc, v) => acc + Math.abs(v - x), 0);
    };

    nums.sort((a, b) => a - b);
    const i: number = search(nums[nums.length >> 1]);
    let ans: number = Number.MAX_SAFE_INTEGER;
    for (let j = i - 1; j <= i + 1; j++) {
        if (j >= 0 && j < ps.length) {
            ans = Math.min(ans, f(ps[j]));
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_cost(nums: Vec<i32>) -> i64 {
        use std::sync::Once;
        use std::cmp::min;

        static INIT: Once = Once::new();
        static mut PS: Vec<i64> = Vec::new();

        INIT.call_once(|| {
            let mut ps_local = Vec::with_capacity(2 * 100_000);
            for i in 1..=100_000 {
                let s = i.to_string();

                let mut t1 = s.clone();
                t1 = t1.chars().rev().collect();
                ps_local.push(format!("{}{}", s, t1).parse::<i64>().unwrap());

                let mut t2 = s[0..s.len() - 1].to_string();
                t2 = t2.chars().rev().collect();
                ps_local.push(format!("{}{}", s, t2).parse::<i64>().unwrap());
            }
            ps_local.sort();
            unsafe {
                PS = ps_local;
            }
        });

        let mut nums = nums;
        nums.sort();

        let mid = nums[nums.len() / 2] as i64;

        let i = unsafe {
            match PS.binary_search(&mid) {
                Ok(i) => i,
                Err(i) => i,
            }
        };

        let f = |x: i64| -> i64 {
            nums.iter().map(|&v| (v as i64 - x).abs()).sum()
        };

        let mut ans = i64::MAX;

        for j in i.saturating_sub(1)..=(i + 1).min(2 * 100_000 - 1) {
            let x = unsafe { PS[j] };
            ans = min(ans, f(x));
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
