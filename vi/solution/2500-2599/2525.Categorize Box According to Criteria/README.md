---
comments: true
difficulty: Easy
rating: 1301
source: Biweekly Contest 95 Q1
tags:
    - Math
---

<!-- problem:start -->

# [2525. Categorize Box According to Criteria](https://leetcode.com/problems/categorize-box-according-to-criteria)

[中文文档](/solution/2500-2599/2525.Categorize%20Box%20According%20to%20Criteria/README.md)

## Mô tả

<!-- description:start -->

<p>Cho bốn số nguyên <code>length</code>, <code>width</code>, <code>height</code> và <code>mass</code>, lần lượt biểu diễn các kích thước và khối lượng của một hộp, hãy trả về <em>một chuỗi biểu diễn <strong>phân loại</strong> của hộp</em>.</p>

<ul>
	<li>Hộp được gọi là <code>&quot;Bulky&quot;</code> nếu:

    <ul>
    <li><strong>Bất kỳ</strong> kích thước nào của hộp lớn hơn hoặc bằng <code>10<sup>4</sup></code>.</li>
    <li>Hoặc, <strong>thể tích</strong> của hộp lớn hơn hoặc bằng <code>10<sup>9</sup></code>.</li>
    </ul>
    </li>
    <li>Nếu khối lượng của hộp lớn hơn hoặc bằng <code>100</code>, hộp được gọi là <code>&quot;Heavy&quot;.</code></li>
    <li>Nếu hộp vừa <code>&quot;Bulky&quot;</code> vừa <code>&quot;Heavy&quot;</code>, phân loại của hộp là <code>&quot;Both&quot;</code>.</li>
    <li>Nếu hộp không <code>&quot;Bulky&quot;</code> cũng không <code>&quot;Heavy&quot;</code>, phân loại của hộp là <code>&quot;Neither&quot;</code>.</li>
    <li>Nếu hộp là <code>&quot;Bulky&quot;</code> nhưng không <code>&quot;Heavy&quot;</code>, phân loại của hộp là <code>&quot;Bulky&quot;</code>.</li>
    <li>Nếu hộp là <code>&quot;Heavy&quot;</code> nhưng không <code>&quot;Bulky&quot;</code>, phân loại của hộp là <code>&quot;Heavy&quot;</code>.</li>

</ul>

<p><strong>Lưu ý</strong> rằng thể tích của hộp là tích của length, width và height.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> length = 1000, width = 35, height = 700, mass = 300
<strong>Đầu ra:</strong> &quot;Heavy&quot;
<strong>Giải thích:</strong>
Không có kích thước nào của hộp lớn hơn hoặc bằng 10<sup>4</sup>.
Thể tích của hộp = 24500000 &lt;= 10<sup>9</sup>. Vì vậy, hộp không thể được phân loại là &quot;Bulky&quot;.
Tuy nhiên, khối lượng &gt;= 100 nên hộp là &quot;Heavy&quot;.
Vì hộp không &quot;Bulky&quot; nhưng &quot;Heavy&quot;, ta trả về &quot;Heavy&quot;.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> length = 200, width = 50, height = 800, mass = 50
<strong>Đầu ra:</strong> &quot;Neither&quot;
<strong>Giải thích:</strong>
Không có kích thước nào của hộp lớn hơn hoặc bằng 10<sup>4</sup>.
Thể tích của hộp = 8 * 10<sup>6</sup> &lt;= 10<sup>9</sup>. Vì vậy, hộp không thể được phân loại là &quot;Bulky&quot;.
Khối lượng của hộp cũng nhỏ hơn 100, nên hộp không thể được phân loại là &quot;Heavy&quot;.
Vì hộp không thuộc hai phân loại trên, ta trả về &quot;Neither&quot;.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= length, width, height &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= mass &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Một hộp được phân loại dựa trên việc nó có cồng kềnh theo kích thước hoặc thể tích hay không, và có nặng theo khối lượng hay không — tổng cộng có bốn nhãn. Mỗi phép kiểm tra đều có thời gian hằng số.
>
> Lưu $\textit{bulky}$ và $\textit{heavy}$ dưới dạng $0/1$, rồi dùng $[\textit{Neither},\textit{Bulky},\textit{Heavy},\textit{Both}]$ làm bảng chỉ số theo $\textit{heavy}\ll 1\mid \textit{bulky}$ để tránh các nhánh lồng nhau.

<!-- thinking:end -->

Ta có thể mô phỏng theo mô tả của đề bài.

Độ phức tạp thời gian là $O(1)$, còn độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def categorizeBox(self, length: int, width: int, height: int, mass: int) -> str:
        v = length * width * height
        bulky = int(any(x >= 10000 for x in (length, width, height)) or v >= 10**9)
        heavy = int(mass >= 100)
        i = heavy << 1 | bulky
        d = ['Neither', 'Bulky', 'Heavy', 'Both']
        return d[i]
