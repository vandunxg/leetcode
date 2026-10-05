---
comments: true
difficulty: Hard
rating: 2372
source: Biweekly Contest 190 Q4
---

<!-- problem:start -->

# [4037. Maximum Valid Split Positions II](https://leetcode.com/problems/maximum-valid-split-positions-ii)

[Tài liệu tiếng Trung](/solution/4000-4099/4037.Maximum%20Valid%20Split%20Positions%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>nums</code>.</p>

<p>Bạn có thể xóa <strong>nhiều nhất một</strong> phần tử khỏi <code>nums</code>. Gọi <code>arr</code> là mảng gồm các phần tử còn lại theo đúng thứ tự ban đầu, và gọi <code>m</code> là độ dài của mảng đó.</p>

<p>Một <strong>vị trí tách</strong> <code>i</code> của <code>arr</code> là <strong>hợp lệ</strong> nếu:</p>

<ul>
	<li><code>0 &lt;= i &lt; m - 1</code>, và</li>
	<li><code>gcd(arr[0..i]) == gcd(arr[i + 1..m - 1])</code>.</li>
</ul>

<p>Mảng có độ dài 1 không có vị trí tách hợp lệ nào.</p>

<p><strong>Điểm số</strong> của <code>arr</code> là số lượng vị trí tách hợp lệ trong mảng.</p>

<p>Trả về <strong>điểm số lớn nhất có thể</strong> của <code>arr</code>.</p>

<p>Ở đây, <code>gcd(a)</code> là <strong>ước chung lớn nhất</strong> của mọi phần tử trong mảng <code>a</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,30,15,10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một phương án tối ưu là xóa <code>nums[2] = 15</code>. Khi đó <code>arr = [10, 30, 10]</code>.</p>

<p>Các vị trí tách là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí tách <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>10</td>
			<td>10</td>
		</tr>
		<tr>
			<td>1</td>
			<td>10</td>
			<td>10</td>
		</tr>
	</tbody>
</table>

<p>Mọi vị trí tách đều hợp lệ. Do đó, đáp án là 2.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,10,14]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một phương án tối ưu là không xóa phần tử nào. Khi đó <code>arr = [2, 10, 14]</code>.</p>

<p>Các vị trí tách là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí tách <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>2</td>
			<td>2</td>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
			<td>14</td>
		</tr>
	</tbody>
</table>

<p>Chỉ vị trí tách tại chỉ số 0 là hợp lệ. Do đó, đáp án là 1.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [2,4]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng còn lại duy nhất có vị trí tách là <code>arr = [2, 4]</code>.</p>

<p>Các vị trí tách là:</p>

<table border="1" bordercolor="#ccc" cellpadding="5" cellspacing="0" style="border-collapse:collapse; text-align:center;">
	<tbody>
		<tr>
			<th>Vị trí tách <code>i</code></th>
			<th><code>gcd(arr[0..i])</code></th>
			<th><code>gcd(arr[i + 1..m - 1])</code></th>
		</tr>
		<tr>
			<td>0</td>
			<td>2</td>
			<td>4</td>
		</tr>
	</tbody>
</table>

<p>Không có vị trí tách hợp lệ nào. Do đó, đáp án là 0.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code>​​​​​​​</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: GCD prefix và suffix + Liệt kê các chỉ số có thể xóa

<!-- thinking:start -->

> **Tư duy**
>
> Việc tính điểm cho mỗi lần xóa trong $O(n)$ không còn phù hợp khi $n=10^5$. Mỗi GCD prefix chia hết cho GCD prefix trước đó, nên chuỗi này chỉ thay đổi nhiều nhất $O(\log M)$ lần.
>
> Nếu cả GCD prefix lẫn GCD suffix đều không thay đổi tại một chỉ số, việc xóa phần tử ở đó giữ nguyên mọi GCD khác và chỉ gộp hai vị trí tách lại với nhau, nên điểm số không thể tăng. Chỉ cần tính lại điểm cho các chỉ số mà một GCD thực sự thay đổi.
>
> Một lần đánh dấu từ trái sang phải và một lần từ phải sang trái tạo ra $O(\log M)$ ứng viên; ta tính lại điểm cho mỗi lần xóa và lấy giá trị lớn nhất cùng với điểm số của mảng nguyên vẹn.

<!-- thinking:end -->

Theo ý tưởng của bài trước, với một mảng $\textit{arr}$ có độ dài $m$, trước hết ta tính các mảng GCD prefix $\textit{pre}$ và GCD suffix $\textit{suf}$. Vị trí tách $i$ hợp lệ khi và chỉ khi $\textit{pre}[i] = \textit{suf}[i + 1]$, vì vậy điểm số của $\textit{arr}$ là số chỉ số thỏa mãn điều kiện này. Tuy nhiên, ở đây $n$ có thể lớn tới $10^5$, nên việc duyệt mọi chỉ số bị xóa và dành $O(n)$ cho mỗi chỉ số là quá chậm.

