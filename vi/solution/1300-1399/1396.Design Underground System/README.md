---
comments: true
difficulty: Medium
rating: 1464
source: Weekly Contest 182 Q3
tags:
    - Design
    - Hash Table
    - String
---

<!-- problem:start -->

# [1396. Design Underground System](https://leetcode.com/problems/design-underground-system)

[中文文档](/solution/1300-1399/1396.Design%20Underground%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Hệ thống tàu điện ngầm đang ghi nhận thời gian di chuyển của hành khách giữa các ga để tính thời gian trung bình đi từ ga này đến ga khác.</p>

<p>Triển khai class <code>UndergroundSystem</code> với các phương thức sau:</p>

<ul>
	<li><code>void checkIn(int id, string stationName, int t)</code>

    <ul>
    	<li>Hành khách có mã thẻ <code>id</code> check in tại ga <code>stationName</code> vào thời điểm <code>t</code>.</li>
    	<li>Mỗi hành khách chỉ có thể check in tại một địa điểm tại một thời điểm.</li>
    </ul>
    </li>
    <li><code>void checkOut(int id, string stationName, int t)</code>
    <ul>
    	<li>Hành khách có mã thẻ <code>id</code> check out tại ga <code>stationName</code> vào thời điểm <code>t</code>.</li>
    </ul>
    </li>
    <li><code>double getAverageTime(string startStation, string endStation)</code>
    <ul>
    	<li>Trả về thời gian trung bình để đi từ ga <code>startStation</code> đến ga <code>endStation</code>.</li>
    	<li>Thời gian trung bình được tính từ tất cả các chuyến đi trước đó diễn ra <strong>trực tiếp</strong> từ <code>startStation</code> đến <code>endStation</code>, tức là hành khách check in tại <code>startStation</code> rồi check out tại <code>endStation</code>.</li>
    	<li>Thời gian đi từ <code>startStation</code> đến <code>endStation</code> <strong>có thể khác</strong> thời gian đi theo chiều ngược lại, từ <code>endStation</code> đến <code>startStation</code>.</li>
    	<li>Đảm bảo đã có ít nhất một hành khách đi từ <code>startStation</code> đến <code>endStation</code> trước khi gọi <code>getAverageTime</code>.</li>
    </ul>
    </li>

</ul>

<p>Có thể giả định mọi lời gọi đến các phương thức <code>checkIn</code> và <code>checkOut</code> đều hợp lệ. Nếu hành khách check in tại thời điểm <code>t<sub>1</sub></code> rồi check out tại thời điểm <code>t<sub>2</sub></code>, thì <code>t<sub>1</sub> &lt; t<sub>2</sub></code>. Tất cả sự kiện xảy ra theo thứ tự thời gian.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;UndergroundSystem&quot;,&quot;checkIn&quot;,&quot;checkIn&quot;,&quot;checkIn&quot;,&quot;checkOut&quot;,&quot;checkOut&quot;,&quot;checkOut&quot;,&quot;getAverageTime&quot;,&quot;getAverageTime&quot;,&quot;checkIn&quot;,&quot;getAverageTime&quot;,&quot;checkOut&quot;,&quot;getAverageTime&quot;]
[[],[45,&quot;Leyton&quot;,3],[32,&quot;Paradise&quot;,8],[27,&quot;Leyton&quot;,10],[45,&quot;Waterloo&quot;,15],[27,&quot;Waterloo&quot;,20],[32,&quot;Cambridge&quot;,22],[&quot;Paradise&quot;,&quot;Cambridge&quot;],[&quot;Leyton&quot;,&quot;Waterloo&quot;],[10,&quot;Leyton&quot;,24],[&quot;Leyton&quot;,&quot;Waterloo&quot;],[10,&quot;Waterloo&quot;,38],[&quot;Leyton&quot;,&quot;Waterloo&quot;]]

<strong>Đầu ra</strong>
[null,null,null,null,null,null,null,14.00000,11.00000,null,11.00000,null,12.00000]

