---
comments: true
difficulty: Hard
rating: 2181
source: Biweekly Contest 55 Q4
tags:
    - Design
    - Array
    - Hash Table
    - Ordered Set
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1912. Design Movie Rental System](https://leetcode.com/problems/design-movie-rental-system)

[中文文档](/solution/1900-1999/1912.Design%20Movie%20Rental%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một công ty cho thuê phim gồm <code>n</code> cửa hàng. Hãy triển khai một hệ thống cho thuê hỗ trợ tìm kiếm, thuê và trả phim. Hệ thống cũng cần hỗ trợ tạo báo cáo về những phim hiện đang được thuê.</p>

<p>Mỗi phim được cho dưới dạng một mảng số nguyên 2 chiều <code>entries</code>, trong đó <code>entries[i] = [shop<sub>i</sub>, movie<sub>i</sub>, price<sub>i</sub>]</code> cho biết cửa hàng <code>shop<sub>i</sub></code> có một bản sao của phim <code>movie<sub>i</sub></code> với giá thuê là <code>price<sub>i</sub></code>. Mỗi cửa hàng có <strong>nhiều nhất một</strong> bản sao của một phim <code>movie<sub>i</sub></code>.</p>

<p>Hệ thống cần hỗ trợ các hàm sau:</p>

<ul>
	<li><strong>Tìm kiếm</strong>: Tìm <strong>5 cửa hàng rẻ nhất</strong> có <strong>bản sao chưa được thuê</strong> của một phim cho trước. Các cửa hàng được sắp xếp theo <strong>giá</strong> tăng dần; nếu bằng nhau, cửa hàng có <strong>nhỏ hơn </strong><code>shop<sub>i</sub></code> đứng trước. Nếu có ít hơn 5 cửa hàng phù hợp, trả về tất cả chúng. Nếu không có cửa hàng nào có bản sao chưa được thuê, trả về một danh sách rỗng.</li>
	<li><strong>Thuê</strong>: Thuê một <strong>bản sao chưa được thuê</strong> của một phim cho trước từ một cửa hàng cho trước.</li>
	<li><strong>Trả</strong>: Trả một <strong>bản sao đã được thuê trước đó</strong> của một phim cho trước tại một cửa hàng cho trước.</li>
	<li><strong>Báo cáo</strong>: Trả về <strong>5 phim được thuê rẻ nhất</strong> (có thể là cùng một movie ID) dưới dạng danh sách 2 chiều <code>res</code>, trong đó <code>res[j] = [shop<sub>j</sub>, movie<sub>j</sub>]</code> cho biết phim <code>movie<sub>j</sub></code> được thuê từ cửa hàng <code>shop<sub>j</sub></code> là phim được thuê rẻ thứ <code>j<sup>th</sup></code>. Các phim trong <code>res</code> được sắp xếp theo <strong>giá</strong> tăng dần; nếu bằng nhau, phim có <strong>nhỏ hơn </strong><code>shop<sub>j</sub></code> đứng trước; nếu vẫn bằng nhau, phim có <strong>nhỏ hơn </strong><code>movie<sub>j</sub></code> đứng trước. Nếu có ít hơn 5 phim được thuê, trả về tất cả chúng. Nếu hiện không có phim nào đang được thuê, trả về một danh sách rỗng.</li>
</ul>

<p>Hãy triển khai lớp <code>MovieRentingSystem</code>:</p>

<ul>
	<li><code>MovieRentingSystem(int n, int[][] entries)</code> Khởi tạo đối tượng <code>MovieRentingSystem</code> với <code>n</code> cửa hàng và các phim trong <code>entries</code>.</li>
	<li><code>List&lt;Integer&gt; search(int movie)</code> Trả về danh sách các cửa hàng có <strong>bản sao chưa được thuê</strong> của phim <code>movie</code> cho trước như mô tả ở trên.</li>
	<li><code>void rent(int shop, int movie)</code> Thuê phim <code>movie</code> cho trước từ cửa hàng <code>shop</code> cho trước.</li>
	<li><code>void drop(int shop, int movie)</code> Trả phim <code>movie</code> đã được thuê trước đó tại cửa hàng <code>shop</code> cho trước.</li>
	<li><code>List&lt;List&lt;Integer&gt;&gt; report()</code> Trả về danh sách các phim <strong>được thuê</strong> rẻ nhất như mô tả ở trên.</li>
</ul>

<p><strong>Lưu ý:</strong> Các test case được tạo sao cho <code>rent</code> chỉ được gọi khi cửa hàng có <strong>bản sao chưa được thuê</strong> của phim, và <code>drop</code> chỉ được gọi khi cửa hàng đã <strong>từng cho thuê</strong> phim đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;MovieRentingSystem&quot;, &quot;search&quot;, &quot;rent&quot;, &quot;rent&quot;, &quot;report&quot;, &quot;drop&quot;, &quot;search&quot;]
[[3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]], [1], [0, 1], [1, 2], [], [1, 2], [2]]
<strong>Đầu ra</strong>
[null, [1, 0, 2], null, null, [[0, 1], [1, 2]], null, [0, 1]]

