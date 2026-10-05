---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [4064. Longest Subarray Divisible by K with At Most One Negation II](https://leetcode.com/problems/longest-subarray-divisible-by-k-with-at-most-one-negation-ii)

[Tài liệu tiếng Trung](/solution/4000-4099/4064.Longest%20Subarray%20Divisible%20by%20K%20with%20At%20Most%20One%20Negation%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>k</code>.</p>

<p>Một mảng con là <strong>hợp lệ</strong> nếu tổng của nó chia hết cho <code>k</code>, hoặc có thể trở nên chia hết cho <code>k</code> bằng cách <strong>đổi dấu một phần tử trong mảng con</strong>.</p>

<p>Đổi dấu một phần tử nghĩa là thay giá trị <code>x</code> của nó bằng <code>-x</code>.</p>

<p>Trả về <strong>độ dài của mảng con hợp lệ dài nhất</strong>. Nếu không tồn tại mảng con hợp lệ nào, trả về 0.</p>

<p><strong>Mảng con</strong> là một dãy phần tử liên tiếp, không rỗng trong một mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,1,2], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 7, và <code>7 % 3 = 1</code>, nên tổng không chia hết cho <code>k = 3</code>.</li>
	<li>Đổi dấu <code>nums[2] = 2</code> làm tổng trở thành <code>4 + 1 &minus; 2 = 3</code>, chia hết cho <code>k</code>.</li>
	<li>Do đó, toàn bộ mảng là một mảng con hợp lệ, có độ dài bằng 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,3,4], k = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 12, và đổi dấu bất kỳ phần tử nào cũng không làm tổng chia hết cho 7.</li>
	<li>Tuy nhiên, mảng con <code>[3, 4]</code> có tổng bằng 7, chia hết cho <code>k = 7</code> mà không cần đổi dấu.</li>
	<li>Do đó, mảng con hợp lệ dài nhất có độ dài bằng 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2,5], k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tổng của toàn bộ mảng là 9, và đổi dấu bất kỳ phần tử nào cũng không làm tổng chia hết cho 6.</li>
	<li>Mảng con <code>[2, 2]</code> có tổng bằng 4. Đổi dấu một trong hai phần tử sẽ biến nó thành <code>[-2, 2]</code> hoặc <code>[2, -2]</code>, cả hai đều có tổng bằng 0.</li>
	<li>Do đó, mảng con hợp lệ dài nhất có độ dài bằng 2.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup>​​​​​​​</code></li>
	<li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 3000</code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> $n$ có thể bằng $10^5$. Nếu chọn chỉ số bị đổi dấu rồi duyệt các tổng tiền tố cho từng lựa chọn như ở bài trước, độ phức tạp sẽ là $O(n^2)$ và không thể hoàn thành trong thời gian cho phép. Ở đây $k\le 3000$, nên số phần dư chỉ có bấy nhiêu.
>
> Tổng mảng con modulo $k$ là phần dư của tiền tố phải trừ phần dư của tiền tố trái. Đổi dấu một phần tử $x$ làm hiệu đó giảm đi $2x$, vì vậy phần dư bên phải bằng phần dư bên trái cộng $2(x\bmod k)$. Với mỗi phần dư bên trái, chỉ cần giữ tiền tố xuất hiện sớm nhất; chỉ số về sau sẽ tạo ra mảng con ngắn hơn.
>
> Các chỉ số xuất hiện sớm nhất được xử lý theo thứ tự xuất hiện. Khi quét đầu phải, phần tử hiện tại cung cấp phần dư dùng cho việc đổi dấu, và mọi phần dư bên trái đã có thể chứa phần tử này nhưng chưa được ghi nhận cho phần dư đó sẽ được ánh xạ sang phần dư đích. Mỗi cặp phần dư chỉ được ghi một lần, còn đầu trái nhỏ nhất lưu cho phần dư bên phải chính là mảng con hợp lệ dài nhất kết thúc tại đó.

<!-- thinking:end -->

Đặt $p[0]=0$ và $p[i+1]=(p[i]+\textit{nums}[i])\bmod k$. Tổng của $\textit{nums}[L..R]$ modulo $k$ là $p[R+1]-p[L]$. Nếu đổi dấu $\textit{nums}[t]$ trong mảng con, với $a=\textit{nums}[t]\bmod k$, tổng sẽ giảm đi $2\textit{nums}[t]$. Mảng con hợp lệ khi tồn tại $t\in[L,R]$ sao cho

