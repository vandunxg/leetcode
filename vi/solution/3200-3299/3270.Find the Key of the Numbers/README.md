---
comments: true
difficulty: Easy
rating: 1205
source: Biweekly Contest 138 Q1
tags:
    - Math
---

<!-- problem:start -->

# [3270. Find the Key of the Numbers](https://leetcode.com/problems/find-the-key-of-the-numbers)

[中文文档](/solution/3200-3299/3270.Find%20the%20Key%20of%20the%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba số nguyên <strong>dương</strong> <code>num1</code>, <code>num2</code> và <code>num3</code>.</p>

<p><code>key</code> của <code>num1</code>, <code>num2</code> và <code>num3</code> được định nghĩa là một số có bốn chữ số, thỏa mãn:</p>

<ul>
	<li>Ban đầu, nếu số nào có <strong>ít hơn</strong> bốn chữ số thì số đó được thêm các số <strong>0 ở đầu</strong>.</li>
	<li>Chữ số thứ <code>i<sup>th</sup></code> (<code>1 &lt;= i &lt;= 4</code>) của <code>key</code> được tạo bằng cách lấy chữ số <strong>nhỏ nhất</strong> trong các chữ số thứ <code>i<sup>th</sup></code> của <code>num1</code>, <code>num2</code> và <code>num3</code>.</li>
</ul>

<p>Trả về <code>key</code> của ba số, <strong>không</strong> có các số 0 ở đầu (<em>nếu có</em>).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 1, num2 = 10, num3 = 1000</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau khi thêm các số 0 ở đầu, <code>num1</code> trở thành <code>&quot;0001&quot;</code>, <code>num2</code> trở thành <code>&quot;0010&quot;</code> và <code>num3</code> vẫn là <code>&quot;1000&quot;</code>.</p>

<ul>
	<li>Chữ số thứ <code>1<sup>st</sup></code> của <code>key</code> là <code>min(0, 0, 1)</code>.</li>
	<li>Chữ số thứ <code>2<sup>nd</sup></code> của <code>key</code> là <code>min(0, 0, 0)</code>.</li>
	<li>Chữ số thứ <code>3<sup>rd</sup></code> của <code>key</code> là <code>min(0, 1, 0)</code>.</li>
	<li>Chữ số thứ <code>4<sup>th</sup></code> của <code>key</code> là <code>min(1, 0, 0)</code>.</li>
</ul>

<p>Do đó, <code>key</code> là <code>&quot;0000&quot;</code>, tức là 0.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 987, num2 = 879, num3 = 798</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">777</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 1, num2 = 2, num3 = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1, num2, num3 &lt;= 9999</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Chữ số $d$ của key là chữ số nhỏ nhất trong chữ số thứ $d$ của ba số, trong đó các chữ số ở đầu còn thiếu được xem là 0. Có bốn chữ số, nên ta lấy giá trị nhỏ nhất ở từng vị trí rồi cộng với trọng số tương ứng.
>
> Với $k=1,10,100,1000$, cộng $\min_i (x_i//k)\% 10$ nhân với $k$. Không cần chuyển đổi sang chuỗi.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình này bằng cách dùng biến $\textit{ans}$ để lưu đáp án và biến $\textit{k}$ biểu diễn vị trí chữ số hiện tại, trong đó $\textit{k} = 1$ biểu diễn hàng đơn vị, $\textit{k} = 10$ biểu diễn hàng chục và tiếp tục như vậy.

Bắt đầu từ hàng đơn vị, với mỗi vị trí chữ số, ta tính chữ số hiện tại của $\textit{num1}$, $\textit{num2}$ và $\textit{num3}$, lấy giá trị nhỏ nhất trong ba chữ số rồi cộng giá trị nhỏ nhất nhân với $\textit{k}$ vào đáp án. Sau đó, nhân $\textit{k}$ với 10 và chuyển sang vị trí chữ số tiếp theo.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def generateKey(self, num1: int, num2: int, num3: int) -> int:
        ans, k = 0, 1
        for _ in range(4):
            x = min(num1 // k % 10, num2 // k % 10, num3 // k % 10)
            ans += x * k
            k *= 10
        return ans
```

#### Java

```java
class Solution {
    public int generateKey(int num1, int num2, int num3) {
        int ans = 0, k = 1;
        for (int i = 0; i < 4; ++i) {
            int x = Math.min(Math.min(num1 / k % 10, num2 / k % 10), num3 / k % 10);
            ans += x * k;
            k *= 10;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int generateKey(int num1, int num2, int num3) {
        int ans = 0, k = 1;
        for (int i = 0; i < 4; ++i) {
            int x = min({num1 / k % 10, num2 / k % 10, num3 / k % 10});
            ans += x * k;
            k *= 10;
        }
        return ans;
    }
};
```

#### Go

```go
func generateKey(num1 int, num2 int, num3 int) (ans int) {
	k := 1
	for i := 0; i < 4; i++ {
		x := min(min(num1/k%10, num2/k%10), num3/k%10)
		ans += x * k
		k *= 10
	}
	return
}
```

#### TypeScript

```ts
function generateKey(num1: number, num2: number, num3: number): number {
    let [ans, k] = [0, 1];
    for (let i = 0; i < 4; ++i) {
        const x = Math.min(((num1 / k) | 0) % 10, ((num2 / k) | 0) % 10, ((num3 / k) | 0) % 10);
        ans += x * k;
        k *= 10;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
