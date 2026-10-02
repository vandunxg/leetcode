---
comments: true
difficulty: Medium
rating: 1618
source: Weekly Contest 196 Q2
tags:
    - Brainteaser
    - Array
    - Simulation
---

<!-- problem:start -->

# [1503. Last Moment Before All Ants Fall Out of a Plank](https://leetcode.com/problems/last-moment-before-all-ants-fall-out-of-a-plank)

[中文文档](/solution/1500-1599/1503.Last%20Moment%20Before%20All%20Ants%20Fall%20Out%20of%20a%20Plank/README.md)

## Mô tả

<!-- description:start -->

<p>Ta có một tấm ván gỗ dài <code>n</code> <strong>đơn vị</strong>. Một số con kiến đang di chuyển trên tấm ván, mỗi con kiến di chuyển với tốc độ <strong>1 đơn vị mỗi giây</strong>. Một số con kiến di chuyển sang <strong>trái</strong>, những con còn lại di chuyển sang <strong>phải</strong>.</p>

<p>Khi hai con kiến di chuyển theo hai hướng <strong>khác nhau</strong> gặp nhau tại một điểm, chúng đổi hướng và tiếp tục di chuyển. Giả sử việc đổi hướng không mất thêm thời gian.</p>

<p>Khi một con kiến chạm vào <strong>một đầu</strong> của tấm ván tại thời điểm <code>t</code>, nó lập tức rơi khỏi tấm ván.</p>

<p>Cho một số nguyên <code>n</code> và hai mảng số nguyên <code>left</code> và <code>right</code>, trong đó lần lượt là vị trí của các con kiến di chuyển sang trái và sang phải, hãy trả về <em>thời điểm con kiến cuối cùng rơi khỏi tấm ván</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1503.Last%20Moment%20Before%20All%20Ants%20Fall%20Out%20of%20a%20Plank/images/ants.jpg" style="width: 450px; height: 610px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, left = [4,3], right = [0,1]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Trong hình trên:
-Con kiến ở chỉ số 0 được đặt tên là A và đang đi sang phải.
-Con kiến ở chỉ số 1 được đặt tên là B và đang đi sang phải.
-Con kiến ở chỉ số 3 được đặt tên là C và đang đi sang trái.
-Con kiến ở chỉ số 4 được đặt tên là D và đang đi sang trái.
Thời điểm cuối cùng một con kiến còn ở trên tấm ván là t = 4 giây. Sau đó, nó lập tức rơi khỏi tấm ván. (Nói cách khác, tại t = 4.0000000001, không còn con kiến nào trên tấm ván).
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1503.Last%20Moment%20Before%20All%20Ants%20Fall%20Out%20of%20a%20Plank/images/ants2.jpg" style="width: 639px; height: 101px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, left = [], right = [0,1,2,3,4,5,6,7]
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Tất cả các con kiến đều đi sang phải, con kiến ở chỉ số 0 cần 7 giây để rơi khỏi tấm ván.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1503.Last%20Moment%20Before%20All%20Ants%20Fall%20Out%20of%20a%20Plank/images/ants3.jpg" style="width: 639px; height: 100px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, left = [0,1,2,3,4,5,6,7], right = []
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong> Tất cả các con kiến đều đi sang trái, con kiến ở chỉ số 7 cần 7 giây để rơi khỏi tấm ván.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= left.length &lt;= n + 1</code></li>
	<li><code>0 &lt;= left[i] &lt;= n</code></li>
	<li><code>0 &lt;= right.length &lt;= n + 1</code></li>
	<li><code>0 &lt;= right[i] &lt;= n</code></li>
	<li><code>1 &lt;= left.length + right.length &lt;= n + 1</code></li>
	<li>Tất cả giá trị của <code>left</code> và <code>right</code> là duy nhất, và mỗi giá trị chỉ có thể xuất hiện trong <strong>một</strong> trong hai mảng.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố mẹo

<!-- thinking:start -->

> **Tư duy**
>
> Mô phỏng từng giây và đổi hướng khi va chạm sẽ tốn thời gian tỷ lệ với độ dài tấm ván nhân với số con kiến. Cách này vẫn có thể chạy được khi $n\le 10^4$, nhưng rườm rà và không cần thiết.
>
> Khi hai con kiến gặp nhau và quay đầu, vị trí của chúng về sau trùng với đường đi mà chúng sẽ đi nếu đi xuyên qua nhau. Vì vậy có thể bỏ qua các va chạm, và đáp án là thời gian lớn nhất để một con kiến đi sang trái đến $0$ hoặc một con kiến đi sang phải đến $n$.

<!-- thinking:end -->

Điểm mấu chốt của bài toán là khi hai con kiến gặp nhau rồi quay đầu, điều này tương đương với việc hai con kiến tiếp tục di chuyển theo hướng ban đầu. Vì vậy, ta chỉ cần tìm quãng đường lớn nhất mà bất kỳ con kiến nào di chuyển được.

Lưu ý rằng độ dài của các mảng $\textit{left}$ và $\textit{right}$ có thể bằng $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của tấm ván. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getLastMoment(self, n: int, left: List[int], right: List[int]) -> int:
        ans = 0
        for x in left:
            ans = max(ans, x)
        for x in right:
            ans = max(ans, n - x)
        return ans
```

#### Java

```java
class Solution {
    public int getLastMoment(int n, int[] left, int[] right) {
        int ans = 0;
        for (int x : left) {
            ans = Math.max(ans, x);
        }
        for (int x : right) {
            ans = Math.max(ans, n - x);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int getLastMoment(int n, vector<int>& left, vector<int>& right) {
        int ans = 0;
        for (int& x : left) {
            ans = max(ans, x);
        }
        for (int& x : right) {
            ans = max(ans, n - x);
        }
        return ans;
    }
};
```

#### Go

```go
func getLastMoment(n int, left []int, right []int) (ans int) {
	for _, x := range left {
		ans = max(ans, x)
	}
	for _, x := range right {
		ans = max(ans, n-x)
	}
	return
}
```

#### TypeScript

```ts
function getLastMoment(n: number, left: number[], right: number[]): number {
    let ans = 0;
    for (const x of left) {
        ans = Math.max(ans, x);
    }
    for (const x of right) {
        ans = Math.max(ans, n - x);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
