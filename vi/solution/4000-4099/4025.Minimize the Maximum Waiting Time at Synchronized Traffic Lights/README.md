---
comments: true
difficulty: Medium
rating: 1456
source: Weekly Contest 515 Q2
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [4025. Minimize the Maximum Waiting Time at Synchronized Traffic Lights](https://leetcode.com/problems/minimize-the-maximum-waiting-time-at-synchronized-traffic-lights)

[中文文档](/solution/4000-4099/4025.Minimize%20the%20Maximum%20Waiting%20Time%20at%20Synchronized%20Traffic%20Lights/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>period</code> và một mảng số nguyên <code>lights</code>, trong đó <code>lights[i]</code> là thời lượng, tính bằng giây, của pha xanh tại đèn giao thông thứ <code>i<sup>th</sup></code>.</p>

<p>Vào thời điểm 0, mọi đèn giao thông đều bắt đầu ở đầu pha xanh. Chu kỳ của chúng được đồng bộ: mọi đèn giao thông bắt đầu một chu kỳ mới cùng thời điểm, và mỗi chu kỳ kéo dài <strong>chính xác</strong> <code>period</code> giây. Do đó, pha đỏ của đèn giao thông thứ <code>i<sup>th</sup></code> kéo dài <code>period - lights[i]</code> giây.</p>

<p>Bạn cũng được cho một mảng số nguyên <code>arrivalTime</code>, trong đó <code>arrivalTime[j]</code> là thời điểm đến, tính bằng giây, của chiếc xe thứ <code>j<sup>th</sup></code>.</p>

<p>Mỗi chiếc xe phải được gán cho <strong>chính xác</strong> một đèn giao thông. Có thể gán nhiều xe cho cùng một đèn giao thông. Bất kỳ số lượng xe nào cũng có thể đi qua cùng một đèn giao thông đồng thời khi đèn đang xanh. Các xe không cản trở hoặc làm chậm nhau.</p>

<p>Với một chiếc xe <code>j</code> được gán cho đèn giao thông thứ <code>i<sup>th</sup></code>, đặt <code>r = arrivalTime[j] % period</code>. Nếu <code>r &lt; lights[i]</code>, thời gian chờ của xe là 0. Ngược lại, thời gian chờ là <code>period - r</code>.</p>

<p><strong>Hình phạt</strong> của một phép gán là <strong>thời gian chờ lớn nhất</strong> trong tất cả các xe.</p>

<p>Hãy trả về một số nguyên biểu thị <strong>hình phạt nhỏ nhất có thể</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">period = 8, lights = [2,3], arrivalTime = [2,5,8,11]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một phương án tối ưu là:</p>

<ul>
	<li>Gán <code>arrivalTime[0]</code> cho đèn giao thông có <code>lights[1] = 3</code>. Ở đây, <code>r = 2 % 8 = 2</code>. Vì <code>2 &lt; 3</code>, thời gian chờ là 0.</li>
	<li>Gán <code>arrivalTime[1]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 5 % 8 = 5</code>. Vì <code>5 &gt;= 2</code>, thời gian chờ là <code>8 - 5 = 3</code>.</li>
	<li>Gán <code>arrivalTime[2]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 8 % 8 = 0</code>. Vì <code>0 &lt; 2</code>, thời gian chờ là 0.</li>
	<li>Gán <code>arrivalTime[3]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 11 % 8 = 3</code>. Vì <code>3 &gt;= 2</code>, thời gian chờ là <code>8 - 3 = 5</code>.</li>
</ul>

<p>Hình phạt của phép gán này là 5, đây là giá trị nhỏ nhất có thể. Có thể tồn tại các phép gán tối ưu khác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">period = 10, lights = [3,6,8], arrivalTime = [4,9,15]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một phương án tối ưu là:</p>

<ul>
	<li>Gán <code>arrivalTime[0]</code> cho đèn giao thông có <code>lights[2] = 8</code>. Ở đây, <code>r = 4 % 10 = 4</code>. Vì <code>4 &lt; 8</code>, thời gian chờ là 0.</li>
	<li>Gán <code>arrivalTime[1]</code> cho đèn giao thông có <code>lights[2] = 8</code>. Ở đây, <code>r = 9 % 10 = 9</code>. Vì <code>9 &gt;= 8</code>, thời gian chờ là <code>10 - 9 = 1</code>.</li>
	<li>Gán <code>arrivalTime[2]</code> cho đèn giao thông có <code>lights[2] = 8</code>. Ở đây, <code>r = 15 % 10 = 5</code>. Vì <code>5 &lt; 8</code>, thời gian chờ là 0.</li>
</ul>

<p>Hình phạt của phép gán này là 1, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">period = 5, lights = [2], arrivalTime = [2,3,4,5,6]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p>Một phương án tối ưu là:</p>

<ul>
	<li>Gán <code>arrivalTime[0]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 2 % 5 = 2</code>. Vì <code>2 &gt;= 2</code>, thời gian chờ là <code>5 - 2 = 3</code>.</li>
	<li>Gán <code>arrivalTime[1]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 3 % 5 = 3</code>. Vì <code>3 &gt;= 2</code>, thời gian chờ là <code>5 - 3 = 2</code>.</li>
	<li>Gán <code>arrivalTime[2]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 4 % 5 = 4</code>. Vì <code>4 &gt;= 2</code>, thời gian chờ là <code>5 - 4 = 1</code>.</li>
	<li>Gán <code>arrivalTime[3]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 5 % 5 = 0</code>. Vì <code>0 &lt; 2</code>, thời gian chờ là 0.</li>
	<li>Gán <code>arrivalTime[4]</code> cho đèn giao thông có <code>lights[0] = 2</code>. Ở đây, <code>r = 6 % 5 = 1</code>. Vì <code>1 &lt; 2</code>, thời gian chờ là 0.</li>
</ul>

<p>Hình phạt của phép gán này là 3, đây là giá trị nhỏ nhất có thể.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= period &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= lights.length &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= lights[i] &lt;= period - 1</code></li>
	<li><code>1 &lt;= arrivalTime.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arrivalTime[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Các đèn dùng chung một chu kỳ. Xe $j$ đến tại phần dư $r=\textit{arrivalTime}[j]\bmod \textit{period}$. Việc liệt kê phép gán cho từng xe sẽ lặp lại cùng một phép tính.
>
> Gọi $\textit{mx}$ là thời lượng pha xanh dài nhất. Nếu $r<\textit{mx}$, gán xe cho đèn đó sẽ cho thời gian chờ bằng $0$; nếu $r\ge\textit{mx}$, xe đã bỏ lỡ pha xanh của mọi đèn và phải chờ $\textit{period}-r$ tại bất kỳ đèn nào.
>
> Vì vậy, hình phạt là thời gian chờ lớn nhất trong số các xe có $r\ge\textit{mx}$, hoặc bằng $0$ nếu không có xe nào như vậy.

<!-- thinking:end -->

Gọi $\textit{mx} = \max(\textit{lights})$ là thời lượng pha xanh dài nhất. Với xe $j$, đặt $r = \textit{arrivalTime}[j] \bmod \textit{period}$.

- Nếu $r < \textit{mx}$, ta có thể gán xe cho đèn có pha xanh dài nhất, và thời gian chờ là $0$.
- Nếu $r \ge \textit{mx}$, thì $r \ge \textit{lights}[i]$ với mọi đèn, nên thời gian chờ là $\textit{period} - r$ bất kể được gán cho đèn nào.

Do đó, hình phạt là giá trị lớn nhất của $\textit{period} - r$ trên tất cả các xe có $r \ge \textit{mx}$. Nếu mọi xe đều có thể đi qua trong pha xanh, đáp án là $0$.

Độ phức tạp thời gian là $O(n + m)$, và độ phức tạp không gian là $O(1)$, trong đó $n$ và $m$ lần lượt là độ dài của $\textit{lights}$ và $\textit{arrivalTime}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minPenalty(self, period: int, lights: list[int], arrivalTime: list[int]) -> int:
        mx = max(lights)
        ans = 0
        for x in arrivalTime:
            r = x % period
            if r >= mx:
                ans = max(ans, period - r)
        return ans
```

#### Java

```java
class Solution {
    public int minPenalty(int period, int[] lights, int[] arrivalTime) {
        int mx = 0;
        for (int x : lights) {
            mx = Math.max(mx, x);
        }

        int ans = 0;

        for (int x : arrivalTime) {
            int r = x % period;

            if (r >= mx) {
                ans = Math.max(ans, period - r);
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
    int minPenalty(int period, vector<int>& lights, vector<int>& arrivalTime) {
        int mx = ranges::max(lights);

        int ans = 0;

        for (int x : arrivalTime) {
            int r = x % period;

            if (r >= mx) {
                ans = max(ans, period - r);
            }
        }

        return ans;
    }
};
```

#### Go

```go
func minPenalty(period int, lights []int, arrivalTime []int) int {
	mx := slices.Max(lights)
	ans := 0

	for _, x := range arrivalTime {
		r := x % period

		if r >= mx {
			ans = max(ans, period-r)
		}
	}

	return ans
}
```

#### TypeScript

```ts
function minPenalty(period: number, lights: number[], arrivalTime: number[]): number {
    const mx = Math.max(...lights);

    let ans = 0;

    for (const x of arrivalTime) {
        const r = x % period;

        if (r >= mx) {
            ans = Math.max(ans, period - r);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
