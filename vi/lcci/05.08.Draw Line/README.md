---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [05.08. Draw Line](https://leetcode.cn/problems/draw-line-lcci)

[中文文档](/lcci/05.08.Draw%20Line/README.md)

## Mô tả

<!-- description:start -->

<p>Một màn hình đơn sắc được lưu dưới dạng một mảng int duy nhất, cho phép lưu 32 pixel liên tiếp trong một int. Màn hình có độ rộng <code>w</code>, trong đó <code>w</code> chia hết cho 32&nbsp;(nghĩa là không có byte nào bị chia giữa các hàng). Chiều cao của màn hình có thể được suy ra từ độ dài của mảng và độ rộng. Hãy triển khai một hàm vẽ một đường ngang từ <code>(x1, y)</code> đến <code>(x2, y)</code>.</p>
<p>Cho độ dài của mảng, độ rộng của mảng (tính theo bit), vị trí bắt đầu <code>x1</code>&nbsp;(tính theo bit) của đường vẽ, vị trí kết thúc <code>x2</code> (tính theo bit) của đường vẽ và số hàng&nbsp;<code>y</code> của đường vẽ, hãy trả về mảng sau khi vẽ.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong> Đầu vào</strong>: length = 1, w = 32, x1 = 30, x2 = 31, y = 0

<strong> Đầu ra</strong>: [3]

<strong> Giải thích</strong>: Sau khi vẽ một đường từ (30, 0) đến (31, 0), màn hình trở thành [0b000000000000000000000000000000011].

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong> Đầu vào</strong>: length = 3, w = 96, x1 = 0, x2 = 95, y = 0

<strong> Đầu ra</strong>: [-1, -1, -1]

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Hàng $y$ cần đặt các bit $[x_1,x_2]$ trong màn hình được đóng gói thành các từ $32$ bit. Ghi từng pixel là đúng nhưng xử lý ở các ranh giới của từ khá rắc rối.
>
> Các từ được phủ hoàn toàn sẽ trở thành toàn bit 1 ($-1$); chỉ từ đầu tiên và từ cuối cùng cần mask.
>
> Các chỉ số $i,j$ được tính từ $y\cdot w+x$. Điền $[i,j]$ bằng $-1$, sau đó xóa các bit trước $x_1\bmod 32$ và sau $x_2\bmod 32$.

<!-- thinking:end -->

Trước tiên, ta tính vị trí của $x_1$ và $x_2$ trong mảng kết quả, lần lượt ký hiệu là $i$ và $j$. Sau đó, ta đặt các phần tử từ $i$ đến $j$ thành $-1$.

Nếu $x_1 \bmod 32 \neq 0$, ta cần đặt $x_1 \bmod 32$ bit đầu tiên của phần tử tại vị trí $i$ thành $0$.

Nếu $x_2 \bmod 32 \neq 31$, ta cần đặt $31 - x_2 \bmod 32$ bit cuối cùng của phần tử tại vị trí $j$ thành $0$.

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def drawLine(self, length: int, w: int, x1: int, x2: int, y: int) -> List[int]:
        ans = [0] * length
        i = (y * w + x1) // 32
        j = (y * w + x2) // 32
        for k in range(i, j + 1):
            ans[k] = -1
        ans[i] = (ans[i] & 0xFFFFFFFF) >> (x1 % 32) if x1 % 32 else -1
        ans[j] &= -0x80000000 >> (x2 % 32)
        return ans
```

#### Java

```java
class Solution {
    public int[] drawLine(int length, int w, int x1, int x2, int y) {
        int[] ans = new int[length];
        int i = (y * w + x1) / 32;
        int j = (y * w + x2) / 32;
        for (int k = i; k <= j; ++k) {
            ans[k] = -1;
        }
        ans[i] = ans[i] >>> (x1 % 32);
        ans[j] &= 0x80000000 >> (x2 % 32);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> drawLine(int length, int w, int x1, int x2, int y) {
        vector<int> ans(length);
        int i = (y * w + x1) / 32;
        int j = (y * w + x2) / 32;
        for (int k = i; k <= j; ++k) {
            ans[k] = -1;
        }
        ans[i] = ans[i] & unsigned(-1) >> (x1 % 32);
        ans[j] = ans[j] & unsigned(-1) << (31 - x2 % 32);
        return ans;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