Ta nhận thấy mọi phần tử trong dãy GCD prefix đều chia hết cho phần tử trước đó, nên mỗi khi thay đổi, giá trị ít nhất giảm một nửa. Do đó, toàn bộ dãy chỉ thay đổi $O(\log M)$ lần. Nếu GCD prefix không thay đổi tại chỉ số $i$, tức là $\textit{pre}[i] = \textit{pre}[i - 1]$, tương đương với việc $\textit{pre}[i - 1]$ chia hết cho $\textit{nums}[i]$, thì việc xóa $\textit{nums}[i]$ giữ nguyên mọi GCD prefix. Tương tự, nếu GCD suffix cũng không thay đổi tại chỉ số $i$, việc xóa phần tử này cũng giữ nguyên mọi GCD suffix. Khi đó, ảnh hưởng duy nhất của việc xóa là gộp hai vị trí tách $i - 1$ và $i$ thành một vị trí, mà hai vị trí này hoặc cùng hợp lệ hoặc cùng không hợp lệ, nên điểm số chỉ có thể giảm.

Vì vậy, chỉ cần liệt kê các chỉ số mà GCD prefix hoặc GCD suffix thay đổi; có nhiều nhất $O(\log M)$ chỉ số như vậy. Ta gọi $\textit{mark}$ một lần từ trái sang phải và một lần từ phải sang trái để thu thập các chỉ số ứng viên, sau đó dùng $\textit{calc}$ để tính điểm sau khi xóa từng ứng viên, rồi lấy giá trị lớn nhất cùng với điểm số của mảng không bị thay đổi.

Độ phức tạp thời gian là $O(n \times \log^2 M)$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{nums}$, còn $M$ là giá trị lớn nhất trong mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxValidSplits(self, nums: List[int]) -> int:
        n = len(nums)

        def calc(arr):
            m = len(arr)
            pre = [0] * m
            suf = [0] * m

            pre[0] = arr[0]
            for i in range(1, m):
                pre[i] = gcd(pre[i - 1], arr[i])

            suf[-1] = arr[-1]
            for i in range(m - 2, -1, -1):
                suf[i] = gcd(suf[i + 1], arr[i])

            ans = 0
            for i in range(m - 1):
                if pre[i] == suf[i + 1]:
                    ans += 1

            return ans

        def mark(arr):
            pos = [False] * n
            pos[0] = True
            g = arr[0]

            for i in range(1, n):
                ng = gcd(g, arr[i])
                pos[i] = ng != g
                g = ng

            return pos

        pos1 = mark(nums)
        pos2 = mark(nums[::-1])

        ans = calc(nums)

        for i in range(n):
            if pos1[i] or pos2[n - 1 - i]:
                arr = nums[:i] + nums[i + 1 :]
                ans = max(ans, calc(arr))

        return ans
```

#### Java

```java
class Solution {
    public int maxValidSplits(int[] nums) {
        int n = nums.length;

        boolean[] pos1 = mark(nums);

        int[] rev = nums.clone();
        for (int i = 0; i < n / 2; ++i) {
            int t = rev[i];
            rev[i] = rev[n - 1 - i];
            rev[n - 1 - i] = t;
        }

        boolean[] pos2 = mark(rev);

        int ans = calc(nums);

        for (int i = 0; i < n; ++i) {
            if (pos1[i] || pos2[n - 1 - i]) {
                int[] arr = new int[n - 1];
                for (int j = 0, k = 0; j < n; ++j) {
                    if (j != i) {
                        arr[k++] = nums[j];
                    }
                }
                ans = Math.max(ans, calc(arr));
            }
        }

        return ans;
    }

    private boolean[] mark(int[] nums) {
        int n = nums.length;
        boolean[] pos = new boolean[n];

        pos[0] = true;
        int g = nums[0];

        for (int i = 1; i < n; ++i) {
            int ng = gcd(g, nums[i]);
            pos[i] = ng != g;
            g = ng;
        }

        return pos;
    }

    private int calc(int[] arr) {
        int n = arr.length;
        int[] pre = new int[n];
        int[] suf = new int[n];

        pre[0] = arr[0];
        for (int i = 1; i < n; ++i) {
            pre[i] = gcd(pre[i - 1], arr[i]);
        }

        suf[n - 1] = arr[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            suf[i] = gcd(suf[i + 1], arr[i]);
        }

        int ans = 0;
        for (int i = 0; i + 1 < n; ++i) {
            if (pre[i] == suf[i + 1]) {
                ++ans;
            }
        }

        return ans;
    }