<strong>Giải thích</strong>
MovieRentingSystem movieRentingSystem = new MovieRentingSystem(3, [[0, 1, 5], [0, 2, 6], [0, 3, 7], [1, 1, 4], [1, 2, 7], [2, 1, 5]]);
movieRentingSystem.search(1);  // return [1, 0, 2], Movies of ID 1 are unrented at shops 1, 0, and 2. Shop 1 is cheapest; shop 0 and 2 are the same price, so order by shop number.
movieRentingSystem.rent(0, 1); // Rent movie 1 from shop 0. Unrented movies at shop 0 are now [2,3].
movieRentingSystem.rent(1, 2); // Rent movie 2 from shop 1. Unrented movies at shop 1 are now [1].
movieRentingSystem.report();   // return [[0, 1], [1, 2]]. Movie 1 from shop 0 is cheapest, followed by movie 2 from shop 1.
movieRentingSystem.drop(1, 2); // Drop off movie 2 at shop 1. Unrented movies at shop 1 are now [1,2].
movieRentingSystem.search(2);  // return [0, 1]. Movies of ID 2 are unrented at shops 0 and 1. Shop 0 is cheapest, followed by shop 1.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 3 * 10<sup>5</sup></code></li>
	<li><code>1 &lt;= entries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= shop<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= movie<sub>i</sub>, price<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Mỗi cửa hàng có <strong>nhiều nhất một</strong> bản sao của một phim <code>movie<sub>i</sub></code>.</li>
	<li><strong>Tổng số</strong> lần gọi đến <code>search</code>, <code>rent</code>, <code>drop</code> và <code>report</code> nhiều nhất là <code>10<sup>5</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Cả $\textit{search}$ và $\textit{report}$ đều cần 5 tuple đầu tiên theo một thứ tự cố định, trong khi rent/drop làm thay đổi các set. Việc sắp xếp lại từ đầu với $n,m\le 10^5$ là quá chậm.
>
> Các bản sao chưa được thuê của mỗi phim nằm trong một danh sách đã sắp xếp gồm $(\textit{price},\textit{shop})$; tất cả bản sao đã được thuê nằm trong một danh sách đã sắp xếp gồm $(\textit{price},\textit{shop},\textit{movie})$. Một hash map lưu giá để tra cứu key trong $O(1)$.
>
> Rent chuyển một entry từ bucket của phim sang danh sách đã thuê; drop thực hiện thao tác ngược lại. Năm dòng đầu tiên là một lát cắt, và mỗi lần cập nhật có độ phức tạp logarit.

<!-- thinking:end -->

Ta định nghĩa một ordered set $\textit{available}$, trong đó $\textit{available}[movie]$ lưu danh sách tất cả cửa hàng chưa cho thuê phim $movie$. Mỗi phần tử trong danh sách là $(\textit{price}, \textit{shop})$, được sắp xếp tăng dần theo $\textit{price}$; nếu giá bằng nhau thì sắp xếp tăng dần theo $\textit{shop}$.

Ngoài ra, ta định nghĩa một hash map $\textit{price\_map}$, trong đó $\textit{price\_map}[f(\textit{shop}, \textit{movie})]$ lưu giá thuê của phim $\textit{movie}$ tại cửa hàng $\textit{shop}$.

Ta cũng định nghĩa một ordered set $\textit{rented}$, lưu tất cả phim đã được thuê dưới dạng $(\textit{price}, \textit{shop}, \textit{movie})$, được sắp xếp tăng dần theo $\textit{price}$, sau đó theo $\textit{shop}$, và nếu cả hai đều bằng nhau thì theo $\textit{movie}$.

Với thao tác $\text{MovieRentingSystem}(n, \text{entries})$, ta duyệt qua $\text{entries}$ và thêm thông tin phim của mỗi cửa hàng vào cả $\textit{available}$ và $\textit{price\_map}$. Độ phức tạp thời gian là $O(m \log m)$, trong đó $m$ là độ dài của $\text{entries}$.

Với thao tác $\text{search}(\text{movie})$, ta trả về ID của 5 cửa hàng đầu tiên trong $\textit{available}[\text{movie}]$. Độ phức tạp thời gian là $O(1)$.

