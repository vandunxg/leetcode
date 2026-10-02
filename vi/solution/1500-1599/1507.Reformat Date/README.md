---
comments: true
difficulty: Easy
rating: 1283
source: Biweekly Contest 30 Q1
tags:
    - String
---

<!-- problem:start -->

# [1507. Reformat Date](https://leetcode.com/problems/reformat-date)

[中文文档](/solution/1500-1599/1507.Reformat%20Date/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>date</code> có dạng&nbsp;<code>Day Month Year</code>, trong đó:</p>

<ul>
	<li><code>Day</code>&nbsp;thuộc tập <code>{&quot;1st&quot;, &quot;2nd&quot;, &quot;3rd&quot;, &quot;4th&quot;, ..., &quot;30th&quot;, &quot;31st&quot;}</code>.</li>
	<li><code>Month</code>&nbsp;thuộc tập <code>{&quot;Jan&quot;, &quot;Feb&quot;, &quot;Mar&quot;, &quot;Apr&quot;, &quot;May&quot;, &quot;Jun&quot;, &quot;Jul&quot;, &quot;Aug&quot;, &quot;Sep&quot;, &quot;Oct&quot;, &quot;Nov&quot;, &quot;Dec&quot;}</code>.</li>
	<li><code>Year</code>&nbsp;thuộc khoảng <code>[1900, 2100]</code>.</li>
</ul>

<p>Hãy chuyển chuỗi ngày sang định dạng <code>YYYY-MM-DD</code>, trong đó:</p>

<ul>
	<li><code>YYYY</code> biểu diễn năm gồm 4 chữ số.</li>
	<li><code>MM</code> biểu diễn tháng gồm 2 chữ số.</li>
	<li><code>DD</code> biểu diễn ngày gồm 2 chữ số.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> date = &quot;20th Oct 2052&quot;
<strong>Đầu ra:</strong> &quot;2052-10-20&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> date = &quot;6th Jun 1933&quot;
<strong>Đầu ra:</strong> &quot;1933-06-06&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> date = &quot;26th May 1960&quot;
<strong>Đầu ra:</strong> &quot;1960-05-26&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Các ngày được cho luôn hợp lệ, vì vậy không cần xử lý lỗi.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Đầu vào luôn có dạng “day month year” và đầu ra có dạng $YYYY$-$MM$-$DD$. Không cần tìm kiếm hay dùng DP; ta chỉ cần viết lại ba trường này.
>
> Tách chuỗi theo dấu cách rồi đảo ngược để năm đứng đầu. Tìm tháng trong một chuỗi gồm các viết tắt tháng; chỉ số chia cho $3$, cộng thêm một, chính là số tháng. Loại bỏ hậu tố của ngày và thêm số 0 vào đầu tháng cũng như ngày khi cần. Ghép các phần bằng dấu gạch nối sẽ thu được ngày theo định dạng chuẩn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reformatDate(self, date: str) -> str:
        s = date.split()
        s.reverse()
        months = " JanFebMarAprMayJunJulAugSepOctNovDec"
        s[1] = str(months.index(s[1]) // 3 + 1).zfill(2)
        s[2] = s[2][:-2].zfill(2)
        return "-".join(s)
```

#### Java

```java
class Solution {
    public String reformatDate(String date) {
        var s = date.split(" ");
        String months = " JanFebMarAprMayJunJulAugSepOctNovDec";
        int day = Integer.parseInt(s[0].substring(0, s[0].length() - 2));
        int month = months.indexOf(s[1]) / 3 + 1;
        return String.format("%s-%02d-%02d", s[2], month, day);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string reformatDate(string date) {
        string months = " JanFebMarAprMayJunJulAugSepOctNovDec";
        stringstream ss(date);
        string year, month, t;
        int day;
        ss >> day >> t >> month >> year;
        month = to_string(months.find(month) / 3 + 1);
        return year + "-" + (month.size() == 1 ? "0" + month : month) + "-" + (day > 9 ? "" : "0") + to_string(day);
    }
};
```

#### Go

```go
func reformatDate(date string) string {
	s := strings.Split(date, " ")
	day, _ := strconv.Atoi(s[0][:len(s[0])-2])
	months := " JanFebMarAprMayJunJulAugSepOctNovDec"
	month := strings.Index(months, s[1])/3 + 1
	year, _ := strconv.Atoi(s[2])
	return fmt.Sprintf("%d-%02d-%02d", year, month, day)
}
```

#### TypeScript

```ts
function reformatDate(date: string): string {
    const s = date.split(' ');
    const months = ' JanFebMarAprMayJunJulAugSepOctNovDec';
    const day = parseInt(s[0].substring(0, s[0].length - 2));
    const month = Math.floor(months.indexOf(s[1]) / 3) + 1;
    return `${s[2]}-${month.toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`;
}
```

#### PHP

```php
class Solution {
    /**
     * @param String $date
     * @return String
     */
    function reformatDate($date) {
        $arr = explode(' ', $date);
        $months = [
            'Jan' => '01',
            'Feb' => '02',
            'Mar' => '03',
            'Apr' => '04',
            'May' => '05',
            'Jun' => '06',
            'Jul' => '07',
            'Aug' => '08',
            'Sep' => '09',
            'Oct' => '10',
            'Nov' => '11',
            'Dec' => '12',
        ];
        $year = $arr[2];
        $month = $months[$arr[1]];
        $day = intval($arr[0]);
        if ($day > 0 && $day < 10) {
            $day = '0' . $day;
        }
        return $year . '-' . $month . '-' . $day;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
