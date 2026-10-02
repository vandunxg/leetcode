---
comments: true
difficulty: Medium
tags:
    - Bit Manipulation
    - Depth-First Search
    - Breadth-First Search
    - Math
---

<!-- problem:start -->

# [672. Bulb Switcher II](https://leetcode.com/problems/bulb-switcher-ii)

[中文文档](/solution/0600-0699/0672.Bulb%20Switcher%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Trong một căn phòng có <code>n</code> bóng đèn được đánh số từ <code>1</code> đến <code>n</code>, ban đầu tất cả đều bật, và trên tường có <strong>bốn nút</strong>. Mỗi nút có chức năng khác nhau:</p>

<ul>
	<li><strong>Nút 1:</strong> Đảo trạng thái của tất cả bóng đèn.</li>
	<li><strong>Nút 2:</strong> Đảo trạng thái của các bóng đèn có số thứ tự chẵn (tức là <code>2, 4, ...</code>).</li>
	<li><strong>Nút 3:</strong> Đảo trạng thái của các bóng đèn có số thứ tự lẻ (tức là <code>1, 3, ...</code>).</li>
	<li><strong>Nút 4:</strong> Đảo trạng thái của các bóng đèn có số thứ tự <code>j = 3k + 1</code>, với <code>k = 0, 1, 2, ...</code> (tức là <code>1, 4, 7, 10, ...</code>).</li>
</ul>

<p>Bạn phải nhấn nút tổng cộng <strong>chính xác</strong> <code>presses</code> lần. Mỗi lần nhấn, bạn có thể chọn <strong>bất kỳ</strong> nút nào trong bốn nút.</p>

<p>Cho hai số nguyên <code>n</code> và <code>presses</code>, hãy trả về <em>số lượng <strong>trạng thái khác nhau có thể có</strong> sau khi thực hiện đủ </em><code>presses</code><em> lần nhấn nút</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1, presses = 1
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trạng thái có thể là:
- [off] khi nhấn nút 1
- [on] khi nhấn nút 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, presses = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trạng thái có thể là:
- [off, off] khi nhấn nút 1
- [on, off] khi nhấn nút 2
- [off, on] khi nhấn nút 3
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, presses = 1
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Trạng thái có thể là:
- [off, off, off] khi nhấn nút 1
- [on, off, on] khi nhấn nút 2
- [off, on, off] khi nhấn nút 3
- [off, on, on] khi nhấn nút 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= presses &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Bốn nút đảo trạng thái bóng đèn bằng phép XOR. Các lần nhấn chẵn triệt tiêu nhau, và bóng đèn $i$ luôn có trạng thái giống bóng đèn $i+6$.
>
> Chỉ cần xét $\min(n,6)$ bóng đèn đầu tiên. Duyệt $2^4$ mask chẵn lẻ có popcount $\le presses$ và đồng dư với $presses$ theo modulo $2$, XOR các thao tác tương ứng rồi đếm số trạng thái khác nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def flipLights(self, n: int, presses: int) -> int:
        ops = (0b111111, 0b010101, 0b101010, 0b100100)
        n = min(n, 6)
        vis = set()
        for mask in range(1 << 4):
            cnt = mask.bit_count()
            if cnt <= presses and cnt % 2 == presses % 2:
                t = 0
                for i, op in enumerate(ops):
                    if (mask >> i) & 1:
                        t ^= op
                t &= (1 << 6) - 1
                t >>= 6 - n
                vis.add(t)
        return len(vis)
```

#### Java

```java
class Solution {
    public int flipLights(int n, int presses) {
        int[] ops = new int[] {0b111111, 0b010101, 0b101010, 0b100100};
        Set<Integer> vis = new HashSet<>();
        n = Math.min(n, 6);
        for (int mask = 0; mask < 1 << 4; ++mask) {
            int cnt = Integer.bitCount(mask);
            if (cnt <= presses && cnt % 2 == presses % 2) {
                int t = 0;
                for (int i = 0; i < 4; ++i) {
                    if (((mask >> i) & 1) == 1) {
                        t ^= ops[i];
                    }
                }
                t &= ((1 << 6) - 1);
                t >>= (6 - n);
                vis.add(t);
            }
        }
        return vis.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int flipLights(int n, int presses) {
        n = min(n, 6);
        vector<int> ops = {0b111111, 0b010101, 0b101010, 0b100100};
        unordered_set<int> vis;
        for (int mask = 0; mask < 1 << 4; ++mask) {
            int cnt = __builtin_popcount(mask);
            if (cnt > presses || cnt % 2 != presses % 2) continue;
            int t = 0;
            for (int i = 0; i < 4; ++i) {
                if (mask >> i & 1) {
                    t ^= ops[i];
                }
            }
            t &= (1 << 6) - 1;
            t >>= (6 - n);
            vis.insert(t);
        }
        return vis.size();
    }
};
```

#### Go

```go
func flipLights(n int, presses int) int {
	if n > 6 {
		n = 6
	}
	ops := []int{0b111111, 0b010101, 0b101010, 0b100100}
	vis := map[int]bool{}
	for mask := 0; mask < 1<<4; mask++ {
		cnt := bits.OnesCount(uint(mask))
		if cnt <= presses && cnt%2 == presses%2 {
			t := 0
			for i, op := range ops {
				if mask>>i&1 == 1 {
					t ^= op
				}
			}
			t &= 1<<6 - 1
			t >>= (6 - n)
			vis[t] = true
		}
	}
	return len(vis)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
