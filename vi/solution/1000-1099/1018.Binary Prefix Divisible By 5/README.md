---
comments: true
difficulty: Easy
rating: 1376
source: Weekly Contest 130 Q1
tags:
    - Bit Manipulation
    - Array
---

<!-- problem:start -->

# [1018. Binary Prefix Divisible By 5](https://leetcode.com/problems/binary-prefix-divisible-by-5)

[中文文档](/solution/1000-1099/1018.Binary%20Prefix%20Divisible%20By%205/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng nhị phân <code>nums</code> (đánh chỉ số từ <strong>0</strong>).</p>

<p>Ta định nghĩa <code>x<sub>i</sub></code> là số có biểu diễn nhị phân bằng mảng con <code>nums[0..i]</code> (theo thứ tự từ bit có trọng số lớn nhất đến bit có trọng số nhỏ nhất).</p>

<ul>
	<li>Ví dụ, nếu <code>nums = [1,0,1]</code>, thì <code>x<sub>0</sub> = 1</code>, <code>x<sub>1</sub> = 2</code> và <code>x<sub>2</sub> = 5</code>.</li>
</ul>

<p>Hãy trả về <em>mảng boolean </em><code>answer</code>, trong đó <code>answer[i]</code><em> là </em><code>true</code><em> nếu </em><code>x<sub>i</sub></code><em> chia hết cho </em><code>5</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1,1]
<strong>Đầu ra:</strong> [true,false,false]
<strong>Giải thích:</strong> Các số đầu vào ở dạng nhị phân là 0, 01, 011; tương ứng ở hệ thập phân là 0, 1 và 3.
Chỉ prefix đầu tiên chia hết cho 5, nên answer[0] là true.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,1]
<strong>Đầu ra:</strong> [false,false,false]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>nums[i]</code> chỉ có thể là <code>0</code> hoặc <code>1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể dựng từng số prefix rồi kiểm tra modulo $5$, nhưng với $n\le 10^5$, giá trị có thể lớn đến $2^{10^5}$. Thực ra chỉ cần giữ lại phần dư modulo $5$.
>
> Prefix tiếp theo bằng giá trị trước đó dịch trái một bit rồi cộng bit mới: $x\leftarrow (2x+v)\bmod 5$.
>
> Ta cập nhật phần dư này cho từng prefix và ghi nhận phần dư có bằng $0$ hay không.

<!-- thinking:end -->

Ta dùng biến $x$ biểu diễn prefix nhị phân hiện tại, rồi duyệt mảng $nums$. Với mỗi phần tử $v$, ta dịch trái $x$ một bit, cộng $v$ rồi lấy modulo $5$. Nếu kết quả bằng $0$, prefix nhị phân hiện tại chia hết cho $5$ nên ta thêm $\textit{true}$ vào mảng kết quả; nếu không thì thêm $\textit{false}$.

Độ phức tạp thời gian là $O(n)$. Nếu không tính bộ nhớ của mảng kết quả, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def prefixesDivBy5(self, nums: List[int]) -> List[bool]:
        ans = []
        x = 0
        for v in nums:
            x = (x << 1 | v) % 5
            ans.append(x == 0)
        return ans
```

#### Java

```java
class Solution {
    public List<Boolean> prefixesDivBy5(int[] nums) {
        List<Boolean> ans = new ArrayList<>();
        int x = 0;
        for (int v : nums) {
            x = (x << 1 | v) % 5;
            ans.add(x == 0);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> prefixesDivBy5(vector<int>& nums) {
        vector<bool> ans;
        int x = 0;
        for (int v : nums) {
            x = (x << 1 | v) % 5;
            ans.push_back(x == 0);
        }
        return ans;
    }
};
```

#### Go

```go
func prefixesDivBy5(nums []int) (ans []bool) {
	x := 0
	for _, v := range nums {
		x = (x<<1 | v) % 5
		ans = append(ans, x == 0)
	}
	return
}
```

#### TypeScript

```ts
function prefixesDivBy5(nums: number[]): boolean[] {
    const ans: boolean[] = [];
    let x = 0;
    for (const v of nums) {
        x = ((x << 1) | v) % 5;
        ans.push(x === 0);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
