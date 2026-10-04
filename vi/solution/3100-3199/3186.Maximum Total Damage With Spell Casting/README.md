---
comments: true
difficulty: Medium
rating: 1840
source: Weekly Contest 402 Q3
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Dynamic Programming
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3186. Maximum Total Damage With Spell Casting](https://leetcode.com/problems/maximum-total-damage-with-spell-casting)

[中文文档](/solution/3100-3199/3186.Maximum%20Total%20Damage%20With%20Spell%20Casting/README.md)

## Mô tả

<!-- description:start -->

<p>Một pháp sư có nhiều phép thuật khác nhau.</p>

<p>Bạn được cho một mảng <code>power</code>, trong đó mỗi phần tử biểu diễn sát thương của một phép thuật. Nhiều phép thuật có thể có cùng giá trị sát thương.</p>

<p>Biết rằng nếu pháp sư quyết định thi triển một phép thuật có sát thương <code>power[i]</code>, họ <strong>không thể</strong> thi triển bất kỳ phép thuật nào có sát thương <code>power[i] - 2</code>, <code>power[i] - 1</code>, <code>power[i] + 1</code> hoặc <code>power[i] + 2</code>.</p>

<p>Mỗi phép thuật chỉ có thể được thi triển <strong>một lần</strong>.</p>

<p>Trả về <strong>tổng sát thương</strong> <em>lớn nhất</em> mà pháp sư có thể gây ra.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">power = [1,1,3,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sát thương lớn nhất có thể đạt được là 6 khi thi triển các phép thuật 0, 1, 3 với sát thương lần lượt là 1, 1, 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">power = [7,1,6,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">13</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sát thương lớn nhất có thể đạt được là 13 khi thi triển các phép thuật 1, 2, 3 với sát thương lần lượt là 1, 6, 6.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= power.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= power[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Việc chọn sát thương $x$ cấm mọi giá trị khác trong $[x-2,x+2]$, còn mọi phép có cùng sát thương $x$ đều có thể được chọn. Các tập con của những mức sát thương phân biệt có số lượng theo cấp số mũ.
>
> Sau khi sắp xếp, lựa chọn tại mỗi giá trị hiện tại là bỏ qua tất cả bản sao của giá trị đó, hoặc chọn toàn bộ chúng rồi nhảy đến giá trị đầu tiên $>x+2$. Bước nhảy này được thực hiện bằng tìm kiếm nhị phân.
>
> Ghi nhớ $dfs(i)=\max(dfs(i+cnt[x]),\,x\cdot cnt[x]+dfs(nxt[i]))$. Mỗi mức sát thương phân biệt chỉ được mở rộng một lần.

<!-- thinking:end -->

Trước tiên, ta sắp xếp mảng $\textit{power}$, dùng một bảng băm $\textit{cnt}$ để ghi lại số lần xuất hiện của mỗi giá trị sát thương, sau đó duyệt qua mảng $\textit{power}$. Với mỗi giá trị sát thương $x$, ta có thể xác định chỉ số của giá trị sát thương tiếp theo có thể được sử dụng khi chọn một phép có sát thương $x$. Đó là chỉ số của giá trị sát thương đầu tiên lớn hơn $x + 2$. Ta có thể dùng tìm kiếm nhị phân để tìm chỉ số này và lưu vào mảng $\textit{nxt}$.

Tiếp theo, ta định nghĩa hàm $\textit{dfs}$ để tính sát thương lớn nhất có thể đạt được khi bắt đầu từ giá trị sát thương thứ $i$.

Trong hàm $\textit{dfs}$, ta có thể bỏ qua giá trị sát thương hiện tại, tức là bỏ qua tất cả các giá trị sát thương giống với giá trị hiện tại và nhảy thẳng đến $i + \textit{cnt}[x]$, nhận được sát thương $\textit{dfs}(i + \textit{cnt}[x])$; hoặc chọn giá trị sát thương hiện tại, tức là sử dụng tất cả các giá trị sát thương giống với giá trị hiện tại, sau đó nhảy đến chỉ số của giá trị sát thương tiếp theo, nhận được sát thương $x \times \textit{cnt}[x] + \textit{dfs}(\textit{nxt}[i])$, trong đó $\textit{nxt}[i]$ đại diện cho chỉ số của giá trị sát thương đầu tiên lớn hơn $x + 2$. Ta lấy giá trị lớn hơn trong hai trường hợp này làm giá trị trả về của hàm.

Để tránh tính toán lặp lại, ta có thể dùng memoization, lưu các kết quả đã tính vào mảng $\textit{f}$. Vì vậy, khi tính $\textit{dfs}(i)$, nếu $\textit{f}[i]$ khác $0$, ta trả về ngay $\textit{f}[i]$.

Đáp án là $\textit{dfs}(0)$.

Độ phức tạp thời gian là $O(n \log n)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{power}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumTotalDamage(self, power: List[int]) -> int:
        @cache
        def dfs(i: int) -> int:
            if i >= n:
                return 0
            a = dfs(i + cnt[power[i]])
            b = power[i] * cnt[power[i]] + dfs(nxt[i])
            return max(a, b)

        n = len(power)
        cnt = Counter(power)
        power.sort()
        nxt = [bisect_right(power, x + 2, lo=i + 1) for i, x in enumerate(power)]
        return dfs(0)
```

#### Java

```java
class Solution {
    private Long[] f;
    private int[] power;
    private Map<Integer, Integer> cnt;
    private int[] nxt;
    private int n;

    public long maximumTotalDamage(int[] power) {
        Arrays.sort(power);
        this.power = power;
        n = power.length;
        f = new Long[n];
        cnt = new HashMap<>(n);
        nxt = new int[n];
        for (int i = 0; i < n; ++i) {
            cnt.merge(power[i], 1, Integer::sum);
            int l = Arrays.binarySearch(power, power[i] + 3);
            l = l < 0 ? -l - 1 : l;
            nxt[i] = l;
        }
        return dfs(0);
    }

    private long dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i] != null) {
            return f[i];
        }
        long a = dfs(i + cnt.get(power[i]));
        long b = 1L * power[i] * cnt.get(power[i]) + dfs(nxt[i]);
        return f[i] = Math.max(a, b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long maximumTotalDamage(vector<int>& power) {
        sort(power.begin(), power.end());
        this->power = power;
        n = power.size();
        f.resize(n);
        nxt.resize(n);
        for (int i = 0; i < n; ++i) {
            cnt[power[i]]++;
            nxt[i] = upper_bound(power.begin() + i + 1, power.end(), power[i] + 2) - power.begin();
        }
        return dfs(0);
    }

private:
    unordered_map<int, int> cnt;
    vector<long long> f;
    vector<int> power;
    vector<int> nxt;
    int n;

    long long dfs(int i) {
        if (i >= n) {
            return 0;
        }
        if (f[i]) {
            return f[i];
        }
        long long a = dfs(i + cnt[power[i]]);
        long long b = 1LL * power[i] * cnt[power[i]] + dfs(nxt[i]);
        return f[i] = max(a, b);
    }
};
```

#### Go

```go
func maximumTotalDamage(power []int) int64 {
	n := len(power)
	sort.Ints(power)
	cnt := map[int]int{}
	nxt := make([]int, n)
	f := make([]int64, n)
	for i, x := range power {
		cnt[x]++
		nxt[i] = sort.SearchInts(power, x+3)
	}
	var dfs func(int) int64
	dfs = func(i int) int64 {
		if i >= n {
			return 0
		}
		if f[i] != 0 {
			return f[i]
		}
		a := dfs(i + cnt[power[i]])
		b := int64(power[i]*cnt[power[i]]) + dfs(nxt[i])
		f[i] = max(a, b)
		return f[i]
	}
	return dfs(0)
}
```

#### TypeScript

```ts
function maximumTotalDamage(power: number[]): number {
    const n = power.length;
    power.sort((a, b) => a - b);
    const f: number[] = Array(n).fill(0);
    const cnt: Record<number, number> = {};
    const nxt: number[] = Array(n).fill(0);
    for (let i = 0; i < n; ++i) {
        cnt[power[i]] = (cnt[power[i]] || 0) + 1;
        let [l, r] = [i + 1, n];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (power[mid] > power[i] + 2) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        nxt[i] = l;
    }
    const dfs = (i: number): number => {
        if (i >= n) {
            return 0;
        }
        if (f[i]) {
            return f[i];
        }
        const a = dfs(i + cnt[power[i]]);
        const b = power[i] * cnt[power[i]] + dfs(nxt[i]);
        return (f[i] = Math.max(a, b));
    };
    return dfs(0);
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn maximum_total_damage(mut power: Vec<i32>) -> i64 {
        power.sort();
        let n = power.len();
        let mut cnt = HashMap::new();
        let mut nxt = vec![0; n];
        let mut f = vec![-1_i64; n];

        for i in 0..n {
            *cnt.entry(power[i]).or_insert(0) += 1;
            let j = match power[i + 1..].binary_search_by(|&x| x.cmp(&(power[i] + 2 + 1))) {
                Ok(pos) | Err(pos) => i + 1 + pos,
            };
            nxt[i] = j;
        }

        fn dfs(
            i: usize,
            n: usize,
            power: &Vec<i32>,
            nxt: &Vec<usize>,
            f: &mut Vec<i64>,
            cnt: &HashMap<i32, i32>,
        ) -> i64 {
            if i >= n {
                return 0;
            }
            if f[i] != -1 {
                return f[i];
            }
            let c = *cnt.get(&power[i]).unwrap();
            let a = dfs(i + c as usize, n, power, nxt, f, cnt);
            let b = power[i] as i64 * c as i64 + dfs(nxt[i], n, power, nxt, f, cnt);
            f[i] = a.max(b);
            f[i]
        }

        dfs(0, n, &power, &nxt, &mut f, &cnt)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
