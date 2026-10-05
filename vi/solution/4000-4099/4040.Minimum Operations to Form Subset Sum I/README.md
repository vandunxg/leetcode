---
comments: true
difficulty: Medium
rating: 1869
source: Weekly Contest 517 Q3
---

<!-- problem:start -->

# [4040. Minimum Operations to Form Subset Sum I](https://leetcode.com/problems/minimum-operations-to-form-subset-sum-i)

[中文文档](/solution/4000-4099/4040.Minimum%20Operations%20to%20Form%20Subset%20Sum%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code> và một số nguyên <code>sum</code>.</p>

<p>Trong một <strong>thao tác</strong>, chọn một phần tử có giá trị hiện tại là <code>x</code> và thay thế nó bằng <code>2 * x</code> hoặc <code>floor(x / 2)</code>.</p>

<p>Với mỗi phần tử, mọi thao tác <strong>nhân</strong> được thực hiện trên phần tử đó phải xảy ra <strong>trước</strong> mọi thao tác <strong>chia</strong> được thực hiện trên phần tử đó.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để một <span data-keyword="subset">tập con</span> nào đó của mảng sau cùng có tổng <strong>chính xác</strong> bằng <code>sum</code>. Nếu không thể, trả về -1.</p>

<p>Hàm <code>floor()</code> trả về phần nguyên của phép chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,6,10], sum = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>nums[0] = 5</code> hai lần: <code>5 &rarr; 2 &rarr; 1</code>, tốn 2 thao tác.</li>
	<li>Chia <code>nums[1] = 6</code> một lần: <code>6 &rarr; 3</code>, tốn 1 thao tác.</li>
	<li>Sau các thao tác này, <code>nums = [1, 3, 10]</code>. Tập con <code>{1, 3}</code> có tổng bằng 4 và tổng cộng tốn 3 thao tác.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,2], sum = 13</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>nums[0] = 10</code> một lần: <code>10 &rarr; 5</code>, tốn 1 thao tác.</li>
	<li>Nhân <code>nums[1] = 2</code> hai lần: <code>2 &rarr; 4 &rarr; 8</code>, tốn 2 thao tác.</li>
	<li>Sau các thao tác này, <code>nums = [5, 8]</code>. Tập con <code>{5, 8}</code> có tổng bằng 13 và tổng cộng tốn 3 thao tác.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,3], sum = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Không có chuỗi thao tác nào giúp một tập con của <code>nums</code> có tổng bằng 8, nên đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 500</code></li>
	<li><code>1 &lt;= sum &lt;= 5000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: 0-1 Knapsack

<!-- thinking:start -->

> **Tư duy**
>
> Việc nhân và chia cùng một phần tử lần lượt $a$ và $b$ lần có thể được thay thế bằng $|a-b|$ thao tác theo một hướng; trộn lẫn hai loại thao tác chỉ làm lãng phí bước. Vì vậy, mỗi phần tử chỉ được phóng đại bằng cách nhân với $2$, chỉ được chia cho $2$, hoặc không được chọn.
>
> Các cặp (giá trị, chi phí) thu được là các vật phẩm của bài toán knapsack $0$-$1$ với sức chứa $\textit{sum}$. Với $n\le 100$ và $S\le 5000$, việc liệt kê $O(\log S)$ cách biến đổi là chấp nhận được.
>
> Cập nhật sức chứa theo chiều ngược đảm bảo mỗi phần tử được sử dụng nhiều nhất một lần. Nếu $f[\textit{sum}]$ vẫn là vô cực, bài toán không có lời giải.

<!-- thinking:end -->

Thực hiện $a$ phép nhân rồi $b$ phép chia trên một phần tử sẽ cho $\lfloor x \times 2^a / 2^b \rfloor$, chính xác là $x \times 2^{a-b}$ hoặc $\lfloor x / 2^{b-a} \rfloor$. Có thể đạt được cùng giá trị bằng chỉ $|a - b|$ thao tác thay vì $a + b$, nên việc trộn lẫn hai hướng không bao giờ có lợi. Do đó, mỗi phần tử chỉ có hai nhóm giá trị có thể đạt được: $x \times 2^i$ hoặc $\lfloor x / 2^i \rfloor$, mỗi giá trị tốn $i$ thao tác; nếu không đưa phần tử vào tập con thì chi phí là 0.

