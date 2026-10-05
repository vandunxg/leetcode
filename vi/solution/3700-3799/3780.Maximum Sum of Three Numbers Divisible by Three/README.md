---
comments: true
difficulty: Medium
rating: 1584
source: Biweekly Contest 172 Q2
tags:
    - Greedy
    - Array
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3780. Maximum Sum of Three Numbers Divisible by Three](https://leetcode.com/problems/maximum-sum-of-three-numbers-divisible-by-three)

[中文文档](/solution/3700-3799/3780.Maximum%20Sum%20of%20Three%20Numbers%20Divisible%20by%20Three/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Nhiệm vụ của bạn là chọn <strong>chính xác ba</strong> số nguyên từ <code>nums</code> sao cho tổng của chúng chia hết cho ba.</p>

<p>Trả về <strong>tổng lớn nhất</strong> có thể có của bộ ba đó. Nếu không tồn tại bộ ba nào thỏa mãn, trả về 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [4,2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các bộ ba hợp lệ có tổng chia hết cho 3 là:</p>

<ul>
	<li><code>(4, 2, 3)</code> với tổng là <code>4 + 2 + 3 = 9</code>.</li>
	<li><code>(2, 3, 1)</code> với tổng là <code>2 + 3 + 1 = 6</code>.</li>
</ul>

<p>Do đó, đáp án là 9.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có bộ ba nào tạo thành tổng chia hết cho 3, nên đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Phân nhóm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Tổng của ba số là bội của $3$ khi và chỉ khi phần dư của chúng là $(0,0,0)$, $(1,1,1)$, $(2,2,2)$ hoặc $(0,1,2)$. Sau khi phân nhóm theo phần dư và sắp xếp mỗi nhóm theo thứ tự giảm dần, ta thử các cặp nhóm và lấy giá trị lớn nhất còn có thể chọn từ nhóm thứ ba.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng $\textit{nums}$, sau đó chia các phần tử trong mảng thành ba nhóm dựa trên phần dư khi chia cho $3$, lần lượt ký hiệu là $\textit{g}[0]$, $\textit{g}[1]$ và $\textit{g}[2]$. Trong đó, $\textit{g}[i]$ lưu tất cả các phần tử thỏa mãn $\textit{nums}[j] \bmod 3 = i$.

Tiếp theo, chúng ta liệt kê các trường hợp chọn một phần tử từ mỗi nhóm $\textit{g}[a]$ và $\textit{g}[b]$, với $a, b \in \{0, 1, 2\}$. Dựa trên phần dư khi chia cho $3$ của hai phần tử đã chọn, chúng ta xác định nhóm cần chọn phần tử thứ ba để tổng của bộ ba chia hết cho $3$. Cụ thể, phần tử thứ ba cần được chọn từ $\textit{g}[c]$, trong đó $c = (3 - (a + b) \bmod 3) \bmod 3$.

Với mỗi tổ hợp $(a, b)$, chúng ta lần lượt lấy phần tử lớn nhất từ $\textit{g}[a]$ và $\textit{g}[b]$, sau đó lấy phần tử lớn nhất từ $\textit{g}[c]$, tính tổng của ba phần tử này và cập nhật đáp án.

Độ phức tạp thời gian là $O(n \log n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumSum(self, nums: List[int]) -> int:
        nums.sort()
        g = [[] for _ in range(3)]
        for x in nums:
            g[x % 3].append(x)
        ans = 0
        for a in range(3):
            if g[a]:
                x = g[a].pop()
                for b in range(3):
                    if g[b]:
                        y = g[b].pop()
                        c = (3 - (a + b) % 3) % 3
                        if g[c]:
                            z = g[c][-1]
                            ans = max(ans, x + y + z)
                        g[b].append(y)
                g[a].append(x)
        return ans
```

#### Java

```java
class Solution {
    public int maximumSum(int[] nums) {
        Arrays.sort(nums);
        List<Integer>[] g = new ArrayList[3];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int x : nums) {
            g[x % 3].add(x);
        }
        int ans = 0;
        for (int a = 0; a < 3; a++) {
            if (!g[a].isEmpty()) {
                int x = g[a].remove(g[a].size() - 1);
                for (int b = 0; b < 3; b++) {
                    if (!g[b].isEmpty()) {
                        int y = g[b].remove(g[b].size() - 1);
                        int c = (3 - (a + b) % 3) % 3;
                        if (!g[c].isEmpty()) {
                            int z = g[c].get(g[c].size() - 1);
                            ans = Math.max(ans, x + y + z);
                        }
                        g[b].add(y);
                    }
                }
                g[a].add(x);
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
    int maximumSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> g(3);
        for (int x : nums) {
            g[x % 3].push_back(x);
        }
        int ans = 0;
        for (int a = 0; a < 3; a++) {
            if (!g[a].empty()) {
                int x = g[a].back();
                g[a].pop_back();
                for (int b = 0; b < 3; b++) {
                    if (!g[b].empty()) {
                        int y = g[b].back();
                        g[b].pop_back();
                        int c = (3 - (a + b) % 3) % 3;
                        if (!g[c].empty()) {
                            int z = g[c].back();
                            ans = max(ans, x + y + z);
                        }
                        g[b].push_back(y);
                    }
                }
                g[a].push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maximumSum(nums []int) int {
	sort.Ints(nums)
	g := make([][]int, 3)
	for _, x := range nums {
		g[x%3] = append(g[x%3], x)
	}
	ans := 0
	for a := 0; a < 3; a++ {
		if len(g[a]) > 0 {
			x := g[a][len(g[a])-1]
			g[a] = g[a][:len(g[a])-1]
			for b := 0; b < 3; b++ {
				if len(g[b]) > 0 {
					y := g[b][len(g[b])-1]
					g[b] = g[b][:len(g[b])-1]
					c := (3 - (a+b)%3) % 3
					if len(g[c]) > 0 {
						z := g[c][len(g[c])-1]
						ans = max(ans, x+y+z)
					}
					g[b] = append(g[b], y)
				}
			}
			g[a] = append(g[a], x)
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maximumSum(nums: number[]): number {
    nums.sort((a, b) => a - b);
    const g: number[][] = Array.from({ length: 3 }, () => []);
    for (const x of nums) {
        g[x % 3].push(x);
    }
    let ans = 0;
    for (let a = 0; a < 3; a++) {
        if (g[a].length > 0) {
            const x = g[a].pop()!;
            for (let b = 0; b < 3; b++) {
                if (g[b].length > 0) {
                    const y = g[b].pop()!;
                    const c = (3 - ((a + b) % 3)) % 3;
                    if (g[c].length > 0) {
                        const z = g[c][g[c].length - 1];
                        ans = Math.max(ans, x + y + z);
                    }
                    g[b].push(y);
                }
            }
            g[a].push(x);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