    private int gcd(int a, int b) {
        while (b != 0) {
            int t = a % b;
            a = b;
            b = t;
        }
        return a;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxValidSplits(vector<int>& nums) {
        int n = nums.size();

        vector<bool> pos1 = mark(nums);

        vector<int> rev = nums;
        reverse(rev.begin(), rev.end());
        vector<bool> pos2 = mark(rev);

        int ans = calc(nums);

        for (int i = 0; i < n; ++i) {
            if (pos1[i] || pos2[n - 1 - i]) {
                vector<int> arr;
                arr.reserve(n - 1);

                for (int j = 0; j < n; ++j) {
                    if (i != j) {
                        arr.push_back(nums[j]);
                    }
                }

                ans = max(ans, calc(arr));
            }
        }

        return ans;
    }

private:
    vector<bool> mark(const vector<int>& nums) {
        int n = nums.size();
        vector<bool> pos(n);

        pos[0] = true;
        int g = nums[0];

        for (int i = 1; i < n; ++i) {
            int ng = gcd(g, nums[i]);
            pos[i] = ng != g;
            g = ng;
        }

        return pos;
    }

    int calc(const vector<int>& arr) {
        int n = arr.size();
        vector<int> pre(n), suf(n);

        pre[0] = arr[0];
        for (int i = 1; i < n; ++i) {
            pre[i] = gcd(pre[i - 1], arr[i]);
        }

        suf[n - 1] = arr[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            suf[i] = gcd(suf[i + 1], arr[i]);
        }

        int ans = 0;
        for (int i = 0; i + 1 < n; ++i) {
            if (pre[i] == suf[i + 1]) {
                ++ans;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maxValidSplits(nums []int) int {
	n := len(nums)

	pos1 := mark(nums)

	rev := make([]int, n)
	for i := 0; i < n; i++ {
		rev[i] = nums[n-1-i]
	}
	pos2 := mark(rev)

	ans := calc(nums)

	for i := 0; i < n; i++ {
		if pos1[i] || pos2[n-1-i] {
			arr := make([]int, 0, n-1)
			for j := 0; j < n; j++ {
				if i != j {
					arr = append(arr, nums[j])
				}
			}
			ans = max(ans, calc(arr))
		}
	}

	return ans
}

func mark(nums []int) []bool {
	n := len(nums)
	pos := make([]bool, n)

	pos[0] = true
	g := nums[0]

	for i := 1; i < n; i++ {
		ng := gcd(g, nums[i])
		pos[i] = ng != g
		g = ng
	}

	return pos
}

func calc(arr []int) int {
	n := len(arr)
	pre := make([]int, n)
	suf := make([]int, n)

	pre[0] = arr[0]
	for i := 1; i < n; i++ {
		pre[i] = gcd(pre[i-1], arr[i])
	}

	suf[n-1] = arr[n-1]
	for i := n - 2; i >= 0; i-- {
		suf[i] = gcd(suf[i+1], arr[i])
	}

	ans := 0
	for i := 0; i+1 < n; i++ {
		if pre[i] == suf[i+1] {
			ans++
		}
	}

	return ans
}

func gcd(a, b int) int {
	for b != 0 {
		a, b = b, a%b
	}
	return a
}
```

#### TypeScript

```ts
function maxValidSplits(nums: number[]): number {
    const n = nums.length;

    const pos1 = mark(nums);

    const rev = [...nums].reverse();
    const pos2 = mark(rev);

    let ans = calc(nums);

    for (let i = 0; i < n; ++i) {
        if (pos1[i] || pos2[n - 1 - i]) {
            const arr = nums.slice(0, i).concat(nums.slice(i + 1));
            ans = Math.max(ans, calc(arr));
        }
    }

    return ans;
}

function mark(nums: number[]): boolean[] {
    const n = nums.length;
    const pos = Array(n).fill(false);

    pos[0] = true;
    let g = nums[0];

    for (let i = 1; i < n; ++i) {
        const ng = gcd(g, nums[i]);
        pos[i] = ng !== g;
        g = ng;
    }

    return pos;
}

function calc(arr: number[]): number {
    const n = arr.length;
    const pre = Array(n);
    const suf = Array(n);

    pre[0] = arr[0];
    for (let i = 1; i < n; ++i) {
        pre[i] = gcd(pre[i - 1], arr[i]);
    }

    suf[n - 1] = arr[n - 1];
    for (let i = n - 2; i >= 0; --i) {
        suf[i] = gcd(suf[i + 1], arr[i]);
    }

    let ans = 0;
    for (let i = 0; i + 1 < n; ++i) {
        if (pre[i] === suf[i + 1]) {
            ++ans;
        }
    }

    return ans;
}

function gcd(a: number, b: number): number {
    while (b !== 0) {
        [a, b] = [b, a % b];
    }
    return a;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
