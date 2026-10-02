---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Memoization
    - Array
    - Dynamic Programming
    - Backtracking
    - Bitmask
    - Knapsack
    - Unbounded Knapsack
---

<!-- problem:start -->

# [638. Shopping Offers](https://leetcode.com/problems/shopping-offers)

[中文文档](/solution/0600-0699/0638.Shopping%20Offers/README.md)

## Mô tả

<!-- description:start -->

<p>Trong cửa hàng LeetCode có <code>n</code> mặt hàng để bán, mỗi mặt hàng có một mức giá. Ngoài ra còn có các ưu đãi đặc biệt; mỗi ưu đãi gồm một hoặc nhiều loại mặt hàng khác nhau với giá ưu đãi.</p>

<p>Cho mảng số nguyên <code>price</code>, trong đó <code>price[i]</code> là giá của mặt hàng thứ <code>i</code>, và mảng số nguyên <code>needs</code>, trong đó <code>needs[i]</code> là số lượng mặt hàng thứ <code>i</code> bạn muốn mua.</p>

<p>Ngoài ra, cho mảng <code>special</code> gồm các phần tử <code>special[i]</code> có kích thước <code>n + 1</code>. Trong đó, <code>special[i][j]</code> là số lượng mặt hàng thứ <code>j</code> trong ưu đãi thứ <code>i</code>, còn <code>special[i][n]</code> (tức số nguyên cuối cùng trong mảng) là giá của ưu đãi thứ <code>i</code>.</p>

<p>Hãy trả về <em>mức giá thấp nhất cần trả để mua đúng số lượng từng mặt hàng đã cho, với cách sử dụng ưu đãi đặc biệt tối ưu</em>. Bạn không được mua nhiều hơn số lượng cần thiết, kể cả khi việc đó làm giảm tổng giá. Có thể sử dụng mỗi ưu đãi đặc biệt bao nhiêu lần tùy ý.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [2,5], special = [[3,0,5],[1,2,10]], needs = [3,2]
<strong>Đầu ra:</strong> 14
<strong>Giải thích:</strong> Có hai loại mặt hàng A và B, có giá lần lượt là $2 và $5. 
Ưu đãi đặc biệt 1 cho phép mua 3A và 0B với giá $5
Ưu đãi đặc biệt 2 cho phép mua 1A và 2B với giá $10. 
Bạn cần mua 3A và 2B, nên có thể trả $10 để mua 1A và 2B (ưu đãi đặc biệt số 2), rồi trả $4 cho 2A.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> price = [2,3,4], special = [[1,1,0,4],[2,2,1,9]], needs = [1,2,1]
<strong>Đầu ra:</strong> 11
<strong>Giải thích:</strong> A có giá $2, B có giá $3 và C có giá $4. 
Bạn có thể trả $4 cho 1A và 1B, hoặc $9 cho 2A, 2B và 1C. 
Bạn cần mua 1A, 2B và 1C, nên có thể trả $4 cho 1A và 1B (ưu đãi đặc biệt số 1), rồi trả $3 cho 1B và $4 cho 1C. 
Bạn không thể mua thêm mặt hàng, dù ưu đãi chỉ tốn $9 cho 2A, 2B và 1C.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == price.length == needs.length</code></li>
	<li><code>1 &lt;= n &lt;= 6</code></li>
	<li><code>0 &lt;= price[i], needs[i] &lt;= 10</code></li>
	<li><code>1 &lt;= special.length &lt;= 100</code></li>
	<li><code>special[i].length == n + 1</code></li>
	<li><code>0 &lt;= special[i][j] &lt;= 50</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho ít nhất một trong các giá trị <code>special[i][j]</code> khác 0 với <code>0 &lt;= j &lt;= n - 1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + tìm kiếm có memoization

<!-- thinking:start -->

