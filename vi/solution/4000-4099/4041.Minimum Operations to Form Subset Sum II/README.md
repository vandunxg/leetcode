---
comments: true
difficulty: Hard
rating: 2099
source: Weekly Contest 517 Q4
---

<!-- problem:start -->

# [4041. Minimum Operations to Form Subset Sum II](https://leetcode.com/problems/minimum-operations-to-form-subset-sum-ii)

[中文文档](/solution/4000-4099/4041.Minimum%20Operations%20to%20Form%20Subset%20Sum%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> và một số nguyên <code>sum</code>.</p>

<p>Trong một <strong>phép toán</strong>, chọn một phần tử có giá trị hiện tại là <code>x</code> và thay thế nó bằng <code>2 * x</code> hoặc <code>floor(x / 2)</code>.</p>

<p>Với mỗi phần tử, có thể thực hiện các phép <strong>nhân</strong> và <strong>chia</strong> theo bất kỳ thứ tự nào.</p>

<p>Trả về số phép toán <strong>nhỏ nhất</strong> cần thực hiện để một <span data-keyword="subset">tập con</span> nào đó của mảng sau biến đổi có tổng <strong>đúng bằng</strong> <code>sum</code>. Nếu không thể, trả về -1.</p>

<p>Hàm <code>floor()</code> trả về phần nguyên của phép chia.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,2], sum = 13</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Chia <code>nums[0] = 10</code> một lần: <code>10 &rarr; 5</code>, tốn 1 phép toán.</li>
	<li>Nhân <code>nums[1] = 2</code> hai lần: <code>2 &rarr; 4 &rarr; 8</code>, tốn 2 phép toán.</li>
	<li>Sau các phép toán này, <code>nums = [5, 8]</code>. Tập con <code>{5, 8}</code> có tổng bằng 13 với tổng cộng 3 phép toán.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [6,3], sum = 8</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Biến <code>nums[1] = 3</code> thành 2 bằng 2 phép toán:

    <ul>
    	<li>Chia <code>nums[1]</code> để được 1.</li>
    	<li>Nhân <code>nums[1] = 1</code> để được 2.</li>
    </ul>
    </li>
    <li>Sau các phép toán này, <code>nums = [6, 2]</code>. Tập con <code>{6, 2}</code> có tổng bằng 8 với tổng cộng 2 phép toán.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,2], sum = 7</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không có chuỗi phép toán nào giúp một tập con của <code>nums</code> có tổng bằng 7, nên đáp án là -1.</li>
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

### Lời giải 1: Bài toán ba lô 0-1

<!-- thinking:start -->

> **Tư duy**
>
> Khác với bài trước, phép nhân và phép chia có thể xen kẽ. Một phép nhân xảy ra trước một phép chia sẽ triệt tiêu nhau, vì vậy mọi chuỗi đều rút gọn thành $i$ lần chia rồi $j$ lần nhân: giá trị là $\lfloor x/2^i\rfloor\times 2^j$ với chi phí $i+j$.
>
> Ta vẫn lấp đầy sức chứa $\textit{sum}$ bằng bài toán ba lô $0$-$1$, nhưng giờ đây phải duyệt thêm một chiều cho mỗi phần tử. Việc cập nhật ngược giúp mỗi phần tử chỉ giữ lại nhiều nhất một cặp $(i,j)$.
>
> Nếu $f[\textit{sum}]$ vẫn là vô hạn thì đáp án là $-1$.

<!-- thinking:end -->

Khác với bài trước, phép nhân và phép chia có thể xen kẽ theo bất kỳ thứ tự nào. Nhận thấy một phép nhân ngay trước một phép chia không làm thay đổi giá trị, vì $\lfloor 2x / 2 \rfloor = x$, nên bất kỳ phép nhân nào xảy ra trước một phép chia đều có thể được triệt tiêu, lãng phí hai phép toán. Sau khi liên tục triệt tiêu các cặp như vậy, mọi chuỗi đều rút gọn thành "chia $i$ lần, sau đó nhân $j$ lần", biến $x$ thành $\lfloor x / 2^i \rfloor \times 2^j$ với chi phí $i + j$ phép toán.

