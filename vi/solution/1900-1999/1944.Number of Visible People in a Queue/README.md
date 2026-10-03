---
comments: true
difficulty: Hard
rating: 2104
source: Biweekly Contest 57 Q4
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [1944. Number of Visible People in a Queue](https://leetcode.com/problems/number-of-visible-people-in-a-queue)

[中文文档](/solution/1900-1999/1944.Number%20of%20Visible%20People%20in%20a%20Queue/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> người đang xếp hàng, được đánh số từ <code>0</code> đến <code>n - 1</code> theo thứ tự <strong>từ trái sang phải</strong>. Bạn được cung cấp một mảng <code>heights</code> gồm các số nguyên <strong>khác nhau</strong>, trong đó <code>heights[i]</code> biểu thị chiều cao của người thứ <code>i<sup>th</sup></code>.</p>

<p>Một người có thể <strong>nhìn thấy</strong> một người khác ở bên phải hàng nếu tất cả những người ở giữa đều <strong>thấp hơn</strong> cả hai người đó. Cụ thể hơn, người thứ <code>i<sup>th</sup></code> có thể nhìn thấy người thứ <code>j<sup>th</sup></code> nếu <code>i &lt; j</code> và <code>min(heights[i], heights[j]) &gt; max(heights[i+1], heights[i+2], ..., heights[j-1])</code>.</p>

<p>Hãy trả về <em>một mảng </em><code>answer</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là <strong>số người</strong> mà người thứ </em><code>i<sup>th</sup></code><em> có thể <strong>nhìn thấy</strong> ở bên phải trong hàng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1944.Number%20of%20Visible%20People%20in%20a%20Queue/images/queue-plane.jpg" style="width: 600px; height: 247px;" /></p>

<pre>
<strong>Đầu vào:</strong> heights = [10,6,8,5,11,9]
<strong>Đầu ra:</strong> [3,1,2,1,1,0]
<strong>Giải thích:</strong>
Người 0 có thể nhìn thấy người 1, 2 và 4.
Người 1 có thể nhìn thấy người 2.
Người 2 có thể nhìn thấy người 3 và 4.
Người 3 có thể nhìn thấy người 4.
Người 4 có thể nhìn thấy người 5.
Người 5 không thể nhìn thấy ai vì không có ai ở bên phải họ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> heights = [5,1,2,3,10]
<strong>Đầu ra:</strong> [4,1,1,1,0]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == heights.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= heights[i] &lt;= 10<sup>5</sup></code></li>
	<li>Tất cả các giá trị của <code>heights</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ngăn xếp đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Người $i$ nhìn thấy $j$ khi mọi người ở giữa đều thấp hơn. Duyệt sang phải cho từng $i$ sẽ có độ phức tạp $O(n^2)$.
>
> Các chiều cao có thể nhìn thấy ở bên phải $i$ tăng nghiêm ngặt. Một stack tăng dần từ đỉnh xuống đáy sẽ loại bỏ mọi người thấp hơn (mỗi người được tính), sau đó nếu stack vẫn còn phần tử thì cộng thêm một người bị che bởi người cao hơn.
>
> Mỗi chiều cao được đưa vào và loại khỏi stack đúng một lần.

<!-- thinking:end -->

Ta nhận thấy rằng đối với người thứ $i$, chiều cao của những người mà họ có thể nhìn thấy phải tăng nghiêm ngặt từ trái sang phải.

Do đó, ta có thể duyệt mảng $\textit{heights}$ theo chiều ngược lại, sử dụng một stack $\textit{stk}$ tăng dần từ đỉnh xuống đáy để lưu chiều cao của những người đã duyệt.

Với người thứ $i$, nếu stack không rỗng và phần tử ở đỉnh nhỏ hơn $\textit{heights}[i]$, ta tăng số người mà người thứ $i$ có thể nhìn thấy lên 1, sau đó loại phần tử ở đỉnh stack. Ta lặp lại thao tác này cho đến khi stack rỗng hoặc phần tử ở đỉnh stack lớn hơn hoặc bằng $\textit{heights}[i]$. Nếu stack vẫn không rỗng ở thời điểm này, điều đó có nghĩa là phần tử ở đỉnh stack lớn hơn hoặc bằng $\textit{heights}[i]$, nên ta tăng số người mà người thứ $i$ có thể nhìn thấy lên 1.

Tiếp theo, ta đưa $\textit{heights}[i]$ vào stack và tiếp tục với người kế tiếp.

Sau khi duyệt xong, ta trả về mảng kết quả $\textit{ans}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $\textit{heights}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canSeePersonsCount(self, heights: List[int]) -> List[int]:
        n = len(heights)
        ans = [0] * n
        stk = []
        for i in range(n - 1, -1, -1):
            while stk and stk[-1] < heights[i]:
                ans[i] += 1
                stk.pop()
            if stk:
                ans[i] += 1
            stk.append(heights[i])
        return ans
```

#### Java

```java
class Solution {
    public int[] canSeePersonsCount(int[] heights) {
        int n = heights.length;
        int[] ans = new int[n];
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && stk.peek() < heights[i]) {
                stk.pop();
                ++ans[i];
            }
            if (!stk.isEmpty()) {
                ++ans[i];
            }
            stk.push(heights[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> canSeePersonsCount(vector<int>& heights) {
        int n = heights.size();
        vector<int> ans(n);
        stack<int> stk;
        for (int i = n - 1; ~i; --i) {
            while (stk.size() && stk.top() < heights[i]) {
                ++ans[i];
                stk.pop();
            }
            if (stk.size()) {
                ++ans[i];
            }
            stk.push(heights[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func canSeePersonsCount(heights []int) []int {
	n := len(heights)
	ans := make([]int, n)
	stk := []int{}
	for i := n - 1; i >= 0; i-- {
		for len(stk) > 0 && stk[len(stk)-1] < heights[i] {
			ans[i]++
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			ans[i]++
		}
		stk = append(stk, heights[i])
	}
	return ans
}
```

#### TypeScript

```ts
function canSeePersonsCount(heights: number[]): number[] {
    const n = heights.length;
    const ans: number[] = new Array(n).fill(0);
    const stk: number[] = [];
    for (let i = n - 1; ~i; --i) {
        while (stk.length && stk.at(-1) < heights[i]) {
            ++ans[i];
            stk.pop();
        }
        if (stk.length) {
            ++ans[i];
        }
        stk.push(heights[i]);
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_see_persons_count(heights: Vec<i32>) -> Vec<i32> {
        let n = heights.len();
        let mut ans = vec![0; n];
        let mut stack = Vec::new();
        for i in (0..n).rev() {
            while !stack.is_empty() {
                ans[i] += 1;
                if heights[i] <= heights[*stack.last().unwrap()] {
                    break;
                }
                stack.pop();
            }
            stack.push(i);
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
int* canSeePersonsCount(int* heights, int heightsSize, int* returnSize) {
    int* ans = malloc(sizeof(int) * heightsSize);
    memset(ans, 0, sizeof(int) * heightsSize);
    int stack[heightsSize];
    int i = 0;
    for (int j = heightsSize - 1; j >= 0; j--) {
        while (i) {
            ans[j]++;
            if (heights[j] <= heights[stack[i - 1]]) {
                break;
            }
            i--;
        }
        stack[i++] = j;
    }
    *returnSize = heightsSize;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
