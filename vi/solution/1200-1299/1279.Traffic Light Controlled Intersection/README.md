---
comments: true
difficulty: Easy
tags:
    - Concurrency
---

<!-- problem:start -->

# [1279. Traffic Light Controlled Intersection 🔒](https://leetcode.com/problems/traffic-light-controlled-intersection)

[中文文档](/solution/1200-1299/1279.Traffic%20Light%20Controlled%20Intersection/README.md)

## Mô tả

<!-- description:start -->

<p>Có một giao lộ giữa hai con đường. Đường thứ nhất là đường A, xe đi từ Bắc xuống Nam theo hướng 1 và từ Nam lên Bắc theo hướng 2. Đường thứ hai là đường B, xe đi từ Tây sang Đông theo hướng 3 và từ Đông sang Tây theo hướng 4.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1279.Traffic%20Light%20Controlled%20Intersection/images/exp.png" style="width: 600px; height: 417px;" /></p>

<p>Mỗi con đường có một đèn giao thông trước giao lộ. Đèn có thể xanh hoặc đỏ.</p>

<ol>
	<li><strong>Đèn xanh</strong> nghĩa là xe có thể đi qua giao lộ theo cả hai hướng của con đường.</li>
	<li><strong>Đèn đỏ</strong> nghĩa là xe ở cả hai hướng không được đi qua giao lộ và phải chờ đèn chuyển xanh.</li>
</ol>

<p>Đèn trên hai con đường không thể cùng xanh một lúc. Khi đèn đường A xanh thì đèn đường B đỏ, và ngược lại.</p>

<p>Ban đầu, đèn đường A <strong>xanh</strong> và đèn đường B <strong>đỏ</strong>. Khi đèn một đường xanh, xe ở cả hai hướng của đường đó có thể đi qua giao lộ cho đến khi đèn đường kia chuyển xanh. Không được để hai xe đi trên hai đường khác nhau băng qua giao lộ cùng lúc.</p>

<p>Hãy thiết kế hệ thống điều khiển đèn giao thông tại giao lộ này sao cho không xảy ra deadlock.</p>

<p>Hãy triển khai hàm <code>void carArrived(carId, roadId, direction, turnGreen, crossCar)</code>, trong đó:</p>

<ul>
	<li><code>carId</code> là ID của xe vừa đến.</li>
	<li><code>roadId</code> là ID của con đường xe đang đi.</li>
	<li><code>direction</code> là hướng di chuyển của xe.</li>
	<li><code>turnGreen</code> là hàm có thể gọi để bật đèn xanh cho con đường hiện tại.</li>
	<li><code>crossCar</code> là hàm có thể gọi để cho xe hiện tại đi qua giao lộ.</li>
</ul>

<p>Đáp án được xem là đúng nếu tránh được deadlock giữa các xe tại giao lộ. Bật đèn xanh cho một con đường khi đèn đã xanh sẵn được xem là đáp án sai.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> cars = [1,3,5,2,4], directions = [2,1,2,4,3], arrivalTimes = [10,20,30,40,50]
<strong>Đầu ra:</strong> [
&quot;Car 1 Has Passed Road A In Direction 2&quot;,    // Traffic light on road A is green, car 1 can cross the intersection.
&quot;Car 3 Has Passed Road A In Direction 1&quot;,    // Car 3 crosses the intersection as the light is still green.
&quot;Car 5 Has Passed Road A In Direction 2&quot;,    // Car 5 crosses the intersection as the light is still green.
&quot;Traffic Light On Road B Is Green&quot;,          // Car 2 requests green light for road B.
&quot;Car 2 Has Passed Road B In Direction 4&quot;,    // Car 2 crosses as the light is green on road B now.
&quot;Car 4 Has Passed Road B In Direction 3&quot;     // Car 4 crosses the intersection as the light is still green.
]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> cars = [1,2,3,4,5], directions = [2,4,3,3,1], arrivalTimes = [10,20,30,40,40]
<strong>Đầu ra:</strong> [
&quot;Car 1 Has Passed Road A In Direction 2&quot;,    // Traffic light on road A is green, car 1 can cross the intersection.
&quot;Traffic Light On Road B Is Green&quot;,          // Car 2 requests green light for road B.
&quot;Car 2 Has Passed Road B In Direction 4&quot;,    // Car 2 crosses as the light is green on road B now.
&quot;Car 3 Has Passed Road B In Direction 3&quot;,    // Car 3 crosses as the light is green on road B now.
&quot;Traffic Light On Road A Is Green&quot;,          // Car 5 requests green light for road A.
&quot;Car 5 Has Passed Road A In Direction 1&quot;,    // Car 5 crosses as the light is green on road A now.
&quot;Traffic Light On Road B Is Green&quot;,          // Car 4 requests green light for road B. Car 4 blocked until car 5 crosses and then traffic light is green on road B.
&quot;Car 4 Has Passed Road B In Direction 3&quot;     // Car 4 crosses as the light is green on road B now.
]
<strong>Giải thích:</strong> Đây là tình huống không xảy ra deadlock. Lưu ý, tình huống xe 4 đi qua trước khi bật đèn xanh cho đường A và cho xe 5 đi qua cũng là một tình huống <strong>đúng</strong> và được <strong>chấp nhận</strong>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= cars.length &lt;= 20</code></li>
	<li><code>cars.length = directions.length</code></li>
	<li><code>cars.length = arrivalTimes.length</code></li>
	<li>Mọi giá trị trong <code>cars</code> đều duy nhất.</li>
	<li><code>1 &lt;= directions[i] &lt;= 4</code></li>
	<li><code>arrivalTimes</code> có thứ tự không giảm.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Hai đèn không thể cùng xanh; thao tác đổi đèn và cho xe qua cần được thực hiện loại trừ lẫn nhau. Số xe ít nên có thể dùng một lock để tuần tự hóa critical section: nếu xe đến từ đường chưa xanh thì bật đèn xanh rồi cho xe qua. Lock đảm bảo mỗi lần chỉ có một xe đổi đèn hoặc đi qua giao lộ.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
from threading import Lock


class TrafficLight:
    def __init__(self):
        self.lock = Lock()
        self.road = 1

    def carArrived(
        self,
        carId: int,  # ID of the car
        # ID of the road the car travels on. Can be 1 (road A) or 2 (road B)
        roadId: int,
        direction: int,  # Direction of the car
        # Use turnGreen() to turn light to green on current road
        turnGreen: 'Callable[[], None]',
        # Use crossCar() to make car cross the intersection
        crossCar: 'Callable[[], None]',
    ) -> None:
        self.lock.acquire()
        if self.road != roadId:
            self.road = roadId
            turnGreen()
        crossCar()
        self.lock.release()
```

#### Java

```java
class TrafficLight {
    private int road = 1;

    public TrafficLight() {
    }

    public synchronized void carArrived(int carId, // ID of the car
        int roadId, // ID of the road the car travels on. Can be 1 (road A) or 2 (road B)
        int direction, // Direction of the car
        Runnable turnGreen, // Use turnGreen.run() to turn light to green on current road
        Runnable crossCar // Use crossCar.run() to make car cross the intersection
    ) {
        if (roadId != road) {
            turnGreen.run();
            road = roadId;
        }
        crossCar.run();
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