$$
p[R+1]\equiv p[L]+2a\pmod{k},
$$

và cũng hợp lệ khi không đổi dấu nếu $p[R+1]\equiv p[L]$. Độ dài là $(R+1)-L$, nên với một đầu phải cố định, ta cần đầu trái nhỏ nhất.

$\textit{first}[q]$ là chỉ số tiền tố đầu tiên có phần dư $q$. Sắp xếp các phần dư xuất hiện theo $\textit{first}$ để được $\textit{order}$. $\textit{best}[s]$ lưu đầu trái nhỏ nhất hiện biết của một tiền tố phải có phần dư $s$. Ban đầu, nó bằng $\textit{first}[s]$, hoặc bằng một giá trị canh gác nếu phần dư đó chưa xuất hiện. Các giá trị khởi tạo này xử lý trường hợp không đổi dấu.

Duyệt chỉ số $i$ từ trái sang phải và đặt $a=\textit{nums}[i]\bmod k$. Một con trỏ ghi nhớ mỗi phần dư $a$ đã đi qua bao xa trong $\textit{order}$. Mọi phần dư $q$ với $\textit{first}[q]\le i$ đều có thể chứa vị trí $i$, nên ta đặt

$$
t=(q+2a)\bmod k,\qquad \textit{best}[t]=\min(\textit{best}[t],\textit{first}[q]).
$$

Sau khi đổi dấu chỉ số $i$, mảng con bắt đầu tại $\textit{first}[q]$ và kết thúc tại bất kỳ đầu phải nào về sau có phần dư $t$ đều hợp lệ. Con trỏ chỉ tiến về phía trước, nên mỗi cặp $(a,q)$ chỉ được xử lý một lần. Các đầu phải về sau vẫn chứa $i$, còn $\textit{best}$ giữ đầu trái nhỏ nhất.

Đặt $s=p[i+1]$. Khi $\textit{best}[s]$ không phải giá trị canh gác, cập nhật đáp án bằng $i+1-\textit{best}[s]$. Các giá trị âm được đưa về khoảng $[0,k)$.

Độ phức tạp thời gian là $O(n+k^2)$ và độ phức tạp không gian là $O(n+k)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def longestSubarray(self, nums: list[int], k: int) -> int:
        n = len(nums)
        p = [0] * (n + 1)
        for i in range(n):
            p[i + 1] = (p[i] + nums[i]) % k

        first = [-1] * k
        for i in range(n + 1):
            if first[p[i]] == -1:
                first[p[i]] = i

        order = [q for q in range(k) if first[q] != -1]
        order.sort(key=lambda q: first[q])

        pos = [0] * k
        best = [x if x != -1 else n + 1 for x in first]

        ans = 0
        for i, x in enumerate(nums):
            a = x % k
            while pos[a] < len(order) and first[order[pos[a]]] <= i:
                q = order[pos[a]]
                pos[a] += 1
                t = (q + 2 * a) % k
                best[t] = min(best[t], first[q])

            s = p[i + 1]
            if best[s] != n + 1:
                ans = max(ans, i + 1 - best[s])

        return ans
