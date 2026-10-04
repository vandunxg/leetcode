---
comments: true
difficulty: Easy
rating: 1255
source: Weekly Contest 401 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [3178. Find the Child Who Has the Ball After K Seconds](https://leetcode.com/problems/find-the-child-who-has-the-ball-after-k-seconds)

[中文文档](/solution/3100-3199/3178.Find%20the%20Child%20Who%20Has%20the%20Ball%20After%20K%20Seconds/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai số nguyên <strong>dương</strong> <code>n</code> và <code>k</code>. Có <code>n</code> đứa trẻ được đánh số từ <code>0</code> đến <code>n - 1</code> xếp hàng <em>theo thứ tự</em> từ trái sang phải.</p>

<p>Ban đầu, đứa trẻ 0 cầm quả bóng và hướng chuyền bóng là sang phải. Sau mỗi giây, đứa trẻ đang cầm bóng chuyền bóng cho đứa trẻ đứng ngay bên cạnh. Khi quả bóng đến <strong>một trong hai</strong> đầu hàng, tức là đứa trẻ 0 hoặc đứa trẻ <code>n - 1</code>, hướng chuyền bóng sẽ được <strong>đảo ngược</strong>.</p>

<p>Hãy trả về số hiệu của đứa trẻ nhận được quả bóng sau <code>k</code> giây.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, k = 5</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Thời gian trôi qua</th>
			<th>Các đứa trẻ</th>
		</tr>
		<tr>
			<td><code>0</code></td>
			<td><code>[<u>0</u>, 1, 2]</code></td>
		</tr>
		<tr>
			<td><code>1</code></td>
			<td><code>[0, <u>1</u>, 2]</code></td>
		</tr>
		<tr>
			<td><code>2</code></td>
			<td><code>[0, 1, <u>2</u>]</code></td>
		</tr>
		<tr>
			<td><code>3</code></td>
			<td><code>[0, <u>1</u>, 2]</code></td>
		</tr>
		<tr>
			<td><code>4</code></td>
			<td><code>[<u>0</u>, 1, 2]</code></td>
		</tr>
		<tr>
			<td><code>5</code></td>
			<td><code>[0, <u>1</u>, 2]</code></td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, k = 6</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Thời gian trôi qua</th>
			<th>Các đứa trẻ</th>
		</tr>
		<tr>
			<td><code>0</code></td>
			<td><code>[<u>0</u>, 1, 2, 3, 4]</code></td>
		</tr>
		<tr>
			<td><code>1</code></td>
			<td><code>[0, <u>1</u>, 2, 3, 4]</code></td>
		</tr>
		<tr>
			<td><code>2</code></td>
			<td><code>[0, 1, <u>2</u>, 3, 4]</code></td>
		</tr>
		<tr>
			<td><code>3</code></td>
			<td><code>[0, 1, 2, <u>3</u>, 4]</code></td>
		</tr>
		<tr>
			<td><code>4</code></td>
			<td><code>[0, 1, 2, 3, <u>4</u>]</code></td>
		</tr>
		<tr>
			<td><code>5</code></td>
			<td><code>[0, 1, 2, <u>3</u>, 4]</code></td>
		</tr>
		<tr>
			<td><code>6</code></td>
			<td><code>[0, 1, <u>2</u>, 3, 4]</code></td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<table>
	<tbody>
		<tr>
			<th>Thời gian trôi qua</th>
			<th>Các đứa trẻ</th>
		</tr>
		<tr>
			<td><code>0</code></td>
			<td><code>[<u>0</u>, 1, 2, 3]</code></td>
		</tr>
		<tr>
			<td><code>1</code></td>
			<td><code>[0, <u>1</u>, 2, 3]</code></td>
		</tr>
		<tr>
			<td><code>2</code></td>
			<td><code>[0, 1, <u>2</u>, 3]</code></td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 50</code></li>
	<li><code>1 &lt;= k &lt;= 50</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Ghi chú:</strong> Bài toán này giống với <a href="https://leetcode.com/problems/pass-the-pillow/description/" target="_blank"> 2582: Pass the Pillow.</a></p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Toán học

<!-- thinking:start -->

> **Tư duy**
>
> Quả bóng di chuyển qua lại trên đoạn $0..n-1$, mỗi giây đi một bước. Mô phỏng $k$ giây sẽ có độ phức tạp tuyến tính theo $k$.
>
> Một lượt đi từ đầu này đến đầu kia có $n-1$ bước. Thương của $k$ khi chia cho $n-1$ là số chẵn khi bóng đang đi sang phải và là số lẻ khi bóng đang đi sang trái.
>
> Viết $k,mod=divmod(k,n-1)$ và trả về $n-mod-1$ nếu thương là số lẻ, ngược lại trả về $mod$.

<!-- thinking:end -->

Ta nhận thấy mỗi vòng có $n - 1$ lần chuyền. Vì vậy, ta có thể lấy $k$ modulo $n - 1$ để nhận được số lần chuyền $mod$ trong vòng hiện tại. Sau đó, ta chia $k$ cho $n - 1$ để nhận được số thứ tự vòng hiện tại $k$.

Tiếp theo, ta xét số thứ tự vòng hiện tại $k$:

- Nếu $k$ là số lẻ, hướng chuyền hiện tại là từ cuối hàng về đầu hàng, nên bóng sẽ được chuyền cho người có số hiệu $n - mod - 1$.
- Nếu $k$ là số chẵn, hướng chuyền hiện tại là từ đầu hàng đến cuối hàng, nên bóng sẽ được chuyền cho người có số hiệu $mod$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfChild(self, n: int, k: int) -> int:
        k, mod = divmod(k, n - 1)
        return n - mod - 1 if k & 1 else mod
```

#### Java

```java
class Solution {
    public int numberOfChild(int n, int k) {
        int mod = k % (n - 1);
        k /= (n - 1);
        return k % 2 == 1 ? n - mod - 1 : mod;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfChild(int n, int k) {
        int mod = k % (n - 1);
        k /= (n - 1);
        return k % 2 == 1 ? n - mod - 1 : mod;
    }
};
```

#### Go

```go
func numberOfChild(n int, k int) int {
	mod := k % (n - 1)
	k /= (n - 1)
	if k%2 == 1 {
		return n - mod - 1
	}
	return mod
}
```

#### TypeScript

```ts
function numberOfChild(n: number, k: number): number {
    const mod = k % (n - 1);
    k = (k / (n - 1)) | 0;
    return k % 2 ? n - mod - 1 : mod;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
