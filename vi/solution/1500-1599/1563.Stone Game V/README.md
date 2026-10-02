---
comments: true
difficulty: Hard
rating: 2087
source: Weekly Contest 203 Q4
tags:
    - Array
    - Math
    - Dynamic Programming
    - Game Theory
---

<!-- problem:start -->

# [1563. Stone Game V](https://leetcode.com/problems/stone-game-v)

[中文文档](/solution/1500-1599/1563.Stone%20Game%20V/README.md)

## Mô tả

<!-- description:start -->

<p>Có một số viên đá <strong>xếp thành một hàng</strong>, mỗi viên có một giá trị nguyên được cho trong mảng <code>stoneValue</code>.</p>

<p>Ở mỗi vòng, Alice chia hàng thành <strong>hai hàng không rỗng</strong> (hàng trái và hàng phải), sau đó Bob tính giá trị mỗi hàng bằng tổng giá trị các viên đá trong hàng đó. Bob bỏ hàng có giá trị lớn hơn, và điểm của Alice tăng thêm giá trị của hàng còn lại. Nếu hai hàng có giá trị bằng nhau, Bob để Alice quyết định bỏ hàng nào. Vòng tiếp theo bắt đầu với hàng còn lại.</p>

<p>Trò chơi kết thúc khi chỉ còn <strong>một viên đá</strong>. Điểm của Alice ban đầu bằng <strong>0</strong>.</p>