Điều này chuyển bài toán thành bài toán knapsack 0-1: mỗi phần tử đóng góp nhiều nhất một cặp (giá trị, chi phí), và ta muốn đạt đúng sức chứa $\textit{sum}$ với chi phí nhỏ nhất.

Ta định nghĩa $f[w]$ là số thao tác nhỏ nhất cần có để một tập con có tổng chính xác bằng $w$, với $f[0] = 0$ và các giá trị còn lại được đặt thành $+\infty$. Với mỗi phần tử $x$, ta duyệt sức chứa $w$ từ lớn đến nhỏ, liệt kê mọi giá trị $y$ mà $x$ có thể trở thành cùng với chi phí $i$, rồi cập nhật $f[w]$ bằng $f[w - y] + i$ khi $y \leq w$. Nếu cuối cùng $f[\textit{sum}]$ vẫn là $+\infty$, không tồn tại chuỗi thao tác hợp lệ và ta trả về $-1$; ngược lại, trả về $f[\textit{sum}]$.

Độ phức tạp thời gian là $O(n \times S \times \log S)$ và độ phức tạp không gian là $O(S)$. Trong đó, $n$ là độ dài mảng $\textit{nums}$ và $S$ là giá trị $\textit{sum}$ đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], sum: int) -> int:
        f = [0] + [inf] * sum
        for x in nums:
            for w in range(sum, -1, -1):
                i, y = 0, x
                while y <= w:
                    f[w] = min(f[w], f[w - y] + i)
                    i += 1
                    y <<= 1
                i, y = 1, x >> 1
                while y > 0:
                    if y <= w:
                        f[w] = min(f[w], f[w - y] + i)
                    i += 1
                    y >>= 1
        return -1 if f[sum] == inf else f[sum]
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int sum) {
        int inf = Integer.MAX_VALUE / 2;
        int[] f = new int[sum + 1];
        Arrays.fill(f, inf);
        f[0] = 0;

        for (int x : nums) {
            for (int w = sum; w >= 0; --w) {
                int i = 0, y = x;
                while (y <= w) {
                    f[w] = Math.min(f[w], f[w - y] + i);
                    ++i;
                    y <<= 1;
                }

                i = 1;
                y = x >> 1;
                while (y > 0) {
                    if (y <= w) {
                        f[w] = Math.min(f[w], f[w - y] + i);
                    }
                    ++i;
                    y >>= 1;
                }
            }
        }

        return f[sum] == inf ? -1 : f[sum];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(vector<int>& nums, int sum) {
        const int inf = 1e9;
        vector<int> f(sum + 1, inf);
        f[0] = 0;

        for (int x : nums) {
            for (int w = sum; w >= 0; --w) {
                int i = 0, y = x;
                while (y <= w) {
                    f[w] = min(f[w], f[w - y] + i);
                    ++i;
                    y <<= 1;
                }

                i = 1;
                y = x >> 1;
                while (y > 0) {
                    if (y <= w) {
                        f[w] = min(f[w], f[w - y] + i);
                    }
                    ++i;
                    y >>= 1;
                }
            }
        }

        return f[sum] == inf ? -1 : f[sum];
    }
};
```

#### Go

```go
func minOperations(nums []int, sum int) int {
	const inf = int(1e9)

	f := make([]int, sum+1)
	for i := range f {
		f[i] = inf
	}
	f[0] = 0

	for _, x := range nums {
		for w := sum; w >= 0; w-- {
			i, y := 0, x
			for y <= w {
				f[w] = min(f[w], f[w-y]+i)
				i++
				y <<= 1
			}

			i, y = 1, x>>1
			for y > 0 {
				if y <= w {
					f[w] = min(f[w], f[w-y]+i)
				}
				i++
				y >>= 1
			}
		}
	}

	if f[sum] == inf {
		return -1
	}
	return f[sum]
}
```

#### TypeScript

```ts
function minOperations(nums: number[], sum: number): number {
    const inf = 1e9;
    const f = Array(sum + 1).fill(inf);
    f[0] = 0;

    for (const x of nums) {
        for (let w = sum; w >= 0; --w) {
            let i = 0;
            let y = x;

            while (y <= w) {
                f[w] = Math.min(f[w], f[w - y] + i);
                ++i;
                y *= 2;
            }

            i = 1;
            y = Math.floor(x / 2);

            while (y > 0) {
                if (y <= w) {
                    f[w] = Math.min(f[w], f[w - y] + i);
                }
                ++i;
                y = Math.floor(y / 2);
            }
        }
    }

    return f[sum] === inf ? -1 : f[sum];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
