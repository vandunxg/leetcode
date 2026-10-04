---
comments: true
difficulty: Medium
rating: 1523
source: Biweekly Contest 152 Q2
tags:
    - Design
    - Array
    - Hash Table
    - String
    - Matrix
---

<!-- problem:start -->

# [3484. Design Spreadsheet](https://leetcode.com/problems/design-spreadsheet)

[中文文档](/solution/3400-3499/3484.Design%20Spreadsheet/README.md)

## Mô tả

<!-- description:start -->

<p>Một spreadsheet là một bảng gồm 26 cột (được gắn nhãn từ <code>&#39;A&#39;</code> đến <code>&#39;Z&#39;</code>) và số <code>rows</code> hàng cho trước. Mỗi ô trong spreadsheet có thể chứa một giá trị nguyên từ 0 đến 10<sup>5</sup>.</p>

<p>Hãy cài đặt class <code>Spreadsheet</code>:</p>

<ul>
    <li><code>Spreadsheet(int rows)</code> khởi tạo một spreadsheet có 26 cột (được gắn nhãn từ <code>&#39;A&#39;</code> đến <code>&#39;Z&#39;</code>) và số hàng được chỉ định. Ban đầu, tất cả các ô đều có giá trị 0.</li>
    <li><code>void setCell(String cell, int value)</code> đặt giá trị cho <code>cell</code> được chỉ định. Tham chiếu đến ô có dạng <code>&quot;AX&quot;</code> (ví dụ: <code>&quot;A1&quot;</code>, <code>&quot;B10&quot;</code>), trong đó chữ cái biểu thị cột (từ <code>&#39;A&#39;</code> đến <code>&#39;Z&#39;</code>) và chữ số biểu thị hàng được <strong>đánh chỉ số từ 1</strong>.</li>
    <li><code>void resetCell(String cell)</code> đặt lại ô được chỉ định về 0.</li>
    <li><code>int getValue(String formula)</code> tính giá trị của công thức có dạng <code>&quot;=X+Y&quot;</code>, trong đó <code>X</code> và <code>Y</code> <strong>có thể là</strong> tham chiếu đến ô hoặc số nguyên không âm, rồi trả về tổng tương ứng.</li>
</ul>

<p><strong>Lưu ý:</strong> Nếu <code>getValue</code> tham chiếu đến một ô chưa từng được đặt giá trị bằng <code>setCell</code>, giá trị của ô đó được xem là 0.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;Spreadsheet&quot;, &quot;getValue&quot;, &quot;setCell&quot;, &quot;getValue&quot;, &quot;setCell&quot;, &quot;getValue&quot;, &quot;resetCell&quot;, &quot;getValue&quot;]<br />
[[3], [&quot;=5+7&quot;], [&quot;A1&quot;, 10], [&quot;=A1+6&quot;], [&quot;B2&quot;, 15], [&quot;=A1+B2&quot;], [&quot;A1&quot;], [&quot;=A1+B2&quot;]]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, 12, null, 16, null, 25, null, 15] </span></p>

<p><strong>Giải thích</strong></p>
Spreadsheet spreadsheet = new Spreadsheet(3); // Initializes a spreadsheet with 3 rows and 26 columns<br data-end="321" data-start="318" />
spreadsheet.getValue(&quot;=5+7&quot;); // returns 12 (5+7)<br data-end="373" data-start="370" />
spreadsheet.setCell(&quot;A1&quot;, 10); // sets A1 to 10<br data-end="423" data-start="420" />
spreadsheet.getValue(&quot;=A1+6&quot;); // returns 16 (10+6)<br data-end="477" data-start="474" />
spreadsheet.setCell(&quot;B2&quot;, 15); // sets B2 to 15<br data-end="527" data-start="524" />
spreadsheet.getValue(&quot;=A1+B2&quot;); // returns 25 (10+15)<br data-end="583" data-start="580" />
spreadsheet.resetCell(&quot;A1&quot;); // resets A1 to 0<br data-end="634" data-start="631" />
spreadsheet.getValue(&quot;=A1+B2&quot;); // returns 15 (0+15)</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= rows &lt;= 10<sup>3</sup></code></li>
    <li><code>0 &lt;= value &lt;= 10<sup>5</sup></code></li>
    <li>Công thức luôn có dạng <code>&quot;=X+Y&quot;</code>, trong đó <code>X</code> và <code>Y</code> là tham chiếu đến ô hợp lệ hoặc số nguyên <strong>không âm</strong> có giá trị nhỏ hơn hoặc bằng <code>10<sup>5</sup></code>.</li>
    <li>Mỗi tham chiếu đến ô gồm một chữ cái viết hoa từ <code>&#39;A&#39;</code> đến <code>&#39;Z&#39;</code>, theo sau là số hàng từ <code>1</code> đến <code>rows</code>.</li>
    <li>Tổng số lần gọi đến <code>setCell</code>, <code>resetCell</code> và <code>getValue</code> <strong>nhiều nhất</strong> là <code>10<sup>4</sup></code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Các ô được định danh bằng tham chiếu; công thức luôn có dạng $=X+Y$, trong đó các toán hạng là tham chiếu hoặc số nguyên. Với $10^4$ lần gọi, một hash map là đủ; không cần lưu cả bảng đầy đủ.
>
> Một tham chiếu chưa tồn tại có giá trị $0$; thao tác reset sẽ xóa key tương ứng.
>
> $\textit{getValue}$ loại bỏ dấu bằng rồi tách theo dấu $+$: token là chữ số thì được phân tích thành số, còn tham chiếu thì dùng $\textit{d.get}(\textit{cell},0)$.

<!-- thinking:end -->

Ta dùng một hash table $\textit{d}$ để lưu giá trị của tất cả các ô, trong đó key là tham chiếu đến ô và value là giá trị của ô đó.

Khi gọi phương thức `setCell`, ta lưu tham chiếu đến ô và giá trị của nó vào hash table.

Khi gọi phương thức `resetCell`, ta xóa tham chiếu đến ô khỏi hash table.

Khi gọi phương thức `getValue`, ta tách công thức thành hai tham chiếu đến ô, tính giá trị của chúng rồi trả về tổng.

Độ phức tạp thời gian là $O(L)$, còn độ phức tạp không gian là $O(L)$. Trong đó $L$ là độ dài của công thức.

<!-- tabs:start -->

#### Python3

```python
class Spreadsheet:

    def __init__(self, rows: int):
        self.d = {}

    def setCell(self, cell: str, value: int) -> None:
        self.d[cell] = value

    def resetCell(self, cell: str) -> None:
        self.d.pop(cell, None)

    def getValue(self, formula: str) -> int:
        ans = 0
        for cell in formula[1:].split("+"):
            ans += int(cell) if cell[0].isdigit() else self.d.get(cell, 0)
        return ans


# Your Spreadsheet object will be instantiated and called as such:
# obj = Spreadsheet(rows)
# obj.setCell(cell,value)
# obj.resetCell(cell)
# param_3 = obj.getValue(formula)
```

#### Java

```java
class Spreadsheet {
    private Map<String, Integer> d = new HashMap<>();

    public Spreadsheet(int rows) {
    }

    public void setCell(String cell, int value) {
        d.put(cell, value);
    }

    public void resetCell(String cell) {
        d.remove(cell);
    }

    public int getValue(String formula) {
        int ans = 0;
        for (String cell : formula.substring(1).split("\\+")) {
            ans += Character.isDigit(cell.charAt(0)) ? Integer.parseInt(cell)
                                                     : d.getOrDefault(cell, 0);
        }
        return ans;
    }
}

/**
 * Your Spreadsheet object will be instantiated and called as such:
 * Spreadsheet obj = new Spreadsheet(rows);
 * obj.setCell(cell,value);
 * obj.resetCell(cell);
 * int param_3 = obj.getValue(formula);
 */
```

#### C++

```cpp
class Spreadsheet {
private:
    unordered_map<string, int> d;

public:
    Spreadsheet(int rows) {}

    void setCell(string cell, int value) {
        d[cell] = value;
    }

    void resetCell(string cell) {
        d.erase(cell);
    }

    int getValue(string formula) {
        int ans = 0;
        stringstream ss(formula.substr(1));
        string cell;
        while (getline(ss, cell, '+')) {
            if (isdigit(cell[0])) {
                ans += stoi(cell);
            } else {
                ans += d.count(cell) ? d[cell] : 0;
            }
        }
        return ans;
    }
};

/**
 * Your Spreadsheet object will be instantiated and called as such:
 * Spreadsheet* obj = new Spreadsheet(rows);
 * obj->setCell(cell,value);
 * obj->resetCell(cell);
 * int param_3 = obj->getValue(formula);
 */
```

#### Go

```go
type Spreadsheet struct {
    d map[string]int
}

func Constructor(rows int) Spreadsheet {
    return Spreadsheet{d: make(map[string]int)}
}

func (this *Spreadsheet) SetCell(cell string, value int) {
    this.d[cell] = value
}

func (this *Spreadsheet) ResetCell(cell string) {
    delete(this.d, cell)
}

func (this *Spreadsheet) GetValue(formula string) int {
    ans := 0
    cells := strings.Split(formula[1:], "+")
    for _, cell := range cells {
        if val, err := strconv.Atoi(cell); err == nil {
            ans += val
        } else {
            ans += this.d[cell]
        }
    }
    return ans
}


/**
 * Your Spreadsheet object will be instantiated and called as such:
 * obj := Constructor(rows);
 * obj.SetCell(cell,value);
 * obj.ResetCell(cell);
 * param_3 := obj.GetValue(formula);
 */
```

#### TypeScript

```ts
class Spreadsheet {
    private d: Map<string, number>;

    constructor(rows: number) {
        this.d = new Map<string, number>();
    }

    setCell(cell: string, value: number): void {
        this.d.set(cell, value);
    }

    resetCell(cell: string): void {
        this.d.delete(cell);
    }

    getValue(formula: string): number {
        let ans = 0;
        const cells = formula.slice(1).split('+');
        for (const cell of cells) {
            ans += isNaN(Number(cell)) ? this.d.get(cell) || 0 : Number(cell);
        }
        return ans;
    }
}

/**
 * Your Spreadsheet object will be instantiated and called as such:
 * var obj = new Spreadsheet(rows)
 * obj.setCell(cell,value)
 * obj.resetCell(cell)
 * var param_3 = obj.getValue(formula)
 */
```

#### Rust

```rust
use std::collections::HashMap;

struct Spreadsheet {
    d: HashMap<String, i32>,
}

impl Spreadsheet {
    fn new(_rows: i32) -> Self {
        Spreadsheet {
            d: HashMap::new(),
        }
    }

    fn set_cell(&mut self, cell: String, value: i32) {
        self.d.insert(cell, value);
    }

    fn reset_cell(&mut self, cell: String) {
        self.d.remove(&cell);
    }

    fn get_value(&self, formula: String) -> i32 {
        let mut ans = 0;
        for cell in formula[1..].split('+') {
            if cell.chars().next().unwrap().is_ascii_digit() {
                ans += cell.parse::<i32>().unwrap();
            } else {
                ans += *self.d.get(cell).unwrap_or(&0);
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Spreadsheet {
    private Dictionary<string, int> d = new Dictionary<string, int>();

    public Spreadsheet(int rows) {
    }

    public void SetCell(string cell, int value) {
        d[cell] = value;
    }

    public void ResetCell(string cell) {
        d.Remove(cell);
    }

    public int GetValue(string formula) {
        int ans = 0;
        foreach (string cell in formula.Substring(1).Split('+')) {
            ans += char.IsDigit(cell[0]) ? int.Parse(cell)
                                         : (d.ContainsKey(cell) ? d[cell] : 0);
        }
        return ans;
    }
}

/**
 * Your Spreadsheet object will be instantiated and called as such:
 * Spreadsheet obj = new Spreadsheet(rows);
 * obj.SetCell(cell,value);
 * obj.ResetCell(cell);
 * int param_3 = obj.GetValue(formula);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