<p>Trả về <i>điểm số lớn nhất Alice có thể đạt được</i>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> stoneValue = [6,2,3,4,5,5]
<strong>Output:</strong> 18
<strong>Giải thích:</strong> Ở vòng đầu, Alice chia hàng thành [6,2,3], [4,5,5]. Hàng trái có giá trị 11 và hàng phải có giá trị 14. Bob bỏ hàng phải, điểm của Alice lúc này là 11.
Ở vòng thứ hai, Alice chia hàng thành [6], [2,3]. Lần này Bob bỏ hàng trái, điểm của Alice thành 16 (11 + 5).
Ở vòng cuối, Alice chỉ có một cách chia thành [2], [3]. Bob bỏ hàng phải, điểm của Alice là 18 (16 + 2). Trò chơi kết thúc vì chỉ còn một viên đá.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> stoneValue = [7,7,7,7,7,7,7]
<strong>Output:</strong> 28
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> stoneValue = [4]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= stoneValue.length &lt;= 500</code></li>
	<li><code>1 &lt;= stoneValue[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Memoization + Cắt tỉa

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi lần chia, ta ghi điểm bằng nửa nhỏ hơn và chỉ tiếp tục trên nửa đó (hoặc một trong hai nửa nếu bằng nhau). Có $O(n^2)$ đoạn và $O(n)$ vị trí chia, phù hợp với $n\le 500$, nhưng đệ quy ngây thơ sẽ tính lại các đoạn.
>
> Gọi $dfs(i,j)$ là điểm tốt nhất của Alice trên $[i,j]$, với prefix sum giúp tính tổng hai nửa trong $O(1)$. Chỉ đệ quy trên nửa nhỏ hơn. Nếu $ans\ge 2l$, một lần chia có nửa trái nhỏ hơn không thể cải thiện kết quả; nếu $ans\ge 2r$, có thể bỏ qua các lần chia sau có nửa phải nhỏ hơn. Memoization lưu mỗi đoạn một lần.

<!-- thinking:end -->

Trước hết, tiền xử lý mảng prefix sum $\textit{s}$, trong đó $\textit{s}[i]$ là tổng $i$ phần tử đầu tiên của mảng $\textit{stoneValue}$.

Tiếp theo, thiết kế hàm $\textit{dfs}(i, j)$ biểu diễn điểm lớn nhất Alice có thể nhận từ mảng con $\textit{stoneValue}$ trong đoạn chỉ số $[i, j]$. Đáp án là $\textit{dfs}(0, n - 1)$.

Các bước tính hàm $\textit{dfs}(i, j)$ như sau:

- Nếu $i \geq j$, chỉ còn một viên đá và Alice không thể chia, nên trả về $0$.
- Ngược lại, duyệt vị trí chia $k$, tức $i \leq k < j$, chia mảng con $\textit{stoneValue}$ trong đoạn $[i, j]$ thành hai phần $[i, k]$ và $[k + 1, j]$. Tính $a$ và $b$ lần lượt là tổng của hai phần, sau đó tính $\textit{dfs}(i, k)$ và $\textit{dfs}(k + 1, j)$ rồi cập nhật đáp án.

Lưu ý, nếu $a < b$ và $\textit{ans} \geq a \times 2$, có thể bỏ qua lần chia này; nếu $a > b$ và $\textit{ans} \geq b \times 2$, có thể bỏ qua mọi lần chia sau và thoát vòng lặp.

Cuối cùng, trả về đáp án.

Để tránh tính lặp, dùng memoization và cắt tỉa để tối ưu việc duyệt.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$. Ở đây, $n$ là độ dài mảng $\textit{stoneValue}$.

<!-- tabs:start -->

#### Python3

```python
def max(a: int, b: int) -> int:
    return a if a > b else b


class Solution:
    def stoneGameV(self, stoneValue: List[int]) -> int:
        @cache
        def dfs(i: int, j: int) -> int:
            if i >= j:
                return 0
            ans = l = 0
            r = s[j + 1] - s[i]
            for k in range(i, j):
                l += stoneValue[k]
                r -= stoneValue[k]
                if l < r:
                    if ans >= l * 2:
                        continue
                    ans = max(ans, l + dfs(i, k))
                elif l > r:
                    if ans >= r * 2:
                        break
                    ans = max(ans, r + dfs(k + 1, j))
                else:
                    ans = max(ans, max(l + dfs(i, k), r + dfs(k + 1, j)))
            return ans

        s = list(accumulate(stoneValue, initial=0))
        return dfs(0, len(stoneValue) - 1)
```

#### Java

```java
class Solution {
    private int n;
    private int[] s;
    private int[] nums;
    private Integer[][] f;

    public int stoneGameV(int[] stoneValue) {
        n = stoneValue.length;
        s = new int[n + 1];
        nums = stoneValue;
        f = new Integer[n][n];
        for (int i = 1; i <= n; ++i) {
            s[i] = s[i - 1] + nums[i - 1];
        }
        return dfs(0, n - 1);
    }

    private int dfs(int i, int j) {
        if (i >= j) {
            return 0;
        }
        if (f[i][j] != null) {
            return f[i][j];
        }
        int ans = 0, l = 0, r = s[j + 1] - s[i];
        for (int k = i; k < j; ++k) {
            l += nums[k];
            r -= nums[k];
            if (l < r) {
                if (ans > l * 2) {
                    continue;
                }
                ans = Math.max(ans, l + dfs(i, k));
            } else if (l > r) {
                if (ans > r * 2) {
                    break;
                }
                ans = Math.max(ans, r + dfs(k + 1, j));
            } else {
                ans = Math.max(ans, Math.max(l + dfs(i, k), r + dfs(k + 1, j)));
            }
        }
        return f[i][j] = ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int stoneGameV(vector<int>& stoneValue) {
        int n = stoneValue.size();
        int s[n + 1];
        s[0] = 0;
        for (int i = 0; i < n; i++) {
            s[i + 1] = s[i] + stoneValue[i];
        }
        int f[n][n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](this auto&& dfs, int i, int j) -> int {
            if (i >= j) {
                return 0;
            }
            if (f[i][j] != -1) {
                return f[i][j];
            }
            int ans = 0, l = 0, r = s[j + 1] - s[i];
            for (int k = i; k < j; ++k) {
                l += stoneValue[k];
                r -= stoneValue[k];
                if (l < r) {
                    if (ans > l * 2) {
                        continue;
                    }
                    ans = max(ans, l + dfs(i, k));
                } else if (l > r) {
                    if (ans > r * 2) {
                        break;
                    }
                    ans = max(ans, r + dfs(k + 1, j));
                } else {
                    ans = max({ans, l + dfs(i, k), r + dfs(k + 1, j)});
                }
            }
            return f[i][j] = ans;
        };
        return dfs(0, n - 1);
    }
};
```

#### Go

```go
func stoneGameV(stoneValue []int) int {
	n := len(stoneValue)
	s := make([]int, n+1)
	for i, x := range stoneValue {
		s[i+1] = s[i] + x
	}
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, n)
		for j := range f[i] {
			f[i][j] = -1
		}
	}
	var dfs func(int, int) int
	dfs = func(i, j int) int {
		if i >= j {
			return 0
		}
		if f[i][j] != -1 {
			return f[i][j]
		}
		ans, l, r := 0, 0, s[j+1]-s[i]
		for k := i; k < j; k++ {
			l += stoneValue[k]
			r -= stoneValue[k]
			if l < r {
				if ans > l*2 {
					continue
				}
				ans = max(ans, dfs(i, k)+l)
			} else if l > r {
				if ans > r*2 {
					break
				}
				ans = max(ans, dfs(k+1, j)+r)
			} else {
				ans = max(ans, max(dfs(i, k), dfs(k+1, j))+l)
			}
		}
		f[i][j] = ans
		return ans
	}
	return dfs(0, n-1)
}
```

#### TypeScript

```ts
function stoneGameV(stoneValue: number[]): number {
    const n = stoneValue.length;
    const s: number[] = Array(n + 1).fill(0);
    for (let i = 0; i < n; i++) {
        s[i + 1] = s[i] + stoneValue[i];
    }
    const f: number[][] = Array.from({ length: n }, () => Array(n).fill(-1));

    const dfs = (i: number, j: number): number => {
        if (i >= j) {
            return 0;
        }
        if (f[i][j] !== -1) {
            return f[i][j];
        }
        let [ans, l, r] = [0, 0, s[j + 1] - s[i]];
        for (let k = i; k < j; ++k) {
            l += stoneValue[k];
            r -= stoneValue[k];
            if (l < r) {
                if (ans > l * 2) {
                    continue;
                }
                ans = Math.max(ans, l + dfs(i, k));
            } else if (l > r) {
                if (ans > r * 2) {
                    break;
                }
                ans = Math.max(ans, r + dfs(k + 1, j));
            } else {
                ans = Math.max(ans, l + dfs(i, k), r + dfs(k + 1, j));
            }
        }
        return (f[i][j] = ans);
    };

    return dfs(0, n - 1);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