<strong>Giải thích</strong>
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(45, &quot;Leyton&quot;, 3);
undergroundSystem.checkIn(32, &quot;Paradise&quot;, 8);
undergroundSystem.checkIn(27, &quot;Leyton&quot;, 10);
undergroundSystem.checkOut(45, &quot;Waterloo&quot;, 15);  // Hành khách 45 đi từ &quot;Leyton&quot; đến &quot;Waterloo&quot; trong 15-3 = 12
undergroundSystem.checkOut(27, &quot;Waterloo&quot;, 20);  // Hành khách 27 đi từ &quot;Leyton&quot; đến &quot;Waterloo&quot; trong 20-10 = 10
undergroundSystem.checkOut(32, &quot;Cambridge&quot;, 22); // Hành khách 32 đi từ &quot;Paradise&quot; đến &quot;Cambridge&quot; trong 22-8 = 14
undergroundSystem.getAverageTime(&quot;Paradise&quot;, &quot;Cambridge&quot;); // trả về 14.00000. Một chuyến đi &quot;Paradise&quot; -&gt; &quot;Cambridge&quot;, (14) / 1 = 14
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Waterloo&quot;);    // trả về 11.00000. Hai chuyến đi &quot;Leyton&quot; -&gt; &quot;Waterloo&quot;, (10 + 12) / 2 = 11
undergroundSystem.checkIn(10, &quot;Leyton&quot;, 24);
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Waterloo&quot;);    // trả về 11.00000
undergroundSystem.checkOut(10, &quot;Waterloo&quot;, 38);  // Hành khách 10 đi từ &quot;Leyton&quot; đến &quot;Waterloo&quot; trong 38-24 = 14
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Waterloo&quot;);    // trả về 12.00000. Ba chuyến đi &quot;Leyton&quot; -&gt; &quot;Waterloo&quot;, (10 + 12 + 14) / 3 = 12
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;UndergroundSystem&quot;,&quot;checkIn&quot;,&quot;checkOut&quot;,&quot;getAverageTime&quot;,&quot;checkIn&quot;,&quot;checkOut&quot;,&quot;getAverageTime&quot;,&quot;checkIn&quot;,&quot;checkOut&quot;,&quot;getAverageTime&quot;]
[[],[10,&quot;Leyton&quot;,3],[10,&quot;Paradise&quot;,8],[&quot;Leyton&quot;,&quot;Paradise&quot;],[5,&quot;Leyton&quot;,10],[5,&quot;Paradise&quot;,16],[&quot;Leyton&quot;,&quot;Paradise&quot;],[2,&quot;Leyton&quot;,21],[2,&quot;Paradise&quot;,30],[&quot;Leyton&quot;,&quot;Paradise&quot;]]

<strong>Đầu ra</strong>
[null,null,null,5.00000,null,null,5.50000,null,null,6.66667]

<strong>Giải thích</strong>
UndergroundSystem undergroundSystem = new UndergroundSystem();
undergroundSystem.checkIn(10, &quot;Leyton&quot;, 3);
undergroundSystem.checkOut(10, &quot;Paradise&quot;, 8); // Hành khách 10 đi từ &quot;Leyton&quot; đến &quot;Paradise&quot; trong 8-3 = 5
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Paradise&quot;); // trả về 5.00000, (5) / 1 = 5
undergroundSystem.checkIn(5, &quot;Leyton&quot;, 10);
undergroundSystem.checkOut(5, &quot;Paradise&quot;, 16); // Hành khách 5 đi từ &quot;Leyton&quot; đến &quot;Paradise&quot; trong 16-10 = 6
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Paradise&quot;); // trả về 5.50000, (5 + 6) / 2 = 5.5
undergroundSystem.checkIn(2, &quot;Leyton&quot;, 21);
undergroundSystem.checkOut(2, &quot;Paradise&quot;, 30); // Hành khách 2 đi từ &quot;Leyton&quot; đến &quot;Paradise&quot; trong 30-21 = 9
undergroundSystem.getAverageTime(&quot;Leyton&quot;, &quot;Paradise&quot;); // trả về 6.66667, (5 + 6 + 9) / 3 = 6.66667
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= id, t &lt;= 10<sup>6</sup></code></li>
	<li><code>1 &lt;= stationName.length, startStation.length, endStation.length &lt;= 10</code></li>
	<li>Tất cả chuỗi chỉ gồm chữ cái tiếng Anh viết hoa, viết thường và chữ số.</li>
	<li>Tổng số lần gọi đến <code>checkIn</code>, <code>checkOut</code> và <code>getAverageTime</code> không quá <code>2 * 10<sup>4</sup></code>.</li>
	<li>Đáp án sai lệch không quá <code>10<sup>-5</sup></code> so với giá trị thực tế sẽ được chấp nhận.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Thao tác check in, check out và lấy thời gian trung bình giữa hai ga cần đạt $O(1)$. Ánh xạ mỗi $\textit{id}$ tới thời điểm và ga check in; khi check out, cộng thời lượng chuyến đi vào bảng $(\textit{start},\textit{end})\mapsto(\textit{total},\textit{count})$. Thời gian trung bình là thương của hai giá trị này.

<!-- thinking:end -->

Ta dùng hai hash table để lưu dữ liệu:

- `ts`: Lưu id, thời điểm check in và ga check in của hành khách. Key là id hành khách, value là tuple `(t, stationName)`.
- `d`: Lưu ga check in, ga check out, tổng thời gian di chuyển và số chuyến. Key là tuple `(startStation, endStation)`, value là tuple `(totalTime, count)`.

