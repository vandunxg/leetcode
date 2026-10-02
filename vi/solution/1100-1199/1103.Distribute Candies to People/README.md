---
comments: true
difficulty: Easy
rating: 1287
source: Weekly Contest 143 Q1
tags:
    - Math
    - Simulation
---

<!-- problem:start -->

# [1103. Distribute Candies to People](https://leetcode.com/problems/distribute-candies-to-people)

[中文文档](/solution/1100-1199/1103.Distribute%20Candies%20to%20People/README.md)

## Mô tả

<!-- description:start -->

<p>Ta chia một số lượng <code>candies</code> cho một hàng gồm <strong><code>n =&nbsp;num_people</code></strong>&nbsp;người theo cách sau:</p>

<p>Đầu tiên, ta đưa 1 viên kẹo cho người thứ nhất, 2 viên cho người thứ hai, cứ tiếp tục như vậy cho đến khi đưa <code>n</code>&nbsp;viên cho người cuối cùng.</p>

<p>Sau đó, ta quay lại đầu hàng, đưa <code>n&nbsp;+ 1</code> viên kẹo cho người thứ nhất, <code>n&nbsp;+ 2</code> viên cho người thứ hai, cứ tiếp tục như vậy cho đến khi đưa <code>2 * n</code>&nbsp;viên cho người cuối cùng.</p>

<p>Quá trình này lặp lại (mỗi lượt tăng thêm một viên kẹo, và quay về đầu hàng sau khi đến cuối hàng) cho đến khi hết kẹo.&nbsp; Người cuối cùng sẽ nhận toàn bộ số kẹo còn lại (không nhất thiết nhiều hơn lượt trước đúng một viên).</p>

<p>Trả về mảng có độ dài <code>num_people</code>&nbsp;và tổng các phần tử bằng <code>candies</code>, biểu diễn cách chia kẹo cuối cùng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = 7, num_people = 4
<strong>Đầu ra:</strong> [1,2,3,1]
<strong>Giải thích:</strong>
Ở lượt đầu, ans[0] += 1, mảng trở thành [1,0,0,0].
Ở lượt thứ hai, ans[1] += 2, mảng trở thành [1,2,0,0].
Ở lượt thứ ba, ans[2] += 3, mảng trở thành [1,2,3,0].
Ở lượt thứ tư, ans[3] += 1 (vì chỉ còn một viên kẹo), và mảng cuối cùng là [1,2,3,1].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = 10, num_people = 3
<strong>Đầu ra:</strong> [5,2,3]
<strong>Giải thích: </strong>
Ở lượt đầu, ans[0] += 1, mảng trở thành [1,0,0].
Ở lượt thứ hai, ans[1] += 2, mảng trở thành [1,2,0].
Ở lượt thứ ba, ans[2] += 3, mảng trở thành [1,2,3].
Ở lượt thứ tư, ans[0] += 4, và mảng cuối cùng là [5,2,3].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>1 &lt;= candies &lt;= 10^9</li>
	<li>1 &lt;= num_people &lt;= 1000</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ở lượt phát thứ $i$ (đánh số từ 0), người thứ $i\bmod \textit{num\_people}$ nhận $\min(\textit{candies}, i+1)$ viên kẹo. Số lượt được xác định bởi các số tam giác, xấp xỉ $\sqrt{2\cdot\textit{candies}}$, nên mô phỏng trực tiếp vẫn nằm trong giới hạn.
>
> Không cần công thức tính gộp cho các vòng đầy đủ: giới hạn mỗi lượt phát theo số kẹo còn lại và dừng khi hết kẹo.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp quá trình phát kẹo cho từng người theo đúng quy tắc của đề bài.

Độ phức tạp thời gian là $O(\max(\sqrt{candies}, num\_people))$ và độ phức tạp không gian là $O(num\_people)$. Trong đó, $candies$ là số viên kẹo.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def distributeCandies(self, candies: int, num_people: int) -> List[int]:
        ans = [0] * num_people
        i = 0
        while candies:
            ans[i % num_people] += min(candies, i + 1)
            candies -= min(candies, i + 1)
            i += 1
        return ans
```

#### Java

```java
class Solution {
    public int[] distributeCandies(int candies, int num_people) {
        int[] ans = new int[num_people];
        for (int i = 0; candies > 0; ++i) {
            ans[i % num_people] += Math.min(candies, i + 1);
            candies -= Math.min(candies, i + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> distributeCandies(int candies, int num_people) {
        vector<int> ans(num_people);
        for (int i = 0; candies > 0; ++i) {
            ans[i % num_people] += min(candies, i + 1);
            candies -= min(candies, i + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func distributeCandies(candies int, num_people int) []int {
	ans := make([]int, num_people)
	for i := 0; candies > 0; i++ {
		ans[i%num_people] += min(candies, i+1)
		candies -= min(candies, i+1)
	}
	return ans
}
```

#### TypeScript

```ts
function distributeCandies(candies: number, num_people: number): number[] {
    const ans: number[] = Array(num_people).fill(0);
    for (let i = 0; candies > 0; ++i) {
        ans[i % num_people] += Math.min(candies, i + 1);
        candies -= Math.min(candies, i + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
