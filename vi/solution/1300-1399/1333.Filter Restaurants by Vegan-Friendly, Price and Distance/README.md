---
comments: true
difficulty: Medium
rating: 1423
source: Weekly Contest 173 Q2
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1333. Filter Restaurants by Vegan-Friendly, Price and Distance](https://leetcode.com/problems/filter-restaurants-by-vegan-friendly-price-and-distance)

[中文文档](/solution/1300-1399/1333.Filter%20Restaurants%20by%20Vegan-Friendly%2C%20Price%20and%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>restaurants</code>, trong đó <code>restaurants[i] = [id<sub>i</sub>, rating<sub>i</sub>, veganFriendly<sub>i</sub>, price<sub>i</sub>, distance<sub>i</sub>]</code>. Hãy lọc nhà hàng theo ba tiêu chí.</p>

<p>Bộ lọc <code>veganFriendly</code> có thể là <em>true</em> (chỉ chọn nhà hàng có <code>veganFriendly<sub>i</sub></code> bằng true) hoặc <em>false</em> (có thể chọn bất kỳ nhà hàng nào). Ngoài ra, <code>maxPrice</code> và <code>maxDistance</code> lần lượt là giá và khoảng cách tối đa của các nhà hàng được xét.</p>

<p>Sau khi lọc, trả về mảng <em><strong>ID</strong></em> của các nhà hàng, sắp xếp theo <strong>rating</strong> giảm dần. Nếu rating bằng nhau, sắp xếp theo <em><strong>id</strong></em> giảm dần. Quy ước <code>veganFriendly<sub>i</sub></code> và <code>veganFriendly</code> có giá trị <em>1</em> khi là <em>true</em>, và <em>0</em> khi là <em>false</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> restaurants = [[1,4,1,40,10],[2,8,0,50,5],[3,8,1,30,4],[4,10,0,10,3],[5,1,1,15,1]], veganFriendly = 1, maxPrice = 50, maxDistance = 10
<strong>Đầu ra:</strong> [3,1,5] 
<strong>Giải thích: 
</strong>Danh sách nhà hàng:
Nhà hàng 1 [id=1, rating=4, veganFriendly=1, price=40, distance=10]
Nhà hàng 2 [id=2, rating=8, veganFriendly=0, price=50, distance=5]
Nhà hàng 3 [id=3, rating=8, veganFriendly=1, price=30, distance=4]
Nhà hàng 4 [id=4, rating=10, veganFriendly=0, price=10, distance=3]
Nhà hàng 5 [id=5, rating=1, veganFriendly=1, price=15, distance=1] 
Sau khi lọc nhà hàng với veganFriendly = 1, maxPrice = 50 và maxDistance = 10, ta còn nhà hàng 3, nhà hàng 1 và nhà hàng 5 (được sắp xếp theo rating giảm dần). 
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> restaurants = [[1,4,1,40,10],[2,8,0,50,5],[3,8,1,30,4],[4,10,0,10,3],[5,1,1,15,1]], veganFriendly = 0, maxPrice = 50, maxDistance = 10
<strong>Đầu ra:</strong> [4,3,2,1,5]
<strong>Giải thích:</strong> Danh sách nhà hàng giống ví dụ 1, nhưng ở đây bộ lọc veganFriendly = 0 nên xét tất cả nhà hàng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> restaurants = [[1,4,1,40,10],[2,8,0,50,5],[3,8,1,30,4],[4,10,0,10,3],[5,1,1,15,1]], veganFriendly = 0, maxPrice = 30, maxDistance = 3
<strong>Đầu ra:</strong> [4,5]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;restaurants.length &lt;= 10^4</code></li>
	<li><code>restaurants[i].length == 5</code></li>
	<li><code>1 &lt;=&nbsp;id<sub>i</sub>, rating<sub>i</sub>, price<sub>i</sub>, distance<sub>i </sub>&lt;= 10^5</code></li>
	<li><code>1 &lt;=&nbsp;maxPrice,&nbsp;maxDistance &lt;= 10^5</code></li>
	<li><code>veganFriendly<sub>i</sub></code> and&nbsp;<code>veganFriendly</code>&nbsp;are&nbsp;0 or 1.</li>
	<li>All <code>id<sub>i</sub></code> are distinct.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Lọc theo trạng thái vegan, giá và khoảng cách, sau đó sắp xếp rating và id theo thứ tự giảm dần. Có thể sắp xếp trước rồi lọc: sắp xếp theo $(-\textit{rating},-\textit{id})$, sau đó loại các nhà hàng không thỏa điều kiện. Các ID còn lại đã đúng thứ tự yêu cầu.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def filterRestaurants(
        self,
        restaurants: List[List[int]],
        veganFriendly: int,
        maxPrice: int,
        maxDistance: int,
    ) -> List[int]:
        restaurants.sort(key=lambda x: (-x[1], -x[0]))
        ans = []
        for idx, _, vegan, price, dist in restaurants:
            if vegan >= veganFriendly and price <= maxPrice and dist <= maxDistance:
                ans.append(idx)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> filterRestaurants(
        int[][] restaurants, int veganFriendly, int maxPrice, int maxDistance) {
        Arrays.sort(restaurants, (a, b) -> a[1] == b[1] ? b[0] - a[0] : b[1] - a[1]);
        List<Integer> ans = new ArrayList<>();
        for (int[] r : restaurants) {
            if (r[2] >= veganFriendly && r[3] <= maxPrice && r[4] <= maxDistance) {
                ans.add(r[0]);
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
    vector<int> filterRestaurants(vector<vector<int>>& restaurants, int veganFriendly, int maxPrice, int maxDistance) {
        sort(restaurants.begin(), restaurants.end(), [](const vector<int>& a, const vector<int>& b) {
            if (a[1] != b[1]) {
                return a[1] > b[1];
            }
            return a[0] > b[0];
        });
        vector<int> ans;
        for (auto& r : restaurants) {
            if (r[2] >= veganFriendly && r[3] <= maxPrice && r[4] <= maxDistance) {
                ans.push_back(r[0]);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func filterRestaurants(restaurants [][]int, veganFriendly int, maxPrice int, maxDistance int) (ans []int) {
	sort.Slice(restaurants, func(i, j int) bool {
		a, b := restaurants[i], restaurants[j]
		if a[1] != b[1] {
			return a[1] > b[1]
		}
		return a[0] > b[0]
	})
	for _, r := range restaurants {
		if r[2] >= veganFriendly && r[3] <= maxPrice && r[4] <= maxDistance {
			ans = append(ans, r[0])
		}
	}
	return
}
```

#### TypeScript

```ts
function filterRestaurants(
    restaurants: number[][],
    veganFriendly: number,
    maxPrice: number,
    maxDistance: number,
): number[] {
    restaurants.sort((a, b) => (a[1] === b[1] ? b[0] - a[0] : b[1] - a[1]));
    const ans: number[] = [];
    for (const [id, _, vegan, price, distance] of restaurants) {
        if (vegan >= veganFriendly && price <= maxPrice && distance <= maxDistance) {
            ans.push(id);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
