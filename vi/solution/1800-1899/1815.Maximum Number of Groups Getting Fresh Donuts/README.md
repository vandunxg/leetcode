---
comments: true
difficulty: Hard
rating: 2559
source: Biweekly Contest 49 Q4
tags:
    - Bit Manipulation
    - Memoization
    - Array
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [1815. Maximum Number of Groups Getting Fresh Donuts](https://leetcode.com/problems/maximum-number-of-groups-getting-fresh-donuts)

[中文文档](/solution/1800-1899/1815.Maximum%20Number%20of%20Groups%20Getting%20Fresh%20Donuts/README.md)

## Mô tả

<!-- description:start -->

<p>Có một cửa hàng bánh rán nướng bánh theo từng mẻ gồm <code>batchSize</code> chiếc. Cửa hàng phải phục vụ <strong>toàn bộ</strong> bánh của một mẻ trước khi phục vụ bất kỳ chiếc bánh nào của mẻ tiếp theo. Cho số nguyên <code>batchSize</code> và mảng số nguyên <code>groups</code>, trong đó <code>groups[i]</code> là một nhóm gồm <code>groups[i]</code> khách hàng sẽ đến cửa hàng. Mỗi khách hàng nhận đúng một chiếc bánh.</p>

<p>Khi một nhóm đến cửa hàng, tất cả khách hàng trong nhóm phải được phục vụ trước khi phục vụ bất kỳ nhóm nào sau đó. Một nhóm hạnh phúc nếu tất cả thành viên đều nhận được bánh mới. Nghĩa là khách hàng đầu tiên của nhóm không nhận chiếc bánh còn lại từ nhóm trước.</p>

<p>Bạn có thể tự do sắp xếp lại thứ tự các nhóm. Hãy trả về <em><strong>số lượng lớn nhất</strong> các nhóm hạnh phúc có thể đạt được sau khi sắp xếp lại.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> batchSize = 3, groups = [1,2,3,4,5,6]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có thể sắp xếp các nhóm thành [6,2,4,5,1,3]. Khi đó các nhóm thứ 1<sup>st</sup>, 2<sup>nd</sup>, 4<sup>th</sup> và 6<sup>th</sup> sẽ hạnh phúc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> batchSize = 4, groups = [1,3,2,5,2,2,1,6]
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= batchSize &lt;= 9</code></li>
	<li><code>1 &lt;= groups.length &lt;= 30</code></li>
	<li><code>1 &lt;= groups[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tham lam + Nén trạng thái + Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> Một nhóm hạnh phúc khi số bánh còn lại bằng $0$ lúc nhóm được phục vụ, tức tổng số người ở tiền tố là bội của $\textit{batchSize}$. Có tối đa $30$ nhóm nên không thể xét $30!$ hoán vị.
>
> Các nhóm có kích thước là bội của $\textit{batchSize}$ không ảnh hưởng tới các nhóm sau và có thể đếm ngay. Các nhóm còn lại được rút gọn thành các phần dư; mỗi phần dư xuất hiện nhiều nhất $30$ lần, nên 5 bit là đủ và mọi số lượng có thể gói trong một số nguyên $state$. Hàm $dfs(state,mod)$ thử thêm một phần dư và cộng một điểm khi $mod=0$.

<!-- thinking:end -->

Bài toán thực chất yêu cầu tìm thứ tự sắp xếp để tối đa hóa số nhóm có tổng tiền tố (ở đây là "số người") modulo $batchSize$ bằng $0$. Vì vậy, ta chia tất cả khách hàng thành hai loại:

- Các nhóm có số người là bội của $batchSize$. Những nhóm này không ảnh hưởng đến số bánh của nhóm khách hàng tiếp theo. Ta có thể sắp xếp tham lam các nhóm này trước, vì vậy chúng sẽ hạnh phúc. "Đáp án ban đầu" là số lượng các nhóm này.
- Các nhóm có số người không phải bội của $batchSize$. Thứ tự sắp xếp các nhóm này ảnh hưởng đến số bánh của nhóm tiếp theo. Ta lấy modulo $batchSize$ của số người $v$ trong mỗi nhóm, các phần dư tạo thành một tập hợp. Giá trị phần tử trong tập nằm trong $[1,2...,batchSize-1]$. Độ dài tối đa của mảng $groups$ là $30$, nên số lần xuất hiện lớn nhất của mỗi phần dư không vượt quá $30$. Ta có thể dùng $5$ bit nhị phân để biểu diễn số lượng của một phần dư. Vì $batchSize$ tối đa là $9$, tổng số bit cần dùng là $5\times (9-1)=40$. Do đó, ta có thể dùng số nguyên $64$-bit $state$ để biểu diễn.

Tiếp theo, ta thiết kế hàm $dfs(state, mod)$, biểu diễn số nhóm có thể hạnh phúc khi trạng thái sắp xếp là $state$ và phần dư tiền tố hiện tại là $mod$. Khi đó, "đáp án ban đầu" cộng với $dfs(state, 0)$ là đáp án cuối cùng.

Logic triển khai của hàm $dfs(state, mod)$: ta liệt kê từng phần dư $i$ từ $1$ đến $batchSize-1$. Nếu số lượng của phần dư $i$ không phải $0$, ta có thể giảm số lượng của phần dư $i$ đi $1$, cộng $i$ vào phần dư tiền tố hiện tại rồi lấy modulo $batchSize$, sau đó gọi đệ quy hàm $dfs$ để tìm lời giải tối ưu của trạng thái con và lấy giá trị lớn nhất. Cuối cùng, ta kiểm tra xem $mod$ có bằng $0$ không. Nếu là $0$, ta trả về sau khi cộng $1$ vào giá trị lớn nhất; nếu không, ta trả về trực tiếp giá trị lớn nhất.

Trong quá trình này, ta dùng tìm kiếm có ghi nhớ để tránh tính lặp lại các trạng thái.

Độ phức tạp thời gian không vượt quá $O(10^7)$ và độ phức tạp không gian không vượt quá $O(10^6)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxHappyGroups(self, batchSize: int, groups: List[int]) -> int:
        @cache
        def dfs(state, mod):
            res = 0
            x = int(mod == 0)
            for i in range(1, batchSize):
                if state >> (i * 5) & 31:
                    t = dfs(state - (1 << (i * 5)), (mod + i) % batchSize)
                    res = max(res, t + x)
            return res

        state = ans = 0
        for v in groups:
            i = v % batchSize
            ans += i == 0
            if i:
                state += 1 << (i * 5)
        ans += dfs(state, 0)
        return ans
```

#### Java

```java
class Solution {
    private Map<Long, Integer> f = new HashMap<>();
    private int size;

    public int maxHappyGroups(int batchSize, int[] groups) {
        size = batchSize;
        int ans = 0;
        long state = 0;
        for (int g : groups) {
            int i = g % size;
            if (i == 0) {
                ++ans;
            } else {
                state += 1l << (i * 5);
            }
        }
        ans += dfs(state, 0);
        return ans;
    }

    private int dfs(long state, int mod) {
        if (f.containsKey(state)) {
            return f.get(state);
        }
        int res = 0;
        for (int i = 1; i < size; ++i) {
            if ((state >> (i * 5) & 31) != 0) {
                int t = dfs(state - (1l << (i * 5)), (mod + i) % size);
                res = Math.max(res, t + (mod == 0 ? 1 : 0));
            }
        }
        f.put(state, res);
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxHappyGroups(int batchSize, vector<int>& groups) {
        using ll = long long;
        unordered_map<ll, int> f;
        ll state = 0;
        int ans = 0;
        for (auto& v : groups) {
            int i = v % batchSize;
            ans += i == 0;
            if (i) {
                state += 1ll << (i * 5);
            }
        }
        function<int(ll, int)> dfs = [&](ll state, int mod) {
            if (f.count(state)) {
                return f[state];
            }
            int res = 0;
            int x = mod == 0;
            for (int i = 1; i < batchSize; ++i) {
                if (state >> (i * 5) & 31) {
                    int t = dfs(state - (1ll << (i * 5)), (mod + i) % batchSize);
                    res = max(res, t + x);
                }
            }
            return f[state] = res;
        };
        ans += dfs(state, 0);
        return ans;
    }
};
```

#### Go

```go
func maxHappyGroups(batchSize int, groups []int) (ans int) {
	state := 0
	for _, v := range groups {
		i := v % batchSize
		if i == 0 {
			ans++
		} else {
			state += 1 << (i * 5)
		}
	}
	f := map[int]int{}
	var dfs func(int, int) int
	dfs = func(state, mod int) int {
		if v, ok := f[state]; ok {
			return v
		}
		res := 0
		x := 0
		if mod == 0 {
			x = 1
		}
		for i := 1; i < batchSize; i++ {
			if state>>(i*5)&31 != 0 {
				t := dfs(state-1<<(i*5), (mod+i)%batchSize)
				res = max(res, t+x)
			}
		}
		f[state] = res
		return res
	}
	ans += dfs(state, 0)
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm có ghi nhớ (Hoán vị nhóm)

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 gói tần suất phần dư vào các trường bit. Ta có thể thay bằng cách đánh dấu trực tiếp các nhóm còn lại: nếu $m$ là số nhóm có phần dư khác 0, $dfs(state,x)$ liệt kê các nhóm chưa dùng (loại bỏ các phần dư trùng nhau) và cộng một điểm khi phần dư tiền tố là $0$. Không gian trạng thái là $O(2^m)$, rõ ràng hơn khi $m$ nhỏ.

<!-- thinking:end -->

Trước hết, ta đếm các nhóm có kích thước là bội của $batchSize$, sau đó chỉ giữ lại phần dư của các nhóm còn lại.

Ta dùng bitmask cho tập các nhóm đã được xếp, và gọi $dfs(state, x)$ là số nhóm hạnh phúc bổ sung khi tập nhóm đã xếp là $state$ và phần dư tiền tố hiện tại là $x$. Ta liệt kê các nhóm chưa dùng, bỏ qua các phần dư trùng nhau rồi đệ quy. Nếu $x = 0$, bước hiện tại làm một nhóm hạnh phúc.

Độ phức tạp thời gian là $O(2^m \times m)$ và độ phức tạp không gian là $O(2^m)$, trong đó $m$ là số nhóm có phần dư khác 0.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxHappyGroups(self, batchSize: int, groups: List[int]) -> int:
        @cache
        def dfs(state, x):
            if state == mask:
                return 0
            vis = [False] * batchSize
            res = 0
            for i, v in enumerate(g):
                if state >> i & 1 == 0 and not vis[v]:
                    vis[v] = True
                    y = (x + v) % batchSize
                    res = max(res, dfs(state | 1 << i, y))
            return res + (x == 0)

        g = [v % batchSize for v in groups if v % batchSize]
        mask = (1 << len(g)) - 1
        return len(groups) - len(g) + dfs(0, 0)
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
