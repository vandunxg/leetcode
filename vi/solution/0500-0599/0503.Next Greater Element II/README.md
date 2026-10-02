---
comments: true
difficulty: Medium
tags:
    - Stack
    - Array
    - Monotonic Stack
---

<!-- problem:start -->

# [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii)

[中文文档](/solution/0500-0599/0503.Next%20Greater%20Element%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên vòng tròn <code>nums</code> (tức là phần tử tiếp theo của <code>nums[nums.length - 1]</code> là <code>nums[0]</code>), hãy trả về <em><strong>số lớn hơn tiếp theo</strong> cho mỗi phần tử trong</em> <code>nums</code>.</p>

<p><strong>Số lớn hơn tiếp theo</strong> của một số <code>x</code> là số đầu tiên lớn hơn nó theo thứ tự duyệt trong mảng; nghĩa là ta có thể tìm vòng tròn để tìm số lớn hơn tiếp theo. Nếu không tồn tại, trả về <code>-1</code> cho số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,1]
<strong>Đầu ra:</strong> [2,-1,2]
Giải thích: Số lớn hơn tiếp theo của số 1 đầu tiên là 2.
Không có số nào lớn hơn tiếp theo của số 2.
Để tìm số lớn hơn tiếp theo của số 1 thứ hai, cần tìm vòng tròn; kết quả cũng là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4,3]
<strong>Đầu ra:</strong> [2,3,4,-1,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>4</sup></code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Quét đơn giản sang phải để tìm giá trị lớn hơn tiếp theo sẽ mất $O(n^2)$, quá chậm với mảng vòng tròn có $n \le 10^4$. Chỉ số quay vòng sau mỗi $n$ phần tử.
>
> Duyệt từ phải sang trái bằng stack giảm dần chứa các ứng viên chưa bị loại. Hai lượt duyệt mô phỏng mảng vòng tròn; phép modulo đưa ta về chỉ số ban đầu. Mỗi giá trị được đưa vào và lấy ra khỏi stack nhiều nhất một lần.

<!-- thinking:end -->

Bài toán yêu cầu tìm phần tử lớn hơn tiếp theo cho mỗi phần tử. Vì vậy, ta có thể duyệt mảng từ cuối về đầu, tương đương với việc tìm phần tử lớn hơn trước đó. Ngoài ra, vì mảng có tính vòng tròn, ta có thể duyệt mảng hai lượt.

Cụ thể, ta bắt đầu duyệt mảng từ chỉ số $n \times 2 - 1$, trong đó $n$ là độ dài mảng. Sau đó, đặt $j = i \bmod n$, với $\bmod$ là phép modulo. Nếu stack không rỗng và phần tử ở đỉnh nhỏ hơn hoặc bằng $nums[j]$, ta liên tục pop phần tử ở đỉnh cho đến khi stack rỗng hoặc phần tử ở đỉnh lớn hơn $nums[j]$. Lúc này, phần tử ở đỉnh là phần tử lớn hơn trước đó của $nums[j]$ và ta gán nó cho $ans[j]$. Cuối cùng, ta push $nums[j]$ vào stack rồi tiếp tục với phần tử tiếp theo.

Sau khi duyệt xong, ta thu được mảng $ans$, trong đó mỗi phần tử là phần tử lớn hơn tiếp theo tương ứng trong mảng $nums$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [-1] * n
        stk = []
        for i in range(n * 2 - 1, -1, -1):
            i %= n
            while stk and stk[-1] <= nums[i]:
                stk.pop()
            if stk:
                ans[i] = stk[-1]
            stk.append(nums[i])
        return ans
```

#### Java

```java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] ans = new int[n];
        Arrays.fill(ans, -1);
        Deque<Integer> stk = new ArrayDeque<>();
        for (int i = n * 2 - 1; i >= 0; --i) {
            int j = i % n;
            while (!stk.isEmpty() && stk.peek() <= nums[j]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                ans[j] = stk.peek();
            }
            stk.push(nums[j]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans(n, -1);
        stack<int> stk;
        for (int i = n * 2 - 1; ~i; --i) {
            int j = i % n;
            while (stk.size() && stk.top() <= nums[j]) {
                stk.pop();
            }
            if (stk.size()) {
                ans[j] = stk.top();
            }
            stk.push(nums[j]);
        }
        return ans;
    }
};
```

#### Go

```go
func nextGreaterElements(nums []int) []int {
	n := len(nums)
	ans := make([]int, n)
	for i := range ans {
		ans[i] = -1
	}
	stk := []int{}
	for i := n*2 - 1; i >= 0; i-- {
		j := i % n
		for len(stk) > 0 && stk[len(stk)-1] <= nums[j] {
			stk = stk[:len(stk)-1]
		}
		if len(stk) > 0 {
			ans[j] = stk[len(stk)-1]
		}
		stk = append(stk, nums[j])
	}
	return ans
}
```

#### TypeScript

```ts
function nextGreaterElements(nums: number[]): number[] {
    const n = nums.length;
    const stk: number[] = [];
    const ans: number[] = Array(n).fill(-1);
    for (let i = n * 2 - 1; ~i; --i) {
        const j = i % n;
        while (stk.length && stk.at(-1)! <= nums[j]) {
            stk.pop();
        }
        if (stk.length) {
            ans[j] = stk.at(-1)!;
        }
        stk.push(nums[j]);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} nums
 * @return {number[]}
 */
var nextGreaterElements = function (nums) {
    const n = nums.length;
    const stk = [];
    const ans = Array(n).fill(-1);
    for (let i = n * 2 - 1; ~i; --i) {
        const j = i % n;
        while (stk.length && stk.at(-1) <= nums[j]) {
            stk.pop();
        }
        if (stk.length) {
            ans[j] = stk.at(-1);
        }
        stk.push(nums[j]);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
