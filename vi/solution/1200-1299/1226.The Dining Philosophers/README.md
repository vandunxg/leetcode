---
comments: true
difficulty: Medium
tags:
    - Concurrency
---

<!-- problem:start -->

# [1226. The Dining Philosophers](https://leetcode.com/problems/the-dining-philosophers)

[中文文档](/solution/1200-1299/1226.The%20Dining%20Philosophers/README.md)

## Mô tả

<!-- description:start -->

<p>Năm triết gia im lặng ngồi quanh một chiếc bàn tròn, trước mặt là các bát mì spaghetti. Giữa mỗi cặp triết gia ngồi cạnh nhau có một chiếc nĩa.</p>

<p>Mỗi triết gia luân phiên suy nghĩ và ăn. Tuy nhiên, một triết gia chỉ có thể ăn spaghetti khi cầm được cả nĩa bên trái và bên phải. Mỗi chiếc nĩa chỉ một triết gia được cầm, vì vậy triết gia chỉ có thể dùng nĩa nếu không có ai khác đang sử dụng nó. Sau khi ăn xong, triết gia cần đặt cả hai chiếc nĩa xuống để người khác có thể dùng. Khi nĩa bên trái hoặc bên phải trở nên khả dụng, triết gia có thể lấy nĩa đó, nhưng không được bắt đầu ăn trước khi có đủ cả hai chiếc.</p>

<p>Việc ăn không bị giới hạn bởi lượng spaghetti còn lại hay sức chứa của dạ dày; giả sử nguồn cung và nhu cầu đều vô hạn.</p>

<p>Hãy thiết kế một quy tắc hoạt động (thuật toán concurrent) sao cho không triết gia nào bị đói; <i>tức là</i>, mỗi người có thể tiếp tục luân phiên ăn và suy nghĩ mãi mãi, với giả định rằng không triết gia nào biết khi nào những người khác muốn ăn hay suy nghĩ.</p>

<p style="text-align: center"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1226.The%20Dining%20Philosophers/images/an_illustration_of_the_dining_philosophers_problem.png" style="width: 400px; height: 415px;" /></p>

<p style="text-align: center"><em>Đề bài và hình minh họa phía trên được lấy từ <a href="https://en.wikipedia.org/wiki/Dining_philosophers_problem" target="_blank">wikipedia.org</a></em></p>

<p>&nbsp;</p>

<p>ID của các triết gia được đánh số từ <strong>0</strong> đến <strong>4</strong> theo chiều <strong>kim đồng hồ</strong>. Hãy triển khai hàm <code>void wantsToEat(philosopher, pickLeftFork, pickRightFork, eat, putLeftFork, putRightFork)</code> với các tham số sau:</p>

<ul>
	<li><code>philosopher</code> là ID của triết gia muốn ăn.</li>
	<li><code>pickLeftFork</code> và <code>pickRightFork</code> là các hàm dùng để lấy nĩa tương ứng của triết gia đó.</li>
	<li><code>eat</code> là hàm cho phép triết gia ăn sau khi đã lấy được cả hai chiếc nĩa.</li>
	<li><code>putLeftFork</code> và <code>putRightFork</code> là các hàm dùng để đặt xuống những chiếc nĩa tương ứng của triết gia đó.</li>
	<li>Giả sử các triết gia đang suy nghĩ chừng nào họ chưa yêu cầu ăn (hàm chưa được gọi với số ID của họ).</li>
</ul>

<p>Năm thread, mỗi thread đại diện cho một triết gia, sẽ đồng thời dùng chung một object của class bạn viết để mô phỏng quá trình. Hàm có thể được gọi nhiều lần cho cùng một triết gia, kể cả khi lần gọi trước chưa kết thúc.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 1
<strong>Đầu ra:</strong> [[3,2,1],[3,1,1],[3,0,3],[3,1,2],[3,2,2],[4,2,1],[4,1,1],[2,2,1],[2,1,1],[1,2,1],[2,0,3],[2,1,2],[2,2,2],[4,0,3],[4,1,2],[4,2,2],[1,1,1],[1,0,3],[1,1,2],[1,2,2],[0,1,1],[0,2,1],[0,0,3],[0,1,2],[0,2,2]]
<strong>Giải thích:</strong>
n là số lần mỗi triết gia sẽ gọi hàm.
Mảng output mô tả các lần gọi hàm điều khiển nĩa và hàm ăn, có định dạng như sau:
output[i] = [a, b, c] (ba số nguyên)
- a là ID của triết gia.
- b xác định chiếc nĩa: {1 : trái, 2 : phải}.
- c xác định thao tác: {1 : lấy, 2 : đặt xuống, 3 : ăn}.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 60</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Solution 1

<!-- thinking:start -->

> **Tư duy**
>
> Năm triết gia dùng chung năm chiếc nĩa. Các triết gia ngồi cạnh nhau tranh chấp cùng một chiếc nĩa; thứ tự lấy lock không nhất quán có thể gây deadlock. Mỗi bữa ăn chỉ cần hai chiếc nĩa bên trái và bên phải của triết gia đó.
>
> $scoped\_lock$ lấy hai mutex theo thứ tự đã cho và giải phóng chúng theo thứ tự ngược lại khi kết thúc scope. Nhờ đó, critical section trên cặp nĩa được bảo vệ độc quyền mà không chặn các triết gia không ngồi cạnh nhau.
>
> Triết gia $i$ lấy các nĩa $i$ và $(i+1)\bmod 5$; các thao tác lấy nĩa, ăn và đặt nĩa đều chạy trong lock, nhờ đó tránh tạo thành chu trình chờ.

<!-- thinking:end -->

<!-- tabs:start -->

#### C++

```cpp
class DiningPhilosophers {
public:
    using Act = function<void()>;

    void wantsToEat(int philosopher, Act pickLeftFork, Act pickRightFork, Act eat, Act putLeftFork, Act putRightFork) {
        /* 这一题实际上是用到了C++17中的scoped_lock知识。
                   作用是传入scoped_lock(mtx1, mtx2)两个锁，然后在作用范围内，依次顺序上锁mtx1和mtx2；然后在作用范围结束时，再反续解锁mtx2和mtx1。
                   从而保证了philosopher1有动作的时候，philosopher2无法操作；但是philosopher3和philosopher4不受影响 */
        std::scoped_lock lock(mutexes_[philosopher], mutexes_[philosopher >= 4 ? 0 : philosopher + 1]);
        pickLeftFork();
        pickRightFork();
        eat();
        putLeftFork();
        putRightFork();
    }

private:
    vector<mutex> mutexes_ = vector<mutex>(5);
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
