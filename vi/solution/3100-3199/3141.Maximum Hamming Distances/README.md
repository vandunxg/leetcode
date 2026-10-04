---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Breadth-First Search
    - Array
---

<!-- problem:start -->

# [3141. Maximum Hamming Distances 🔒](https://leetcode.com/problems/maximum-hamming-distances)

[中文文档](/solution/3100-3199/3141.Maximum%20Hamming%20Distances/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code> và một số nguyên <code>m</code>, trong đó mỗi phần tử <code>nums[i]</code> thỏa mãn <code>0 &lt;= nums[i] &lt; 2<sup>m</sup></code>, hãy trả về một mảng <code>answer</code>. Mảng <code>answer</code> có cùng độ dài với <code>nums</code>, trong đó mỗi phần tử <code>answer[i]</code> biểu thị <strong>khoảng cách Hamming </strong><em>lớn nhất</em> giữa <code>nums[i]</code> và một phần tử bất kỳ <code>nums[j]</code> khác trong mảng.</p>

<p><strong>Khoảng cách Hamming</strong> giữa hai số nguyên nhị phân được định nghĩa là số vị trí mà các bit tương ứng khác nhau (thêm các số 0 ở đầu nếu cần).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [9,12,9,11], m = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,3,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của <code>nums = [1001,1100,1001,1011]</code>.</p>

<p>Khoảng cách Hamming lớn nhất cho mỗi chỉ số là:</p>

<ul>
	<li><code>nums[0]</code>: 1001 và 1100 có khoảng cách bằng 2.</li>
	<li><code>nums[1]</code>: 1100 và 1011 có khoảng cách bằng 3.</li>
	<li><code>nums[2]</code>: 1001 và 1100 có khoảng cách bằng 2.</li>
	<li><code>nums[3]</code>: 1011 và 1100 có khoảng cách bằng 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,4,6,10], m = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,3,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Biểu diễn nhị phân của <code>nums = [0011,0100,0110,1010]</code>.</p>

<p>Khoảng cách Hamming lớn nhất cho mỗi chỉ số là:</p>

<ul>
	<li><code>nums[0]</code>: 0011 và 0100 có khoảng cách bằng 3.</li>
	<li><code>nums[1]</code>: 0100 và 0011 có khoảng cách bằng 3.</li>
	<li><code>nums[2]</code>: 0110 và 1010 có khoảng cách bằng 2.</li>
	<li><code>nums[3]</code>: 1010 và 0100 có khoảng cách bằng 3.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= m &lt;= 17</code></li>
	<li><code>2 &lt;= nums.length &lt;= 2<sup>m</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>m</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi giá trị, ta cần tìm khoảng cách Hamming lớn nhất của nó tới một phần tử trong mảng. So sánh từng cặp sẽ quá chậm khi $m$ đạt đến $17$.
>
> Khoảng cách lớn nhất đó bằng $m$ trừ đi khoảng cách nhỏ nhất tới một phần tử trong mảng. Thực hiện BFS trên siêu lập phương, lần lượt đảo từng bit bắt đầu từ các giá trị trong mảng, ta có thể tìm được láng giềng gần nhất trong mảng cho mọi mask.
>
> Gán $dist[x]=0$ với mọi $x$ trong $nums$ rồi mở rộng. Với một truy vấn $x$, đáp án là $m-dist[x\oplus(2^m-1)]$, biến khoảng cách tới phần bù thành khoảng cách lớn nhất.

<!-- thinking:end -->

Bài toán yêu cầu tìm khoảng cách Hamming lớn nhất giữa mỗi phần tử và các phần tử khác trong mảng. Ta có thể suy nghĩ ngược: với mỗi phần tử, lấy phần bù của nó rồi tìm khoảng cách Hamming nhỏ nhất tới các phần tử khác trong mảng. Khi đó, khoảng cách Hamming lớn nhất cần tìm bằng $m$ trừ đi khoảng cách Hamming nhỏ nhất này.

Ta có thể sử dụng Breadth-First Search (BFS) để tìm khoảng cách Hamming nhỏ nhất từ mỗi phần tử sau khi lấy phần bù tới các phần tử khác trong mảng.

Các bước cụ thể như sau:

