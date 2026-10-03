---
comments: true
difficulty: Medium
tags:
    - Design
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [2408. Design SQL](https://leetcode.com/problems/design-sql)

[Tài liệu tiếng Trung](/solution/2400-2499/2408.Design%20SQL/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>names</code> và <code>columns</code>, cả hai đều có kích thước <code>n</code>. Bảng thứ <code>i<sup>th</sup></code> được biểu diễn bởi tên <code>names[i]</code> và chứa số cột bằng giá trị <code>columns[i]</code>.</p>

<p>Bạn cần cài đặt một class hỗ trợ các <strong>thao tác</strong> sau:</p>

<ul>
	<li><strong>Chèn</strong> một hàng vào một bảng cụ thể với id được gán bằng phương thức <em>auto-increment</em>, trong đó id của hàng đầu tiên được chèn là 1, còn id của mỗi hàng <em>mới </em>được chèn vào cùng bảng sẽ <strong>lớn hơn một đơn vị</strong> so với id của hàng <strong>được chèn gần nhất</strong>, ngay cả khi hàng cuối cùng đã bị <em>xóa</em>.</li>
	<li><strong>Xóa</strong> một hàng khỏi một bảng cụ thể. Việc xóa một hàng <strong>không</strong> ảnh hưởng đến id của hàng được chèn tiếp theo.</li>
	<li><strong>Chọn</strong> một ô cụ thể từ bất kỳ bảng nào và trả về giá trị của ô đó.</li>
	<li><strong>Xuất</strong> tất cả các hàng từ bất kỳ bảng nào theo định dạng csv.</li>
</ul>

<p>Cài đặt class <code>SQL</code>:</p>

<ul>
	<li><code>SQL(String[] names, int[] columns)</code>

    <ul>
      <li>Tạo <code>n</code> bảng.</li>
    </ul>
    </li>
    <li><code>bool ins(String name, String[] row)</code>
    <ul>
      <li>Chèn <code>row</code> vào bảng <code>name</code> và trả về <code>true</code>.</li>
      <li>Nếu <code>row.length</code> <strong>không</strong> khớp với số cột cần có, hoặc <code>name</code> <strong>không</strong> phải là một bảng hợp lệ, trả về <code>false</code> mà không thực hiện chèn.</li>
    </ul>
    </li>
    <li><code>void rmv(String name, int rowId)</code>
    <ul>
      <li>Xóa hàng <code>rowId</code> khỏi bảng <code>name</code>.</li>
      <li>Nếu <code>name</code> <strong>không</strong> phải là một bảng hợp lệ hoặc không có hàng nào có id <code>rowId</code>, không thực hiện việc xóa.</li>
    </ul>
    </li>
    <li><code>String sel(String name, int rowId, int columnId)</code>
    <ul>
      <li>Trả về giá trị của ô tại <code>rowId</code> và <code>columnId</code> được chỉ định trong bảng <code>name</code>.</li>
      <li>Nếu <code>name</code> <strong>không</strong> phải là một bảng hợp lệ, hoặc ô <code>(rowId, columnId)</code> <strong>không hợp lệ</strong>, trả về <code>&quot;&lt;null&gt;&quot;</code>.</li>
    </ul>
    </li>
    <li><code>String[] exp(String name)</code>
    <ul>
      <li>Trả về các hàng hiện có trong bảng <code>name</code>.</li>
      <li>Nếu name <strong>không</strong> phải là một bảng hợp lệ, trả về một mảng rỗng. Mỗi hàng được biểu diễn dưới dạng một chuỗi, trong đó giá trị của mỗi ô (<strong>bao gồm</strong> cả id của hàng) được phân tách bằng <code>&quot;,&quot;</code>.</li>
    </ul>
    </li>

</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<pre class="example-io">
[&quot;SQL&quot;,&quot;ins&quot;,&quot;sel&quot;,&quot;ins&quot;,&quot;exp&quot;,&quot;rmv&quot;,&quot;sel&quot;,&quot;exp&quot;]
[[[&quot;one&quot;,&quot;two&quot;,&quot;three&quot;],[2,3,1]],[&quot;two&quot;,[&quot;first&quot;,&quot;second&quot;,&quot;third&quot;]],[&quot;two&quot;,1,3],[&quot;two&quot;,[&quot;fourth&quot;,&quot;fifth&quot;,&quot;sixth&quot;]],[&quot;two&quot;],[&quot;two&quot;,1],[&quot;two&quot;,2,2],[&quot;two&quot;]]
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
[null,true,&quot;third&quot;,true,[&quot;1,first,second,third&quot;,&quot;2,fourth,fifth,sixth&quot;],null,&quot;fifth&quot;,[&quot;2,fourth,fifth,sixth&quot;]]</pre>

<p><strong>Giải thích:</strong></p>

<pre class="example-io">
// Creates three tables.
SQL sql = new SQL([&quot;one&quot;, &quot;two&quot;, &quot;three&quot;], [2, 3, 1]);

// Adds a row to the table &quot;two&quot; with id 1. Returns True.
sql.ins(&quot;two&quot;, [&quot;first&quot;, &quot;second&quot;, &quot;third&quot;]);

// Returns the value &quot;third&quot; from the third column
// in the row with id 1 of the table &quot;two&quot;.
sql.sel(&quot;two&quot;, 1, 3);

// Adds another row to the table &quot;two&quot; with id 2. Returns True.
sql.ins(&quot;two&quot;, [&quot;fourth&quot;, &quot;fifth&quot;, &quot;sixth&quot;]);

// Exports the rows of the table &quot;two&quot;.
// Currently, the table has 2 rows with ids 1 and 2.
sql.exp(&quot;two&quot;);

// Removes the first row of the table &quot;two&quot;. Note that the second row
// will still have the id 2.
sql.rmv(&quot;two&quot;, 1);

// Returns the value &quot;fifth&quot; from the second column
// in the row with id 2 of the table &quot;two&quot;.
sql.sel(&quot;two&quot;, 2, 2);

// Exports the rows of the table &quot;two&quot;.
// Currently, the table has 1 row with id 2.
sql.exp(&quot;two&quot;);
</pre>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong></p>

<pre class="example-io">
[&quot;SQL&quot;,&quot;ins&quot;,&quot;sel&quot;,&quot;rmv&quot;,&quot;sel&quot;,&quot;ins&quot;,&quot;ins&quot;]
[[[&quot;one&quot;,&quot;two&quot;,&quot;three&quot;],[2,3,1]],[&quot;two&quot;,[&quot;first&quot;,&quot;second&quot;,&quot;third&quot;]],[&quot;two&quot;,1,3],[&quot;two&quot;,1],[&quot;two&quot;,1,2],[&quot;two&quot;,[&quot;fourth&quot;,&quot;fifth&quot;]],[&quot;two&quot;,[&quot;fourth&quot;,&quot;fifth&quot;,&quot;sixth&quot;]]]
</pre>

<p><strong>Đầu ra:</strong></p>

<pre class="example-io">
[null,true,&quot;third&quot;,null,&quot;&lt;null&gt;&quot;,false,true]
</pre>

<p><strong>Giải thích:</strong></p>

<pre class="example-io">
// Creates three tables.
SQL sQL = new SQL([&quot;one&quot;, &quot;two&quot;, &quot;three&quot;], [2, 3, 1]);

// Adds a row to the table &quot;two&quot; with id 1. Returns True.
sQL.ins(&quot;two&quot;, [&quot;first&quot;, &quot;second&quot;, &quot;third&quot;]);

// Returns the value &quot;third&quot; from the third column
// in the row with id 1 of the table &quot;two&quot;.
sQL.sel(&quot;two&quot;, 1, 3);

// Removes the first row of the table &quot;two&quot;.
sQL.rmv(&quot;two&quot;, 1);

// Returns &quot;&lt;null&gt;&quot; as the cell with id 1
// has been removed from table &quot;two&quot;.
sQL.sel(&quot;two&quot;, 1, 2);

// Returns False as number of columns are not correct.
sQL.ins(&quot;two&quot;, [&quot;fourth&quot;, &quot;fifth&quot;]);

// Adds a row to the table &quot;two&quot; with id 2. Returns True.
sQL.ins(&quot;two&quot;, [&quot;fourth&quot;, &quot;fifth&quot;, &quot;sixth&quot;]);
</pre>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == names.length == columns.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= names[i].length, row[i].length, name.length &lt;= 10</code></li>
	<li><code>names[i]</code>, <code>row[i]</code> và <code>name</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
	<li><code>1 &lt;= columns[i] &lt;= 10</code></li>
	<li><code>1 &lt;= row.length &lt;= 10</code></li>
	<li>Tất cả <code>names[i]</code> đều <strong>khác nhau</strong>.</li>
	<li>Sẽ có nhiều nhất <code>2000</code> lời gọi đến <code>ins</code> và <code>rmv</code>.</li>
	<li>Sẽ có nhiều nhất <code>10<sup>4</sup></code> lời gọi đến <code>sel</code>.</li>
	<li>Sẽ có nhiều nhất <code>500</code> lời gọi đến <code>exp</code>.</li>
</ul>

<p>&nbsp;</p>
<strong>Câu hỏi mở rộng:</strong> Bạn sẽ chọn cách tiếp cận nào nếu bảng có thể trở nên thưa do có nhiều lần xóa, và tại sao? Hãy cân nhắc ảnh hưởng đến mức sử dụng bộ nhớ và hiệu năng.

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Chúng ta cần chèn các hàng theo tên bảng và đọc các ô theo hàng và cột bắt đầu từ $1$. Số lượng thao tác bị giới hạn, nên không cần đến một relational engine đầy đủ.
>
> Một hash map ánh xạ tên bảng đến danh sách các hàng là đủ: $rowId$ dùng để truy cập phần tử ở vị trí $rowId-1$. Các hàng đã xóa không bao giờ được chọn, nên $\textit{deleteRow}$ có thể không làm gì.

<!-- thinking:end -->

Tạo một hash map `tables` để lưu ánh xạ từ tên bảng đến dữ liệu các hàng của bảng. Mô phỏng trực tiếp các thao tác trong đề bài.

Độ phức tạp thời gian của mỗi thao tác là $O(1)$, còn độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class SQL:
    def __init__(self, names: List[str], columns: List[int]):
        self.tables = defaultdict(list)

    def insertRow(self, name: str, row: List[str]) -> None:
        self.tables[name].append(row)

    def deleteRow(self, name: str, rowId: int) -> None:
        pass

    def selectCell(self, name: str, rowId: int, columnId: int) -> str:
        return self.tables[name][rowId - 1][columnId - 1]


# Your SQL object will be instantiated and called as such:
# obj = SQL(names, columns)
# obj.insertRow(name,row)
# obj.deleteRow(name,rowId)
# param_3 = obj.selectCell(name,rowId,columnId)
```

#### Java

```java
class SQL {
    private Map<String, List<List<String>>> tables;

    public SQL(List<String> names, List<Integer> columns) {
        tables = new HashMap<>(names.size());
    }

    public void insertRow(String name, List<String> row) {
        tables.computeIfAbsent(name, k -> new ArrayList<>()).add(row);
    }

    public void deleteRow(String name, int rowId) {
    }

    public String selectCell(String name, int rowId, int columnId) {
        return tables.get(name).get(rowId - 1).get(columnId - 1);
    }
}

/**
 * Your SQL object will be instantiated and called as such:
 * SQL obj = new SQL(names, columns);
 * obj.insertRow(name,row);
 * obj.deleteRow(name,rowId);
 * String param_3 = obj.selectCell(name,rowId,columnId);
 */
```

#### C++

```cpp
class SQL {
public:
    unordered_map<string, vector<vector<string>>> tables;
    SQL(vector<string>& names, vector<int>& columns) {
    }

    void insertRow(string name, vector<string> row) {
        tables[name].push_back(row);
    }

    void deleteRow(string name, int rowId) {
    }

    string selectCell(string name, int rowId, int columnId) {
        return tables[name][rowId - 1][columnId - 1];
    }
};

/**
 * Your SQL object will be instantiated and called as such:
 * SQL* obj = new SQL(names, columns);
 * obj->insertRow(name,row);
 * obj->deleteRow(name,rowId);
 * string param_3 = obj->selectCell(name,rowId,columnId);
 */
```

#### Go

```go
type SQL struct {
	tables map[string][][]string
}

func Constructor(names []string, columns []int) SQL {
	return SQL{map[string][][]string{}}
}

func (this *SQL) InsertRow(name string, row []string) {
	this.tables[name] = append(this.tables[name], row)
}

func (this *SQL) DeleteRow(name string, rowId int) {

}

func (this *SQL) SelectCell(name string, rowId int, columnId int) string {
	return this.tables[name][rowId-1][columnId-1]
}

/**
 * Your SQL object will be instantiated and called as such:
 * obj := Constructor(names, columns);
 * obj.InsertRow(name,row);
 * obj.DeleteRow(name,rowId);
 * param_3 := obj.SelectCell(name,rowId,columnId);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