> **Tư duy**
>
> Có tối đa $6$ loại mặt hàng và cần không quá $10$ món mỗi loại; liệt kê số lần dùng từng ưu đãi sẽ khiến ta gặp lại cùng một danh sách nhu cầu còn lại.
>
> Mã hóa nhu cầu của mỗi loại bằng $4$ bit. Memoize theo mask nhu cầu còn lại: hoặc mua theo giá lẻ, hoặc áp dụng một ưu đãi còn phù hợp rồi gọi đệ quy.

<!-- thinking:end -->

Ta nhận thấy số loại mặt hàng $n \leq 6$ và nhu cầu của mỗi loại không vượt quá $10$. Có thể dùng $4$ bit nhị phân để biểu diễn số lượng cần mua của mỗi loại. Vì vậy, chỉ cần tối đa $6 \times 4 = 24$ bit để biểu diễn toàn bộ danh sách mua sắm.

Trước tiên, chuyển danh sách mua sắm $\textit{needs}$ thành số nguyên $\textit{mask}$, trong đó nhu cầu của mặt hàng thứ $i$ được lưu trong các bit từ $i \times 4$ đến $(i + 1) \times 4 - 1$ của $\textit{mask}$. Ví dụ, với $\textit{needs} = [1, 2, 1]$, ta có $\textit{mask} = 0b0001 0010 0001$.

Tiếp theo, định nghĩa hàm $\textit{dfs}(cur)$ biểu diễn số tiền ít nhất cần chi khi trạng thái hiện tại của danh sách mua sắm là $\textit{cur}$. Khi đó, đáp án là $\textit{dfs}(\textit{mask})$.

Hàm $\textit{dfs}(cur)$ được tính như sau:

- Trước hết, tính chi phí mua danh sách hiện tại $\textit{cur}$ theo giá lẻ, không dùng ưu đãi nào; gọi chi phí này là $\textit{ans}$.
- Sau đó, duyệt từng ưu đãi $\textit{offer}$. Nếu danh sách hiện tại $\textit{cur}$ có thể áp dụng ưu đãi này, tức số lượng mỗi mặt hàng trong $\textit{cur}$ không nhỏ hơn số lượng tương ứng trong $\textit{offer}$, ta thử dùng ưu đãi. Trừ số lượng từng mặt hàng trong $\textit{offer}$ khỏi $\textit{cur}$ để tạo danh sách mới $\textit{nxt}$, rồi đệ quy tính chi phí thấp nhất của $\textit{nxt}$ và cộng giá ưu đãi $\textit{offer}[n]$. Cập nhật $\textit{ans}$ theo công thức $\textit{ans} = \min(\textit{ans}, \textit{offer}[n] + \textit{dfs}(\textit{nxt}))$.
- Cuối cùng, trả về $\textit{ans}$.

Để tránh tính toán lặp lại, dùng hash table $\textit{f}$ để lưu chi phí thấp nhất ứng với mỗi trạng thái $\textit{cur}$.

