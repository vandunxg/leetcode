---
comments: true
difficulty: Hard
tags:
    - Bit Manipulation
    - Brainteaser
    - Array
    - Math
    - Game Theory
    - Zero-Sum Game
    - Impartial Game
---

<!-- problem:start -->

# [810. Chalkboard XOR Game](https://leetcode.com/problems/chalkboard-xor-game)

[中文文档](/solution/0800-0899/0810.Chalkboard%20XOR%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>nums</code> biểu diễn các số được viết trên bảng.</p>

<p>Alice và Bob lần lượt xóa đúng một số trên bảng, Alice đi trước. Nếu việc xóa một số khiến XOR bitwise của tất cả phần tử trên bảng trở thành <code>0</code>, người chơi vừa xóa sẽ thua. XOR bitwise của một phần tử bằng chính phần tử đó; XOR bitwise của tập rỗng bằng <code>0</code>.</p>

<p>Ngoài ra, nếu người chơi bắt đầu lượt của mình khi XOR bitwise của tất cả phần tử trên bảng bằng <code>0</code>, người đó sẽ thắng.</p>

<p>Trả về <code>true</code> <em>khi và chỉ khi Alice thắng, giả sử cả hai người chơi đều chơi tối ưu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,1,2]
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> 
Alice có hai lựa chọn: xóa số 1 hoặc số 2. 
Nếu cô ấy xóa số 1, mảng nums trở thành [1, 2]. XOR bitwise của tất cả phần tử trên bảng là 1 XOR 2 = 3. Khi đó Bob có thể xóa phần tử nào cũng được, vì Alice sẽ phải xóa phần tử cuối cùng và thua. 
Nếu Alice xóa số 2 trước, nums trở thành [1, 1]. XOR bitwise của tất cả phần tử trên bảng là 1 XOR 1 = 0. Alice sẽ thua.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [0,1]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,2,3]
<strong>Đầu ra:</strong> true
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>0 &lt;= nums[i] &lt; 2<sup>16</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Người chơi thua nếu sau nước đi của đối thủ, XOR của các số còn lại đã bằng $0$. Tìm kiếm toàn bộ game tree quá tốn kém với $n\le 1000$. Nếu XOR hiện tại bằng $0$, người bắt đầu thắng ngay; nếu không, khi $n$ chẵn luôn có nước đi để lại trạng thái có độ dài chẵn và XOR khác $0$, nên người bắt đầu vẫn thắng.
>
> Vì vậy, đáp án là “độ dài chẵn hoặc XOR tổng bằng $0$”, có thể tính bằng một lượt duyệt XOR.

<!-- thinking:end -->

Theo luật chơi, nếu XOR của tất cả các số trên bảng bằng $0$ khi đến lượt một người chơi, người đó thắng. Vì Alice đi trước, nếu XOR của tất cả số trong $\textit{nums}$ bằng $0$, Alice sẽ thắng.

Khi XOR của tất cả số trong $\textit{nums}$ khác $0$, hãy xét khả năng thắng của Alice dựa trên tính chẵn lẻ của độ dài mảng $\textit{nums}$.

Khi độ dài $\textit{nums}$ chẵn, nếu Alice chắc chắn thua thì chỉ có thể xảy ra một trường hợp: dù Alice xóa số nào, XOR của các số còn lại cũng bằng $0$. Hãy xét xem trường hợp này có thể xảy ra hay không.

Giả sử mảng $\textit{nums}$ có độ dài $n$ chẵn. Gọi XOR của tất cả các số là $S$, ta có:

$$
S = \textit{nums}[0] \oplus \textit{nums}[1] \oplus \cdots \oplus \textit{nums}[n-1] \neq 0
$$

Gọi $S_i$ là XOR sau khi xóa số thứ $i$ khỏi mảng $\textit{nums}$, khi đó:

$$
S_i \oplus \textit{nums}[i] = S
$$

Lấy XOR hai vế với $\textit{nums}[i]$, ta được:

$$
S_i = S \oplus \textit{nums}[i]
$$

Nếu dù Alice xóa số nào thì XOR của các số còn lại cũng bằng $0$, thì với mọi $i$, ta có $S_i = 0$, tức là:

$$
S_0 \oplus S_1 \oplus \cdots \oplus S_{n-1} = 0
$$

Thay $S_i = S \oplus \textit{nums}[i]$ vào phương trình trên, ta được:

$$
S \oplus \textit{nums}[0] \oplus S \oplus \textit{nums}[1] \oplus \cdots \oplus S \oplus \textit{nums}[n-1] = 0
$$

Trong phương trình trên có $n$ số hạng $S$ (với $n$ chẵn), còn $\textit{nums}[0] \oplus \textit{nums}[1] \oplus \cdots \oplus \textit{nums}[n-1]$ cũng bằng $S$, nên phương trình tương đương với $0 \oplus S = 0$. Điều này mâu thuẫn với $S \neq 0$, do đó trường hợp này không thể xảy ra. Vì vậy, khi độ dài $\textit{nums}$ chẵn, Alice chắc chắn thắng.

Nếu độ dài mảng lẻ, sau khi Alice xóa một số, số phần tử còn lại sẽ chẵn. Khi đó Bob ở vào thế có độ dài chẵn và chắc chắn thắng, nên Alice chắc chắn thua.

Tóm lại, Alice thắng khi độ dài $\textit{nums}$ chẵn hoặc XOR của tất cả số trong $\textit{nums}$ bằng $0$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def xorGame(self, nums: List[int]) -> bool:
        return len(nums) % 2 == 0 or reduce(xor, nums) == 0
```

#### Java

```java
class Solution {
    public boolean xorGame(int[] nums) {
        return nums.length % 2 == 0 || Arrays.stream(nums).reduce(0, (a, b) -> a ^ b) == 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool xorGame(vector<int>& nums) {
        if (nums.size() % 2 == 0) return true;
        int x = 0;
        for (int& v : nums) x ^= v;
        return x == 0;
    }
};
```

#### Go

```go
func xorGame(nums []int) bool {
	if len(nums)%2 == 0 {
		return true
	}
	x := 0
	for _, v := range nums {
		x ^= v
	}
	return x == 0
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
