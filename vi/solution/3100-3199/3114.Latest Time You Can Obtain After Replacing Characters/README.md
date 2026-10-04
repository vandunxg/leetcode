---
comments: true
difficulty: Easy
rating: 1290
source: Weekly Contest 393 Q1
tags:
    - String
    - Enumeration
---

<!-- problem:start -->

# [3114. Latest Time You Can Obtain After Replacing Characters](https://leetcode.com/problems/latest-time-you-can-obtain-after-replacing-characters)

[中文文档](/solution/3100-3199/3114.Latest%20Time%20You%20Can%20Obtain%20After%20Replacing%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> biểu diễn thời gian theo định dạng 12 giờ, trong đó một số chữ số (có thể không có chữ số nào) được thay thế bằng <code>&quot;?&quot;</code>.</p>

<p>Thời gian theo định dạng 12 giờ có dạng <code>&quot;HH:MM&quot;</code>, trong đó <code>HH</code> nằm trong khoảng từ <code>00</code> đến <code>11</code>, còn <code>MM</code> nằm trong khoảng từ <code>00</code> đến <code>59</code>. Thời gian sớm nhất là <code>00:00</code>, và thời gian muộn nhất là <code>11:59</code>.</p>

<p>Bạn phải thay thế <strong>tất cả</strong> ký tự <code>&quot;?&quot;</code> trong <code>s</code> bằng các chữ số sao cho thời gian thu được từ chuỗi kết quả là thời gian theo định dạng 12 giờ <strong>hợp lệ</strong> và <strong>muộn nhất</strong> có thể.</p>

<p>Trả về <em>chuỗi kết quả</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;1?:?4&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;11:54&quot;</span></p>

<p><strong>Giải thích:</strong> Thời gian theo định dạng 12 giờ muộn nhất có thể thu được bằng cách thay thế các ký tự <code>&quot;?&quot;</code> là <code>&quot;11:54&quot;</code>.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;0?:5?&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">&quot;09:59&quot;</span></p>

<p><strong>Giải thích:</strong> Thời gian theo định dạng 12 giờ muộn nhất có thể thu được bằng cách thay thế các ký tự <code>&quot;?&quot;</code> là <code>&quot;09:59&quot;</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>s.length == 5</code></li>
	<li><code>s[2]</code> bằng ký tự <code>&quot;:&quot;</code>.</li>
	<li>Mọi ký tự ngoại trừ <code>s[2]</code> đều là chữ số hoặc ký tự <code>&quot;?&quot;</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho <strong>ít nhất</strong> một thời gian trong khoảng từ <code>&quot;00:00&quot;</code> đến <code>&quot;11:59&quot;</code> có thể thu được sau khi thay thế các ký tự <code>&quot;?&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ có $12\times 60$ thời gian hợp lệ. Ta duyệt giờ và phút từ lớn đến nhỏ, đối chiếu với mẫu ở các vị trí không phải `?`, và thời gian đầu tiên khớp là thời gian muộn nhất có thể.
>
> Không gian tìm kiếm là hằng số, nên không cần quay lui. Duyệt từ $11{:}59$ xuống $00{:}00$ đảm bảo tìm được thời gian lớn nhất theo thứ tự từ điển.
>
> Tạo từng thời gian `HH:MM` theo thứ tự đó, so sánh với $s$ và xem `?` là ký tự đại diện, rồi trả về kết quả khớp đầu tiên.

<!-- thinking:end -->

Ta có thể liệt kê tất cả thời gian từ lớn đến nhỏ, trong đó giờ $h$ chạy từ $11$ đến $0$, còn phút $m$ chạy từ $59$ đến $0$. Với mỗi thời gian $t$, ta kiểm tra xem từng chữ số của $t$ có khớp với ký tự tương ứng trong $s$ hay không (nếu ký tự tương ứng trong $s$ không phải là "?"). Nếu khớp, ta đã tìm được đáp án và trả về $t$.

Độ phức tạp thời gian là $O(h \times m)$, trong đó $h = 12$ và $m = 60$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLatestTime(self, s: str) -> str:
        for h in range(11, -1, -1):
            for m in range(59, -1, -1):
                t = f"{h:02d}:{m:02d}"
                if all(a == b for a, b in zip(s, t) if a != "?"):
                    return t
```

#### Java

```java
class Solution {
    public String findLatestTime(String s) {
        for (int h = 11;; h--) {
            for (int m = 59; m >= 0; m--) {
                String t = String.format("%02d:%02d", h, m);
                boolean ok = true;
                for (int i = 0; i < s.length(); i++) {
                    if (s.charAt(i) != '?' && s.charAt(i) != t.charAt(i)) {
                        ok = false;
                        break;
                    }
                }
                if (ok) {
                    return t;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findLatestTime(string s) {
        for (int h = 11;; h--) {
            for (int m = 59; m >= 0; m--) {
                char t[6];
                sprintf(t, "%02d:%02d", h, m);
                bool ok = true;
                for (int i = 0; i < s.length(); i++) {
                    if (s[i] != '?' && s[i] != t[i]) {
                        ok = false;
                        break;
                    }
                }
                if (ok) {
                    return t;
                }
            }
        }
    }
};
```

#### Go

```go
func findLatestTime(s string) string {
	for h := 11; ; h-- {
		for m := 59; m >= 0; m-- {
			t := fmt.Sprintf("%02d:%02d", h, m)
			ok := true
			for i := 0; i < len(s); i++ {
				if s[i] != '?' && s[i] != t[i] {
					ok = false
					break
				}
			}
			if ok {
				return t
			}
		}
	}
}
```

#### TypeScript

```ts
function findLatestTime(s: string): string {
    for (let h = 11; ; h--) {
        for (let m = 59; m >= 0; m--) {
            const t: string = `${h.toString().padStart(2, '0')}:${m.toString().padStart(2, '0')}`;
            let ok: boolean = true;
            for (let i = 0; i < s.length; i++) {
                if (s[i] !== '?' && s[i] !== t[i]) {
                    ok = false;
                    break;
                }
            }
            if (ok) {
                return t;
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Xét từng chữ số

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt toàn bộ không gian tuy có thời gian hằng số nhưng vẫn phải tạo hàng trăm ứng viên. Ta có thể viết trực tiếp miền giá trị của từng chữ số trong đồng hồ 12 giờ.
>
> Chữ số hàng chục của giờ là $0$ hoặc $1$, chữ số hàng đơn vị nhiều nhất là $1$ khi chữ số hàng chục là $1$, chữ số hàng chục của phút nhiều nhất là $5$, còn chữ số hàng đơn vị có thể là $9$.
>
> Thay từng `?` từ trái sang phải bằng chữ số lớn nhất vẫn hợp lệ: quyết định $s[0]$, sau đó là $s[1]$, rồi điền các chữ số phút bằng $5$ và $9$. Chỉ cần duyệt một lượt.

<!-- thinking:end -->

Ta có thể xét từng chữ số của $s$. Nếu là "?", ta xác định giá trị của chữ số này dựa trên các ký tự đứng trước và sau nó. Cụ thể, ta có các quy tắc sau:

- Nếu $s[0]$ là "?", giá trị của $s[0]$ phải là "1" hoặc "0", tùy thuộc vào giá trị của $s[1]$. Nếu $s[1]$ là "?" hoặc $s[1]$ nhỏ hơn "2", thì giá trị của $s[0]$ phải là "1"; ngược lại, giá trị của $s[0]$ phải là "0".
- Nếu $s[1]$ là "?", giá trị của $s[1]$ phải là "1" hoặc "9", tùy thuộc vào giá trị của $s[0]$. Nếu $s[0]$ là "1", thì giá trị của $s[1]$ phải là "1"; ngược lại, giá trị của $s[1]$ phải là "9".
- Nếu $s[3]$ là "?", giá trị của $s[3]$ phải là "5".
- Nếu $s[4]$ là "?", giá trị của $s[4]$ phải là "9".

Độ phức tạp thời gian là $O(1)$, và độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLatestTime(self, s: str) -> str:
        s = list(s)
        if s[0] == "?":
            s[0] = "1" if s[1] == "?" or s[1] < "2" else "0"
        if s[1] == "?":
            s[1] = "1" if s[0] == "1" else "9"
        if s[3] == "?":
            s[3] = "5"
        if s[4] == "?":
            s[4] = "9"
        return "".join(s)
```

#### Java

```java
class Solution {
    public String findLatestTime(String s) {
        char[] cs = s.toCharArray();
        if (cs[0] == '?') {
            cs[0] = cs[1] == '?' || cs[1] < '2' ? '1' : '0';
        }
        if (cs[1] == '?') {
            cs[1] = cs[0] == '1' ? '1' : '9';
        }
        if (cs[3] == '?') {
            cs[3] = '5';
        }
        if (cs[4] == '?') {
            cs[4] = '9';
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string findLatestTime(string s) {
        if (s[0] == '?') {
            s[0] = s[1] == '?' || s[1] < '2' ? '1' : '0';
        }
        if (s[1] == '?') {
            s[1] = s[0] == '1' ? '1' : '9';
        }
        if (s[3] == '?') {
            s[3] = '5';
        }
        if (s[4] == '?') {
            s[4] = '9';
        }
        return s;
    }
};
```

#### Go

```go
func findLatestTime(s string) string {
	cs := []byte(s)
	if cs[0] == '?' {
		if cs[1] == '?' || cs[1] < '2' {
			cs[0] = '1'
		} else {
			cs[0] = '0'
		}
	}
	if cs[1] == '?' {
		if cs[0] == '1' {
			cs[1] = '1'
		} else {
			cs[1] = '9'
		}
	}
	if cs[3] == '?' {
		cs[3] = '5'
	}
	if cs[4] == '?' {
		cs[4] = '9'
	}
	return string(cs)
}
```

#### TypeScript

```ts
function findLatestTime(s: string): string {
    const cs = s.split('');
    if (cs[0] === '?') {
        cs[0] = cs[1] === '?' || cs[1] < '2' ? '1' : '0';
    }
    if (cs[1] === '?') {
        cs[1] = cs[0] === '1' ? '1' : '9';
    }
    if (cs[3] === '?') {
        cs[3] = '5';
    }
    if (cs[4] === '?') {
        cs[4] = '9';
    }
    return cs.join('');
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