Độ phức tạp thời gian là $O(n \times k \times m^n)$, trong đó $n$ là số loại mặt hàng, còn $k$ và $m$ lần lượt là số ưu đãi và nhu cầu tối đa của mỗi loại. Độ phức tạp không gian là $O(n \times m^n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shoppingOffers(
        self, price: List[int], special: List[List[int]], needs: List[int]
    ) -> int:
        @cache
        def dfs(cur: int) -> int:
            ans = sum(p * (cur >> (i * bits) & 0xF) for i, p in enumerate(price))
            for offer in special:
                nxt = cur
                for j in range(len(needs)):
                    if (cur >> (j * bits) & 0xF) < offer[j]:
                        break
                    nxt -= offer[j] << (j * bits)
                else:
                    ans = min(ans, offer[-1] + dfs(nxt))
            return ans

        bits, mask = 4, 0
        for i, need in enumerate(needs):
            mask |= need << i * bits
        return dfs(mask)
```

#### Java

```java
class Solution {
    private final int bits = 4;
    private int n;
    private List<Integer> price;
    private List<List<Integer>> special;
    private Map<Integer, Integer> f = new HashMap<>();

    public int shoppingOffers(
        List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        n = needs.size();
        this.price = price;
        this.special = special;
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            mask |= needs.get(i) << (i * bits);
        }
        return dfs(mask);
    }

    private int dfs(int cur) {
        if (f.containsKey(cur)) {
            return f.get(cur);
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += price.get(i) * (cur >> (i * bits) & 0xf);
        }
        for (List<Integer> offer : special) {
            int nxt = cur;
            boolean ok = true;
            for (int j = 0; j < n; ++j) {
                if ((cur >> (j * bits) & 0xf) < offer.get(j)) {
                    ok = false;
                    break;
                }
                nxt -= offer.get(j) << (j * bits);
            }
            if (ok) {
                ans = Math.min(ans, offer.get(n) + dfs(nxt));
            }
        }
        f.put(cur, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        const int bits = 4;
        int n = needs.size();
        unordered_map<int, int> f;
        int mask = 0;
        for (int i = 0; i < n; ++i) {
            mask |= needs[i] << (i * bits);
        }
        function<int(int)> dfs = [&](int cur) {
            if (f.contains(cur)) {
                return f[cur];
            }
            int ans = 0;
            for (int i = 0; i < n; ++i) {
                ans += price[i] * ((cur >> (i * bits)) & 0xf);
            }
            for (const auto& offer : special) {
                int nxt = cur;
                bool ok = true;
                for (int j = 0; j < n; ++j) {
                    if (((cur >> (j * bits)) & 0xf) < offer[j]) {
                        ok = false;
                        break;
                    }
                    nxt -= offer[j] << (j * bits);
                }
                if (ok) {
                    ans = min(ans, offer[n] + dfs(nxt));
                }
            }
            f[cur] = ans;
            return ans;
        };
        return dfs(mask);
    }
};
```

#### Go

```go
func shoppingOffers(price []int, special [][]int, needs []int) int {
	const bits = 4
	n := len(needs)
	f := make(map[int]int)
	mask := 0
	for i, need := range needs {
		mask |= need << (i * bits)
	}

	var dfs func(int) int
	dfs = func(cur int) int {
		if v, ok := f[cur]; ok {
			return v
		}
		ans := 0
		for i := 0; i < n; i++ {
			ans += price[i] * ((cur >> (i * bits)) & 0xf)
		}
		for _, offer := range special {
			nxt := cur
			ok := true
			for j := 0; j < n; j++ {
				if ((cur >> (j * bits)) & 0xf) < offer[j] {
					ok = false
					break
				}
				nxt -= offer[j] << (j * bits)
			}
			if ok {
				ans = min(ans, offer[n]+dfs(nxt))
			}
		}
		f[cur] = ans
		return ans
	}

	return dfs(mask)
}
```

#### TypeScript

```ts
function shoppingOffers(price: number[], special: number[][], needs: number[]): number {
    const bits = 4;
    const n = needs.length;
    const f: Map<number, number> = new Map();

    let mask = 0;
    for (let i = 0; i < n; i++) {
        mask |= needs[i] << (i * bits);
    }

    const dfs = (cur: number): number => {
        if (f.has(cur)) {
            return f.get(cur)!;
        }
        let ans = 0;
        for (let i = 0; i < n; i++) {
            ans += price[i] * ((cur >> (i * bits)) & 0xf);
        }
        for (const offer of special) {
            let nxt = cur;
            let ok = true;
            for (let j = 0; j < n; j++) {
                if (((cur >> (j * bits)) & 0xf) < offer[j]) {
                    ok = false;
                    break;
                }
                nxt -= offer[j] << (j * bits);
            }
            if (ok) {
                ans = Math.min(ans, offer[n] + dfs(nxt));
            }
        }
        f.set(cur, ans);
        return ans;
    };

    return dfs(mask);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
