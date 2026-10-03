---
comments: true
difficulty: Medium
rating: 1294
source: Weekly Contest 229 Q2
tags:
    - Array
    - String
    - Prefix Sum
---

<!-- problem:start -->

# [1769. Minimum Number of Operations to Move All Balls to Each Box](https://leetcode.com/problems/minimum-number-of-operations-to-move-all-balls-to-each-box)

[中文文档](/solution/1700-1799/1769.Minimum%20Number%20of%20Operations%20to%20Move%20All%20Balls%20to%20Each%20Box/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có <code>n</code> hộp. Bạn được cho chuỗi nhị phân <code>boxes</code> độ dài <code>n</code>, trong đó <code>boxes[i]</code> là <code>&#39;0&#39;</code> nếu hộp thứ <code>i<sup>th</sup></code> <strong>rỗng</strong>, và là <code>&#39;1&#39;</code> nếu chứa <strong>một</strong> quả bóng.</p>

<p>Trong một thao tác, bạn có thể chuyển <strong>một</strong> quả bóng từ một hộp sang hộp kề bên. Hộp <code>i</code> kề hộp <code>j</code> nếu <code>abs(i - j) == 1</code>. Sau thao tác, một số hộp có thể chứa nhiều hơn một quả bóng.</p>

<p>Trả về mảng <code>answer</code> kích thước <code>n</code>, trong đó <code>answer[i]</code> là số thao tác <strong>nhỏ nhất</strong> cần để chuyển tất cả bóng vào hộp thứ <code>i<sup>th</sup></code>.</p>

<p>Mỗi <code>answer[i]</code> được tính dựa trên trạng thái <strong>ban đầu</strong> của các hộp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> boxes = &quot;110&quot;
<strong>Đầu ra:</strong> [1,1,3]
<strong>Giải thích:</strong> Đáp án cho mỗi hộp như sau:
1) Hộp thứ nhất: cần chuyển một quả bóng từ hộp thứ hai sang hộp thứ nhất trong một thao tác.
2) Hộp thứ hai: cần chuyển một quả bóng từ hộp thứ nhất sang hộp thứ hai trong một thao tác.
3) Hộp thứ ba: cần chuyển một quả bóng từ hộp thứ nhất sang hộp thứ ba trong hai thao tác, và chuyển một quả bóng từ hộp thứ hai sang hộp thứ ba trong một thao tác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> boxes = &quot;001011&quot;
<strong>Đầu ra:</strong> [11,8,5,4,3,4]</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == boxes.length</code></li>
	<li><code>1 &lt;= n &lt;= 2000</code></li>
	<li><code>boxes[i]</code> is either <code>&#39;0&#39;</code> or <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Chi phí gom mọi quả bóng vào hộp $i$ là tổng khoảng cách chỉ số. Với $n\le 2000$, vòng lặp kép vẫn được, nhưng có thể dùng công thức truy hồi tuyến tính.
>
> Chi phí bên trái (bên phải) được suy ra từ hộp kế bên: thêm một hộp bóng ở phía đó làm chi phí tăng bằng số bóng. Tiền xử lý $left[i]$ và $right[i]$, rồi cộng chúng.

<!-- thinking:end -->

Tiền xử lý $\textit{left}[i]$ là chi phí chuyển mọi bóng bên trái $i$ đến vị trí $i$, và $\textit{right}[i]$ là chi phí chuyển mọi bóng bên phải $i$ đến vị trí $i$. Đáp án tại $i$ là $\textit{left}[i] + \textit{right}[i]$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{boxes}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, boxes: str) -> List[int]:
        n = len(boxes)
        left = [0] * n
        right = [0] * n
        cnt = 0
        for i in range(1, n):
            if boxes[i - 1] == '1':
                cnt += 1
            left[i] = left[i - 1] + cnt
        cnt = 0
        for i in range(n - 2, -1, -1):
            if boxes[i + 1] == '1':
                cnt += 1
            right[i] = right[i + 1] + cnt
        return [a + b for a, b in zip(left, right)]