Với thao tác $\text{rent}(\text{shop}, \text{movie})$, ta xóa $(\textit{price}, \textit{shop})$ khỏi $\textit{available}[\text{movie}]$ và thêm $(\textit{price}, \textit{shop}, \textit{movie})$ vào $\textit{rented}$. Độ phức tạp thời gian là $O(\log m)$.

Với thao tác $\text{drop}(\text{shop}, \text{movie})$, ta xóa $(\textit{price}, \textit{shop}, \textit{movie})$ khỏi $\textit{rented}$ và thêm lại $(\textit{price}, \textit{shop})$ vào $\textit{available}[\text{movie}]$. Độ phức tạp thời gian là $O(\log m)$.

Với thao tác $\text{report}()$, ta trả về ID cửa hàng và ID phim của 5 phim đầu tiên trong $\textit{rented}$. Độ phức tạp thời gian là $O(1)$.

Độ phức tạp không gian là $O(m)$, trong đó $m$ là độ dài của $\text{entries}$.

<!-- tabs:start -->

#### Python3

```python
class MovieRentingSystem:

    def __init__(self, n: int, entries: List[List[int]]):
        self.available = defaultdict(lambda: SortedList())
        self.price_map = {}
        for shop, movie, price in entries:
            self.available[movie].add((price, shop))
            self.price_map[self.f(shop, movie)] = price
        self.rented = SortedList()

    def search(self, movie: int) -> List[int]:
        return [shop for _, shop in self.available[movie][:5]]

    def rent(self, shop: int, movie: int) -> None:
        price = self.price_map[self.f(shop, movie)]
        self.available[movie].remove((price, shop))
        self.rented.add((price, shop, movie))

    def drop(self, shop: int, movie: int) -> None:
        price = self.price_map[self.f(shop, movie)]
        self.rented.remove((price, shop, movie))
        self.available[movie].add((price, shop))

    def report(self) -> List[List[int]]:
        return [[shop, movie] for _, shop, movie in self.rented[:5]]

    def f(self, shop: int, movie: int) -> int:
        return shop << 30 | movie


# Your MovieRentingSystem object will be instantiated and called as such:
# obj = MovieRentingSystem(n, entries)
# param_1 = obj.search(movie)
# obj.rent(shop,movie)
# obj.drop(shop,movie)
# param_4 = obj.report()
```

#### Java

```java
class MovieRentingSystem {
    private Map<Integer, TreeSet<int[]>> available = new HashMap<>();
    private Map<Long, Integer> priceMap = new HashMap<>();
    private TreeSet<int[]> rented = new TreeSet<>((a, b) -> {
        if (a[0] != b[0]) {
            return a[0] - b[0];
        }
        if (a[1] != b[1]) {
            return a[1] - b[1];
        }
        return a[2] - b[2];
    });

    public MovieRentingSystem(int n, int[][] entries) {
        for (int[] entry : entries) {
            int shop = entry[0], movie = entry[1], price = entry[2];
            available
                .computeIfAbsent(movie, k -> new TreeSet<>((a, b) -> {
                    if (a[0] != b[0]) {
                        return a[0] - b[0];
                    }
                    return a[1] - b[1];
                }))
                .add(new int[] {price, shop});
            priceMap.put(f(shop, movie), price);
        }
    }

    public List<Integer> search(int movie) {
        List<Integer> res = new ArrayList<>();
        if (!available.containsKey(movie)) {
            return res;
        }
        int cnt = 0;
        for (int[] item : available.get(movie)) {
            res.add(item[1]);
            if (++cnt == 5) {
                break;
            }
        }
        return res;
    }

    public void rent(int shop, int movie) {
        int price = priceMap.get(f(shop, movie));
        available.get(movie).remove(new int[] {price, shop});
        rented.add(new int[] {price, shop, movie});
    }

    public void drop(int shop, int movie) {
        int price = priceMap.get(f(shop, movie));
        rented.remove(new int[] {price, shop, movie});
        available.get(movie).add(new int[] {price, shop});
    }

    public List<List<Integer>> report() {
        List<List<Integer>> res = new ArrayList<>();
        int cnt = 0;
        for (int[] item : rented) {
            res.add(Arrays.asList(item[1], item[2]));
            if (++cnt == 5) {
                break;
            }
        }
        return res;
    }

    private long f(int shop, int movie) {
        return ((long) shop << 30) | movie;
    }
}

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * MovieRentingSystem obj = new MovieRentingSystem(n, entries);
 * List<Integer> param_1 = obj.search(movie);
 * obj.rent(shop,movie);
 * obj.drop(shop,movie);
 * List<List<Integer>> param_4 = obj.report();
 */
```

#### C++

