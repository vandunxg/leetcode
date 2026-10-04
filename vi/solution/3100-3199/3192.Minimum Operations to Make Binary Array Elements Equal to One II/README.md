---
comments: true
difficulty: Medium
rating: 1432
source: Biweekly Contest 133 Q3
tags:
    - Greedy
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [3192. Minimum Operations to Make Binary Array Elements Equal to One II](https://leetcode.com/problems/minimum-operations-to-make-binary-array-elements-equal-to-one-ii)

[Tài liệu tiếng Trung](/solution/3100-3199/3192.Minimum%20Operations%20to%20Make%20Binary%20Array%20Elements%20Equal%20to%20One%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một <span data-keyword="binary-array">mảng nhị phân</span> <code>nums</code>.</p>

<p>Bạn có thể thực hiện thao tác sau trên mảng <strong>bất kỳ</strong> số lần nào (có thể bằng 0):</p>

<ul>
    <li>Chọn <strong>bất kỳ</strong> chỉ số <code>i</code> nào trong mảng và <strong>đảo</strong> <strong>tất cả</strong> phần tử từ chỉ số <code>i</code> đến cuối mảng.</li>
</ul>

<p><strong>Đảo</strong> một phần tử nghĩa là thay đổi giá trị của nó từ 0 thành 1 và từ 1 thành 0.</p>

<p>Trả về số thao tác <strong>nhỏ nhất</strong> cần thiết để đưa tất cả phần tử trong <code>nums</code> về 1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,1,1,0,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể thực hiện các thao tác sau:</p>

<ul>
    <li>Chọn chỉ số <code>i = 1</code><span class="example-io">. Mảng kết quả là <code>nums = [0,<u><strong>0</strong></u>,<u><strong>0</strong></u>,<u><strong>1</strong></u>,<u><strong>0</strong></u>]</code>.</span></li>
    <li>Chọn chỉ số <code>i = 0</code><span class="example-io">. Mảng kết quả là <code>nums = [<u><strong>1</strong></u>,<u><strong>1</strong></u>,<u><strong>1</strong></u>,<u><strong>0</strong></u>,<u><strong>1</strong></u>]</code>.</span></li>
    <li>Chọn chỉ số <code>i = 4</code><span class="example-io">. Mảng kết quả là <code>nums = [1,1,1,0,<u><strong>0</strong></u>]</code>.</span></li>
    <li>Chọn chỉ số <code>i = 3</code><span class="example-io">. Mảng kết quả là <code>nums = [1,1,1,<u><strong>1</strong></u>,<u><strong>1</strong></u>]</code>.</span></li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,0,0,0]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong><br />
Ta có thể thực hiện thao tác sau:</p>

<ul>
    <li>Chọn chỉ số <code>i = 1</code><span class="example-io">. Mảng kết quả là <code>nums = [1,<u><strong>1</strong></u>,<u><strong>1</strong></u>,<u><strong>1</strong></u>]</code>.</span></li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= nums[i] &lt;= 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bit Manipulation

<!-- thinking:start -->

> **Tư duy**
>
> Một thao tác đảo suffix bắt đầu từ $i$. Ta có thể lưu số lần đảo của các phần tử phía sau theo tính chẵn lẻ trong một bit thay vì cập nhật lại mảng.
>
> Giá trị thực là $x\oplus v$. Nếu kết quả là $0$, ta cần thực hiện thêm một lần đảo suffix và đổi trạng thái của $v$.
>
> Duyệt một lượt sẽ cộng dồn số lần đổi trạng thái. Mỗi chỉ số chỉ được kiểm tra một lần.

<!-- thinking:end -->

Ta nhận thấy rằng mỗi khi đưa một phần tử tại một vị trí nào đó về 1, tất cả phần tử bên phải nó đều bị đảo. Vì vậy, ta có thể dùng một biến $v$ để ghi nhận liệu vị trí hiện tại và tất cả phần tử bên phải nó đã bị đảo hay chưa. Nếu đã bị đảo, giá trị của $v$ là 1, ngược lại là 0.

Ta duyệt qua mảng $\textit{nums}$. Với mỗi phần tử $x$, ta thực hiện phép XOR giữa $x$ và $v$. Nếu $x$ bằng 0, ta cần đổi $x$ thành 1, việc này yêu cầu một thao tác đảo. Ta tăng đáp án lên 1 và đảo giá trị của $v$.

Sau khi duyệt xong, ta thu được số thao tác nhỏ nhất.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, nums: List[int]) -> int:
        ans = v = 0
        for x in nums:
            x ^= v
            if x == 0:
                ans += 1
                v ^= 1
        return ans
```

#### Java

```java
class Solution {
    public int minOperations(int[] nums) {
        int ans = 0, v = 0;
        for (int x : nums) {
            x ^= v;
            if (x == 0) {
                v ^= 1;
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
    int minOperations(vector<int>& nums) {
        int ans = 0, v = 0;
        for (int x : nums) {
            x ^= v;
            if (x == 0) {
                v ^= 1;
                ++ans;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minOperations(nums []int) (ans int) {
    v := 0
    for _, x := range nums {
        x ^= v
        if x == 0 {
            v ^= 1
            ans++
        }
    }
    return
}
```

#### TypeScript

```ts
function minOperations(nums: number[]): number {
    let [ans, v] = [0, 0];
    for (let x of nums) {
        x ^= v;
        if (x === 0) {
            v ^= 1;
            ++ans;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
