---
comments: true
difficulty: Easy
rating: 1115
source: Weekly Contest 383 Q1
tags:
    - Array
    - Prefix Sum
    - Simulation
---

<!-- problem:start -->

# [3028. Ant on the Boundary](https://leetcode.com/problems/ant-on-the-boundary)

[中文文档](/solution/3000-3099/3028.Ant%20on%20the%20Boundary/README.md)

## Mô tả

<!-- description:start -->

<p>Một con kiến đang ở trên một đường biên. Đôi khi nó đi <strong>sang trái</strong> và đôi khi đi <strong>sang phải</strong>.</p>

<p>Bạn được cung cấp một mảng số nguyên <strong>khác 0</strong> <code>nums</code>. Con kiến bắt đầu đọc <code>nums</code> từ phần tử đầu tiên đến phần tử cuối cùng. Ở mỗi bước, nó di chuyển theo giá trị của phần tử hiện tại:</p>

<ul>
	<li>Nếu <code>nums[i] &lt; 0</code>, nó di chuyển <strong>sang trái</strong> <!-- notionvc: 55fee232-4fc9-445f-952a-f1b979415864 --><code>-nums[i]</code> đơn vị.</li>
	<li>Nếu <code>nums[i] &gt; 0</code>, nó di chuyển <strong>sang phải</strong> <code>nums[i]</code> đơn vị.</li>
</ul>

<p>Hãy trả về <em>số lần con kiến <strong>quay lại</strong> đường biên.</em></p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li>Có không gian vô hạn ở cả hai phía của đường biên.</li>
	<li>Chúng ta chỉ kiểm tra xem con kiến có ở trên đường biên hay không sau khi nó đã di chuyển <code>|nums[i]|</code> đơn vị. Nói cách khác, nếu con kiến vượt qua đường biên trong quá trình di chuyển thì không được tính.<!-- notionvc: 5ff95338-8634-4d02-a085-1e83c0be6fcd --></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,3,-5]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Sau bước đầu tiên, con kiến cách đường biên 2 bước về bên phải<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Sau bước thứ hai, con kiến cách đường biên 5 bước về bên phải<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Sau bước thứ ba, con kiến ở trên đường biên.
Vì vậy, đáp án là 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [3,2,-3,-4]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Sau bước đầu tiên, con kiến cách đường biên 3 bước về bên phải<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Sau bước thứ hai, con kiến cách đường biên 5 bước về bên phải<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Sau bước thứ ba, con kiến cách đường biên 2 bước về bên phải<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Sau bước thứ tư, con kiến cách đường biên 2 bước về bên trái<!-- notionvc: 61ace51c-559f-4bc6-800f-0a0db2540433 -->.
Con kiến không bao giờ quay lại đường biên, vì vậy đáp án là 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>-10 &lt;= nums[i] &lt;= 10</code></li>
	<li><code>nums[i] != 0</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Con kiến bắt đầu tại gốc tọa độ và $n \le 100$. Sau một bước, nó ở trên đường biên khi và chỉ khi vị trí bằng $0$.
>
> Vị trí chính là tổng tiền tố, nên chúng ta đếm số lần tổng đó bằng 0.
>
> Chỉ cần cộng dồn một lần; không cần mô phỏng đường đi theo hình học.

<!-- thinking:end -->

Theo mô tả đề bài, chúng ta chỉ cần tính xem có bao nhiêu số 0 trong tất cả các tổng tiền tố của `nums`.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của `nums`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def returnToBoundaryCount(self, nums: List[int]) -> int:
        return sum(s == 0 for s in accumulate(nums))
```

#### Java

```java
class Solution {
    public int returnToBoundaryCount(int[] nums) {
        int ans = 0, s = 0;
        for (int x : nums) {
            s += x;
            if (s == 0) {
                ++ans;
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
    int returnToBoundaryCount(vector<int>& nums) {
        int ans = 0, s = 0;
        for (int x : nums) {
            s += x;
            ans += s == 0;
        }
        return ans;
    }
};
```

#### Go

```go
func returnToBoundaryCount(nums []int) (ans int) {
	s := 0
	for _, x := range nums {
		s += x
		if s == 0 {
			ans++
		}
	}
	return
}
```

#### TypeScript

```ts
function returnToBoundaryCount(nums: number[]): number {
    let [ans, s] = [0, 0];
    for (const x of nums) {
        s += x;
        ans += s === 0 ? 1 : 0;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