```

#### Java

```java
class Solution {
    public int[] minOperations(String boxes) {
        int n = boxes.length();
        int[] left = new int[n];
        int[] right = new int[n];
        for (int i = 1, cnt = 0; i < n; ++i) {
            if (boxes.charAt(i - 1) == '1') {
                ++cnt;
            }
            left[i] = left[i - 1] + cnt;
        }
        for (int i = n - 2, cnt = 0; i >= 0; --i) {
            if (boxes.charAt(i + 1) == '1') {
                ++cnt;
            }
            right[i] = right[i + 1] + cnt;
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = left[i] + right[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minOperations(string boxes) {
        int n = boxes.size();
        int left[n];
        int right[n];
        memset(left, 0, sizeof left);
        memset(right, 0, sizeof right);
        for (int i = 1, cnt = 0; i < n; ++i) {
            cnt += boxes[i - 1] == '1';
            left[i] = left[i - 1] + cnt;
        }
        for (int i = n - 2, cnt = 0; ~i; --i) {
            cnt += boxes[i + 1] == '1';
            right[i] = right[i + 1] + cnt;
        }
        vector<int> ans(n);
        for (int i = 0; i < n; ++i) ans[i] = left[i] + right[i];
        return ans;
    }
};
```

#### Go

```go
func minOperations(boxes string) []int {
	n := len(boxes)
	left := make([]int, n)
	right := make([]int, n)
	for i, cnt := 1, 0; i < n; i++ {
		if boxes[i-1] == '1' {
			cnt++
		}
		left[i] = left[i-1] + cnt
	}
	for i, cnt := n-2, 0; i >= 0; i-- {
		if boxes[i+1] == '1' {
			cnt++
		}
		right[i] = right[i+1] + cnt
	}
	ans := make([]int, n)
	for i := range ans {
		ans[i] = left[i] + right[i]
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(boxes: string): number[] {
    const n = boxes.length;
    const left = new Array(n).fill(0);
    const right = new Array(n).fill(0);
    for (let i = 1, count = 0; i < n; i++) {
        if (boxes[i - 1] == '1') {
            count++;
        }
        left[i] = left[i - 1] + count;
    }
    for (let i = n - 2, count = 0; i >= 0; i--) {
        if (boxes[i + 1] == '1') {
            count++;
        }
        right[i] = right[i + 1] + count;
    }
    return left.map((v, i) => v + right[i]);
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(boxes: String) -> Vec<i32> {
        let s = boxes.as_bytes();
        let n = s.len();
        let mut left = vec![0; n];
        let mut right = vec![0; n];
        let mut count = 0;
        for i in 1..n {
            if s[i - 1] == b'1' {
                count += 1;
            }
            left[i] = left[i - 1] + count;
        }
        count = 0;
        for i in (0..n - 1).rev() {
            if s[i + 1] == b'1' {
                count += 1;
            }
            right[i] = right[i + 1] + count;
        }
        (0..n).into_iter().map(|i| left[i] + right[i]).collect()
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* minOperations(char* boxes, int* returnSize) {
    int n = strlen(boxes);
    int* left = malloc(sizeof(int) * n);
    int* right = malloc(sizeof(int) * n);
    memset(left, 0, sizeof(int) * n);
    memset(right, 0, sizeof(int) * n);
    for (int i = 1, count = 0; i < n; i++) {
        if (boxes[i - 1] == '1') {
            count++;
        }
        left[i] = left[i - 1] + count;
    }
    for (int i = n - 2, count = 0; i >= 0; i--) {
        if (boxes[i + 1] == '1') {
            count++;
        }
        right[i] = right[i + 1] + count;
    }
    int* ans = malloc(sizeof(int) * n);
    for (int i = 0; i < n; i++) {
        ans[i] = left[i] + right[i];
    }
    free(left);
    free(right);
    *returnSize = n;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 2: Prefix Sums (Space Optimization)

<!-- thinking:start -->

> **Thinking**
>
> Hai mảng trong Lời giải 1 chỉ phụ thuộc vào ô trước đó. Ta cộng dồn các công thức truy hồi tương tự trực tiếp vào $ans$ khi duyệt từ trái sang phải và từ phải sang trái, nhờ đó chỉ cần không gian phụ hằng số.

<!-- thinking:end -->

$\textit{left}[i]$ và $\textit{right}[i]$ trong Lời giải 1 chỉ phụ thuộc vào vị trí trước đó, nên ta có thể bỏ hai mảng này và cộng dồn vào $\textit{ans}$ bằng một lượt duyệt từ trái sang phải và một lượt từ phải sang trái.

Độ phức tạp thời gian là $O(n)$. Không tính mảng kết quả, độ phức tạp không gian phụ là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, boxes: str) -> List[int]:
        n = len(boxes)
        ans = [0] * n
        cnt = 0
        for i in range(1, n):
            if boxes[i - 1] == '1':
                cnt += 1
            ans[i] = ans[i - 1] + cnt
        cnt = s = 0
        for i in range(n - 2, -1, -1):
            if boxes[i + 1] == '1':
                cnt += 1
            s += cnt
            ans[i] += s
        return ans
```

#### Java

```java
class Solution {
    public int[] minOperations(String boxes) {
        int n = boxes.length();
        int[] ans = new int[n];
        for (int i = 1, cnt = 0; i < n; ++i) {
            if (boxes.charAt(i - 1) == '1') {
                ++cnt;
            }
            ans[i] = ans[i - 1] + cnt;
        }
        for (int i = n - 2, cnt = 0, s = 0; i >= 0; --i) {
            if (boxes.charAt(i + 1) == '1') {
                ++cnt;
            }
            s += cnt;
            ans[i] += s;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minOperations(string boxes) {
        int n = boxes.size();
        vector<int> ans(n);
        for (int i = 1, cnt = 0; i < n; ++i) {
            cnt += boxes[i - 1] == '1';
            ans[i] = ans[i - 1] + cnt;
        }
        for (int i = n - 2, cnt = 0, s = 0; ~i; --i) {
            cnt += boxes[i + 1] == '1';
            s += cnt;
            ans[i] += s;
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(boxes string) []int {
	n := len(boxes)
	ans := make([]int, n)
	for i, cnt := 1, 0; i < n; i++ {
		if boxes[i-1] == '1' {
			cnt++
		}
		ans[i] = ans[i-1] + cnt
	}
	for i, cnt, s := n-2, 0, 0; i >= 0; i-- {
		if boxes[i+1] == '1' {
			cnt++
		}
		s += cnt
		ans[i] += s
	}
	return ans
}
```

#### TypeScript

```ts
function minOperations(boxes: string): number[] {
    const n = boxes.length;
    const ans = new Array(n).fill(0);
    for (let i = 1, count = 0; i < n; i++) {
        if (boxes[i - 1] === '1') {
            count++;
        }
        ans[i] = ans[i - 1] + count;
    }
    for (let i = n - 2, count = 0, sum = 0; i >= 0; i--) {
        if (boxes[i + 1] === '1') {
            count++;
        }
        sum += count;
        ans[i] += sum;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn min_operations(boxes: String) -> Vec<i32> {
        let s = boxes.as_bytes();
        let n = s.len();
        let mut ans = vec![0; n];
        let mut count = 0;
        for i in 1..n {
            if s[i - 1] == b'1' {
                count += 1;
            }
            ans[i] = ans[i - 1] + count;
        }
        let mut sum = 0;
        count = 0;
        for i in (0..n - 1).rev() {
            if s[i + 1] == b'1' {
                count += 1;
            }
            sum += count;
            ans[i] += sum;
        }
        ans
    }
}
```

#### C

```c
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* minOperations(char* boxes, int* returnSize) {
    int n = strlen(boxes);
    int* ans = malloc(sizeof(int) * n);
    memset(ans, 0, sizeof(int) * n);
    for (int i = 1, count = 0; i < n; i++) {
        if (boxes[i - 1] == '1') {
            count++;
        }
        ans[i] = ans[i - 1] + count;
    }
    for (int i = n - 2, count = 0, sum = 0; i >= 0; i--) {
        if (boxes[i + 1] == '1') {
            count++;
        }
        sum += count;
        ans[i] += sum;
    }
    *returnSize = n;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Solution 3: Enumeration

<!-- thinking:start -->

> **Thinking**
>
> Cách cài đặt trực tiếp lưu chỉ số của mọi quả bóng rồi, với mỗi hộp, tính tổng $|i-j|$. Cách này vẫn đạt với giới hạn $n$ hiện tại, nhưng có hằng số lớn hơn so với các công thức truy hồi tiền tố.

<!-- thinking:end -->

Thu thập vị trí của mọi quả bóng, rồi với mỗi hộp $i$, cộng $|i - j|$ cho mọi quả bóng $j$.

Độ phức tạp thời gian là $O(n \times m)$ và độ phức tạp không gian là $O(m)$, trong đó $n$ là độ dài của $\textit{boxes}$ và $m$ là số quả bóng.

<!-- tabs:start -->

#### TypeScript

```ts
function minOperations(boxes: string): number[] {
    const n = boxes.length;
    const ans = Array(n).fill(0);
    const ones: number[] = [];

    for (let i = 0; i < n; i++) {
        if (+boxes[i]) {
            ones.push(i);
        }
    }

    for (let i = 0; i < n; i++) {
        for (const j of ones) {
            ans[i] += Math.abs(i - j);
        }
    }

    return ans;
}
```

#### JavaScript

```js
function minOperations(boxes) {
    const n = boxes.length;
    const ans = Array(n).fill(0);
    const ones = [];

    for (let i = 0; i < n; i++) {
        if (+boxes[i]) {
            ones.push(i);
        }
    }

    for (let i = 0; i < n; i++) {
        for (const j of ones) {
            ans[i] += Math.abs(i - j);
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
