---
comments: true
difficulty: Hard
tags:
    - Stack
    - Array
    - Dynamic Programming
    - Ordered Set
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [975. Odd Even Jump](https://leetcode.com/problems/odd-even-jump)

[中文文档](/solution/0900-0999/0975.Odd%20Even%20Jump/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>. Bắt đầu từ một chỉ số bất kỳ, bạn có thể thực hiện một chuỗi bước nhảy. Các bước nhảy thứ (1<sup>st</sup>, 3<sup>rd</sup>, 5<sup>th</sup>, ...) được gọi là <strong>bước nhảy lẻ</strong>, còn các bước thứ (2<sup>nd</sup>, 4<sup>th</sup>, 6<sup>th</sup>, ...) được gọi là <strong>bước nhảy chẵn</strong>. Lưu ý, thứ tự lẻ/chẵn áp dụng cho <strong>bước nhảy</strong>, không phải chỉ số.</p>

<p>Bạn có thể nhảy tiến từ chỉ số <code>i</code> đến chỉ số <code>j</code> (với <code>i &lt; j</code>) theo quy tắc sau:</p>

<ul>
	<li>Trong <strong>bước nhảy lẻ</strong> (tức bước 1, 3, 5, ...), bạn nhảy đến chỉ số <code>j</code> sao cho <code>arr[i] &lt;= arr[j]</code> và <code>arr[j]</code> có giá trị nhỏ nhất có thể. Nếu có nhiều chỉ số <code>j</code> thỏa mãn, chỉ số đích phải là <strong>nhỏ nhất</strong>.</li>
	<li>Trong <strong>bước nhảy chẵn</strong> (tức bước 2, 4, 6, ...), bạn nhảy đến chỉ số <code>j</code> sao cho <code>arr[i] &gt;= arr[j]</code> và <code>arr[j]</code> có giá trị lớn nhất có thể. Nếu có nhiều chỉ số <code>j</code> thỏa mãn, chỉ số đích phải là <strong>nhỏ nhất</strong>.</li>
	<li>Có thể tồn tại chỉ số <code>i</code> không có bước nhảy hợp lệ nào.</li>
</ul>

<p>Một chỉ số bắt đầu được gọi là <strong>tốt</strong> nếu từ đó, bạn có thể đến cuối mảng (chỉ số <code>arr.length - 1</code>) sau một số bước nhảy bất kỳ (có thể là 0 hoặc nhiều bước).</p>

<p>Trả về <em>số chỉ số bắt đầu <strong>tốt</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [10,13,12,14,15]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> 
Từ chỉ số bắt đầu i = 0, bước nhảy đầu tiên có thể đến i = 2 (vì arr[2] là giá trị nhỏ nhất trong arr[1], arr[2], arr[3], arr[4] thỏa điều kiện lớn hơn hoặc bằng arr[0]); sau đó không thể nhảy tiếp.
Từ chỉ số bắt đầu i = 1 hoặc i = 2, bước nhảy đầu tiên đến i = 3, rồi không thể nhảy tiếp.
Từ chỉ số bắt đầu i = 3, bước nhảy đầu tiên đến i = 4 và ta tới cuối mảng.
Bắt đầu tại i = 4 thì ta đã ở cuối mảng.
Tổng cộng có 2 chỉ số bắt đầu khác nhau là i = 3 và i = 4 mà từ đó ta có thể đến cuối mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,3,1,1,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> 
Từ chỉ số bắt đầu i = 0, ta lần lượt nhảy đến i = 1, i = 2, i = 3:
Ở bước nhảy thứ nhất (bước lẻ), ta nhảy đến i = 1 vì arr[1] là giá trị nhỏ nhất trong [arr[1], arr[2], arr[3], arr[4]] lớn hơn hoặc bằng arr[0].
Ở bước nhảy thứ hai (bước chẵn), ta nhảy từ i = 1 đến i = 2 vì arr[2] là giá trị lớn nhất trong [arr[2], arr[3], arr[4]] nhỏ hơn hoặc bằng arr[1]. arr[3] cũng có giá trị lớn nhất này, nhưng chỉ số 2 nhỏ hơn nên ta chỉ có thể nhảy đến i = 2, không phải i = 3.
Ở bước nhảy thứ ba (bước lẻ), ta nhảy từ i = 2 đến i = 3 vì arr[3] là giá trị nhỏ nhất trong [arr[3], arr[4]] lớn hơn hoặc bằng arr[2].
Ta không thể nhảy từ i = 3 đến i = 4, nên chỉ số bắt đầu i = 0 không tốt.
Tương tự, ta có thể suy ra:
Bắt đầu tại i = 1, ta nhảy đến i = 4 và tới cuối mảng.
Bắt đầu tại i = 2, ta nhảy đến i = 3 rồi không thể nhảy tiếp.
Bắt đầu tại i = 3, ta nhảy đến i = 4 và tới cuối mảng.
Bắt đầu tại i = 4 thì ta đã ở cuối mảng.
Tổng cộng có 3 chỉ số bắt đầu khác nhau là i = 1, i = 3 và i = 4 mà từ đó ta có thể đến cuối mảng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5,1,3,4,2]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đến cuối mảng khi bắt đầu tại các chỉ số 1, 2 và 4.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt; 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set + Tìm kiếm có Memoization

<!-- thinking:start -->

> **Tư duy**
>
> Bước nhảy lẻ đến giá trị nhỏ nhất ở bên phải mà vẫn lớn hơn hoặc bằng giá trị hiện tại; bước nhảy chẵn đến giá trị lớn nhất không vượt quá giá trị hiện tại. Mô phỏng từng đường đi để đếm điểm bắt đầu tới đích sẽ lặp lại trạng thái. Duyệt từ phải sang trái, ordered map các giá trị đã gặp giúp tìm chỉ số tiếp theo $g[i][0/1]$ trong $O(\log n)$. Khả năng tới đích chỉ phụ thuộc vào $(i,\text{parity})$, nên dùng DFS có memoization là đủ.

<!-- thinking:end -->

Trước tiên, ta dùng ordered set để tiền xử lý vị trí có thể nhảy tới từ mỗi vị trí, lưu trong mảng $g$. Trong đó, $g[i][1]$ và $g[i][0]$ lần lượt là vị trí có thể nhảy tới khi bước hiện tại là bước lẻ hoặc bước chẵn. Nếu không có vị trí nào để nhảy tới, cả $g[i][1]$ và $g[i][0]$ đều bằng $-1$.

Sau đó, dùng tìm kiếm có memoization, bắt đầu từ mỗi vị trí với bước kế tiếp là bước lẻ, để xác định liệu ta có thể nhảy đến cuối mảng hay không. Nếu có, tăng kết quả lên một.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oddEvenJumps(self, arr: List[int]) -> int:
        @cache
        def dfs(i: int, k: int) -> bool:
            if i == n - 1:
                return True
            if g[i][k] == -1:
                return False
            return dfs(g[i][k], k ^ 1)

        n = len(arr)
        g = [[0] * 2 for _ in range(n)]
        sd = SortedDict()
        for i in range(n - 1, -1, -1):
            j = sd.bisect_left(arr[i])
            g[i][1] = sd.values()[j] if j < len(sd) else -1
            j = sd.bisect_right(arr[i]) - 1
            g[i][0] = sd.values()[j] if j >= 0 else -1
            sd[arr[i]] = i
        return sum(dfs(i, 1) for i in range(n))
```

#### Java

```java
class Solution {
    private int n;
    private Integer[][] f;
    private int[][] g;

    public int oddEvenJumps(int[] arr) {
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        n = arr.length;
        f = new Integer[n][2];
        g = new int[n][2];
        for (int i = n - 1; i >= 0; --i) {
            var hi = tm.ceilingEntry(arr[i]);
            g[i][1] = hi == null ? -1 : hi.getValue();
            var lo = tm.floorEntry(arr[i]);
            g[i][0] = lo == null ? -1 : lo.getValue();
            tm.put(arr[i], i);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += dfs(i, 1);
        }
        return ans;
    }

    private int dfs(int i, int k) {
        if (i == n - 1) {
            return 1;
        }
        if (g[i][k] == -1) {
            return 0;
        }
        if (f[i][k] != null) {
            return f[i][k];
        }
        return f[i][k] = dfs(g[i][k], k ^ 1);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int oddEvenJumps(vector<int>& arr) {
        int n = arr.size();
        map<int, int> d;
        int f[n][2];
        int g[n][2];
        memset(f, 0, sizeof(f));
        for (int i = n - 1; ~i; --i) {
            auto it = d.lower_bound(arr[i]);
            g[i][1] = it == d.end() ? -1 : it->second;
            it = d.upper_bound(arr[i]);
            g[i][0] = it == d.begin() ? -1 : prev(it)->second;
            d[arr[i]] = i;
        }
        function<int(int, int)> dfs = [&](int i, int k) -> int {
            if (i == n - 1) {
                return 1;
            }
            if (g[i][k] == -1) {
                return 0;
            }
            if (f[i][k] != 0) {
                return f[i][k];
            }
            return f[i][k] = dfs(g[i][k], k ^ 1);
        };
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += dfs(i, 1);
        }
        return ans;
    }
};
```

#### Go

```go
func oddEvenJumps(arr []int) (ans int) {
	n := len(arr)
	rbt := redblacktree.NewWithIntComparator()
	f := make([][2]int, n)
	g := make([][2]int, n)
	for i := n - 1; i >= 0; i-- {
		if v, ok := rbt.Ceiling(arr[i]); ok {
			g[i][1] = v.Value.(int)
		} else {
			g[i][1] = -1
		}
		if v, ok := rbt.Floor(arr[i]); ok {
			g[i][0] = v.Value.(int)
		} else {
			g[i][0] = -1
		}
		rbt.Put(arr[i], i)
	}
	var dfs func(int, int) int
	dfs = func(i, k int) int {
		if i == n-1 {
			return 1
		}
		if g[i][k] == -1 {
			return 0
		}
		if f[i][k] != 0 {
			return f[i][k]
		}
		f[i][k] = dfs(g[i][k], k^1)
		return f[i][k]
	}
	for i := 0; i < n; i++ {
		if dfs(i, 1) == 1 {
			ans++
		}
	}
	return
}
```

#### Rust

```rust
use std::collections::BTreeMap;

impl Solution {
    pub fn odd_even_jumps(arr: Vec<i32>) -> i32 {
        let n: usize = arr.len();
        let mut f: Vec<Vec<Option<i32>>> = vec![vec![None; 2]; n];
        let mut g: Vec<Vec<i32>> = vec![vec![-1; 2]; n];
        let mut tm: BTreeMap<i32, usize> = BTreeMap::new();

        for i in (0..n).rev() {
            if let Some((_, &v)) = tm.range(arr[i]..).next() {
                g[i][1] = v as i32;
            }
            if let Some((_, &v)) = tm.range(..=arr[i]).next_back() {
                g[i][0] = v as i32;
            }
            tm.insert(arr[i], i);
        }

        fn dfs(
            i: usize,
            k: usize,
            n: usize,
            f: &mut Vec<Vec<Option<i32>>>,
            g: &Vec<Vec<i32>>,
        ) -> i32 {
            if i == n - 1 {
                return 1;
            }
            if g[i][k] == -1 {
                return 0;
            }
            if let Some(v) = f[i][k] {
                return v;
            }
            let res = dfs(g[i][k] as usize, k ^ 1, n, f, g);
            f[i][k] = Some(res);
            res
        }

        let mut ans: i32 = 0;
        for i in 0..n {
            ans += dfs(i, 1, n, &mut f, &g);
        }
        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