```

#### Java

```java
class Solution {
    public int longestSubarray(int[] nums, int k) {
        int n = nums.length;
        int[] p = new int[n + 1];
        for (int i = 0; i < n; ++i) {
            p[i + 1] = (p[i] + nums[i]) % k;
            if (p[i + 1] < 0) {
                p[i + 1] += k;
            }
        }

        int[] first = new int[k];
        Arrays.fill(first, -1);
        for (int i = 0; i <= n; ++i) {
            if (first[p[i]] == -1) {
                first[p[i]] = i;
            }
        }

        Integer[] order = new Integer[k];
        int m = 0;
        for (int q = 0; q < k; ++q) {
            if (first[q] != -1) {
                order[m++] = q;
            }
        }

        Arrays.sort(order, 0, m, (a, b) -> Integer.compare(first[a], first[b]));

        int[] pos = new int[k];
        int[] best = new int[k];
        Arrays.fill(best, Integer.MAX_VALUE);
        for (int q = 0; q < k; ++q) {
            if (first[q] != -1) {
                best[q] = first[q];
            }
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int a = nums[i] % k;
            if (a < 0) {
                a += k;
            }

            while (pos[a] < m && first[order[pos[a]]] <= i) {
                int q = order[pos[a]++];
                int t = (q + 2 * a) % k;
                best[t] = Math.min(best[t], first[q]);
            }

            int s = p[i + 1];
            if (best[s] != Integer.MAX_VALUE) {
                ans = Math.max(ans, i + 1 - best[s]);
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
    int longestSubarray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> p(n + 1);
        for (int i = 0; i < n; ++i) {
            p[i + 1] = (p[i] + nums[i]) % k;
            if (p[i + 1] < 0) {
                p[i + 1] += k;
            }
        }

        vector<int> first(k, -1);
        for (int i = 0; i <= n; ++i) {
            if (first[p[i]] == -1) {
                first[p[i]] = i;
            }
        }

        vector<int> order;
        for (int q = 0; q < k; ++q) {
            if (first[q] != -1) {
                order.push_back(q);
            }
        }

        ranges::sort(order, [&](int a, int b) {
            return first[a] < first[b];
        });

        vector<int> pos(k);
        vector<int> best(k, INT_MAX);
        for (int q = 0; q < k; ++q) {
            best[q] = first[q] == -1 ? INT_MAX : first[q];
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int a = nums[i] % k;
            if (a < 0) {
                a += k;
            }

            while (pos[a] < order.size() && first[order[pos[a]]] <= i) {
                int q = order[pos[a]++];
                int t = (q + 2 * a) % k;
                best[t] = min(best[t], first[q]);
            }

            int s = p[i + 1];
            if (best[s] != INT_MAX) {
                ans = max(ans, i + 1 - best[s]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func longestSubarray(nums []int, k int) int {
	n := len(nums)
	p := make([]int, n+1)
	for i := 0; i < n; i++ {
		p[i+1] = (p[i] + nums[i]) % k
		if p[i+1] < 0 {
			p[i+1] += k
		}
	}

	first := make([]int, k)
	for i := range first {
		first[i] = -1
	}
	for i := 0; i <= n; i++ {
		if first[p[i]] == -1 {
			first[p[i]] = i
		}
	}

	order := make([]int, 0, k)
	for q := 0; q < k; q++ {
		if first[q] != -1 {
			order = append(order, q)
		}
	}

	slices.SortFunc(order, func(a, b int) int {
		return first[a] - first[b]
	})

	pos := make([]int, k)
	best := make([]int, k)
	for q := 0; q < k; q++ {
		if first[q] == -1 {
			best[q] = int(^uint(0) >> 1)
		} else {
			best[q] = first[q]
		}
	}

	ans := 0
	for i, x := range nums {
		a := x % k
		if a < 0 {
			a += k
		}

		for pos[a] < len(order) && first[order[pos[a]]] <= i {
			q := order[pos[a]]
			pos[a]++
			t := (q + 2*a) % k
			best[t] = min(best[t], first[q])
		}

		s := p[i+1]
		if best[s] != int(^uint(0)>>1) {
			ans = max(ans, i+1-best[s])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function longestSubarray(nums: number[], k: number): number {
    const n = nums.length;
    const p = new Array<number>(n + 1).fill(0);
    for (let i = 0; i < n; ++i) {
        p[i + 1] = (p[i] + nums[i]) % k;
        if (p[i + 1] < 0) {
            p[i + 1] += k;
        }
    }

    const first = new Array<number>(k).fill(-1);
    for (let i = 0; i <= n; ++i) {
        if (first[p[i]] === -1) {
            first[p[i]] = i;
        }
    }

    const order = Array.from({ length: k }, (_, q) => q).filter(q => first[q] !== -1);

    order.sort((a, b) => first[a] - first[b]);

    const pos = new Array<number>(k).fill(0);
    const best = first.map(x => (x === -1 ? n + 1 : x));

    let ans = 0;
    for (let i = 0; i < n; ++i) {
        let a = nums[i] % k;
        if (a < 0) {
            a += k;
        }

        while (pos[a] < order.length && first[order[pos[a]]] <= i) {
            const q = order[pos[a]++];
            const t = (q + 2 * a) % k;
            best[t] = Math.min(best[t], first[q]);
        }

        const s = p[i + 1];
        if (best[s] !== n + 1) {
            ans = Math.max(ans, i + 1 - best[s]);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
