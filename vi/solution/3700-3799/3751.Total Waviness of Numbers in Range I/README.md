---
comments: true
difficulty: Medium
rating: 1404
source: Biweekly Contest 170 Q2
tags:
    - Math
    - Dynamic Programming
    - Enumeration
---

<!-- problem:start -->

# [3751. Total Waviness of Numbers in Range I](https://leetcode.com/problems/total-waviness-of-numbers-in-range-i)

[中文文档](/solution/3700-3799/3751.Total%20Waviness%20of%20Numbers%20in%20Range%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai số nguyên <code>num1</code> và <code>num2</code> biểu diễn một đoạn <strong>bao gồm cả hai đầu mút</strong> <code>[num1, num2]</code>.</p>

<p><strong>Waviness</strong> của một số được định nghĩa là tổng số <strong>đỉnh</strong> và <strong>đáy</strong> của số đó:</p>

<ul>
	<li>Một chữ số là <strong>đỉnh</strong> nếu nó <strong>lớn hơn nghiêm ngặt</strong> cả hai chữ số liền kề.</li>
	<li>Một chữ số là <strong>đáy</strong> nếu nó <strong>nhỏ hơn nghiêm ngặt</strong> cả hai chữ số liền kề.</li>
	<li>Chữ số đầu tiên và chữ số cuối cùng của một số <strong>không thể</strong> là đỉnh hoặc đáy.</li>
	<li>Mọi số có ít hơn 3 chữ số đều có waviness bằng 0.</li>
</ul>

Trả về tổng waviness của tất cả các số trong đoạn <code>[num1, num2]</code>.
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 120, num2 = 130</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>
Trong đoạn <code>[120, 130]</code>:

<ul>
	<li><code>120</code>: chữ số ở giữa 2 là một đỉnh, waviness = 1.</li>
	<li><code>121</code>: chữ số ở giữa 2 là một đỉnh, waviness = 1.</li>
	<li><code>130</code>: chữ số ở giữa 3 là một đỉnh, waviness = 1.</li>
	<li>Tất cả các số còn lại trong đoạn đều có waviness bằng 0.</li>
</ul>

<p>Do đó, tổng waviness là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 198, num2 = 202</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>
Trong đoạn <code>[198, 202]</code>:

<ul>
	<li><code>198</code>: chữ số ở giữa 9 là một đỉnh, waviness = 1.</li>
	<li><code>201</code>: chữ số ở giữa 0 là một đáy, waviness = 1.</li>
	<li><code>202</code>: chữ số ở giữa 0 là một đáy, waviness = 1.</li>
	<li>Tất cả các số còn lại trong đoạn đều có waviness bằng 0.</li>
</ul>

<p>Do đó, tổng waviness là <code>1 + 1 + 1 = 3</code>.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">num1 = 4848, num2 = 4848</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<p>Số <code>4848</code>: chữ số thứ hai 8 là một đỉnh, còn chữ số thứ ba 4 là một đáy, nên waviness bằng 2.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num1 &lt;= num2 &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Cận trên chỉ là $10^5$, nên ta có thể tách các chữ số của từng số nguyên. Những số có ít hơn $3$ chữ số đóng góp $0$; mỗi chữ số ở giữa là một đỉnh hoặc đáy khi nó khác nghiêm ngặt với cả hai chữ số lân cận, sau đó ta cộng các giá trị trên $[num1,num2]$.

<!-- thinking:end -->

Ta định nghĩa một hàm phụ $f(x)$ để tính giá trị waviness của số nguyên $x$. Trong hàm này, ta lưu từng chữ số của số nguyên $x$ vào một mảng $\textit{nums}$. Nếu số có ít hơn 3 chữ số thì giá trị waviness là 0. Ngược lại, ta duyệt qua từng chữ số không ở đầu và không ở cuối trong mảng $\textit{nums}$, xác định xem chữ số đó có phải là đỉnh hoặc đáy hay không, rồi đếm giá trị waviness.

Sau đó, ta duyệt qua từng số nguyên $x$ trong đoạn $[\textit{num1}, \textit{num2}]$ và cộng dồn giá trị waviness $f(x)$ để thu được kết quả cuối cùng.

Độ phức tạp thời gian là $O((\textit{num2} - \textit{num1} + 1) \cdot \log \textit{num2})$ và độ phức tạp không gian là $O(\log \textit{num2})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalWaviness(self, num1: int, num2: int) -> int:
        def f(x: int) -> int:
            nums = []
            while x:
                nums.append(x % 10)
                x //= 10
            m = len(nums)
            if m < 3:
                return 0
            s = 0
            for i in range(1, m - 1):
                if nums[i] > nums[i - 1] and nums[i] > nums[i + 1]:
                    s += 1
                elif nums[i] < nums[i - 1] and nums[i] < nums[i + 1]:
                    s += 1
            return s

        return sum(f(x) for x in range(num1, num2 + 1))
```

#### Java

```java
class Solution {
    public int totalWaviness(int num1, int num2) {
        int ans = 0;
        for (int x = num1; x <= num2; x++) {
            ans += f(x);
        }
        return ans;
    }

    private int f(int x) {
        int[] nums = new int[20];
        int m = 0;
        while (x > 0) {
            nums[m++] = x % 10;
            x /= 10;
        }
        if (m < 3) {
            return 0;
        }
        int s = 0;
        for (int i = 1; i < m - 1; i++) {
            if ((nums[i] > nums[i - 1] && nums[i] > nums[i + 1])
                || (nums[i] < nums[i - 1] && nums[i] < nums[i + 1])) {
                s++;
            }
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalWaviness(int num1, int num2) {
        int ans = 0;
        for (int x = num1; x <= num2; x++) {
            ans += f(x);
        }
        return ans;
    }

    int f(int x) {
        int nums[20], m = 0;
        while (x > 0) {
            nums[m++] = x % 10;
            x /= 10;
        }
        if (m < 3) {
            return 0;
        }
        int s = 0;
        for (int i = 1; i < m - 1; i++) {
            if ((nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) || (nums[i] < nums[i - 1] && nums[i] < nums[i + 1])) {
                s++;
            }
        }
        return s;
    }
};
```

#### Go

```go
func totalWaviness(num1 int, num2 int) (ans int) {
	for x := num1; x <= num2; x++ {
		ans += f(x)
	}
	return
}

func f(x int) int {
	nums := make([]int, 0, 20)
	for x > 0 {
		nums = append(nums, x%10)
		x /= 10
	}
	m := len(nums)
	if m < 3 {
		return 0
	}
	s := 0
	for i := 1; i < m-1; i++ {
		if (nums[i] > nums[i-1] && nums[i] > nums[i+1]) ||
			(nums[i] < nums[i-1] && nums[i] < nums[i+1]) {
			s++
		}
	}
	return s
}
```

#### TypeScript

```ts
function totalWaviness(num1: number, num2: number): number {
    let ans = 0;
    for (let x = num1; x <= num2; x++) {
        ans += f(x);
    }
    return ans;
}

function f(x: number): number {
    const nums: number[] = [];
    while (x > 0) {
        nums.push(x % 10);
        x = Math.floor(x / 10);
    }
    const m = nums.length;
    if (m < 3) return 0;

    let s = 0;
    for (let i = 1; i < m - 1; i++) {
        if (
            (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) ||
            (nums[i] < nums[i - 1] && nums[i] < nums[i + 1])
        ) {
            s++;
        }
    }
    return s;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