```cpp
class MovieRentingSystem {
private:
    unordered_map<int, set<pair<int, int>>> available; // movie -> {(price, shop)}
    unordered_map<long long, int> priceMap;
    set<tuple<int, int, int>> rented; // {(price, shop, movie)}

    long long f(int shop, int movie) {
        return ((long long) shop << 30) | movie;
    }

public:
    MovieRentingSystem(int n, vector<vector<int>>& entries) {
        for (auto& e : entries) {
            int shop = e[0], movie = e[1], price = e[2];
            available[movie].insert({price, shop});
            priceMap[f(shop, movie)] = price;
        }
    }

    vector<int> search(int movie) {
        vector<int> res;
        if (!available.count(movie)) {
            return res;
        }
        int cnt = 0;
        for (auto& [price, shop] : available[movie]) {
            res.push_back(shop);
            if (++cnt == 5) {
                break;
            }
        }
        return res;
    }

    void rent(int shop, int movie) {
        int price = priceMap[f(shop, movie)];
        available[movie].erase({price, shop});
        rented.insert({price, shop, movie});
    }

    void drop(int shop, int movie) {
        int price = priceMap[f(shop, movie)];
        rented.erase({price, shop, movie});
        available[movie].insert({price, shop});
    }

    vector<vector<int>> report() {
        vector<vector<int>> res;
        int cnt = 0;
        for (auto& [price, shop, movie] : rented) {
            res.push_back({shop, movie});
            if (++cnt == 5) {
                break;
            }
        }
        return res;
    }
};

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * MovieRentingSystem* obj = new MovieRentingSystem(n, entries);
 * vector<int> param_1 = obj->search(movie);
 * obj->rent(shop,movie);
 * obj->drop(shop,movie);
 * vector<vector<int>> param_4 = obj->report();
 */
```

#### Go

```go
type MovieRentingSystem struct {
	available map[int]*treeset.Set // movie -> (price, shop)
	priceMap  map[int64]int
	rented    *treeset.Set // (price, shop, movie)
}

func Constructor(n int, entries [][]int) MovieRentingSystem {
	// comparator for (price, shop)
	cmpAvail := func(a, b any) int {
		x := a.([2]int)
		y := b.([2]int)
		if x[0] != y[0] {
			return x[0] - y[0]
		}
		return x[1] - y[1]
	}
	// comparator for (price, shop, movie)
	cmpRented := func(a, b any) int {
		x := a.([3]int)
		y := b.([3]int)
		if x[0] != y[0] {
			return x[0] - y[0]
		}
		if x[1] != y[1] {
			return x[1] - y[1]
		}
		return x[2] - y[2]
	}

	mrs := MovieRentingSystem{
		available: make(map[int]*treeset.Set),
		priceMap:  make(map[int64]int),
		rented:    treeset.NewWith(cmpRented),
	}

	for _, e := range entries {
		shop, movie, price := e[0], e[1], e[2]
		if _, ok := mrs.available[movie]; !ok {
			mrs.available[movie] = treeset.NewWith(cmpAvail)
		}
		mrs.available[movie].Add([2]int{price, shop})
		mrs.priceMap[f(shop, movie)] = price
	}

	return mrs
}

func (this *MovieRentingSystem) Search(movie int) []int {
	res := []int{}
	if _, ok := this.available[movie]; !ok {
		return res
	}
	it := this.available[movie].Iterator()
	it.Begin()
	cnt := 0
	for it.Next() && cnt < 5 {
		pair := it.Value().([2]int)
		res = append(res, pair[1])
		cnt++
	}
	return res
}

func (this *MovieRentingSystem) Rent(shop int, movie int) {
	price := this.priceMap[f(shop, movie)]
	this.available[movie].Remove([2]int{price, shop})
	this.rented.Add([3]int{price, shop, movie})
}

func (this *MovieRentingSystem) Drop(shop int, movie int) {
	price := this.priceMap[f(shop, movie)]
	this.rented.Remove([3]int{price, shop, movie})
	this.available[movie].Add([2]int{price, shop})
}

func (this *MovieRentingSystem) Report() [][]int {
	res := [][]int{}
	it := this.rented.Iterator()
	it.Begin()
	cnt := 0
	for it.Next() && cnt < 5 {
		t := it.Value().([3]int)
		res = append(res, []int{t[1], t[2]})
		cnt++
	}
	return res
}

func f(shop, movie int) int64 {
	return (int64(shop) << 30) | int64(movie)
}

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * obj := Constructor(n, entries);
 * param_1 := obj.Search(movie);
 * obj.Rent(shop,movie);
 * obj.Drop(shop,movie);
 * param_4 := obj.Report();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