Điều này đưa bài toán về bài toán ba lô 0-1: mỗi phần tử đóng góp nhiều nhất một cặp (giá trị, chi phí), và ta muốn lấp đầy chính xác sức chứa $\textit{sum}$ với chi phí nhỏ nhất.

Ta định nghĩa $f[w]$ là số phép toán nhỏ nhất cần thiết để một tập con có tổng đúng bằng $w$, với $f[0] = 0$ và tất cả các phần tử còn lại được đặt thành $+\infty$. Với mỗi phần tử $x$, ta duyệt sức chứa $w$ từ lớn đến nhỏ, liệt kê số lần chia $i$ và số lần nhân $j$ để nhận được giá trị $y = \lfloor x / 2^i \rfloor \times 2^j$, rồi cập nhật $f[w]$ bằng $f[w - y] + i + j$ khi $y \leq w$. Nếu cuối cùng $f[\textit{sum}]$ vẫn là $+\infty$, không tồn tại chuỗi phép toán hợp lệ và ta trả về $-1$; ngược lại, trả về $f[\textit{sum}]$.

Độ phức tạp thời gian là $O(n \times S \times \log M \times \log S)$, và độ phức tạp không gian là $O(S)$. Trong đó, $n$ và $M$ lần lượt là độ dài và giá trị lớn nhất của mảng $\textit{nums}$, còn $S$ là giá trị $\textit{sum}$ đã cho.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int], sum: int) -> int:
        inf = 10**9
        f = [0] + [inf] * sum

        for x in nums:
            for w in range(sum, -1, -1):
                i, y = 0, x
                while y <= w:
                    f[w] = min(f[w], f[w - y] + i)
                    i += 1
                    y *= 2

                i, y = 1, x // 2
                while y > 0:
                    j, z = 0, y
                    while z <= w:
                        f[w] = min(f[w], f[w - z] + i + j)
                        j += 1
                        z *= 2
                    i += 1
                    y //= 2

        return -1 if f[sum] == inf else f[sum]
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums, int sum) {
        int inf = (int) 1e9;
        int[] f = new int[sum + 1];
        Arrays.fill(f, inf);
        f[0] = 0;

        for (int x : nums) {
            for (int w = sum; w >= 0; --w) {
                int i = 0, y = x;
                while (y <= w) {
                    f[w] = Math.min(f[w], f[w - y] + i);
                    ++i;
                    y *= 2;
                }

                i = 1;
                y = x / 2;
                while (y > 0) {
                    int j = 0, z = y;
                    while (z <= w) {
                        f[w] = Math.min(f[w], f[w - z] + i + j);
                        ++j;
                        z *= 2;
                    }
                    ++i;
                    y /= 2;
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
                for (int i = 0, y = x; y <= w; i++, y *= 2) {
                    f[w] = min(f[w], f[w - y] + i);
                }

                for (int i = 1, y = x / 2; y > 0; i++, y /= 2) {
                    for (int j = 0, z = y; z <= w; j++, z *= 2) {
                        f[w] = min(f[w], f[w - z] + i + j);
                    }
                }
            }
        }

        return f[sum] < inf ? f[sum] : -1;
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
			for i, y := 0, x; y <= w; i, y = i+1, y*2 {
				f[w] = min(f[w], f[w-y]+i)
			}

			for i, y := 1, x/2; y > 0; i, y = i+1, y/2 {
				for j, z := 0, y; z <= w; j, z = j+1, z*2 {
					f[w] = min(f[w], f[w-z]+i+j)
				}
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
            for (let i = 0, y = x; y <= w; ++i, y *= 2) {
                f[w] = Math.min(f[w], f[w - y] + i);
            }

            for (let i = 1, y = Math.floor(x / 2); y > 0; ++i, y = Math.floor(y / 2)) {
                for (let j = 0, z = y; z <= w; ++j, z *= 2) {
                    f[w] = Math.min(f[w], f[w - z] + i + j);
                }
            }
        }
    }

    return f[sum] === inf ? -1 : f[sum];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