1. Khởi tạo một mảng $\textit{dist}$ có độ dài $2^m$ để ghi lại khoảng cách Hamming nhỏ nhất từ mỗi phần tử sau khi lấy phần bù tới các phần tử khác trong mảng. Ban đầu, tất cả các giá trị được đặt thành $-1$.
2. Duyệt mảng $\textit{nums}$, đặt phần bù của mỗi phần tử thành $0$ và thêm nó vào hàng đợi $\textit{q}$.
3. Bắt đầu từ $k = 1$, liên tục duyệt hàng đợi $\textit{q}$. Mỗi lần, lấy các phần tử trong hàng đợi ra, thực hiện $m$ phép đảo bit trên chúng, thêm các phần tử sau khi đảo vào hàng đợi $\textit{t}$ và đặt khoảng cách Hamming nhỏ nhất tới phần tử ban đầu là $k$.
4. Lặp lại bước 3 cho đến khi hàng đợi rỗng.

Cuối cùng, duyệt mảng $\textit{nums}$, lấy phần bù của mỗi phần tử làm chỉ số và lấy khoảng cách Hamming nhỏ nhất tương ứng từ mảng $\textit{dist}$. Khi đó, $m$ trừ đi giá trị này chính là khoảng cách Hamming lớn nhất cần tìm.

Độ phức tạp thời gian là $O(2^m)$, và độ phức tạp không gian là $O(2^m)$, trong đó $m$ là số nguyên được cho trong đề bài.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxHammingDistances(self, nums: List[int], m: int) -> List[int]:
        dist = [-1] * (1 << m)
        for x in nums:
            dist[x] = 0
        q = nums
        k = 1
        while q:
            t = []
            for x in q:
                for i in range(m):
                    y = x ^ (1 << i)
                    if dist[y] == -1:
                        t.append(y)
                        dist[y] = k
            q = t
            k += 1
        return [m - dist[x ^ ((1 << m) - 1)] for x in nums]
```

#### Java

```java
class Solution {
    public int[] maxHammingDistances(int[] nums, int m) {
        int[] dist = new int[1 << m];
        Arrays.fill(dist, -1);
        Deque<Integer> q = new ArrayDeque<>();
        for (int x : nums) {
            dist[x] = 0;
            q.offer(x);
        }
        for (int k = 1; !q.isEmpty(); ++k) {
            for (int t = q.size(); t > 0; --t) {
                int x = q.poll();
                for (int i = 0; i < m; ++i) {
                    int y = x ^ (1 << i);
                    if (dist[y] == -1) {
                        q.offer(y);
                        dist[y] = k;
                    }
                }
            }
        }
        for (int i = 0; i < nums.length; ++i) {
            nums[i] = m - dist[nums[i] ^ ((1 << m) - 1)];
        }
        return nums;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> maxHammingDistances(vector<int>& nums, int m) {
        int dist[1 << m];
        memset(dist, -1, sizeof(dist));
        queue<int> q;
        for (int x : nums) {
            dist[x] = 0;
            q.push(x);
        }
        for (int k = 1; q.size(); ++k) {
            for (int t = q.size(); t; --t) {
                int x = q.front();
                q.pop();
                for (int i = 0; i < m; ++i) {
                    int y = x ^ (1 << i);
                    if (dist[y] == -1) {
                        dist[y] = k;
                        q.push(y);
                    }
                }
            }
        }
        for (int& x : nums) {
            x = m - dist[x ^ ((1 << m) - 1)];
        }
        return nums;
    }
};
```

#### Go

```go
func maxHammingDistances(nums []int, m int) []int {
	dist := make([]int, 1<<m)
	for i := range dist {
		dist[i] = -1
	}
	q := []int{}
	for _, x := range nums {
		dist[x] = 0
		q = append(q, x)
	}
	for k := 1; len(q) > 0; k++ {
		t := []int{}
		for _, x := range q {
			for i := 0; i < m; i++ {
				y := x ^ (1 << i)
				if dist[y] == -1 {
					dist[y] = k
					t = append(t, y)
				}
			}
		}
		q = t
	}
	for i, x := range nums {
		nums[i] = m - dist[x^(1<<m-1)]
	}
	return nums
}
```

#### TypeScript

```ts
function maxHammingDistances(nums: number[], m: number): number[] {
    const dist: number[] = Array.from({ length: 1 << m }, () => -1);
    const q: number[] = [];
    for (const x of nums) {
        dist[x] = 0;
        q.push(x);
    }
    for (let k = 1; q.length; ++k) {
        const t: number[] = [];
        for (const x of q) {
            for (let i = 0; i < m; ++i) {
                const y = x ^ (1 << i);
                if (dist[y] === -1) {
                    dist[y] = k;
                    t.push(y);
                }
            }
        }
        q.splice(0, q.length, ...t);
    }
    for (let i = 0; i < nums.length; ++i) {
        nums[i] = m - dist[nums[i] ^ ((1 << m) - 1)];
    }
    return nums;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
