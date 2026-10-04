---
comments: true
difficulty: Medium
tags:
    - Hash Table
    - String
    - Counting
    - Sorting
---

<!-- problem:start -->

# [3167. Better Compression of String 🔒](https://leetcode.com/problems/better-compression-of-string)

[中文文档](/solution/3100-3199/3167.Better%20Compression%20of%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>compressed</code> biểu diễn phiên bản nén của một chuỗi. Định dạng gồm một ký tự theo sau bởi tần suất xuất hiện của ký tự đó. Ví dụ, <code>&quot;a3b1a1c2&quot;</code> là phiên bản nén của chuỗi <code>&quot;aaabacc&quot;</code>.</p>

<p>Chúng ta muốn có một <strong>phiên bản nén tốt hơn</strong> với các điều kiện sau:</p>

<ol>
    <li>Mỗi ký tự chỉ được xuất hiện <strong>một lần duy nhất</strong> trong phiên bản nén.</li>
    <li>Các ký tự phải được sắp xếp theo <strong>thứ tự bảng chữ cái</strong>.</li>
</ol>

<p>Hãy trả về <em>phiên bản nén tốt hơn</em> của <code>compressed</code>.</p>

<p><strong>Lưu ý:</strong> Trong phiên bản nén tốt hơn, thứ tự của các chữ cái có thể thay đổi, điều này được chấp nhận.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">compressed = &quot;a3c9b2c1&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a3b2c10&quot;</span></p>

<p><strong>Giải thích:</strong></p>

<p>Các ký tự &quot;a&quot; và &quot;b&quot; chỉ xuất hiện một lần trong đầu vào, nhưng &quot;c&quot; xuất hiện hai lần, một lần với số lượng là 9 và một lần với số lượng là 1.</p>

<p>Vì vậy, trong chuỗi kết quả, số lượng của nó phải là 10.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">compressed = &quot;c2b3a1&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a1b3c2&quot;</span></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">compressed = &quot;a2b4c1&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;a2b4c1&quot;</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= compressed.length &lt;= 6 * 10<sup>4</sup></code></li>
    <li><code>compressed</code> chỉ gồm các chữ cái tiếng Anh viết thường và chữ số.</li>
    <li><code>compressed</code> là một phiên bản nén hợp lệ, tức là mỗi ký tự đều được theo sau bởi tần suất của nó.</li>
    <li>Tần suất nằm trong khoảng <code>[1, 10<sup>4</sup>]</code> và không có các số 0 ở đầu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Chuỗi gồm các đoạn một chữ cái đi kèm với một số đếm ở hệ thập phân, và các đoạn này phải được gộp theo thứ tự bảng chữ cái. Việc chèn các đoạn vào một danh sách đã sắp xếp khá bất tiện.
>
> Chỉ có $26$ chữ cái có thể xuất hiện, nên ta có thể cộng dồn các số đếm rồi xuất chúng theo thứ tự của khóa. Hai con trỏ dùng để phân tích từng số.
>
> Chỉ số $i$ đứng tại một chữ cái, $j$ đọc các chữ số để tạo thành $cnt$, và đáp án nối các cặp $k+v$ đã được sắp xếp.

<!-- thinking:end -->

Ta có thể sử dụng một bảng băm để đếm tần suất của từng ký tự, sau đó dùng hai con trỏ để duyệt chuỗi `compressed` và cộng tần suất của mỗi ký tự vào bảng băm. Cuối cùng, ta nối các ký tự và tần suất thành một chuỗi theo thứ tự bảng chữ cái.

Độ phức tạp thời gian là $O(n + |\Sigma| \log |\Sigma|)$, còn độ phức tạp không gian là $O(|\Sigma|)$. Trong đó, $n$ là độ dài của chuỗi `compressed`, còn $|\Sigma|$ là kích thước của tập ký tự. Ở đây, tập ký tự gồm các chữ cái viết thường, nên $|\Sigma| = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def betterCompression(self, compressed: str) -> str:
        cnt = Counter()
        i, n = 0, len(compressed)
        while i < n:
            j = i + 1
            x = 0
            while j < n and compressed[j].isdigit():
                x = x * 10 + int(compressed[j])
                j += 1
            cnt[compressed[i]] += x
            i = j
        return "".join(sorted(f"{k}{v}" for k, v in cnt.items()))
```

#### Java

```java
class Solution {
    public String betterCompression(String compressed) {
        Map<Character, Integer> cnt = new TreeMap<>();
        int i = 0;
        int n = compressed.length();
        while (i < n) {
            char c = compressed.charAt(i);
            int j = i + 1;
            int x = 0;
            while (j < n && Character.isDigit(compressed.charAt(j))) {
                x = x * 10 + (compressed.charAt(j) - '0');
                j++;
            }
            cnt.merge(c, x, Integer::sum);
            i = j;
        }
        StringBuilder ans = new StringBuilder();
        for (var e : cnt.entrySet()) {
            ans.append(e.getKey()).append(e.getValue());
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string betterCompression(string compressed) {
        map<char, int> cnt;
        int i = 0;
        int n = compressed.length();
        while (i < n) {
            char c = compressed[i];
            int j = i + 1;
            int x = 0;
            while (j < n && isdigit(compressed[j])) {
                x = x * 10 + (compressed[j] - '0');
                j++;
            }
            cnt[c] += x;
            i = j;
        }
        stringstream ans;
        for (const auto& entry : cnt) {
            ans << entry.first << entry.second;
        }
        return ans.str();
    }
};
```

#### Go

```go
func betterCompression(compressed string) string {
    cnt := map[byte]int{}
    n := len(compressed)
    for i := 0; i < n; {
        c := compressed[i]
        j := i + 1
        x := 0
        for j < n && compressed[j] >= '0' && compressed[j] <= '9' {
            x = x*10 + int(compressed[j]-'0')
            j++
        }
        cnt[c] += x
        i = j
    }
    ans := strings.Builder{}
    for c := byte('a'); c <= byte('z'); c++ {
        if cnt[c] > 0 {
            ans.WriteByte(c)
            ans.WriteString(strconv.Itoa(cnt[c]))
        }
    }
    return ans.String()
}
```

#### TypeScript

```ts
function betterCompression(compressed: string): string {
    const cnt = new Map<string, number>();
    const n = compressed.length;
    let i = 0;

    while (i < n) {
        const c = compressed[i];
        let j = i + 1;
        let x = 0;
        while (j < n && /\d/.test(compressed[j])) {
            x = x * 10 + +compressed[j];
            j++;
        }
        cnt.set(c, (cnt.get(c) || 0) + x);
        i = j;
    }
    const keys = Array.from(cnt.keys()).sort();
    const ans: string[] = [];
    for (const k of keys) {
        ans.push(`${k}${cnt.get(k)}`);
    }
    return ans.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
