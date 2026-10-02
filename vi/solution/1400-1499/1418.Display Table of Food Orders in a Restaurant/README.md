---
comments: true
difficulty: Medium
rating: 1485
source: Weekly Contest 185 Q2
tags:
    - Array
    - Hash Table
    - String
    - Ordered Set
    - Sorting
---

<!-- problem:start -->

# [1418. Display Table of Food Orders in a Restaurant](https://leetcode.com/problems/display-table-of-food-orders-in-a-restaurant)

[中文文档](/solution/1400-1499/1418.Display%20Table%20of%20Food%20Orders%20in%20a%20Restaurant/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>orders</code>, biểu diễn các order mà khách hàng đã gọi trong nhà hàng. Cụ thể hơn, <code>orders[i]=[customerName<sub>i</sub>,tableNumber<sub>i</sub>,foodItem<sub>i</sub>]</code>, trong đó <code>customerName<sub>i</sub></code> là tên khách hàng, <code>tableNumber<sub>i</sub></code>&nbsp;là số bàn khách hàng ngồi, và <code>foodItem<sub>i</sub></code>&nbsp;là món khách hàng gọi.</p>

<p><em>Trả về &ldquo;<strong>display table</strong>&rdquo; của nhà hàng</em>. &ldquo;<strong>Display table</strong>&rdquo; là một bảng trong đó các ô biểu diễn số lượng mỗi món mà từng bàn đã gọi. Cột đầu tiên là số bàn, các cột còn lại tương ứng với từng món theo thứ tự alphabet. Hàng đầu tiên phải là header, với cột đầu tiên là &ldquo;Table&rdquo;, theo sau là tên các món. Lưu ý rằng tên khách hàng không được đưa vào bảng. Ngoài ra, các hàng phải được sắp xếp theo thứ tự tăng dần về số.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> orders = [[&quot;David&quot;,&quot;3&quot;,&quot;Ceviche&quot;],[&quot;Corina&quot;,&quot;10&quot;,&quot;Beef Burrito&quot;],[&quot;David&quot;,&quot;3&quot;,&quot;Fried Chicken&quot;],[&quot;Carla&quot;,&quot;5&quot;,&quot;Water&quot;],[&quot;Carla&quot;,&quot;5&quot;,&quot;Ceviche&quot;],[&quot;Rous&quot;,&quot;3&quot;,&quot;Ceviche&quot;]]
<strong>Đầu ra:</strong> [[&quot;Table&quot;,&quot;Beef Burrito&quot;,&quot;Ceviche&quot;,&quot;Fried Chicken&quot;,&quot;Water&quot;],[&quot;3&quot;,&quot;0&quot;,&quot;2&quot;,&quot;1&quot;,&quot;0&quot;],[&quot;5&quot;,&quot;0&quot;,&quot;1&quot;,&quot;0&quot;,&quot;1&quot;],[&quot;10&quot;,&quot;1&quot;,&quot;0&quot;,&quot;0&quot;,&quot;0&quot;]]
<strong>Giải thích:
</strong>Bảng hiển thị có dạng:
<strong>Table,Beef Burrito,Ceviche,Fried Chicken,Water</strong>
3    ,0           ,2      ,1            ,0
5    ,0           ,1      ,0            ,1
10   ,1           ,0      ,0            ,0
Với bàn 3: David gọi &quot;Ceviche&quot; và &quot;Fried Chicken&quot;, còn Rous gọi &quot;Ceviche&quot;.
Với bàn 5: Carla gọi &quot;Water&quot; và &quot;Ceviche&quot;.
Với bàn 10: Corina gọi &quot;Beef Burrito&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> orders = [[&quot;James&quot;,&quot;12&quot;,&quot;Fried Chicken&quot;],[&quot;Ratesh&quot;,&quot;12&quot;,&quot;Fried Chicken&quot;],[&quot;Amadeus&quot;,&quot;12&quot;,&quot;Fried Chicken&quot;],[&quot;Adam&quot;,&quot;1&quot;,&quot;Canadian Waffles&quot;],[&quot;Brianna&quot;,&quot;1&quot;,&quot;Canadian Waffles&quot;]]
<strong>Đầu ra:</strong> [[&quot;Table&quot;,&quot;Canadian Waffles&quot;,&quot;Fried Chicken&quot;],[&quot;1&quot;,&quot;2&quot;,&quot;0&quot;],[&quot;12&quot;,&quot;0&quot;,&quot;3&quot;]]
<strong>Giải thích:</strong>
Với bàn 1: Adam và Brianna gọi &quot;Canadian Waffles&quot;.
Với bàn 12: James, Ratesh và Amadeus gọi &quot;Fried Chicken&quot;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> orders = [[&quot;Laura&quot;,&quot;2&quot;,&quot;Bean Burrito&quot;],[&quot;Jhon&quot;,&quot;2&quot;,&quot;Beef Burrito&quot;],[&quot;Melissa&quot;,&quot;2&quot;,&quot;Soda&quot;]]
<strong>Đầu ra:</strong> [[&quot;Table&quot;,&quot;Bean Burrito&quot;,&quot;Beef Burrito&quot;,&quot;Soda&quot;],[&quot;2&quot;,&quot;1&quot;,&quot;1&quot;,&quot;1&quot;]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;=&nbsp;orders.length &lt;= 5 * 10^4</code></li>
	<li><code>orders[i].length == 3</code></li>
	<li><code>1 &lt;= customerName<sub>i</sub>.length, foodItem<sub>i</sub>.length &lt;= 20</code></li>
	<li><code>customerName<sub>i</sub></code> và <code>foodItem<sub>i</sub></code> chỉ gồm các chữ cái tiếng Anh viết thường, viết hoa và ký tự khoảng trắng.</li>
	<li><code>tableNumber<sub>i</sub>&nbsp;</code>là một số nguyên hợp lệ trong khoảng từ <code>1</code> đến <code>500</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Các order đến dưới dạng bộ ba, nhưng đầu ra là một bảng được sắp xếp theo số bàn và tên món. Vì $n\le 5\times 10^4$, trước tiên ta gom nhóm rồi chỉ sắp xếp một lần.
>
> Ánh xạ mỗi bàn tới các món của bàn đó và thu thập tập tất cả món ăn. Sắp xếp tên món cho header, sau đó với mỗi bàn, xuất các số lượng tương ứng với header đó.

<!-- thinking:end -->

Ta có thể dùng một hash table $\textit{tables}$ để lưu các món được gọi tại mỗi bàn, và một set $\textit{items}$ để lưu tất cả các món.

Duyệt qua $\textit{orders}$, lưu các món được gọi tại mỗi bàn vào $\textit{tables}$ và $\textit{items}$.

Sau đó sắp xếp $\textit{items}$ để thu được $\textit{sortedItems}$.

Tiếp theo, ta xây dựng mảng kết quả $\textit{ans}$. Trước tiên, thêm hàng header $\textit{header}$ vào $\textit{ans}$. Sau đó, duyệt qua các bàn trong $\textit{tables}$ đã được sắp xếp. Với mỗi bàn, dùng một counter $\textit{cnt}$ để đếm số lượng từng món, rồi tạo một hàng $\textit{row}$ và thêm vào $\textit{ans}$.

Cuối cùng, trả về $\textit{ans}$.

Độ phức tạp thời gian là $O(n + m \times \log m + k \times \log k + m \times k)$, và độ phức tạp không gian là $O(n + m + k)$. Ở đây, $n$ là độ dài của mảng $\textit{orders}$, còn $m$ và $k$ lần lượt là số loại món và số bàn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def displayTable(self, orders: List[List[str]]) -> List[List[str]]:
        tables = defaultdict(list)
        items = set()
        for _, table, foodItem in orders:
            tables[int(table)].append(foodItem)
            items.add(foodItem)
        sorted_items = sorted(items)
        ans = [["Table"] + sorted_items]
        for table in sorted(tables):
            cnt = Counter(tables[table])
            row = [str(table)] + [str(cnt[item]) for item in sorted_items]
            ans.append(row)
        return ans
```

#### Java

```java
class Solution {
    public List<List<String>> displayTable(List<List<String>> orders) {
        TreeMap<Integer, List<String>> tables = new TreeMap<>();
        Set<String> items = new HashSet<>();
        for (List<String> o : orders) {
            int table = Integer.parseInt(o.get(1));
            String foodItem = o.get(2);
            tables.computeIfAbsent(table, k -> new ArrayList<>()).add(foodItem);
            items.add(foodItem);
        }
        List<String> sortedItems = new ArrayList<>(items);
        Collections.sort(sortedItems);
        List<List<String>> ans = new ArrayList<>();
        List<String> header = new ArrayList<>();
        header.add("Table");
        header.addAll(sortedItems);
        ans.add(header);
        for (Map.Entry<Integer, List<String>> entry : tables.entrySet()) {
            Map<String, Integer> cnt = new HashMap<>();
            for (String item : entry.getValue()) {
                cnt.merge(item, 1, Integer::sum);
            }
            List<String> row = new ArrayList<>();
            row.add(String.valueOf(entry.getKey()));
            for (String item : sortedItems) {
                row.add(String.valueOf(cnt.getOrDefault(item, 0)));
            }
            ans.add(row);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<string>> displayTable(vector<vector<string>>& orders) {
        map<int, vector<string>> tables;
        set<string> sortedItems;
        for (auto& o : orders) {
            int table = stoi(o[1]);
            string foodItem = o[2];
            tables[table].push_back(foodItem);
            sortedItems.insert(foodItem);
        }
        vector<vector<string>> ans;
        vector<string> header = {"Table"};
        header.insert(header.end(), sortedItems.begin(), sortedItems.end());
        ans.push_back(header);
        for (auto& [table, items] : tables) {
            unordered_map<string, int> cnt;
            for (string& item : items) {
                cnt[item]++;
            }
            vector<string> row;
            row.push_back(to_string(table));
            for (const string& item : sortedItems) {
                row.push_back(to_string(cnt[item]));
            }
            ans.push_back(row);
        }
        return ans;
    }
};
```

#### Go

```go
func displayTable(orders [][]string) [][]string {
	tables := make(map[int]map[string]int)
	items := make(map[string]bool)
	for _, order := range orders {
		table, _ := strconv.Atoi(order[1])
		foodItem := order[2]
		if tables[table] == nil {
			tables[table] = make(map[string]int)
		}
		tables[table][foodItem]++
		items[foodItem] = true
	}
	sortedItems := make([]string, 0, len(items))
	for item := range items {
		sortedItems = append(sortedItems, item)
	}
	sort.Strings(sortedItems)
	ans := [][]string{}
	header := append([]string{"Table"}, sortedItems...)
	ans = append(ans, header)
	tableNums := make([]int, 0, len(tables))
	for table := range tables {
		tableNums = append(tableNums, table)
	}
	sort.Ints(tableNums)
	for _, table := range tableNums {
		row := []string{strconv.Itoa(table)}
		for _, item := range sortedItems {
			count := tables[table][item]
			row = append(row, strconv.Itoa(count))
		}
		ans = append(ans, row)
	}
	return ans
}
```

#### TypeScript

```ts
function displayTable(orders: string[][]): string[][] {
    const tables: Record<number, Record<string, number>> = {};
    const items: Set<string> = new Set();
    for (const [_, table, foodItem] of orders) {
        const t = +table;
        if (!tables[t]) {
            tables[t] = {};
        }
        if (!tables[t][foodItem]) {
            tables[t][foodItem] = 0;
        }
        tables[t][foodItem]++;
        items.add(foodItem);
    }
    const sortedItems = Array.from(items).sort();
    const ans: string[][] = [];
    const header: string[] = ['Table', ...sortedItems];
    ans.push(header);
    const sortedTableNumbers = Object.keys(tables)
        .map(Number)
        .sort((a, b) => a - b);
    for (const table of sortedTableNumbers) {
        const row: string[] = [table.toString()];
        for (const item of sortedItems) {
            row.push((tables[table][item] || 0).toString());
        }
        ans.push(row);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
