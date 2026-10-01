---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.20. T9](https://leetcode.cn/problems/t9-lcci)

[中文文档](/lcci/16.20.T9/README.md)

## Mô tả

<!-- description:start -->

<p>Trên những chiếc điện thoại di động cũ, người dùng nhập liệu bằng bàn phím số và điện thoại sẽ cung cấp một danh sách các từ khớp với những chữ số đó. Mỗi chữ số ánh xạ tới một tập hợp gồm từ 0 đến 4 chữ cái. Hãy triển khai một thuật toán trả về danh sách các từ khớp với một dãy chữ số. Bạn được cung cấp một danh sách các từ hợp lệ. Phép ánh xạ được minh họa trong hình dưới đây:</p>
![](https://fastly.jsdelivr.net/gh/doocs/leetcode@main/lcci/16.20.T9/images/17_telephone_keypad.png)
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:</strong> num = &quot;8733&quot;, words = [&quot;tree&quot;, &quot;used&quot;]

<strong>Đầu ra:</strong> [&quot;tree&quot;, &quot;used&quot;]

</pre>
<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:</strong> num = &quot;2&quot;, words = [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;, &quot;d&quot;]

<strong>Đầu ra:</strong> [&quot;a&quot;, &quot;b&quot;, &quot;c&quot;]</pre>

<p>Lưu ý:</p>
<ul>
	<li><code>num.length &lt;= 1000</code></li>
	<li><code>words.length &lt;= 500</code></li>
	<li><code>words[i].length == num.length</code></li>
	<li><code>There are no number 0 and 1 in num</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tư duy ngược

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi chữ số T9 có thể mở rộng thành nhiều chữ cái. Việc triển khai mọi tổ hợp có độ lớn khoảng $4^{|num|}$.
>
> Danh sách từ thường nhỏ hơn nhiều, nên hãy kiểm tra xem từng từ có gõ ra chính xác $num$ hay không.
>
> Bảng ánh xạ chữ cái sang chữ số $d$ được xây dựng một lần; `check` so sánh $d[c]$ với $num[i]$ theo từng vị trí. Thời gian chạy tỉ lệ với tổng độ dài các từ.

<!-- thinking:end -->

Chúng ta xem xét một lời giải xuôi, duyệt qua từng chữ số trong chuỗi $num$, ánh xạ nó tới các chữ cái tương ứng, kết hợp tất cả các chữ cái để thu được mọi từ có thể, rồi so sánh chúng với danh sách từ đã cho. Nếu từ đó nằm trong danh sách, ta thêm nó vào đáp án. Độ phức tạp thời gian của lời giải này là $O(4^n)$, trong đó $n$ là độ dài của chuỗi $num$, và cách này rõ ràng sẽ vượt quá thời gian.

Thay vào đó, ta có thể xét một lời giải ngược, duyệt qua danh sách từ đã cho và với mỗi từ $w$, xác định xem nó có thể được tạo thành từ các chữ số trong chuỗi $num$ hay không. Nếu có thể, ta thêm từ đó vào đáp án. Mấu chốt của bài toán là xác định xem một từ có thể được tạo thành từ các chữ số trong chuỗi $num$ hay không. Chỉ cần duyệt qua từng chữ cái trong từ $w$, khôi phục nó về chữ số tương ứng, rồi lần lượt so sánh với từng chữ số trong chuỗi $num$. Nếu chúng giống nhau, điều đó có nghĩa là từ $w$ có thể được tạo thành từ các chữ số trong chuỗi $num$.

Độ phức tạp thời gian là $O(m \times n)$, còn độ phức tạp không gian là $O(C)$. Trong đó, $m$ và $n$ lần lượt là độ dài của danh sách từ và chuỗi $num$, còn $C$ là kích thước của tập ký tự, bằng $26$ trong bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getValidT9Words(self, num: str, words: List[str]) -> List[str]:
        def check(w: str) -> bool:
            return all(d[c] == num[i] for i, c in enumerate(w))

        d = {c: d for c, d in zip(ascii_lowercase, "22233344455566677778889999")}
        return [w for w in words if check(w)]
```

#### Python3

```python
class Solution:
    def getValidT9Words(self, num: str, words: List[str]) -> List[str]:
        trans = str.maketrans(ascii_lowercase, "22233344455566677778889999")
        return [w for w in words if w.translate(trans) == num]
```

#### Java

```java
class Solution {
    public List<String> getValidT9Words(String num, String[] words) {
        String s = "22233344455566677778889999";
        int[] d = new int[26];
        for (int i = 0; i < 26; ++i) {
            d[i] = s.charAt(i);
        }
        List<String> ans = new ArrayList<>();
        int n = num.length();
        for (String w : words) {
            boolean ok = true;
            for (int i = 0; i < n; ++i) {
                if (d[w.charAt(i) - 'a'] != num.charAt(i)) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                ans.add(w);
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
    vector<string> getValidT9Words(string num, vector<string>& words) {
        string s = "22233344455566677778889999";
        int d[26];
        for (int i = 0; i < 26; ++i) {
            d[i] = s[i];
        }
        vector<string> ans;
        int n = num.size();
        for (auto& w : words) {
            bool ok = true;
            for (int i = 0; i < n; ++i) {
                if (d[w[i] - 'a'] != num[i]) {
                    ok = false;
                }
            }
            if (ok) {
                ans.emplace_back(w);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func getValidT9Words(num string, words []string) (ans []string) {
	s := "22233344455566677778889999"
	d := [26]rune{}
	for i, c := range s {
		d[i] = c
	}
	for _, w := range words {
		ok := true
		for i, c := range w {
			if d[c-'a'] != rune(num[i]) {
				ok = false
				break
			}
		}
		if ok {
			ans = append(ans, w)
		}
	}
	return
}
```

#### TypeScript

```ts
function getValidT9Words(num: string, words: string[]): string[] {
    const s = '22233344455566677778889999';
    const d: string[] = Array(26);
    for (let i = 0; i < 26; ++i) {
        d[i] = s[i];
    }
    const ans: string[] = [];
    const n = num.length;
    for (const w of words) {
        let ok = true;
        for (let i = 0; i < n; ++i) {
            if (d[w[i].charCodeAt(0) - 97] !== num[i]) {
                ok = false;
                break;
            }
        }
        if (ok) {
            ans.push(w);
        }
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func getValidT9Words(_ num: String, _ words: [String]) -> [String] {
        let s = "22233344455566677778889999"
        var d = Array(repeating: 0, count: 26)
        for i in 0..<26 {
            d[i] = Int(s[s.index(s.startIndex, offsetBy: i)].asciiValue! - Character("0").asciiValue!)
        }
        var ans: [String] = []
        let n = num.count
        for w in words {
            var ok = true
            for i in 0..<n {
                let numChar = Int(num[num.index(num.startIndex, offsetBy: i)].asciiValue! - Character("0").asciiValue!)
                if d[Int(w[w.index(w.startIndex, offsetBy: i)].asciiValue! - Character("a").asciiValue!)] != numChar {
                    ok = false
                    break
                }
            }
            if ok {
                ans.append(w)
            }
        }
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
