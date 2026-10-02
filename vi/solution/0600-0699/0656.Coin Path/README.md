---
comments: true
difficulty: Hard
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [656. Coin Path 🔒](https://leetcode.com/problems/coin-path)

[中文文档](/solution/0600-0699/0656.Coin%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>coins</code> có độ dài <code>n</code> (đánh chỉ số từ <strong>1</strong>) và số nguyên <code>maxJump</code>. Bạn có thể nhảy đến chỉ số <code>i</code> trong mảng <code>coins</code> nếu <code>coins[i] != -1</code>; khi đến chỉ số <code>i</code>, bạn phải trả chi phí <code>coins[i]</code>. Ngoài ra, nếu đang ở chỉ số <code>i</code>, bạn chỉ có thể nhảy đến chỉ số <code>i + k</code> sao cho <code>i + k &lt;= n</code> và <code>k</code> nằm trong khoảng <code>[1, maxJump]</code>.</p>

<p>Ban đầu, bạn ở chỉ số <code>1</code> (<code>coins[1]</code> khác <code>-1</code>). Hãy tìm đường đi đến chỉ số n với chi phí nhỏ nhất.</p>

<p>Hãy trả về mảng số nguyên chứa các chỉ số bạn sẽ lần lượt đi qua để đến chỉ số n với chi phí nhỏ nhất. Nếu có nhiều đường đi cùng chi phí, hãy trả về đường đi <strong>nhỏ nhất theo thứ tự từ điển</strong>. Nếu không thể đến chỉ số n, hãy trả về mảng rỗng.</p>

<p>Đường đi <code>p1 = [Pa<sub>1</sub>, Pa<sub>2</sub>, ..., Pa<sub>x</sub>]</code> có độ dài <code>x</code> được xem là <strong>nhỏ hơn theo thứ tự từ điển</strong> đường đi <code>p2 = [Pb<sub>1</sub>, Pb<sub>2</sub>, ..., Pb<sub>x</sub>]</code> có độ dài <code>y</code> khi và chỉ khi tại chỉ số <code>j</code> đầu tiên mà <code>Pa<sub>j</sub></code> và <code>Pb<sub>j</sub></code> khác nhau, ta có <code>Pa<sub>j</sub> &lt; Pb<sub>j</sub></code>; nếu không có chỉ số <code>j</code> như vậy thì <code>x &lt; y</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> coins = [1,2,4,-1,2], maxJump = 2
<strong>Đầu ra:</strong> [1,3,5]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> coins = [1,2,4,-1,2], maxJump = 1
<strong>Đầu ra:</strong> []
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= coins.length &lt;= 1000</code></li>
	<li><code>-1 &lt;= coins[i] &lt;= 100</code></li>
	<li><code>coins[1] != -1</code></li>
	<li><code>1 &lt;= maxJump &lt;= 100</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Nhảy đến cuối với mỗi bước $\le \textit{maxJump}$, bỏ qua vị trí có giá trị $-1$, rồi tối ưu chi phí và sau đó là dãy chỉ số. Nếu lưu parent theo chiều xuôi, cần xử lý trường hợp hòa theo thứ tự từ điển.
>
> Gọi $f[i]$ là chi phí nhỏ nhất để đi từ $i$ đến cuối. Khôi phục đường đi từ trái sang phải bằng cách luôn chọn chỉ số $i$ nhỏ nhất sao cho $f[i]$ bằng ngân sách còn lại; cách này cho đường đi nhỏ nhất theo thứ tự từ điển.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def cheapestJump(self, coins: List[int], maxJump: int) -> List[int]:
        if coins[-1] == -1:
            return []
        n = len(coins)
        f = [inf] * n
        f[-1] = coins[-1]
        for i in range(n - 2, -1, -1):
            if coins[i] != -1:
                for j in range(i + 1, min(n, i + maxJump + 1)):
                    if f[i] > f[j] + coins[i]:
                        f[i] = f[j] + coins[i]
        if f[0] == inf:
            return []
        ans = []
        s = f[0]
        for i in range(n):
            if f[i] == s:
                s -= coins[i]
                ans.append(i + 1)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> cheapestJump(int[] coins, int maxJump) {
        int n = coins.length;
        List<Integer> ans = new ArrayList<>();
        if (coins[n - 1] == -1) {
            return ans;
        }
        int[] f = new int[n];
        final int inf = 1 << 30;
        Arrays.fill(f, inf);
        f[n - 1] = coins[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            if (coins[i] != -1) {
                for (int j = i + 1; j < Math.min(n, i + maxJump + 1); ++j) {
                    if (f[i] > f[j] + coins[i]) {
                        f[i] = f[j] + coins[i];
                    }
                }
            }
        }
        if (f[0] == inf) {
            return ans;
        }
        for (int i = 0, s = f[0]; i < n; ++i) {
            if (f[i] == s) {
                s -= coins[i];
                ans.add(i + 1);
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
    vector<int> cheapestJump(vector<int>& coins, int maxJump) {
        int n = coins.size();
        vector<int> ans;
        if (coins[n - 1] == -1) {
            return ans;
        }
        int f[n];
        const int inf = 1 << 30;
        f[n - 1] = coins[n - 1];
        for (int i = n - 2; ~i; --i) {
            f[i] = inf;
            if (coins[i] != -1) {
                for (int j = i + 1; j < min(n, i + maxJump + 1); ++j) {
                    f[i] = min(f[i], f[j] + coins[i]);
                }
            }
        }
        if (f[0] == inf) {
            return ans;
        }
        for (int i = 0, s = f[0]; i < n; ++i) {
            if (f[i] == s) {
                s -= coins[i];
                ans.push_back(i + 1);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func cheapestJump(coins []int, maxJump int) (ans []int) {
	n := len(coins)
	if coins[n-1] == -1 {
		return
	}
	f := make([]int, n)
	f[n-1] = coins[n-1]
	const inf = 1 << 30
	for i := n - 2; i >= 0; i-- {
		f[i] = inf
		if coins[i] != -1 {
			for j := i + 1; j < n && j < i+maxJump+1; j++ {
				if f[i] > f[j]+coins[i] {
					f[i] = f[j] + coins[i]
				}
			}
		}
	}
	if f[0] == inf {
		return
	}
	for i, s := 0, f[0]; i < n; i++ {
		if f[i] == s {
			s -= coins[i]
			ans = append(ans, i+1)
		}
	}
	return
}
```

#### TypeScript

```ts
function cheapestJump(coins: number[], maxJump: number): number[] {
    const n = coins.length;
    const ans: number[] = [];
    if (coins[n - 1] == -1) {
        return ans;
    }
    const inf = 1 << 30;
    const f: number[] = new Array(n).fill(inf);
    f[n - 1] = coins[n - 1];
    for (let i = n - 2; i >= 0; --i) {
        if (coins[i] !== -1) {
            for (let j = i + 1; j < Math.min(n, i + maxJump + 1); ++j) {
                f[i] = Math.min(f[i], f[j] + coins[i]);
            }
        }
    }
    if (f[0] === inf) {
        return ans;
    }
    for (let i = 0, s = f[0]; i < n; ++i) {
        if (f[i] == s) {
            s -= coins[i];
            ans.push(i + 1);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
