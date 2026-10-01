---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.18. Pattern Matching](https://leetcode.cn/problems/pattern-matching-lcci)

[中文文档](/lcci/16.18.Pattern%20Matching/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai chuỗi, pattern và value. Chuỗi pattern chỉ gồm các chữ cái a và b, dùng để mô tả một pattern trong một chuỗi. Ví dụ, chuỗi catcatgocatgo khớp với pattern aabab (trong đó cat là a và go là b). Nó cũng khớp với các pattern như a, ab và b. Hãy viết một method để xác định liệu value có khớp với pattern hay không. a và b không được là cùng một chuỗi.</p>
<p><strong>Ví dụ 1: </strong></p>
<pre>

<strong>Đầu vào: </strong> pattern = &quot;abba&quot;, value = &quot;dogcatcatdog&quot;

<strong>Đầu ra: </strong> true

</pre>
<p><strong>Ví dụ 2: </strong></p>
<pre>

<strong>Đầu vào: </strong> pattern = &quot;abba&quot;, value = &quot;dogcatcatfish&quot;

<strong>Đầu ra: </strong> false

</pre>
<p><strong>Ví dụ 3: </strong></p>
<pre>

<strong>Đầu vào: </strong> pattern = &quot;aaaa&quot;, value = &quot;dogcatcatdog&quot;

<strong>Đầu ra: </strong> false

</pre>
<p><strong>Ví dụ 4: </strong></p>
<pre>

<strong>Đầu vào: </strong> pattern = &quot;abba&quot;, value = &quot;dogdogdogdog&quot;

<strong>Đầu ra: </strong> true

<strong>Giải thích: </strong> &quot;a&quot;=&quot;dogdog&quot;,b=&quot;&quot;，và ngược lại.

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>0 &lt;= len(pattern) &lt;= 1000</code></li>
	<li><code>0 &lt;= len(value) &lt;= 1000</code></li>
	<li><code>pattern</code> chỉ chứa <code>&quot;a&quot;</code> và <code>&quot;b&quot;</code>, còn <code>value</code> chỉ chứa các chữ cái thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> `a` và `b` lần lượt đại diện cho một chuỗi (có thể rỗng). Không gian tìm kiếm các tổ hợp nội dung của chúng là rất lớn.
>
> Khi độ dài của hai chuỗi được xác định, kết quả khớp cũng được xác định; chỉ còn kiểm tra tính nhất quán và $a\ne b$.
>
> Trước hết xử lý pattern chỉ có một chữ cái. Duyệt các giá trị của $la$, suy ra $lb$ từ tổng độ dài, rồi dùng `check` để cắt các đoạn theo pattern. Cả việc duyệt và mỗi lần kiểm tra đều tuyến tính.

<!-- thinking:end -->

Trước tiên, chúng ta đếm số ký tự `'a'` và `'b'` trong chuỗi $pattern$, lần lượt ký hiệu là $cnt[0]$ và $cnt[1]$. Gọi độ dài của chuỗi $value$ là $n$.

Nếu $cnt[0]=0$, điều đó có nghĩa là chuỗi pattern chỉ chứa ký tự `'b'`. Ta cần kiểm tra xem $n$ có phải là bội số của $cnt[1]$ hay không, đồng thời kiểm tra xem $value$ có thể được chia thành $cnt[1]$ chuỗi con có độ dài $n/cnt[1]$ và tất cả các chuỗi con này có giống nhau hay không. Nếu không thỏa mãn, trả về $false$ ngay.

Nếu $cnt[1]=0$, điều đó có nghĩa là chuỗi pattern chỉ chứa ký tự `'a'`. Ta cần kiểm tra xem $n$ có phải là bội số của $cnt[0]$ hay không, đồng thời kiểm tra xem $value$ có thể được chia thành $cnt[0]$ chuỗi con có độ dài $n/cnt[0]$ và tất cả các chuỗi con này có giống nhau hay không. Nếu không thỏa mãn, trả về $false$ ngay.

Tiếp theo, gọi độ dài của chuỗi mà ký tự `'a'` khớp với là $la$, và độ dài của chuỗi mà ký tự `'b'` khớp với là $lb$. Khi đó, ta có $la \times cnt[0] + lb \times cnt[1] = n$. Nếu duyệt $la$, ta có thể xác định giá trị của $lb$. Vì vậy, ta có thể duyệt $la$ và kiểm tra xem có tồn tại một số nguyên $lb$ thỏa mãn phương trình trên hay không.

Độ phức tạp thời gian là $O(n^2)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của chuỗi $value$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def patternMatching(self, pattern: str, value: str) -> bool:
        def check(la: int, lb: int) -> bool:
            i = 0
            a, b = "", ""
            for c in pattern:
                if c == "a":
                    if a and value[i : i + la] != a:
                        return False
                    a = value[i : i + la]
                    i += la
                else:
                    if b and value[i : i + lb] != b:
                        return False
                    b = value[i : i + lb]
                    i += lb
            return a != b

        n = len(value)
        cnt = Counter(pattern)
        if cnt["a"] == 0:
            return n % cnt["b"] == 0 and value[: n // cnt["b"]] * cnt["b"] == value
        if cnt["b"] == 0:
            return n % cnt["a"] == 0 and value[: n // cnt["a"]] * cnt["a"] == value

        for la in range(n + 1):
            if la * cnt["a"] > n:
                break
            lb, mod = divmod(n - la * cnt["a"], cnt["b"])
            if mod == 0 and check(la, lb):
                return True
        return False
```

#### Java

```java
class Solution {
    private String pattern;
    private String value;

    public boolean patternMatching(String pattern, String value) {
        this.pattern = pattern;
        this.value = value;
        int[] cnt = new int[2];
        for (char c : pattern.toCharArray()) {
            ++cnt[c - 'a'];
        }
        int n = value.length();
        if (cnt[0] == 0) {
            return n % cnt[1] == 0 && value.substring(0, n / cnt[1]).repeat(cnt[1]).equals(value);
        }
        if (cnt[1] == 0) {
            return n % cnt[0] == 0 && value.substring(0, n / cnt[0]).repeat(cnt[0]).equals(value);
        }
        for (int la = 0; la <= n; ++la) {
            if (la * cnt[0] > n) {
                break;
            }
            if ((n - la * cnt[0]) % cnt[1] == 0) {
                int lb = (n - la * cnt[0]) / cnt[1];
                if (check(la, lb)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean check(int la, int lb) {
        int i = 0;
        String a = null, b = null;
        for (char c : pattern.toCharArray()) {
            if (c == 'a') {
                if (a != null && !a.equals(value.substring(i, i + la))) {
                    return false;
                }
                a = value.substring(i, i + la);
                i += la;
            } else {
                if (b != null && !b.equals(value.substring(i, i + lb))) {
                    return false;
                }
                b = value.substring(i, i + lb);
                i += lb;
            }
        }
        return !a.equals(b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool patternMatching(string pattern, string value) {
        int n = value.size();
        int cnt[2]{};
        for (char c : pattern) {
            cnt[c - 'a']++;
        }
        if (cnt[0] == 0) {
            return n % cnt[1] == 0 && repeat(value.substr(0, n / cnt[1]), cnt[1]) == value;
        }
        if (cnt[1] == 0) {
            return n % cnt[0] == 0 && repeat(value.substr(0, n / cnt[0]), cnt[0]) == value;
        }
        auto check = [&](int la, int lb) {
            int i = 0;
            string a, b;
            for (char c : pattern) {
                if (c == 'a') {
                    if (!a.empty() && a != value.substr(i, la)) {
                        return false;
                    }
                    a = value.substr(i, la);
                    i += la;
                } else {
                    if (!b.empty() && b != value.substr(i, lb)) {
                        return false;
                    }
                    b = value.substr(i, lb);
                    i += lb;
                }
            }
            return a != b;
        };
        for (int la = 0; la <= n; ++la) {
            if (la * cnt[0] > n) {
                break;
            }
            if ((n - la * cnt[0]) % cnt[1] == 0) {
                int lb = (n - la * cnt[0]) / cnt[1];
                if (check(la, lb)) {
                    return true;
                }
            }
        }
        return false;
    }

    string repeat(string s, int n) {
        string ans;
        while (n--) {
            ans += s;
        }
        return ans;
    }
};
```

#### Go

```go
func patternMatching(pattern string, value string) bool {
	cnt := [2]int{}
	for _, c := range pattern {
		cnt[c-'a']++
	}
	n := len(value)
	if cnt[0] == 0 {
		return n%cnt[1] == 0 && strings.Repeat(value[:n/cnt[1]], cnt[1]) == value
	}
	if cnt[1] == 0 {
		return n%cnt[0] == 0 && strings.Repeat(value[:n/cnt[0]], cnt[0]) == value
	}
	check := func(la, lb int) bool {
		i := 0
		a, b := "", ""
		for _, c := range pattern {
			if c == 'a' {
				if a != "" && value[i:i+la] != a {
					return false
				}
				a = value[i : i+la]
				i += la
			} else {
				if b != "" && value[i:i+lb] != b {
					return false
				}
				b = value[i : i+lb]
				i += lb
			}
		}
		return a != b
	}
	for la := 0; la <= n; la++ {
		if la*cnt[0] > n {
			break
		}
		if (n-la*cnt[0])%cnt[1] == 0 {
			lb := (n - la*cnt[0]) / cnt[1]
			if check(la, lb) {
				return true
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function patternMatching(pattern: string, value: string): boolean {
    const cnt: number[] = [0, 0];
    for (const c of pattern) {
        cnt[c === 'a' ? 0 : 1]++;
    }
    const n = value.length;
    if (cnt[0] === 0) {
        return n % cnt[1] === 0 && value.slice(0, (n / cnt[1]) | 0).repeat(cnt[1]) === value;
    }
    if (cnt[1] === 0) {
        return n % cnt[0] === 0 && value.slice(0, (n / cnt[0]) | 0).repeat(cnt[0]) === value;
    }
    const check = (la: number, lb: number) => {
        let i = 0;
        let a = '';
        let b = '';
        for (const c of pattern) {
            if (c === 'a') {
                if (a && a !== value.slice(i, i + la)) {
                    return false;
                }
                a = value.slice(i, (i += la));
            } else {
                if (b && b !== value.slice(i, i + lb)) {
                    return false;
                }
                b = value.slice(i, (i += lb));
            }
        }
        return a !== b;
    };
    for (let la = 0; la <= n; ++la) {
        if (la * cnt[0] > n) {
            break;
        }
        if ((n - la * cnt[0]) % cnt[1] === 0) {
            const lb = ((n - la * cnt[0]) / cnt[1]) | 0;
            if (check(la, lb)) {
                return true;
            }
        }
    }
    return false;
}
```

#### Swift

```swift
class Solution {
    private var pattern: String = ""
    private var value: String = ""

    func patternMatching(_ pattern: String, _ value: String) -> Bool {
        self.pattern = pattern
        self.value = value
        var cnt = [Int](repeating: 0, count: 2)
        for c in pattern {
            cnt[Int(c.asciiValue! - Character("a").asciiValue!)] += 1
        }
        let n = value.count
        if cnt[0] == 0 {
            return n % cnt[1] == 0 && String(repeating: String(value.prefix(n / cnt[1])), count: cnt[1]) == value
        }
        if cnt[1] == 0 {
            return n % cnt[0] == 0 && String(repeating: String(value.prefix(n / cnt[0])), count: cnt[0]) == value
        }
        for la in 0...n {
            if la * cnt[0] > n {
                break
            }
            if (n - la * cnt[0]) % cnt[1] == 0 {
                let lb = (n - la * cnt[0]) / cnt[1]
                if check(la, lb) {
                    return true
                }
            }
        }
        return false
    }

    private func check(_ la: Int, _ lb: Int) -> Bool {
        var a: String? = nil
        var b: String? = nil
        var index = value.startIndex

        for c in pattern {
            if c == "a" {
                let end = value.index(index, offsetBy: la)
                if let knownA = a {
                    if knownA != value[index..<end] {
                        return false
                    }
                } else {
                    a = String(value[index..<end])
                }
                index = end
            } else {
                let end = value.index(index, offsetBy: lb)
                if let knownB = b {
                    if knownB != value[index..<end] {
                        return false
                    }
                } else {
                    b = String(value[index..<end])
                }
                index = end
            }
        }
        return a != b
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
