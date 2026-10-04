---
comments: true
difficulty: Easy
rating: 1342
source: Biweekly Contest 162 Q1
tags:
    - Greedy
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3633. Earliest Finish Time for Land and Water Rides I](https://leetcode.com/problems/earliest-finish-time-for-land-and-water-rides-i)

[中文文档](/solution/3600-3699/3633.Earliest%20Finish%20Time%20for%20Land%20and%20Water%20Rides%20I/README.md)

## Mô tả

<!-- description:start -->

<p data-end="143" data-start="53">Bạn được cho hai loại trò chơi trong công viên giải trí: <strong data-end="122" data-start="108">trò chơi trên cạn</strong> và <strong data-end="142" data-start="127">trò chơi dưới nước</strong>.</p>

<ul>
    <li data-end="163" data-start="147"><strong data-end="161" data-start="147">Trò chơi trên cạn</strong>

    <ul>
        <li data-end="245" data-start="168"><code data-end="186" data-start="168">landStartTime[i]</code> &ndash; thời điểm sớm nhất mà trò chơi trên cạn thứ <code>i<sup>th</sup></code> có thể bắt đầu.</li>
        <li data-end="306" data-start="250"><code data-end="267" data-start="250">landDuration[i]</code> &ndash; thời lượng của trò chơi trên cạn thứ <code>i<sup>th</sup></code>.</li>
    </ul>
    </li>
    <li><strong data-end="325" data-start="310">Trò chơi dưới nước</strong>
    <ul>
        <li><code data-end="351" data-start="332">waterStartTime[j]</code> &ndash; thời điểm sớm nhất mà trò chơi dưới nước thứ <code>j<sup>th</sup></code> có thể bắt đầu.</li>
        <li><code data-end="434" data-start="416">waterDuration[j]</code> &ndash; thời lượng của trò chơi dưới nước thứ <code>j<sup>th</sup></code>.</li>
    </ul>
    </li>

</ul>

<p data-end="569" data-start="476">Một khách du lịch phải trải nghiệm <strong data-end="517" data-start="502">chính xác một</strong> trò chơi thuộc <strong data-end="536" data-start="528">mỗi</strong> loại, theo <strong data-end="566" data-start="550">thứ tự bất kỳ</strong>.</p>

<ul>
    <li data-end="641" data-start="573">Một trò chơi có thể bắt đầu vào thời điểm mở cửa hoặc <strong data-end="638" data-start="618">bất kỳ thời điểm nào sau đó</strong>.</li>
    <li data-end="715" data-start="644">Nếu một trò chơi bắt đầu tại thời điểm <code data-end="676" data-start="673">t</code>, nó kết thúc tại thời điểm <code data-end="712" data-start="698">t + duration</code>.</li>
    <li data-end="834" data-start="718">Ngay sau khi kết thúc một trò chơi, khách du lịch có thể tham gia trò chơi còn lại (nếu trò chơi đó đã mở cửa) hoặc chờ đến khi trò chơi đó mở cửa.</li>
 </ul>

<p data-end="917" data-start="836">Hãy trả về <strong data-end="873" data-start="847">thời điểm sớm nhất có thể</strong> mà khách du lịch hoàn thành cả hai trò chơi.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">landStartTime = [2,8], landDuration = [4,1], waterStartTime = [6], waterDuration = [3]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
    <li data-end="181" data-start="145">Kế hoạch A (trò chơi trên cạn 0 &rarr; trò chơi dưới nước 0):
    <ul>
        <li data-end="272" data-start="186">Bắt đầu trò chơi trên cạn 0 tại thời điểm <code data-end="234" data-start="212">landStartTime[0] = 2</code>. Kết thúc tại <code data-end="271" data-start="246">2 + landDuration[0] = 6</code>.</li>
        <li data-end="392" data-start="277">Trò chơi dưới nước 0 mở cửa tại thời điểm <code data-end="327" data-start="304">waterStartTime[0] = 6</code>. Bắt đầu ngay tại thời điểm <code data-end="353" data-start="350">6</code>, kết thúc tại <code data-end="391" data-start="365">6 + waterDuration[0] = 9</code>.</li>
    </ul>
    </li>
    <li data-end="432" data-start="396">Kế hoạch B (trò chơi dưới nước 0 &rarr; trò chơi trên cạn 1):
    <ul>
        <li data-end="526" data-start="437">Bắt đầu trò chơi dưới nước 0 tại thời điểm <code data-end="487" data-start="464">waterStartTime[0] = 6</code>. Kết thúc tại <code data-end="525" data-start="499">6 + waterDuration[0] = 9</code>.</li>
        <li data-end="632" data-start="531">Trò chơi trên cạn 1 mở cửa tại <code data-end="574" data-start="552">landStartTime[1] = 8</code>. Bắt đầu tại thời điểm <code data-end="593" data-start="590">9</code>, kết thúc tại <code data-end="631" data-start="605">9 + landDuration[1] = 10</code>.</li>
    </ul>
    </li>
    <li data-end="672" data-start="636">Kế hoạch C (trò chơi trên cạn 1 &rarr; trò chơi dưới nước 0):
    <ul>
        <li data-end="763" data-start="677">Bắt đầu trò chơi trên cạn 1 tại thời điểm <code data-end="725" data-start="703">landStartTime[1] = 8</code>. Kết thúc tại <code data-end="762" data-start="737">8 + landDuration[1] = 9</code>.</li>
        <li data-end="873" data-start="768">Trò chơi dưới nước 0 đã mở cửa tại <code data-end="814" data-start="791">waterStartTime[0] = 6</code>. Bắt đầu tại thời điểm <code data-end="833" data-start="830">9</code>, kết thúc tại <code data-end="872" data-start="845">9 + waterDuration[0] = 12</code>.</li>
    </ul>
    </li>
    <li data-end="913" data-start="877">Kế hoạch D (trò chơi dưới nước 0 &rarr; trò chơi trên cạn 0):
    <ul>
        <li data-end="1007" data-start="918">Bắt đầu trò chơi dưới nước 0 tại thời điểm <code data-end="968" data-start="945">waterStartTime[0] = 6</code>. Kết thúc tại <code data-end="1006" data-start="980">6 + waterDuration[0] = 9</code>.</li>
        <li data-end="1114" data-start="1012">Trò chơi trên cạn 0 đã mở cửa tại <code data-end="1056" data-start="1034">landStartTime[0] = 2</code>. Bắt đầu tại thời điểm <code data-end="1075" data-start="1072">9</code>, kết thúc tại <code data-end="1113" data-start="1087">9 + landDuration[0] = 13</code>.</li>
    </ul>
    </li>
</ul>

<p data-end="1161" data-is-last-node="" data-is-only-node="" data-start="1116">Kế hoạch A cho thời điểm hoàn thành sớm nhất là 9.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">landStartTime = [5], landDuration = [3], waterStartTime = [1], waterDuration = [10]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">14</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul data-end="1589" data-start="1086">
    <li data-end="1124" data-start="1088">Kế hoạch A (trò chơi dưới nước 0 &rarr; trò chơi trên cạn 0):
    <ul>
        <li data-end="1219" data-start="1129">Bắt đầu trò chơi dưới nước 0 tại thời điểm <code data-end="1179" data-start="1156">waterStartTime[0] = 1</code>. Kết thúc tại <code data-end="1218" data-start="1191">1 + waterDuration[0] = 11</code>.</li>
        <li data-end="1338" data-start="1224">Trò chơi trên cạn 0 đã mở cửa tại <code data-end="1268" data-start="1246">landStartTime[0] = 5</code>. Bắt đầu ngay tại thời điểm <code data-end="1295" data-start="1291">11</code> và kết thúc tại <code data-end="1337" data-start="1310">11 + landDuration[0] = 14</code>.</li>
    </ul>
    </li>
    <li data-end="1378" data-start="1342">Kế hoạch B (trò chơi trên cạn 0 &rarr; trò chơi dưới nước 0):
    <ul>
        <li data-end="1469" data-start="1383">Bắt đầu trò chơi trên cạn 0 tại thời điểm <code data-end="1431" data-start="1409">landStartTime[0] = 5</code>. Kết thúc tại <code data-end="1468" data-start="1443">5 + landDuration[0] = 8</code>.</li>
        <li data-end="1589" data-start="1474">Trò chơi dưới nước 0 đã mở cửa tại <code data-end="1520" data-start="1497">waterStartTime[0] = 1</code>. Bắt đầu ngay tại thời điểm <code data-end="1546" data-start="1543">8</code> và kết thúc tại <code data-end="1588" data-start="1561">8 + waterDuration[0] = 18</code>.</li>
    </ul>
    </li>
</ul>

<p data-end="1640" data-is-last-node="" data-is-only-node="" data-start="1591">Kế hoạch A cho thời điểm hoàn thành sớm nhất là 14.<strong>​​​​​​​</strong></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li data-end="38" data-start="16"><code data-end="36" data-start="16">1 &lt;= n, m &lt;= 100</code></li>
    <li data-end="93" data-start="41"><code data-end="91" data-start="41">landStartTime.length == landDuration.length == n</code></li>
    <li data-end="150" data-start="96"><code data-end="148" data-start="96">waterStartTime.length == waterDuration.length == m</code></li>
    <li data-end="237" data-start="153"><code data-end="235" data-start="153">1 &lt;= landStartTime[i], landDuration[i], waterStartTime[j], waterDuration[j] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê + Tham lam

<!-- thinking:start -->

> **Tư duy**
>
> Cần chọn một trò chơi trên cạn và một trò chơi dưới nước, theo thứ tự bất kỳ. Liệt kê từng cặp có độ phức tạp bậc hai. Ở loại trò chơi thứ nhất, chỉ cần chọn trò chơi kết thúc sớm nhất.
>
> Thời điểm kết thúc sớm nhất đó là $\textit{minEnd}=\min(s+d)$. Khi đó, một trò chơi thuộc loại thứ hai kết thúc tại $\max(s,\textit{minEnd})+d$; ta lấy giá trị nhỏ nhất trong số các trò chơi đó.
>
> Tính cho cả hai thứ tự trên cạn trước rồi đến dưới nước và dưới nước trước rồi đến trên cạn, sau đó giữ lại giá trị nhỏ hơn. Mỗi phía chỉ cần duyệt tuyến tính.

<!-- thinking:end -->

Ta có thể xét hai thứ tự của các trò chơi: chơi các trò trên cạn trước rồi đến các trò dưới nước, hoặc chơi các trò dưới nước trước rồi đến các trò trên cạn.

Với mỗi thứ tự, trước tiên ta tính thời điểm kết thúc sớm nhất $\textit{minEnd}$ của loại trò chơi thứ nhất, sau đó liệt kê các trò chơi thuộc loại thứ hai và tính thời điểm kết thúc sớm nhất của trò chơi thứ hai là $\max(\textit{minEnd}, \textit{startTime}) + \textit{duration}$, trong đó $\textit{startTime}$ là thời điểm bắt đầu của trò chơi thuộc loại thứ hai. Đáp án là giá trị nhỏ nhất trong tất cả các thời điểm kết thúc sớm nhất có thể.

Cuối cùng, ta trả về giá trị nhỏ hơn giữa đáp án của hai thứ tự.

Độ phức tạp thời gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số trò chơi trên cạn và số trò chơi dưới nước. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def earliestFinishTime(self, landStartTime: List[int], landDuration: List[int], waterStartTime: List[int], waterDuration: List[int]) -> int:
        def calc(a1, t1, a2, t2):
            min_end = min(a + t for a, t in zip(a1, t1))
            return min(max(a, min_end) + t for a, t in zip(a2, t2))

        x = calc(landStartTime, landDuration, waterStartTime, waterDuration)
        y = calc(waterStartTime, waterDuration, landStartTime, landDuration)
        return min(x, y)
```

#### Java

```java
class Solution {
    public int earliestFinishTime(
        int[] landStartTime, int[] landDuration, int[] waterStartTime, int[] waterDuration) {
        int x = calc(landStartTime, landDuration, waterStartTime, waterDuration);
        int y = calc(waterStartTime, waterDuration, landStartTime, landDuration);
        return Math.min(x, y);
    }

    private int calc(int[] a1, int[] t1, int[] a2, int[] t2) {
        int minEnd = Integer.MAX_VALUE;
        for (int i = 0; i < a1.length; ++i) {
            minEnd = Math.min(minEnd, a1[i] + t1[i]);
        }
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < a2.length; ++i) {
            ans = Math.min(ans, Math.max(minEnd, a2[i]) + t2[i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int earliestFinishTime(vector<int>& landStartTime, vector<int>& landDuration, vector<int>& waterStartTime, vector<int>& waterDuration) {
        int x = calc(landStartTime, landDuration, waterStartTime, waterDuration);
        int y = calc(waterStartTime, waterDuration, landStartTime, landDuration);
        return min(x, y);
    }

    int calc(vector<int>& a1, vector<int>& t1, vector<int>& a2, vector<int>& t2) {
        int minEnd = INT_MAX;
        for (int i = 0; i < a1.size(); ++i) {
            minEnd = min(minEnd, a1[i] + t1[i]);
        }
        int ans = INT_MAX;
        for (int i = 0; i < a2.size(); ++i) {
            ans = min(ans, max(minEnd, a2[i]) + t2[i]);
        }
        return ans;
    }
};
```

#### Go

```go
func earliestFinishTime(landStartTime []int, landDuration []int, waterStartTime []int, waterDuration []int) int {
    x := calc(landStartTime, landDuration, waterStartTime, waterDuration)
    y := calc(waterStartTime, waterDuration, landStartTime, landDuration)
    return min(x, y)
}

func calc(a1 []int, t1 []int, a2 []int, t2 []int) int {
    minEnd := math.MaxInt32
    for i := 0; i < len(a1); i++ {
        minEnd = min(minEnd, a1[i]+t1[i])
    }
    ans := math.MaxInt32
    for i := 0; i < len(a2); i++ {
        ans = min(ans, max(minEnd, a2[i])+t2[i])
    }
    return ans
}
```

#### TypeScript

```ts
function earliestFinishTime(
    landStartTime: number[],
    landDuration: number[],
    waterStartTime: number[],
    waterDuration: number[],
): number {
    const x = calc(landStartTime, landDuration, waterStartTime, waterDuration);
    const y = calc(waterStartTime, waterDuration, landStartTime, landDuration);
    return Math.min(x, y);
}

function calc(a1: number[], t1: number[], a2: number[], t2: number[]): number {
    let minEnd = Number.MAX_SAFE_INTEGER;
    for (let i = 0; i < a1.length; i++) {
        minEnd = Math.min(minEnd, a1[i] + t1[i]);
    }
    let ans = Number.MAX_SAFE_INTEGER;
    for (let i = 0; i < a2.length; i++) {
        ans = Math.min(ans, Math.max(minEnd, a2[i]) + t2[i]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
