---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Two Pointers
---

<!-- problem:start -->

# [2422. Merge Operations to Turn Array Into a Palindrome 🔒](https://leetcode.com/problems/merge-operations-to-turn-array-into-a-palindrome)

[中文文档](/solution/2400-2499/2422.Merge%20Operations%20to%20Turn%20Array%20Into%20a%20Palindrome/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên <strong>dương</strong>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn hai phần tử <strong>liền kề</strong> bất kỳ và <strong>thay thế</strong> chúng bằng <strong>tổng</strong> của chúng.

    <ul>
    <li>Ví dụ, nếu <code>nums = [1,<u>2,3</u>,1]</code>, bạn có thể thực hiện một thao tác để biến nó thành <code>[1,5,1]</code>.</li>
    </ul>
    </li>

</ul>

<p>Hãy trả về <em>số thao tác <strong>ít nhất</strong> cần thực hiện để biến mảng thành một <strong>palindrome</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [4,3,2,1,2,3,1]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể biến mảng thành một palindrome sau 2 thao tác như sau:
- Thực hiện thao tác trên phần tử thứ tư và thứ năm của mảng, nums trở thành [4,3,2,<strong><u>3</u></strong>,3,1].
- Thực hiện thao tác trên phần tử thứ năm và thứ sáu của mảng, nums trở thành [4,3,2,3,<strong><u>4</u></strong>].
Mảng [4,3,2,3,4] là một palindrome.
Có thể chứng minh rằng 2 là số thao tác ít nhất cần thực hiện.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta thực hiện thao tác 3 lần ở bất kỳ vị trí nào, cuối cùng thu được mảng [10], đây là một palindrome.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Các lần gộp sẽ cộng các giá trị liền kề và số lần gộp cần được tối thiểu; mảng phải trở thành một palindrome. Hai đầu cuối cùng phải bằng nhau, nên ta xử lý từ ngoài vào: gộp ở phía có tổng đang nhỏ hơn.
>
> Hai con trỏ giữ các tổng $a$ và $b$. Tiến con trỏ ở phía nhỏ hơn và đếm một lần gộp; khi hai tổng bằng nhau, di chuyển cả hai vào trong. Mỗi phần tử được gộp vào nhiều nhất một lần.

<!-- thinking:end -->

Xác định hai con trỏ $i$ và $j$, lần lượt trỏ đến đầu và cuối mảng, dùng các biến $a$ và $b$ để biểu diễn giá trị của phần tử đầu và cuối, và biến $ans$ để biểu diễn số thao tác.

Nếu $a < b$, ta di chuyển con trỏ $i$ sang phải một bước, tức là $i \leftarrow i + 1$, sau đó cộng giá trị của phần tử mà $i$ trỏ đến vào $a$, tức là $a \leftarrow a + nums[i]$, rồi tăng số thao tác lên một, tức là $ans \leftarrow ans + 1$.

Nếu $a > b$, ta di chuyển con trỏ $j$ sang trái một bước, tức là $j \leftarrow j - 1$, sau đó cộng giá trị của phần tử mà $j$ trỏ đến vào $b$, tức là $b \leftarrow b + nums[j]$, rồi tăng số thao tác lên một, tức là $ans \leftarrow ans + 1$.

Ngược lại, nghĩa là $a = b$, lúc này ta di chuyển con trỏ $i$ sang phải một bước, tức là $i \leftarrow i + 1$, di chuyển con trỏ $j$ sang trái một bước, tức là $j \leftarrow j - 1$, và cập nhật các giá trị của $a$ và $b$, tức là $a \leftarrow nums[i]$ và $b \leftarrow nums[j]$.

Lặp lại quy trình trên cho đến khi $i \ge j$, rồi trả về số thao tác $ans$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumOperations(self, nums: List[int]) -> int:
        i, j = 0, len(nums) - 1
        a, b = nums[i], nums[j]
        ans = 0
        while i < j:
            if a < b:
                i += 1
                a += nums[i]
                ans += 1
            elif b < a:
                j -= 1
                b += nums[j]
                ans += 1
            else:
                i, j = i + 1, j - 1
                a, b = nums[i], nums[j]
        return ans
```

#### Java

```java
class Solution {
    public int minimumOperations(int[] nums) {
        int i = 0, j = nums.length - 1;
        long a = nums[i], b = nums[j];
        int ans = 0;
        while (i < j) {
            if (a < b) {
                a += nums[++i];
                ++ans;
            } else if (b < a) {
                b += nums[--j];
                ++ans;
            } else {
                a = nums[++i];
                b = nums[--j];
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
    int minimumOperations(vector<int>& nums) {
        int i = 0, j = nums.size() - 1;
        long a = nums[i], b = nums[j];
        int ans = 0;
        while (i < j) {
            if (a < b) {
                a += nums[++i];
                ++ans;
            } else if (b < a) {
                b += nums[--j];
                ++ans;
            } else {
                a = nums[++i];
                b = nums[--j];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumOperations(nums []int) int {
	i, j := 0, len(nums)-1
	a, b := nums[i], nums[j]
	ans := 0
	for i < j {
		if a < b {
			i++
			a += nums[i]
			ans++
		} else if b < a {
			j--
			b += nums[j]
			ans++
		} else {
			i, j = i+1, j-1
			a, b = nums[i], nums[j]
		}
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
