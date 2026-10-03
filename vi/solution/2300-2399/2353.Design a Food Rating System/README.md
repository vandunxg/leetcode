---
comments: true
difficulty: Medium
rating: 1781
source: Weekly Contest 303 Q3
tags:
    - Design
    - Array
    - Hash Table
    - String
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2353. Design a Food Rating System](https://leetcode.com/problems/design-a-food-rating-system)

[中文文档](/solution/2300-2399/2353.Design%20a%20Food%20Rating%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một hệ thống đánh giá món ăn có thể thực hiện các thao tác sau:</p>

<ul>
	<li><strong>Thay đổi</strong> đánh giá của một món ăn trong hệ thống.</li>
	<li>Trả về món ăn được đánh giá cao nhất của một loại ẩm thực trong hệ thống.</li>
</ul>

<p>Cài đặt lớp <code>FoodRatings</code>:</p>

<ul>
	<li><code>FoodRatings(String[] foods, String[] cuisines, int[] ratings)</code> Khởi tạo hệ thống. Các món ăn được mô tả bởi <code>foods</code>, <code>cuisines</code> và <code>ratings</code>, tất cả đều có độ dài <code>n</code>.

    <ul>
    <li><code>foods[i]</code> là tên của món ăn thứ <code>i<sup>th</sup></code>,</li>
    <li><code>cuisines[i]</code> là loại ẩm thực của món ăn thứ <code>i<sup>th</sup></code>, và</li>
    <li><code>ratings[i]</code> là đánh giá ban đầu của món ăn thứ <code>i<sup>th</sup></code>.</li>
    </ul>
    </li>
    <li><code>void changeRating(String food, int newRating)</code> Thay đổi đánh giá của món ăn có tên <code>food</code>.</li>
    <li><code>String highestRated(String cuisine)</code> Trả về tên của món ăn có đánh giá cao nhất thuộc loại ẩm thực <code>cuisine</code> đã cho. Nếu có nhiều món cùng hạng, trả về món có tên <strong>nhỏ hơn theo thứ tự từ điển</strong>.</li>

</ul>

<p>Lưu ý rằng chuỗi <code>x</code> nhỏ hơn chuỗi <code>y</code> theo thứ tự từ điển nếu <code>x</code> đứng trước <code>y</code> trong thứ tự từ điển, tức là <code>x</code> là tiền tố của <code>y</code>, hoặc nếu <code>i</code> là vị trí đầu tiên sao cho <code>x[i] != y[i]</code>, thì <code>x[i]</code> đứng trước <code>y[i]</code> theo thứ tự alphabet.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;FoodRatings&quot;, &quot;highestRated&quot;, &quot;highestRated&quot;, &quot;changeRating&quot;, &quot;highestRated&quot;, &quot;changeRating&quot;, &quot;highestRated&quot;]
[[[&quot;kimchi&quot;, &quot;miso&quot;, &quot;sushi&quot;, &quot;moussaka&quot;, &quot;ramen&quot;, &quot;bulgogi&quot;], [&quot;korean&quot;, &quot;japanese&quot;, &quot;japanese&quot;, &quot;greek&quot;, &quot;japanese&quot;, &quot;korean&quot;], [9, 12, 8, 15, 14, 7]], [&quot;korean&quot;], [&quot;japanese&quot;], [&quot;sushi&quot;, 16], [&quot;japanese&quot;], [&quot;ramen&quot;, 16], [&quot;japanese&quot;]]
<strong>Đầu ra</strong>
[null, &quot;kimchi&quot;, &quot;ramen&quot;, null, &quot;sushi&quot;, null, &quot;ramen&quot;]

<strong>Giải thích</strong>
FoodRatings foodRatings = new FoodRatings([&quot;kimchi&quot;, &quot;miso&quot;, &quot;sushi&quot;, &quot;moussaka&quot;, &quot;ramen&quot;, &quot;bulgogi&quot;], [&quot;korean&quot;, &quot;japanese&quot;, &quot;japanese&quot;, &quot;greek&quot;, &quot;japanese&quot;, &quot;korean&quot;], [9, 12, 8, 15, 14, 7]);
foodRatings.highestRated(&quot;korean&quot;); // trả về &quot;kimchi&quot;
                                     // &quot;kimchi&quot; là món ăn Hàn Quốc được đánh giá cao nhất với điểm 9.
foodRatings.highestRated(&quot;japanese&quot;); // trả về &quot;ramen&quot;
                                       // &quot;ramen&quot; là món ăn Nhật Bản được đánh giá cao nhất với điểm 14.
foodRatings.changeRating(&quot;sushi&quot;, 16); // &quot;sushi&quot; hiện có điểm 16.
foodRatings.highestRated(&quot;japanese&quot;); // trả về &quot;sushi&quot;
                                       // &quot;sushi&quot; là món ăn Nhật Bản được đánh giá cao nhất với điểm 16.
foodRatings.changeRating(&quot;ramen&quot;, 16); // &quot;ramen&quot; hiện có điểm 16.
foodRatings.highestRated(&quot;japanese&quot;); // trả về &quot;ramen&quot;
                                       // &quot;sushi&quot; và &quot;ramen&quot; đều có điểm 16.
                                       // Tuy nhiên, &quot;ramen&quot; nhỏ hơn &quot;sushi&quot; theo thứ tự từ điển.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>4</sup></code></li>
	<li><code>n == foods.length == cuisines.length == ratings.length</code></li>
	<li><code>1 &lt;= foods[i].length, cuisines[i].length &lt;= 10</code></li>
	<li><code>foods[i]</code>, <code>cuisines[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= ratings[i] &lt;= 10<sup>8</sup></code></li>
	<li>Tất cả các chuỗi trong <code>foods</code> đều <strong>khác nhau</strong>.</li>
	<li><code>food</code> sẽ là tên của một món ăn trong hệ thống trong mọi lần gọi <code>changeRating</code>.</li>
	<li><code>cuisine</code> sẽ là loại ẩm thực của <strong>ít nhất một</strong> món ăn trong hệ thống trong mọi lần gọi <code>highestRated</code>.</li>
	<li>Sẽ có nhiều nhất <code>2 * 10<sup>4</sup></code> lần gọi <strong>tổng cộng</strong> đến <code>changeRating</code> và <code>highestRated</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần truy vấn món ăn được đánh giá cao nhất của một loại ẩm thực, đồng thời xử lý trường hợp hòa bằng thứ tự từ điển, và cập nhật điểm đánh giá. Có tới $2 \times 10^4$ lần gọi nên việc duyệt tuyến tính sẽ quá chậm.
>
> Với mỗi loại ẩm thực, ta duy trì một sorted set gồm các cặp $(-rating, food)$; một map lưu điểm đánh giá và loại ẩm thực của từng món ăn. Khi cập nhật, ta xóa cặp cũ rồi thêm cặp mới; khi truy vấn, ta lấy tên món ăn đầu tiên.

<!-- thinking:end -->

Ta có thể sử dụng một hash table $\textit{d}$ để lưu các món ăn theo từng loại ẩm thực, trong đó key là loại ẩm thực và value là một ordered set. Mỗi phần tử trong ordered set là một tuple $(\textit{rating}, \textit{food})$, được sắp xếp theo điểm đánh giá giảm dần; nếu điểm đánh giá bằng nhau thì sắp xếp theo tên món ăn theo thứ tự từ điển.

Ta cũng có thể sử dụng một hash table $\textit{g}$ để lưu điểm đánh giá và loại ẩm thực của từng món ăn. Cụ thể, $\textit{g}[\textit{food}] = (\textit{rating}, \textit{cuisine})$.

Trong hàm khởi tạo, ta duyệt qua $\textit{foods}$, $\textit{cuisines}$ và $\textit{ratings}$, rồi lưu điểm đánh giá và loại ẩm thực của từng món ăn vào $\textit{d}$ và $\textit{g}$.

Trong hàm $\textit{changeRating}$, trước hết ta lấy điểm đánh giá ban đầu $\textit{oldRating}$ và loại ẩm thực $\textit{cuisine}$ của món ăn $\textit{food}$, sau đó cập nhật điểm đánh giá của $\textit{g}[\textit{food}]$ thành $\textit{newRating}$, xóa $(\textit{oldRating}, \textit{food})$ khỏi $\textit{d}[\textit{cuisine}]$ và thêm $(\textit{newRating}, \textit{food})$ vào $\textit{d}[\textit{cuisine}]$.

Trong hàm $\textit{highestRated}$, ta trực tiếp trả về tên món ăn của phần tử đầu tiên trong $\textit{d}[\textit{cuisine}]$.

Về độ phức tạp thời gian, hàm khởi tạo có độ phức tạp $O(n \log n)$, trong đó $n$ là số lượng món ăn. Các thao tác còn lại có độ phức tạp $O(\log n)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class FoodRatings:

    def __init__(self, foods: List[str], cuisines: List[str], ratings: List[int]):
        self.d = defaultdict(SortedList)
        self.g = {}
        for food, cuisine, rating in zip(foods, cuisines, ratings):
            self.d[cuisine].add((-rating, food))
            self.g[food] = (rating, cuisine)

    def changeRating(self, food: str, newRating: int) -> None:
        oldRating, cuisine = self.g[food]
        self.g[food] = (newRating, cuisine)
        self.d[cuisine].remove((-oldRating, food))
        self.d[cuisine].add((-newRating, food))

    def highestRated(self, cuisine: str) -> str:
        return self.d[cuisine][0][1]


# Your FoodRatings object will be instantiated and called as such:
# obj = FoodRatings(foods, cuisines, ratings)
# obj.changeRating(food,newRating)
# param_2 = obj.highestRated(cuisine)
```

#### Java

```java
class FoodRatings {
    private Map<String, TreeSet<Pair<Integer, String>>> d = new HashMap<>();
    private Map<String, Pair<Integer, String>> g = new HashMap<>();
    private final Comparator<Pair<Integer, String>> cmp = (a, b) -> {
        if (!a.getKey().equals(b.getKey())) {
            return b.getKey().compareTo(a.getKey());
        }
        return a.getValue().compareTo(b.getValue());
    };

    public FoodRatings(String[] foods, String[] cuisines, int[] ratings) {
        for (int i = 0; i < foods.length; ++i) {
            String food = foods[i], cuisine = cuisines[i];
            int rating = ratings[i];
            d.computeIfAbsent(cuisine, k -> new TreeSet<>(cmp)).add(new Pair<>(rating, food));
            g.put(food, new Pair<>(rating, cuisine));
        }
    }

    public void changeRating(String food, int newRating) {
        Pair<Integer, String> old = g.get(food);
        int oldRating = old.getKey();
        String cuisine = old.getValue();
        g.put(food, new Pair<>(newRating, cuisine));
        d.get(cuisine).remove(new Pair<>(oldRating, food));
        d.get(cuisine).add(new Pair<>(newRating, food));
    }

    public String highestRated(String cuisine) {
        return d.get(cuisine).first().getValue();
    }
}

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings obj = new FoodRatings(foods, cuisines, ratings);
 * obj.changeRating(food,newRating);
 * String param_2 = obj.highestRated(cuisine);
 */
```

#### C++

```cpp
class FoodRatings {
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for (int i = 0; i < foods.size(); ++i) {
            string food = foods[i], cuisine = cuisines[i];
            int rating = ratings[i];
            d[cuisine].insert({-rating, food});
            g[food] = {rating, cuisine};
        }
    }

    void changeRating(string food, int newRating) {
        auto [oldRating, cuisine] = g[food];
        g[food] = {newRating, cuisine};
        d[cuisine].erase({-oldRating, food});
        d[cuisine].insert({-newRating, food});
    }

    string highestRated(string cuisine) {
        return d[cuisine].begin()->second;
    }

private:
    unordered_map<string, set<pair<int, string>>> d;
    unordered_map<string, pair<int, string>> g;
};

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings* obj = new FoodRatings(foods, cuisines, ratings);
 * obj->changeRating(food,newRating);
 * string param_2 = obj->highestRated(cuisine);
 */
```

#### Go

```go
import (
	"github.com/emirpasic/gods/v2/trees/redblacktree"
)

type pair struct {
	rating int
	food   string
}

type FoodRatings struct {
	d map[string]*redblacktree.Tree[pair, struct{}]
	g map[string]pair
}

func Constructor(foods []string, cuisines []string, ratings []int) FoodRatings {
	d := make(map[string]*redblacktree.Tree[pair, struct{}])
	g := make(map[string]pair)

	for i, food := range foods {
		rating, cuisine := ratings[i], cuisines[i]
		g[food] = pair{rating, cuisine}

		if d[cuisine] == nil {
			d[cuisine] = redblacktree.NewWith[pair, struct{}](func(a, b pair) int {
				return cmp.Or(b.rating-a.rating, strings.Compare(a.food, b.food))
			})
		}
		d[cuisine].Put(pair{rating, food}, struct{}{})
	}

	return FoodRatings{d, g}
}

func (this *FoodRatings) ChangeRating(food string, newRating int) {
	p := this.g[food]
	t := this.d[p.food]

	t.Remove(pair{p.rating, food})
	t.Put(pair{newRating, food}, struct{}{})

	p.rating = newRating
	this.g[food] = p
}

func (this *FoodRatings) HighestRated(cuisine string) string {
	return this.d[cuisine].Left().Key.food
}

/**
 * Your FoodRatings object will be instantiated and called as such:
 * obj := Constructor(foods, cuisines, ratings);
 * obj.ChangeRating(food,newRating);
 * param_2 := obj.HighestRated(cuisine);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
