---
comments: true
difficulty: Hard
rating: 3039
source: Weekly Contest 379 Q4
tags:
    - Bit Manipulation
    - String
    - Dynamic Programming
    - Bitmask
---

<!-- problem:start -->

# [3003. Maximize the Number of Partitions After Operations](https://leetcode.com/problems/maximize-the-number-of-partitions-after-operations)

[中文文档](/solution/3000-3099/3003.Maximize%20the%20Number%20of%20Partitions%20After%20Operations/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code> và một số nguyên <code>k</code>.</p>

<p>Trước tiên, bạn được phép đổi <strong>nhiều nhất</strong> <strong>một</strong> vị trí trong <code>s</code> thành một chữ cái tiếng Anh thường khác.</p>

<p>Sau đó, thực hiện thao tác phân hoạch sau cho đến khi <code>s</code> <strong>rỗng</strong>:</p>

<ul>
	<li>Chọn <strong>tiền tố</strong> <strong>dài nhất</strong> của <code>s</code> chứa nhiều nhất <code>k</code> ký tự <strong>phân biệt</strong>.</li>
	<li><strong>Xóa</strong> tiền tố đó khỏi <code>s</code> và tăng số phân hoạch lên một. Các ký tự còn lại (nếu có) trong <code>s</code> vẫn giữ nguyên thứ tự ban đầu.</li>
</ul>

<p>Trả về một số nguyên biểu thị <strong>số phân hoạch lớn nhất</strong> có thể thu được sau các thao tác bằng cách chọn tối ưu nhiều nhất một vị trí để thay đổi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;accca&quot;, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách tối ưu là đổi <code>s[2]</code> thành một ký tự khác a và c, chẳng hạn như b. Khi đó, chuỗi trở thành <code>&quot;acbca&quot;</code>.</p>

<p>Sau đó, ta thực hiện các thao tác:</p>

<ol>
	<li>Tiền tố dài nhất chứa nhiều nhất 2 ký tự phân biệt là <code>&quot;ac&quot;</code>, ta xóa nó và <code>s</code> trở thành <code>&quot;bca&quot;</code>.</li>
	<li>Bây giờ, tiền tố dài nhất chứa nhiều nhất 2 ký tự phân biệt là <code>&quot;bc&quot;</code>, nên ta xóa nó và <code>s</code> trở thành <code>&quot;a&quot;</code>.</li>
	<li>Cuối cùng, ta xóa <code>&quot;a&quot;</code> và <code>s</code> trở thành rỗng, nên quy trình kết thúc.</li>
</ol>

<p>Qua các thao tác, chuỗi được chia thành 3 phân hoạch, nên đáp án là 3.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aabaab&quot;, k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, <code>s</code> chứa 2 ký tự phân biệt, nên dù ta thay đổi ký tự nào, nó vẫn chứa nhiều nhất 3 ký tự phân biệt. Vì vậy, tiền tố dài nhất có nhiều nhất 3 ký tự phân biệt luôn là toàn bộ chuỗi, do đó đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;xxyz&quot;, k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Cách tối ưu là đổi <code>s[0]</code> hoặc <code>s[1]</code> thành một ký tự không xuất hiện trong <code>s</code>, chẳng hạn đổi <code>s[0]</code> thành <code>w</code>.</p>

<p>Khi đó, <code>s</code> trở thành <code>&quot;wxyz&quot;</code>, gồm 4 ký tự phân biệt. Vì <code>k</code> bằng 1, chuỗi sẽ được chia thành 4 phân hoạch.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh thường.</li>
	<li><code>1 &lt;= k &lt;= 26</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm có ghi nhớ

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 10^4$ và ta có thể thay đổi một ký tự. Thử mọi thay đổi rồi mô phỏng lại các phân hoạch sẽ tốn khoảng $O(n^2 |\Sigma|)$, khá sát với giới hạn.
>
> Một lần cắt chỉ xảy ra khi tập các chữ cái phân biệt của segment hiện tại vượt quá $k$. Tập này có thể biểu diễn bằng mask $26$-bit, còn số lần thay đổi còn lại chỉ là $0$ hoặc $1$, nên có thể dùng memoization.
>
> Vì vậy, ta tìm kiếm $\textit{dfs}(i, \textit{cur}, t)$ với chỉ số $i$, mask của segment hiện tại là $\textit{cur}$ và còn $t$ lần thay đổi. Khi thêm $s[i]$, ta mở segment mới nếu popcount vượt quá $k$; nếu còn lượt thay đổi, ta cũng thử mọi chữ cái thay thế.

<!-- thinking:end -->

Ta xây dựng hàm $\textit{dfs}(i, \textit{cur}, t)$ biểu thị số phân hoạch lớn nhất có thể thu được khi đang xử lý chỉ số $i$ của chuỗi $s$, tiền tố hiện tại đã chứa tập ký tự $\textit{cur}$ và ta còn có thể thay đổi $t$ ký tự. Khi đó, đáp án là $\textit{dfs}(0, 0, 1)$.

Logic thực thi của hàm $\textit{dfs}(i, \textit{cur}, t)$ như sau:

1. Nếu $i \geq n$, nghĩa là ta đã xử lý xong chuỗi $s$, trả về 1.
2. Tính bitmask $v = 1 \ll (s[i] - 'a')$ tương ứng với ký tự hiện tại $s[i]$, đồng thời tính tập ký tự mới $\textit{nxt} = \textit{cur} \mid v$.
3. Nếu số bit trong $\textit{nxt}$ vượt quá $k$, nghĩa là tiền tố hiện tại đã chứa nhiều hơn $k$ ký tự phân biệt. Ta cần tạo một phân hoạch, tăng số phân hoạch lên 1 và gọi đệ quy $\textit{dfs}(i + 1, v, t)$. Nếu không, tiếp tục gọi đệ quy $\textit{dfs}(i + 1, \textit{nxt}, t)$.
4. Nếu $t > 0$, nghĩa là ta vẫn có thể thay đổi một ký tự. Ta thử đổi ký tự hiện tại $s[i]$ thành bất kỳ chữ cái thường nào (tổng cộng 26 lựa chọn). Với mỗi lựa chọn, tính tập ký tự mới $\textit{nxt} = \textit{cur} \mid (1 \ll j)$, rồi dựa vào việc nó có vượt quá $k$ ký tự phân biệt hay không để chọn cách gọi đệ quy tương ứng và cập nhật số phân hoạch lớn nhất.
5. Dùng một hash table để cache các trạng thái đã tính, tránh tính toán lặp lại.

Độ phức tạp thời gian là $O(n \times |\Sigma| \times k)$ và độ phức tạp không gian là $O(n \times |\Sigma| \times k)$, trong đó $n$ là độ dài chuỗi $s$ và $|\Sigma|$ là kích thước tập ký tự.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxPartitionsAfterOperations(self, s: str, k: int) -> int:
        @cache
        def dfs(i: int, cur: int, t: int) -> int:
            if i >= n:
                return 1
            v = 1 << (ord(s[i]) - ord("a"))
            nxt = cur | v
            if nxt.bit_count() > k:
                ans = dfs(i + 1, v, t) + 1
            else:
                ans = dfs(i + 1, nxt, t)
            if t:
                for j in range(26):
                    nxt = cur | (1 << j)
                    if nxt.bit_count() > k:
                        ans = max(ans, dfs(i + 1, 1 << j, 0) + 1)
                    else:
                        ans = max(ans, dfs(i + 1, nxt, 0))
            return ans

        n = len(s)
        return dfs(0, 0, 1)
```

#### Java

```java
class Solution {
    private Map<List<Integer>, Integer> f = new HashMap<>();
    private String s;
    private int k;

    public int maxPartitionsAfterOperations(String s, int k) {
        this.s = s;
        this.k = k;
        return dfs(0, 0, 1);
    }

    private int dfs(int i, int cur, int t) {
        if (i >= s.length()) {
            return 1;
        }
        var key = List.of(i, cur, t);
        if (f.containsKey(key)) {
            return f.get(key);
        }
        int v = 1 << (s.charAt(i) - 'a');
        int nxt = cur | v;
        int ans = Integer.bitCount(nxt) > k ? dfs(i + 1, v, t) + 1 : dfs(i + 1, nxt, t);
        if (t > 0) {
            for (int j = 0; j < 26; ++j) {
                nxt = cur | (1 << j);
                if (Integer.bitCount(nxt) > k) {
                    ans = Math.max(ans, dfs(i + 1, 1 << j, 0) + 1);
                } else {
                    ans = Math.max(ans, dfs(i + 1, nxt, 0));
                }
            }
        }
        f.put(key, ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxPartitionsAfterOperations(string s, int k) {
        int n = s.size();
        unordered_map<long long, int> f;
        auto dfs = [&](this auto&& dfs, int i, int cur, int t) -> int {
            if (i >= n) {
                return 1;
            }
            long long key = (long long) i << 32 | cur << 1 | t;
            if (f.count(key)) {
                return f[key];
            }
            int v = 1 << (s[i] - 'a');
            int nxt = cur | v;
            int ans = __builtin_popcount(nxt) > k ? dfs(i + 1, v, t) + 1 : dfs(i + 1, nxt, t);
            if (t) {
                for (int j = 0; j < 26; ++j) {
                    nxt = cur | (1 << j);
                    if (__builtin_popcount(nxt) > k) {
                        ans = max(ans, dfs(i + 1, 1 << j, 0) + 1);
                    } else {
                        ans = max(ans, dfs(i + 1, nxt, 0));
                    }
                }
            }
            return f[key] = ans;
        };
        return dfs(0, 0, 1);
    }
};
```

#### Go

```go
func maxPartitionsAfterOperations(s string, k int) int {
	n := len(s)
	type tuple struct{ i, cur, t int }
	f := map[tuple]int{}
	var dfs func(i, cur, t int) int
	dfs = func(i, cur, t int) int {
		if i >= n {
			return 1
		}
		key := tuple{i, cur, t}
		if v, ok := f[key]; ok {
			return v
		}
		v := 1 << (s[i] - 'a')
		nxt := cur | v
		var ans int
		if bits.OnesCount(uint(nxt)) > k {
			ans = dfs(i+1, v, t) + 1
		} else {
			ans = dfs(i+1, nxt, t)
		}
		if t > 0 {
			for j := 0; j < 26; j++ {
				nxt = cur | (1 << j)
				if bits.OnesCount(uint(nxt)) > k {
					ans = max(ans, dfs(i+1, 1<<j, 0)+1)
				} else {
					ans = max(ans, dfs(i+1, nxt, 0))
				}
			}
		}
		f[key] = ans
		return ans
	}
	return dfs(0, 0, 1)
}
```

#### TypeScript

```ts
function maxPartitionsAfterOperations(s: string, k: number): number {
    const n = s.length;
    const f: Map<bigint, number> = new Map();
    const dfs = (i: number, cur: number, t: number): number => {
        if (i >= n) {
            return 1;
        }
        const key = (BigInt(i) << 27n) | (BigInt(cur) << 1n) | BigInt(t);
        if (f.has(key)) {
            return f.get(key)!;
        }
        const v = 1 << (s.charCodeAt(i) - 97);
        let nxt = cur | v;
        let ans = 0;
        if (bitCount(nxt) > k) {
            ans = dfs(i + 1, v, t) + 1;
        } else {
            ans = dfs(i + 1, nxt, t);
        }
        if (t) {
            for (let j = 0; j < 26; ++j) {
                nxt = cur | (1 << j);
                if (bitCount(nxt) > k) {
                    ans = Math.max(ans, dfs(i + 1, 1 << j, 0) + 1);
                } else {
                    ans = Math.max(ans, dfs(i + 1, nxt, 0));
                }
            }
        }
        f.set(key, ans);
        return ans;
    };
    return dfs(0, 0, 1);
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
