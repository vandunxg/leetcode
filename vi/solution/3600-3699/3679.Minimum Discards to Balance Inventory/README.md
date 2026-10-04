---
comments: true
difficulty: Medium
rating: 1638
source: Biweekly Contest 165 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Sliding Window
    - Simulation
---

<!-- problem:start -->

# [3679. Minimum Discards to Balance Inventory](https://leetcode.com/problems/minimum-discards-to-balance-inventory)

[中文文档](/solution/3600-3699/3679.Minimum%20Discards%20to%20Balance%20Inventory/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>w</code> và <code>m</code>, cùng một mảng số nguyên <code>arrivals</code>, trong đó <code>arrivals[i]</code> là loại vật phẩm đến vào ngày <code>i</code> (các ngày được đánh số từ <strong>1</strong>).</p>

<p>Các vật phẩm được quản lý theo những quy tắc sau:</p>

<ul>
	<li>Mỗi vật phẩm đến có thể được <strong>giữ lại</strong> hoặc <strong>loại bỏ</strong>; một vật phẩm chỉ có thể bị loại bỏ vào đúng ngày nó đến.</li>
	<li>Với mỗi ngày <code>i</code>, xét cửa sổ các ngày <code>[max(1, i - w + 1), i]</code> ( <code>w</code> ngày gần nhất tính đến ngày <code>i</code>):
	<ul>
		<li>Trong <strong>bất kỳ</strong> cửa sổ nào như vậy, mỗi loại vật phẩm xuất hiện <strong>không quá</strong> <code>m</code> lần trong số các vật phẩm được giữ lại có ngày đến nằm trong cửa sổ đó.</li>
		<li>Nếu giữ vật phẩm đến vào ngày <code>i</code> khiến loại vật phẩm đó xuất hiện <strong>quá</strong> <code>m</code> lần trong cửa sổ, vật phẩm đó <strong>bắt buộc</strong> phải bị loại bỏ.</li>
	</ul>
	</li>
</ul>

<p>Trả về <strong>số lượng nhỏ nhất</strong> vật phẩm cần loại bỏ sao cho mọi cửa sổ gồm <code>w</code> ngày đều chứa không quá <code>m</code> vật phẩm của mỗi loại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arrivals = [1,2,1,3,1], w = 4, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ngày 1, vật phẩm loại 1 đến; cửa sổ chứa không quá <code>m</code> vật phẩm loại này, nên ta giữ lại.</li>
	<li>Ngày 2, vật phẩm loại 2 đến; cửa sổ các ngày 1 - 2 không có vấn đề gì.</li>
	<li>Ngày 3, vật phẩm loại 1 đến; cửa sổ <code>[1, 2, 1]</code> có hai vật phẩm loại 1, vẫn trong giới hạn.</li>
	<li>Ngày 4, vật phẩm loại 3 đến; cửa sổ <code>[1, 2, 1, 3]</code> có hai vật phẩm loại 1, được phép.</li>
	<li>Ngày 5, vật phẩm loại 1 đến; cửa sổ <code>[2, 1, 3, 1]</code> có hai vật phẩm loại 1, vẫn hợp lệ.</li>
</ul>

<p>Không có vật phẩm nào bị loại bỏ, nên kết quả là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">arrivals = [1,2,3,3,3,4], w = 3, m = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Ngày 1, vật phẩm loại 1 đến. Ta giữ lại.</li>
	<li>Ngày 2, vật phẩm loại 2 đến; cửa sổ <code>[1, 2]</code> không có vấn đề gì.</li>
	<li>Ngày 3, vật phẩm loại 3 đến; cửa sổ <code>[1, 2, 3]</code> có một vật phẩm loại 3.</li>
	<li>Ngày 4, vật phẩm loại 3 đến; cửa sổ <code>[2, 3, 3]</code> có hai vật phẩm loại 3, được phép.</li>
	<li>Ngày 5, vật phẩm loại 3 đến; cửa sổ <code>[3, 3, 3]</code> có ba vật phẩm loại 3, vượt quá giới hạn, nên vật phẩm đến vào ngày này phải bị loại bỏ.</li>
	<li>Ngày 6, vật phẩm loại 4 đến; cửa sổ <code>[3, 4]</code> không có vấn đề gì.</li>
</ul>

<p>Vật phẩm loại 3 vào ngày 5 bị loại bỏ, đây là số lượng vật phẩm tối thiểu cần loại bỏ, nên kết quả là 1.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arrivals.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arrivals[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= w &lt;= arrivals.length</code></li>
	<li><code>1 &lt;= m &lt;= w</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng + Cửa sổ trượt

<!-- thinking:start -->

> **Tư duy**
>
> Trong mọi cửa sổ có độ dài $w$, ta chỉ được giữ lại nhiều nhất $m$ bản sao của một vật phẩm; những bản sao dư phải bị loại bỏ ngay khi đến. Ta mô phỏng từ trái sang phải và trừ đi số lượng của những vật phẩm đã rời khỏi cửa sổ.
>
> $\textit{cnt}$ lưu số bản sao được giữ lại trong cửa sổ hiện tại; $\textit{marked}[i]$ ghi lại việc vật phẩm của ngày $i$ có được giữ lại hay không. Chỉ vật phẩm được giữ lại mới được trừ đi khi trượt ra khỏi cửa sổ.
>
> Nếu $\textit{cnt}[x]$ đã bằng $m$, ta loại bỏ vật phẩm; nếu không thì giữ lại. Mỗi vật phẩm đến chỉ được xử lý một lần.

<!-- thinking:end -->

Ta dùng một hash map $\textit{cnt}$ để ghi nhận số lượng của mỗi loại vật phẩm trong cửa sổ hiện tại, và một mảng $\textit{marked}$ để ghi lại việc mỗi vật phẩm có được giữ lại hay không.

Ta duyệt mảng từ trái sang phải. Với mỗi vật phẩm $x$:

1. Nếu ngày hiện tại $i$ lớn hơn hoặc bằng kích thước cửa sổ $w$, ta trừ $\textit{marked}[i - w]$ khỏi số lượng của vật phẩm ngoài cùng bên trái cửa sổ (nếu vật phẩm đó được giữ lại).
2. Nếu số lượng vật phẩm hiện tại trong cửa sổ đã đạt $m$, ta loại bỏ vật phẩm đó.
3. Nếu không, ta giữ lại vật phẩm và tăng số lượng của nó lên một.

Cuối cùng, đáp án là số lượng vật phẩm bị loại bỏ.

Độ phức tạp thời gian là $O(n)$, và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minArrivalsToDiscard(self, arrivals: List[int], w: int, m: int) -> int:
        cnt = Counter()
        n = len(arrivals)
        marked = [0] * n
        ans = 0
        for i, x in enumerate(arrivals):
            if i >= w:
                cnt[arrivals[i - w]] -= marked[i - w]
            if cnt[x] >= m:
                ans += 1
            else:
                marked[i] = 1
                cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public int minArrivalsToDiscard(int[] arrivals, int w, int m) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int n = arrivals.length;
        int[] marked = new int[n];
        int ans = 0;
        for (int i = 0; i < n; i++) {
            int x = arrivals[i];
            if (i >= w) {
                int prev = arrivals[i - w];
                cnt.merge(prev, -marked[i - w], Integer::sum);
            }
            if (cnt.getOrDefault(x, 0) >= m) {
                ans++;
            } else {
                marked[i] = 1;
                cnt.merge(x, 1, Integer::sum);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minArrivalsToDiscard(vector<int>& arrivals, int w, int m) {
        unordered_map<int, int> cnt;
        int n = arrivals.size();
        vector<int> marked(n, 0);
        int ans = 0;
        for (int i = 0; i < n; i++) {
            int x = arrivals[i];
            if (i >= w) {
                cnt[arrivals[i - w]] -= marked[i - w];
            }
            if (cnt[x] >= m) {
                ans++;
            } else {
                marked[i] = 1;
                cnt[x] += 1;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minArrivalsToDiscard(arrivals []int, w int, m int) (ans int) {
	cnt := make(map[int]int)
	n := len(arrivals)
	marked := make([]int, n)
	for i, x := range arrivals {
		if i >= w {
			cnt[arrivals[i-w]] -= marked[i-w]
		}
		if cnt[x] >= m {
			ans++
		} else {
			marked[i] = 1
			cnt[x]++
		}
	}
	return
}
```

#### TypeScript

```ts
function minArrivalsToDiscard(arrivals: number[], w: number, m: number): number {
    const cnt = new Map<number, number>();
    const n = arrivals.length;
    const marked = Array<number>(n).fill(0);
    let ans = 0;

    for (let i = 0; i < n; i++) {
        const x = arrivals[i];
        if (i >= w) {
            cnt.set(arrivals[i - w], (cnt.get(arrivals[i - w]) || 0) - marked[i - w]);
        }
        if ((cnt.get(x) || 0) >= m) {
            ans++;
        } else {
            marked[i] = 1;
            cnt.set(x, (cnt.get(x) || 0) + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
