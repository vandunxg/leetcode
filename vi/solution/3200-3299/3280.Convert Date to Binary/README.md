---
comments: true
difficulty: Easy
rating: 1205
source: Weekly Contest 414 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [3280. Convert Date to Binary](https://leetcode.com/problems/convert-date-to-binary)

[中文文档](/solution/3200-3299/3280.Convert%20Date%20to%20Binary/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>date</code> biểu diễn một ngày theo lịch Gregory dưới định dạng <code>yyyy-mm-dd</code>.</p>

<p>Có thể viết <code>date</code> dưới dạng biểu diễn nhị phân bằng cách chuyển năm, tháng và ngày sang dạng nhị phân tương ứng, không có các số 0 ở đầu, rồi viết chúng theo định dạng <code>year-month-day</code>.</p>

<p>Hãy trả về biểu diễn <strong>nhị phân</strong> của <code>date</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">date = &quot;2080-02-29&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;100000100000-10-11101&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><span class="example-io">100000100000, 10 và 11101 lần lượt là biểu diễn nhị phân của 2080, 02 và 29.</span></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">date = &quot;1900-01-01&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;11101101100-1-1&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p><span class="example-io">11101101100, 1 và 1 lần lượt là biểu diễn nhị phân của 1900, 1 và 1.</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>date.length == 10</code></li>
	<li><code>date[4] == date[7] == &#39;-&#39;</code>, và mọi <code>date[i]</code> còn lại đều là chữ số.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>date</code> biểu diễn một ngày Gregory hợp lệ trong khoảng từ ngày 1<sup>st</sup> tháng 1 năm 1900 đến ngày 31<sup>st</sup> tháng 12 năm 2100 (bao gồm cả hai ngày).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Thay từng phần của `yyyy-mm-dd` bằng biểu diễn nhị phân tương ứng. Ngày tháng hợp lệ và có định dạng cố định, nên chỉ cần tách theo `-` rồi định dạng lại.
>
> Chuyển từng phần sang `int`, xuất dạng nhị phân và nối chúng bằng `-`. Không cần thực hiện phép tính lịch nào khác.

<!-- thinking:end -->

Trước hết, chúng ta tách chuỗi $\textit{date}$ theo `-`, sau đó chuyển từng phần sang biểu diễn nhị phân và cuối cùng nối ba phần này bằng `-`.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $\textit{date}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def convertDateToBinary(self, date: str) -> str:
        return "-".join(f"{int(s):b}" for s in date.split("-"))
```

#### Java

```java
class Solution {
    public String convertDateToBinary(String date) {
        List<String> ans = new ArrayList<>();
        for (var s : date.split("-")) {
            int x = Integer.parseInt(s);
            ans.add(Integer.toBinaryString(x));
        }
        return String.join("-", ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string convertDateToBinary(string date) {
        auto bin = [](string s) -> string {
            string t = bitset<32>(stoi(s)).to_string();
            return t.substr(t.find('1'));
        };
        return bin(date.substr(0, 4)) + "-" + bin(date.substr(5, 2)) + "-" + bin(date.substr(8, 2));
    }
};
```

#### Go

```go
func convertDateToBinary(date string) string {
	ans := []string{}
	for _, s := range strings.Split(date, "-") {
		x, _ := strconv.Atoi(s)
		ans = append(ans, strconv.FormatUint(uint64(x), 2))
	}
	return strings.Join(ans, "-")
}
```

#### TypeScript

```ts
function convertDateToBinary(date: string): string {
    return date
        .split('-')
        .map(s => (+s).toString(2))
        .join('-');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
