---
comments: true
difficulty: Easy
tags:
    - Array
    - Interactive
---

<!-- problem:start -->

# [2728. Count Houses in a Circular Street 🔒](https://leetcode.com/problems/count-houses-in-a-circular-street)

[中文文档](/solution/2700-2799/2728.Count%20Houses%20in%20a%20Circular%20Street/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một đối tượng <code>street</code> thuộc lớp <code>Street</code>, biểu diễn một con phố hình tròn, và một số nguyên dương <code>k</code> là cận trên của số ngôi nhà trên con phố đó (nói cách khác, số ngôi nhà nhỏ hơn hoặc bằng <code>k</code>). Ban đầu, cửa các ngôi nhà có thể đang mở hoặc đóng.</p>

<p>Ban đầu, bạn đứng trước cửa một ngôi nhà trên con phố này. Nhiệm vụ của bạn là đếm số ngôi nhà trên con phố.</p>

<p>Lớp <code>Street</code> cung cấp các hàm sau để bạn sử dụng:</p>

<ul>
	<li><code>void openDoor()</code>: Mở cửa của ngôi nhà mà bạn đang đứng trước.</li>
	<li><code>void closeDoor()</code>: Đóng cửa của ngôi nhà mà bạn đang đứng trước.</li>
	<li><code>boolean isDoorOpen()</code>: Trả về <code>true</code> nếu cửa của ngôi nhà hiện tại đang mở và <code>false</code> nếu ngược lại.</li>
	<li><code>void moveRight()</code>: Di chuyển sang ngôi nhà bên phải.</li>
	<li><code>void moveLeft()</code>: Di chuyển sang ngôi nhà bên trái.</li>
</ul>

<p>Trả về <code>ans</code> <em>là số ngôi nhà trên con phố này.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> street = [0,0,0,0], k = 10
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có 4 ngôi nhà và cửa của tất cả các ngôi nhà đều đang đóng.
Số ngôi nhà nhỏ hơn k, với k = 10.</pre>

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
	<li><code>1 &lt;= n &lt;= k &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều nhất $k$ ngôi nhà trên một vòng tròn, và ta chỉ có thể mở hoặc đóng cửa rồi di chuyển sang trái hoặc phải. Nếu chỉ đóng tất cả cửa trước rồi đi một vòng để đếm, ta không thể phân biệt cửa bắt đầu với thời điểm đã đi hết một vòng.
>
> Di chuyển sang trái $k$ lần và mở cửa để đảm bảo toàn bộ vòng tròn đều mở. Sau đó tiếp tục đi sang trái: đóng từng cửa đang mở và đếm, dừng lại khi gặp cửa đã đóng sẵn. Số đếm được chính là số ngôi nhà.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# Definition for a street.
# class Street:
#     def openDoor(self):
#         pass
#     def closeDoor(self):
#         pass
#     def isDoorOpen(self):
#         pass
#     def moveRight(self):
#         pass
#     def moveLeft(self):
#         pass
class Solution:
    def houseCount(self, street: Optional["Street"], k: int) -> int:
        for _ in range(k):
            street.openDoor()
            street.moveLeft()
        ans = 0
        while street.isDoorOpen():
            street.closeDoor()
            street.moveLeft()
            ans += 1
        return ans
```

#### Java

```java
/**
 * Definition for a street.
 * class Street {
 *     public Street(int[] doors);
 *     public void openDoor();
 *     public void closeDoor();
 *     public boolean isDoorOpen();
 *     public void moveRight();
 *     public void moveLeft();
 * }
 */
class Solution {
    public int houseCount(Street street, int k) {
        while (k-- > 0) {
            street.openDoor();
            street.moveLeft();
        }
        int ans = 0;
        while (street.isDoorOpen()) {
            ++ans;
            street.closeDoor();
            street.moveLeft();
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
 *     void openDoor();
 *     void closeDoor();
 *     bool isDoorOpen();
 *     void moveRight();
 *     void moveLeft();
 * };
 */
class Solution {
public:
    int houseCount(Street* street, int k) {
        while (k--) {
            street->openDoor();
            street->moveLeft();
        }
        int ans = 0;
        while (street->isDoorOpen()) {
            ans++;
            street->closeDoor();
            street->moveLeft();
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
 *     OpenDoor()
 *     CloseDoor()
 *     IsDoorOpen() bool
 *     MoveRight()
 *     MoveLeft()
 * }
 */
func houseCount(street Street, k int) (ans int) {
	for ; k > 0; k-- {
		street.OpenDoor()
		street.MoveLeft()
	}
	for ; street.IsDoorOpen(); street.MoveLeft() {
		ans++
		street.CloseDoor()
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
 *     public openDoor(): void;
 *     public closeDoor(): void;
 *     public isDoorOpen(): boolean;
 *     public moveRight(): void;
 *     public moveLeft(): void;
 * }
 */
function houseCount(street: Street | null, k: number): number {
    while (k-- > 0) {
        street.openDoor();
        street.moveLeft();
    }
    let ans = 0;
    while (street.isDoorOpen()) {
        ++ans;
        street.closeDoor();
        street.moveLeft();
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
