---
comments: true
difficulty: Medium
rating: 2029
source: Weekly Contest 353 Q4
tags:
    - Array
    - Prefix Sum
---

<!-- problem:start -->

# [2772. Apply Operations to Make All Array Elements Equal to Zero](https://leetcode.com/problems/apply-operations-to-make-all-array-elements-equal-to-zero)

[中文文档](/solution/2700-2799/2772.Apply%20Operations%20to%20Make%20All%20Array%20Elements%20Equal%20to%20Zero/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code> được đánh chỉ số từ <strong>0</strong> và một số nguyên dương <code>k</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần nào:</p>

<ul>
	<li>Chọn <strong>bất kỳ</strong> mảng con nào có kích thước <code>k</code> và <strong>giảm</strong> tất cả phần tử của nó đi <code>1</code>.</li>
</ul>

<p>Trả về <code>true</code><em> nếu bạn có thể đưa tất cả phần tử của mảng về </em><code>0</code><em>, hoặc </em><code>false</code><em> nếu không thể</em>.</p>

<p><strong>Mảng con</strong> là một phần liên tiếp khác rỗng của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [2,2,3,1,1,0], k = 3
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ta có thể thực hiện các thao tác sau:
- Chọn mảng con [2,2,3]. Mảng sau thao tác sẽ là nums = [<strong><u>1</u></strong>,<strong><u>1</u></strong>,<strong><u>2</u></strong>,1,1,0].
- Chọn mảng con [2,1,1]. Mảng sau thao tác sẽ là nums = [1,1,<strong><u>1</u></strong>,<strong><u>0</u></strong>,<strong><u>0</u></strong>,0].
- Chọn mảng con [1,1,1]. Mảng sau thao tác sẽ là nums = [<u><strong>0</strong></u>,<u><strong>0</strong></u>,<u><strong>0</strong></u>,0,0,0].
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> nums = [1,3,1,1], k = 2
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không thể đưa tất cả phần tử của mảng về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mảng hiệu + Tổng tiền tố

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác trừ đi một đơn vị trên một cửa sổ có độ dài $k$; ta cần xác định liệu mảng có thể trở thành toàn số 0 hay không. Thực hiện lần lượt từng phép trừ là quá chậm, còn số lần tác động lên mỗi chỉ số được xác định từ trái sang phải.
>
> Chỉ số khác 0 ngoài cùng bên trái $i$ chỉ có thể được đưa về 0 bằng các cửa sổ chứa $i$, với số lần đúng bằng giá trị hiện tại của nó. Mảng hiệu ghi lại phép giảm trên đoạn đó; tổng tiền tố khôi phục giá trị hiện tại. Nếu giá trị âm hoặc cửa sổ vượt quá cuối mảng thì không thể thực hiện.

<!-- thinking:end -->

Trước hết, hãy xét phần tử đầu tiên của $nums$, $nums[0]$:

- Nếu $nums[0] = 0$, ta không cần làm gì.
- Nếu $nums[0] > 0$, ta cần thực hiện thao tác trên $nums[0..k-1]$ tổng cộng $nums[0]$ lần, giảm tất cả phần tử trong $nums[0..k-1]$ đi $nums[0]$, để $nums[0]$ trở thành $0$.

Để thực hiện đồng thời các thao tác cộng và trừ trên một đoạn liên tiếp, ta có thể dùng mảng hiệu để quản lý các thao tác này. Ta biểu diễn mảng hiệu bằng $d[i]$; tính tổng tiền tố của mảng hiệu sẽ cho biết mức thay đổi của giá trị tại mỗi vị trí.

Vì vậy, ta duyệt qua $nums$. Với mỗi phần tử $nums[i]$, mức thay đổi tại vị trí hiện tại là $s = \sum_{j=0}^{i} d[j]$. Cộng $s$ vào $nums[i]$ để nhận được giá trị thực tế của $nums[i]$.

- Nếu $nums[i] = 0$, ta không cần thực hiện thao tác nào và có thể chuyển sang phần tử tiếp theo.
- Nếu $nums[i]=0$ hoặc $i + k > n$, điều đó cho biết sau các thao tác trước đó, $nums[i]$ đã trở thành số âm, hoặc $nums[i..i+k-1]$ vượt ra ngoài giới hạn mảng. Vì vậy, không thể đưa tất cả phần tử trong $nums$ về $0$. Ta trả về `false`. Ngược lại, ta cần giảm tất cả phần tử trong đoạn $[i..i+k-1]$ đi $nums[i]$. Do đó, ta trừ $nums[i]$ khỏi $s$ và cộng $nums[i]$ vào $d[i+k]$.
- Tiếp tục duyệt phần tử kế tiếp.

Nếu quá trình duyệt kết thúc, điều đó có nghĩa là tất cả phần tử trong $nums$ có thể được đưa về $0$, nên ta trả về `true`.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkArray(self, nums: List[int], k: int) -> bool:
        n = len(nums)
        d = [0] * (n + 1)
        s = 0
        for i, x in enumerate(nums):
            s += d[i]
            x += s
            if x == 0:
                continue
            if x < 0 or i + k > n:
                return False
            s -= x
            d[i + k] += x
        return True
```

#### Java

```java
class Solution {
    public boolean checkArray(int[] nums, int k) {
        int n = nums.length;
        int[] d = new int[n + 1];
        int s = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            nums[i] += s;
            if (nums[i] == 0) {
                continue;
            }
            if (nums[i] < 0 || i + k > n) {
                return false;
            }
            s -= nums[i];
            d[i + k] += nums[i];
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkArray(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> d(n + 1);
        int s = 0;
        for (int i = 0; i < n; ++i) {
            s += d[i];
            nums[i] += s;
            if (nums[i] == 0) {
                continue;
            }
            if (nums[i] < 0 || i + k > n) {
                return false;
            }
            s -= nums[i];
            d[i + k] += nums[i];
        }
        return true;
    }
};
```

#### Go

```go
func checkArray(nums []int, k int) bool {
	n := len(nums)
	d := make([]int, n+1)
	s := 0
	for i, x := range nums {
		s += d[i]
		x += s
		if x == 0 {
			continue
		}
		if x < 0 || i+k > n {
			return false
		}
		s -= x
		d[i+k] += x
	}
	return true
}
```

#### TypeScript

```ts
function checkArray(nums: number[], k: number): boolean {
    const n = nums.length;
    const d: number[] = Array(n + 1).fill(0);
    let s = 0;
    for (let i = 0; i < n; ++i) {
        s += d[i];
        nums[i] += s;
        if (nums[i] === 0) {
            continue;
        }
        if (nums[i] < 0 || i + k > n) {
            return false;
        }
        s -= nums[i];
        d[i + k] += nums[i];
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
