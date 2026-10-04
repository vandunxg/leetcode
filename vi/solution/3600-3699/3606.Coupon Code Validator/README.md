---
comments: true
difficulty: Easy
rating: 1312
source: Weekly Contest 457 Q1
tags:
    - Array
    - Hash Table
    - String
    - Sorting
---

<!-- problem:start -->

# [3606. Coupon Code Validator](https://leetcode.com/problems/coupon-code-validator)

[中文文档](/solution/3600-3699/3606.Coupon%20Code%20Validator/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho ba mảng có độ dài <code>n</code> mô tả thuộc tính của <code>n</code> coupon: <code>code</code>, <code>businessLine</code> và <code>isActive</code>. Coupon thứ <code>i<sup>th</sup> </code> có:</p>

<ul>
    <li><code>code[i]</code>: một <strong>chuỗi</strong> đại diện cho mã coupon.</li>
    <li><code>businessLine[i]</code>: một <strong>chuỗi</strong> cho biết ngành hàng của coupon.</li>
    <li><code>isActive[i]</code>: một <strong>boolean</strong> cho biết coupon hiện có đang hoạt động hay không.</li>
</ul>

<p>Một coupon được coi là <strong>hợp lệ</strong> nếu thỏa mãn tất cả các điều kiện sau:</p>

<ol>
    <li><code>code[i]</code> không rỗng và chỉ gồm các ký tự chữ và số (a-z, A-Z, 0-9) cùng dấu gạch dưới (<code>_</code>).</li>
    <li><code>businessLine[i]</code> là một trong bốn ngành hàng sau: <code>&quot;electronics&quot;</code>, <code>&quot;grocery&quot;</code>, <code>&quot;pharmacy&quot;</code>, <code>&quot;restaurant&quot;</code>.</li>
    <li><code>isActive[i]</code> là <strong>true</strong>.</li>
</ol>

<p>Trả về một mảng chứa các <strong>code</strong> của tất cả coupon hợp lệ, được <strong>sắp xếp</strong> trước theo <strong>businessLine</strong> với thứ tự: <code>&quot;electronics&quot;</code>, <code>&quot;grocery&quot;</code>, <code>&quot;pharmacy&quot;, &quot;restaurant&quot;</code>, sau đó theo <strong>code</strong> theo thứ tự từ điển (tăng dần) trong từng ngành hàng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">code = [&quot;SAVE20&quot;,&quot;&quot;,&quot;PHARMA5&quot;,&quot;SAVE@20&quot;], businessLine = [&quot;restaurant&quot;,&quot;grocery&quot;,&quot;pharmacy&quot;,&quot;restaurant&quot;], isActive = [true,true,true,true]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;PHARMA5&quot;,&quot;SAVE20&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Coupon đầu tiên hợp lệ.</li>
    <li>Coupon thứ hai có code rỗng (không hợp lệ).</li>
    <li>Coupon thứ ba hợp lệ.</li>
    <li>Coupon thứ tư có ký tự đặc biệt <code>@</code> (không hợp lệ).</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">code = [&quot;GROCERY15&quot;,&quot;ELECTRONICS_50&quot;,&quot;DISCOUNT10&quot;], businessLine = [&quot;grocery&quot;,&quot;electronics&quot;,&quot;invalid&quot;], isActive = [false,true,true]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[&quot;ELECTRONICS_50&quot;]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Coupon đầu tiên không hoạt động (không hợp lệ).</li>
    <li>Coupon thứ hai hợp lệ.</li>
    <li>Coupon thứ ba có ngành hàng không hợp lệ (không hợp lệ).</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>n == code.length == businessLine.length == isActive.length</code></li>
    <li><code>1 &lt;= n &lt;= 100</code></li>
    <li><code>0 &lt;= code[i].length, businessLine[i].length &lt;= 100</code></li>
    <li><code>code[i]</code> và <code>businessLine[i]</code> chỉ gồm các ký tự ASCII có thể in được.</li>
    <li><code>isActive[i]</code> là <code>true</code> hoặc <code>false</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Vì $n\le 100$, chỉ cần lọc theo các quy tắc đã nêu là đủ. Một coupon hợp lệ phải có mã không rỗng, chỉ gồm chữ cái, chữ số và dấu gạch dưới, có ngành hàng thuộc một trong bốn giá trị được cho phép, đồng thời đang hoạt động.
>
> Ta thu thập các chỉ số thỏa mãn, sắp xếp chúng theo $(\textit{businessLine},\textit{code})$, rồi lấy ra các mã. Các khóa sắp xếp này tương ứng với thứ tự ngành hàng và tiêu chí phụ là thứ tự từ điển được yêu cầu.

<!-- thinking:end -->

Ta có thể mô phỏng trực tiếp các điều kiện được mô tả trong đề bài để lọc ra các coupon hợp lệ. Các bước cụ thể như sau:

1. **Kiểm tra mã**: Với mã của mỗi coupon, kiểm tra xem mã có không rỗng và chỉ chứa chữ cái, chữ số cùng dấu gạch dưới hay không.
2. **Kiểm tra ngành hàng**: Kiểm tra xem ngành hàng của mỗi coupon có thuộc một trong bốn ngành hàng hợp lệ hay không.
3. **Kiểm tra trạng thái hoạt động**: Kiểm tra xem coupon có đang hoạt động hay không.
4. **Thu thập coupon hợp lệ**: Thu thập mã của tất cả coupon thỏa mãn các điều kiện trên.
5. **Sắp xếp**: Sắp xếp các coupon hợp lệ theo ngành hàng và mã.
6. **Trả về kết quả**: Trả về danh sách mã của các coupon hợp lệ đã sắp xếp.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, trong đó $n$ là số coupon.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validateCoupons(
        self, code: List[str], businessLine: List[str], isActive: List[bool]
    ) -> List[str]:
        def check(s: str) -> bool:
            if not s:
                return False
            for c in s:
                if not (c.isalpha() or c.isdigit() or c == "_"):
                    return False
            return True

        idx = []
        bs = {"electronics", "grocery", "pharmacy", "restaurant"}
        for i, (c, b, a) in enumerate(zip(code, businessLine, isActive)):
            if a and b in bs and check(c):
                idx.append(i)
        idx.sort(key=lambda i: (businessLine[i], code[i]))
        return [code[i] for i in idx]
```

#### Java

```java
class Solution {
    public List<String> validateCoupons(String[] code, String[] businessLine, boolean[] isActive) {
        List<Integer> idx = new ArrayList<>();
        Set<String> bs
            = new HashSet<>(Arrays.asList("electronics", "grocery", "pharmacy", "restaurant"));

        for (int i = 0; i < code.length; i++) {
            if (isActive[i] && bs.contains(businessLine[i]) && check(code[i])) {
                idx.add(i);
            }
        }

        idx.sort((i, j) -> {
            int cmp = businessLine[i].compareTo(businessLine[j]);
            if (cmp != 0) {
                return cmp;
            }
            return code[i].compareTo(code[j]);
        });

        List<String> ans = new ArrayList<>();
        for (int i : idx) {
            ans.add(code[i]);
        }
        return ans;
    }

    private boolean check(String s) {
        if (s.isEmpty()) {
            return false;
        }
        for (char c : s.toCharArray()) {
            if (!Character.isLetterOrDigit(c) && c != '_') {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> validateCoupons(vector<string>& code, vector<string>& businessLine, vector<bool>& isActive) {
        vector<int> idx;
        unordered_set<string> bs = {"electronics", "grocery", "pharmacy", "restaurant"};

        for (int i = 0; i < code.size(); ++i) {
            const string& c = code[i];
            const string& b = businessLine[i];
            bool a = isActive[i];
            if (a && bs.count(b) && check(c)) {
                idx.push_back(i);
            }
        }

        sort(idx.begin(), idx.end(), [&](int i, int j) {
            if (businessLine[i] != businessLine[j]) return businessLine[i] < businessLine[j];
            return code[i] < code[j];
        });

        vector<string> ans;
        for (int i : idx) {
            ans.push_back(code[i]);
        }
        return ans;
    }

private:
    bool check(const string& s) {
        if (s.empty()) return false;
        for (char c : s) {
            if (!isalnum(c) && c != '_') {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func validateCoupons(code []string, businessLine []string, isActive []bool) []string {
    idx := []int{}
    bs := map[string]struct{}{
        "electronics": {},
        "grocery":     {},
        "pharmacy":    {},
        "restaurant":  {},
    }

    check := func(s string) bool {
        if len(s) == 0 {
            return false
        }
        for _, c := range s {
            if !unicode.IsLetter(c) && !unicode.IsDigit(c) && c != '_' {
                return false
            }
        }
        return true
    }

    for i := range code {
        if isActive[i] {
            if _, ok := bs[businessLine[i]]; ok && check(code[i]) {
                idx = append(idx, i)
            }
        }
    }

    sort.Slice(idx, func(i, j int) bool {
        if businessLine[idx[i]] != businessLine[idx[j]] {
            return businessLine[idx[i]] < businessLine[idx[j]]
        }
        return code[idx[i]] < code[idx[j]]
    })

    ans := make([]string, 0, len(idx))
    for _, i := range idx {
        ans = append(ans, code[i])
    }
    return ans
}
```

#### TypeScript

```ts
function validateCoupons(code: string[], businessLine: string[], isActive: boolean[]): string[] {
    const idx: number[] = [];
    const bs = new Set(['electronics', 'grocery', 'pharmacy', 'restaurant']);

    const check = (s: string): boolean => {
        if (s.length === 0) return false;
        for (let i = 0; i < s.length; i++) {
            const c = s[i];
            if (!/[a-zA-Z0-9_]/.test(c)) {
                return false;
            }
        }
        return true;
    };

    for (let i = 0; i < code.length; i++) {
        if (isActive[i] && bs.has(businessLine[i]) && check(code[i])) {
            idx.push(i);
        }
    }

    idx.sort((i, j) => {
        if (businessLine[i] !== businessLine[j]) {
            return businessLine[i] < businessLine[j] ? -1 : 1;
        }
        return code[i] < code[j] ? -1 : 1;
    });

    return idx.map(i => code[i]);
}
```

#### Rust

```rust
use std::collections::HashSet;

impl Solution {
    pub fn validate_coupons(
        code: Vec<String>,
        business_line: Vec<String>,
        is_active: Vec<bool>,
    ) -> Vec<String> {
        fn check(s: &str) -> bool {
            if s.is_empty() {
                return false;
            }
            s.chars()
                .all(|c| c.is_ascii_alphanumeric() || c == '_')
        }

        let bs: HashSet<&str> =
            ["electronics", "grocery", "pharmacy", "restaurant"]
                .iter()
                .copied()
                .collect();

        let mut idx: Vec<usize> = Vec::new();
        for i in 0..code.len() {
            if is_active[i] && bs.contains(business_line[i].as_str()) && check(&code[i]) {
                idx.push(i);
            }
        }

        idx.sort_by(|&i, &j| {
            let cmp = business_line[i].cmp(&business_line[j]);
            if cmp == std::cmp::Ordering::Equal {
                code[i].cmp(&code[j])
            } else {
                cmp
            }
        });

        idx.into_iter().map(|i| code[i].clone()).collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