```

#### Java

```java
class Solution {
    public String categorizeBox(int length, int width, int height, int mass) {
        long v = (long) length * width * height;
        int bulky = length >= 10000 || width >= 10000 || height >= 10000 || v >= 1000000000 ? 1 : 0;
        int heavy = mass >= 100 ? 1 : 0;
        String[] d = {"Neither", "Bulky", "Heavy", "Both"};
        int i = heavy << 1 | bulky;
        return d[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    string categorizeBox(int length, int width, int height, int mass) {
        long v = (long) length * width * height;
        int bulky = length >= 10000 || width >= 10000 || height >= 10000 || v >= 1000000000 ? 1 : 0;
        int heavy = mass >= 100 ? 1 : 0;
        string d[4] = {"Neither", "Bulky", "Heavy", "Both"};
        int i = heavy << 1 | bulky;
        return d[i];
    }
};
```

#### Go

```go
func categorizeBox(length int, width int, height int, mass int) string {
	v := length * width * height
	i := 0
	if length >= 10000 || width >= 10000 || height >= 10000 || v >= 1000000000 {
		i |= 1
	}
	if mass >= 100 {
		i |= 2
	}
	d := [4]string{"Neither", "Bulky", "Heavy", "Both"}
	return d[i]
}
```

#### TypeScript

```ts
function categorizeBox(length: number, width: number, height: number, mass: number): string {
    const v = length * width * height;
    let i = 0;
    if (length >= 10000 || width >= 10000 || height >= 10000 || v >= 1000000000) {
        i |= 1;
    }
    if (mass >= 100) {
        i |= 2;
    }
    return ['Neither', 'Bulky', 'Heavy', 'Both'][i];
}
```

#### Rust

```rust
impl Solution {
    pub fn categorize_box(length: i32, width: i32, height: i32, mass: i32) -> String {
        let v = (length as i64) * (width as i64) * (height as i64);
        let mut i = 0;

        if length >= 10000 || width >= 10000 || height >= 10000 || v >= 1000000000 {
            i |= 1;
        }

        if mass >= 100 {
            i |= 2;
        }

        let d = vec!["Neither", "Bulky", "Heavy", "Both"];
        d[i].to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 gói hai cờ vào một chỉ số. Bốn trường hợp tương tự cũng có thể được viết thành các điều kiện tuần tự — cả hai, chỉ cồng kềnh, chỉ nặng, không trường hợp nào — với kết quả giống hệt nhau.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def categorizeBox(self, length: int, width: int, height: int, mass: int) -> str:
        v = length * width * height
        bulky = any(x >= 10000 for x in (length, width, height)) or v >= 10**9
        heavy = mass >= 100

        if bulky and heavy:
            return "Both"
        if bulky:
            return "Bulky"
        if heavy:
            return "Heavy"

        return "Neither"
```

#### Java

```java
class Solution {
    public String categorizeBox(int length, int width, int height, int mass) {
        long v = (long) length * width * height;
        boolean bulky = length >= 1e4 || width >= 1e4 || height >= 1e4 || v >= 1e9;
        boolean heavy = mass >= 100;

        if (bulky && heavy) {
            return "Both";
        }
        if (bulky) {
            return "Bulky";
        }
        if (heavy) {
            return "Heavy";
        }

        return "Neither";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string categorizeBox(int length, int width, int height, int mass) {
        long v = (long) length * width * height;
        bool bulky = length >= 1e4 || width >= 1e4 || height >= 1e4 || v >= 1e9;
        bool heavy = mass >= 100;

        if (bulky && heavy) {
            return "Both";
        }
        if (bulky) {
            return "Bulky";
        }
        if (heavy) {
            return "Heavy";
        }

        return "Neither";
    }
};
```

#### Go

```go
func categorizeBox(length int, width int, height int, mass int) string {
	v := length * width * height
	bulky := length >= 1e4 || width >= 1e4 || height >= 1e4 || v >= 1e9
	heavy := mass >= 100
	if bulky && heavy {
		return "Both"
	}
	if bulky {
		return "Bulky"
	}
	if heavy {
		return "Heavy"
	}
	return "Neither"
}
```

#### TypeScript

```ts
function categorizeBox(length: number, width: number, height: number, mass: number): string {
    const v = length * width * height;
    const bulky = length >= 1e4 || width >= 1e4 || height >= 1e4 || v >= 1e9;
    const heavy = mass >= 100;
    if (bulky && heavy) {
        return 'Both';
    }
    if (bulky) {
        return 'Bulky';
    }
    if (heavy) {
        return 'Heavy';
    }
    return 'Neither';
}
```

#### Rust

```rust
impl Solution {
    pub fn categorize_box(length: i32, width: i32, height: i32, mass: i32) -> String {
        let v = length * width * height;
        let bulky = length >= 10000
            || width >= 10000
            || height >= 10000
            || (length as i64) * (width as i64) * (height as i64) >= 1000000000;

        let heavy = mass >= 100;

        if bulky && heavy {
            return "Both".to_string();
        }
        if bulky {
            return "Bulky".to_string();
        }
        if heavy {
            return "Heavy".to_string();
        }

        "Neither".to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