Khi hành khách check in, ta lưu id, thời điểm và ga check in vào `ts`, tức là `ts[id] = (t, stationName)`.

Khi hành khách check out, ta lấy thời điểm và ga check in `(t0, station)` từ `ts`, tính thời gian di chuyển $t - t_0$, rồi cập nhật tổng thời gian và số chuyến trong `d`.

Khi cần tính thời gian di chuyển trung bình giữa hai ga, ta lấy tổng thời gian và số chuyến `(totalTime, count)` từ `d`, rồi tính trung bình bằng $totalTime / count$.

Độ phức tạp thời gian là $O(1)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số hành khách.

<!-- tabs:start -->

#### Python3

```python
class UndergroundSystem:
    def __init__(self):
        self.ts = {}
        self.d = {}

    def checkIn(self, id: int, stationName: str, t: int) -> None:
        self.ts[id] = (t, stationName)

    def checkOut(self, id: int, stationName: str, t: int) -> None:
        t0, station = self.ts[id]
        x = self.d.get((station, stationName), (0, 0))
        self.d[(station, stationName)] = (x[0] + t - t0, x[1] + 1)

    def getAverageTime(self, startStation: str, endStation: str) -> float:
        x = self.d[(startStation, endStation)]
        return x[0] / x[1]


# Your UndergroundSystem object will be instantiated and called as such:
# obj = UndergroundSystem()
# obj.checkIn(id,stationName,t)
# obj.checkOut(id,stationName,t)
# param_3 = obj.getAverageTime(startStation,endStation)
```

#### Java

```java
class UndergroundSystem {
    private Map<Integer, Integer> ts = new HashMap<>();
    private Map<Integer, String> names = new HashMap<>();
    private Map<String, int[]> d = new HashMap<>();

    public UndergroundSystem() {
    }

    public void checkIn(int id, String stationName, int t) {
        ts.put(id, t);
        names.put(id, stationName);
    }

    public void checkOut(int id, String stationName, int t) {
        String key = names.get(id) + "-" + stationName;
        int[] v = d.getOrDefault(key, new int[2]);
        v[0] += t - ts.get(id);
        v[1]++;
        d.put(key, v);
    }

    public double getAverageTime(String startStation, String endStation) {
        String key = startStation + "-" + endStation;
        int[] v = d.get(key);
        return (double) v[0] / v[1];
    }
}

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem obj = new UndergroundSystem();
 * obj.checkIn(id,stationName,t);
 * obj.checkOut(id,stationName,t);
 * double param_3 = obj.getAverageTime(startStation,endStation);
 */
```

#### C++

```cpp
class UndergroundSystem {
public:
    UndergroundSystem() {
    }

    void checkIn(int id, string stationName, int t) {
        ts[id] = {stationName, t};
    }

    void checkOut(int id, string stationName, int t) {
        auto [station, t0] = ts[id];
        auto key = station + "-" + stationName;
        auto [tot, cnt] = d[key];
        d[key] = {tot + t - t0, cnt + 1};
    }

    double getAverageTime(string startStation, string endStation) {
        auto [tot, cnt] = d[startStation + "-" + endStation];
        return (double) tot / cnt;
    }

private:
    unordered_map<int, pair<string, int>> ts;
    unordered_map<string, pair<int, int>> d;
};

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * UndergroundSystem* obj = new UndergroundSystem();
 * obj->checkIn(id,stationName,t);
 * obj->checkOut(id,stationName,t);
 * double param_3 = obj->getAverageTime(startStation,endStation);
 */
```

#### Go

```go
type UndergroundSystem struct {
	ts map[int]pair
	d  map[station][2]int
}

func Constructor() UndergroundSystem {
	return UndergroundSystem{
		ts: make(map[int]pair),
		d:  make(map[station][2]int),
	}
}

func (this *UndergroundSystem) CheckIn(id int, stationName string, t int) {
	this.ts[id] = pair{t, stationName}
}

func (this *UndergroundSystem) CheckOut(id int, stationName string, t int) {
	p := this.ts[id]
	s := station{p.a, stationName}
	if _, ok := this.d[s]; !ok {
		this.d[s] = [2]int{t - p.t, 1}
	} else {
		this.d[s] = [2]int{this.d[s][0] + t - p.t, this.d[s][1] + 1}
	}

}

func (this *UndergroundSystem) GetAverageTime(startStation string, endStation string) float64 {
	s := station{startStation, endStation}
	return float64(this.d[s][0]) / float64(this.d[s][1])
}

type station struct {
	a string
	b string
}

type pair struct {
	t int
	a string
}

/**
 * Your UndergroundSystem object will be instantiated and called as such:
 * obj := Constructor();
 * obj.CheckIn(id,stationName,t);
 * obj.CheckOut(id,stationName,t);
 * param_3 := obj.GetAverageTime(startStation,endStation);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
