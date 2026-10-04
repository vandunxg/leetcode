---
comments: true
difficulty: Hard
---

<!-- problem:start -->

# [2753. Count Houses in a Circular Street II 🔒](https://leetcode.com/problems/count-houses-in-a-circular-street-ii)

[中文文档](/solution/2700-2799/2753.Count%20Houses%20in%20a%20Circular%20Street%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một đối tượng <code>street</code> thuộc lớp <code>Street</code>, biểu diễn một con phố <strong>hình tròn</strong>, và một số nguyên dương <code>k</code> là cận trên của số ngôi nhà trên con phố đó (nói cách khác, số ngôi nhà nhỏ hơn hoặc bằng <code>k</code>). Ban đầu, cửa các ngôi nhà có thể đang mở hoặc đóng (ít nhất một cửa đang mở).</p>

<p>Ban đầu, bạn đứng trước cửa một ngôi nhà trên con phố này. Nhiệm vụ của bạn là đếm số ngôi nhà trên con phố.</p>

<p>Lớp <code>Street</code> cung cấp các hàm sau để bạn sử dụng:</p>

<ul>
	<li><code>void closeDoor()</code>: Đóng cửa của ngôi nhà mà bạn đang đứng trước.</li>
	<li><code>boolean isDoorOpen()</code>: Trả về <code>true</code> nếu cửa của ngôi nhà hiện tại đang mở và <code>false</code> nếu ngược lại.</li>
	<li><code>void moveRight()</code>: Di chuyển sang ngôi nhà bên phải.</li>
</ul>

<p><strong>Lưu ý</strong> rằng <strong>hình tròn</strong> nghĩa là nếu đánh số các ngôi nhà từ <code>1</code> đến <code>n</code>, thì ngôi nhà bên phải của <code>house<sub>i</sub></code> là <code>house<sub>i+1</sub></code> với <code>i &lt; n</code>, còn ngôi nhà bên phải của <code>house<sub>n</sub></code> là <code>house<sub>1</sub></code>.</p>

<p>Trả về <code>ans</code> <em>là số ngôi nhà trên con phố này.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> street = [1,1,1,1], k = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 ngôi nhà và cửa của tất cả các ngôi nhà đều đang mở.
Số ngôi nhà nhỏ hơn k = 10.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> street = [1,0,1,1,0], k = 5
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> Có 5 ngôi nhà. Cửa của ngôi nhà thứ 1, thứ 3 và thứ 4 (khi di chuyển sang phải) đang mở, các cửa còn lại đang đóng.
Số ngôi nhà bằng k = 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == number of houses</code></li>
	<li><code>1 &lt;= n &lt;= k &lt;= 10<sup>5</sup></code></li>
	<li>Theo định nghĩa trong đề bài, <code>street</code> là một con phố hình tròn.</li>
	<li>Dữ liệu đầu vào được tạo sao cho ít nhất một cửa đang mở.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Câu đố tư duy

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $k$ ngôi nhà trên một vòng tròn, ít nhất một cửa đang mở, và ta chỉ có thể di chuyển sang phải hoặc đóng cửa. Không thể mở tất cả các cửa như ở bài trước.
>
> Hãy đi đến một cửa đang mở để làm mốc, sau đó di chuyển sang phải nhiều nhất $k$ lần. Đóng mọi cửa đang mở và ghi nhớ số bước đã đi. Cửa cuối cùng vẫn mở cách điểm bắt đầu đúng một vòng, nên số bước đó chính là số ngôi nhà.

<!-- thinking:end -->

Ta nhận thấy rằng đề bài đảm bảo có ít nhất một cửa đang mở. Trước tiên, ta có thể tìm một cửa đang mở.

Sau đó, ta bỏ qua cửa đang mở này và di chuyển sang phải. Mỗi lần di chuyển, ta tăng bộ đếm lên một. Nếu gặp một cửa đang mở, ta đóng cửa đó. Đáp án là giá trị của bộ đếm ở lần cuối cùng ta gặp một cửa đang mở.

Độ phức tạp thời gian là $O(k)$ và độ phức tạp không gian là $O(1)$.

Bài toán liên quan:

- [2728. Count Houses in a Circular Street](https://github.com/doocs/leetcode/blob/main/solution/2700-2799/2728.Count%20Houses%20in%20a%20Circular%20Street/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
# Definition for a street.
# class Street:
#     def closeDoor(self):
#         pass
#     def isDoorOpen(self):
#         pass
#     def moveRight(self):
#         pass
class Solution:
    def houseCount(self, street: Optional["Street"], k: int) -> int:
        while not street.isDoorOpen():
            street.moveRight()
        for i in range(1, k + 1):
            street.moveRight()
            if street.isDoorOpen():
                ans = i
                street.closeDoor()
        return ans
```

#### Java

```java
/**
 * Definition for a street.
 * class Street {
 *     public Street(int[] doors);
 *     public void closeDoor();
 *     public boolean isDoorOpen();
 *     public void moveRight();
 * }
 */
class Solution {
    public int houseCount(Street street, int k) {
        while (!street.isDoorOpen()) {
            street.moveRight();
        }
        int ans = 0;
        for (int i = 1; i <= k; ++i) {
            street.moveRight();
            if (street.isDoorOpen()) {
                ans = i;
                street.closeDoor();
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
/**
 * Definition for a street.
 * class Street {
 * public:
 *     Street(vector<int> doors);
 *     void closeDoor();
 *     bool isDoorOpen();
 *     void moveRight();
 * };
 */
class Solution {
public:
    int houseCount(Street* street, int k) {
        while (!street->isDoorOpen()) {
            street->moveRight();
        }
        int ans = 0;
        for (int i = 1; i <= k; ++i) {
            street->moveRight();
            if (street->isDoorOpen()) {
                ans = i;
                street->closeDoor();
            }
        }
        return ans;
    }
};
```

#### Go

```go
/**
 * Definition for a street.
 * type Street interface {
 *     CloseDoor()
 *     IsDoorOpen() bool
 *     MoveRight()
 * }
 */
func houseCount(street Street, k int) (ans int) {
	for !street.IsDoorOpen() {
		street.MoveRight()
	}
	for i := 1; i <= k; i++ {
		street.MoveRight()
		if street.IsDoorOpen() {
			ans = i
			street.CloseDoor()
		}
	}
	return
}
```

#### TypeScript

```ts
/**
 * Definition for a street.
 * class Street {
 *     constructor(doors: number[]);
 *     public closeDoor(): void;
 *     public isDoorOpen(): boolean;
 *     public moveRight(): void;
 * }
 */
function houseCount(street: Street | null, k: number): number {
    while (!street.isDoorOpen()) {
        street.moveRight();
    }
    let ans = 0;
    for (let i = 1; i <= k; ++i) {
        street.moveRight();
        if (street.isDoorOpen()) {
            ans = i;
            street.closeDoor();
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
