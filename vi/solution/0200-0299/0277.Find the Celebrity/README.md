---
comments: true
difficulty: Medium
tags:
    - Graph
    - Two Pointers
    - Interactive
---

<!-- problem:start -->

# [277. Find the Celebrity 🔒](https://leetcode.com/problems/find-the-celebrity)

[中文文档](/solution/0200-0299/0277.Find%20the%20Celebrity/README.md)

## Mô tả

<!-- description:start -->

<p>Giả sử bạn đang ở một buổi tiệc có <code>n</code> người được đánh số từ <code>0</code> đến <code>n - 1</code>, và trong số đó có thể có một người nổi tiếng. Người nổi tiếng được định nghĩa là người mà tất cả <code>n - 1</code> người còn lại đều biết, nhưng người đó không biết bất kỳ ai trong số họ.</p>

<p>Bạn cần tìm người nổi tiếng hoặc xác nhận rằng không có ai như vậy. Bạn chỉ được phép hỏi những câu như: &quot;Chào A. Bạn có biết B không?&quot; để biết A có biết B hay không. Hãy tìm người nổi tiếng (hoặc xác nhận không có người nổi tiếng) bằng cách đặt ít câu hỏi nhất có thể (xét theo bậc tiệm cận).</p>

<p>Cho số nguyên <code>n</code> và hàm hỗ trợ <code>bool knows(a, b)</code> cho biết <code>a</code> có biết <code>b</code> hay không. Hãy triển khai hàm <code>int findCelebrity(n)</code>. Nếu có người nổi tiếng tại buổi tiệc thì người đó là duy nhất.</p>

<p>Trả về <em>nhãn của người nổi tiếng nếu có người đó tại buổi tiệc</em>. Nếu không có, trả về <code>-1</code>.</p>

<p><strong>Lưu ý</strong> rằng mảng 2D <code>n x n</code> <code>graph</code> được cung cấp làm input <strong>không</strong> khả dụng trực tiếp; bạn chỉ có thể truy cập thông qua hàm hỗ trợ <code>knows</code>. <code>graph[i][j] == 1</code> biểu thị người <code>i</code> biết người <code>j</code>, trong khi <code>graph[i][j] == 0</code> biểu thị người <code>j</code> không biết người <code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0277.Find%20the%20Celebrity/images/g1.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,1,0],[0,1,0],[1,1,1]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Có ba người được đánh số 0, 1 và 2. graph[i][j] = 1 nghĩa là người i biết người j; ngược lại, graph[i][j] = 0 nghĩa là người i không biết người j. Người nổi tiếng là người mang số 1 vì cả người 0 và 2 đều biết người đó, còn người 1 không biết ai.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0277.Find%20the%20Celebrity/images/g2.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> graph = [[1,0,1],[1,1,0],[0,1,1]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có người nổi tiếng.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == graph.length == graph[i].length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>graph[i][j]</code> là <code>0</code> hoặc <code>1</code>.</li>
	<li><code>graph[i][i] == 1</code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Câu hỏi mở rộng:</strong> Nếu số lần gọi API <code>knows</code> tối đa được phép là <code>3 * n</code>, bạn có thể tìm lời giải mà không vượt quá số lần gọi này không?</p>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Gọi $knows$ cho mọi cặp người sẽ có độ phức tạp bậc hai. Chỉ có thể có nhiều nhất một người nổi tiếng: nếu $a$ biết $b$, thì $a$ không thể là người đó và ta chuyển ứng viên sang $b$.
>
> Duyệt một lượt sẽ còn lại một ứng viên; lượt thứ hai kiểm tra rằng ứng viên không biết ai và mọi người đều biết ứng viên.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# The knows API is already defined for you.
# return a bool, whether a knows b
# def knows(a: int, b: int) -> bool:


class Solution:
    def findCelebrity(self, n: int) -> int:
        ans = 0
        for i in range(1, n):
            if knows(ans, i):
                ans = i
        for i in range(n):
            if ans != i:
                if knows(ans, i) or not knows(i, ans):
                    return -1
        return ans
```

#### Java

```java
/* The knows API is defined in the parent class Relation.
      boolean knows(int a, int b); */

public class Solution extends Relation {
    public int findCelebrity(int n) {
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            if (knows(ans, i)) {
                ans = i;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (ans != i) {
                if (knows(ans, i) || !knows(i, ans)) {
                    return -1;
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/* The knows API is defined for you.
      bool knows(int a, int b); */

class Solution {
public:
    int findCelebrity(int n) {
        int ans = 0;
        for (int i = 1; i < n; ++i) {
            if (knows(ans, i)) {
                ans = i;
            }
        }
        for (int i = 0; i < n; ++i) {
            if (ans != i) {
                if (knows(ans, i) || !knows(i, ans)) {
                    return -1;
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * The knows API is already defined for you.
 *     knows := func(a int, b int) bool
 */
func solution(knows func(a int, b int) bool) func(n int) int {
	return func(n int) int {
		ans := 0
		for i := 1; i < n; i++ {
			if knows(ans, i) {
				ans = i
			}
		}
		for i := 0; i < n; i++ {
			if ans != i {
				if knows(ans, i) || !knows(i, ans) {
					return -1
				}
			}
		}
		return ans
	}
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
