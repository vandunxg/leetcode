---
comments: true
difficulty: Medium
rating: 1859
source: Weekly Contest 492 Q3
tags:
    - String
---

<!-- problem:start -->

# [3863. Minimum Operations to Sort a String](https://leetcode.com/problems/minimum-operations-to-sort-a-string)

[中文文档](/solution/3800-3899/3863.Minimum%20Operations%20to%20Sort%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</p>

<p>Trong một thao tác, bạn có thể chọn một <strong><span data-keyword="substring-nonempty">chuỗi con</span></strong> bất kỳ của <code>s</code> <strong>không phải</strong> là toàn bộ chuỗi, rồi <strong>sắp xếp</strong> chuỗi con đó theo <strong>thứ tự bảng chữ cái không giảm</strong>.</p>

<p>Hãy trả về số thao tác <strong>nhỏ nhất</strong> cần thực hiện để sắp xếp <code>s</code> theo <strong>thứ tự không giảm</strong>. Nếu không thể, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;dog&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong>​​​​​​​</p>

<ul>
	<li>Sắp xếp chuỗi con <code>&quot;og&quot;</code> thành <code>&quot;go&quot;</code>.</li>
	<li>Khi đó, <code>s = &quot;dgo&quot;</code>, đã được sắp xếp theo thứ tự tăng dần. Do đó, đáp án là 1.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;card&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Sắp xếp chuỗi con <code>&quot;car&quot;</code> thành <code>&quot;acr&quot;</code>, khi đó <code>s = &quot;acrd&quot;</code>.</li>
	<li>Sắp xếp chuỗi con <code>&quot;rd&quot;</code> thành <code>&quot;dr&quot;</code>, khi đó <code>s = &quot;acdr&quot;</code>, đã được sắp xếp theo thứ tự tăng dần. Do đó, đáp án là 2.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;gf&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không thể sắp xếp <code>s</code> với các ràng buộc đã cho. Do đó, đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích các trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi thao tác sắp xếp một chuỗi con thực sự. Ta cần dùng ít thao tác nhất để biến $s$ thành chuỗi không giảm. Với $|s| \le 10^5$, không thể mô phỏng các lần sắp xếp.
>
> Một thao tác có thể sắp xếp bất kỳ chuỗi con thực sự nào, nên đáp án chỉ phụ thuộc vào việc các chữ cái nhỏ nhất và lớn nhất có thể được đưa về hai đầu trong một hoặc hai bước hay không.
>
> Chuỗi đã được sắp xếp có đáp án là $0$. Chuỗi độ dài $2$ bị đảo thứ tự không có chuỗi con thực sự nào chứa cả hai chữ cái, nên đáp án là $-1$. Nếu chữ cái nhỏ nhất đã ở đầu hoặc chữ cái lớn nhất đã ở cuối thì cần một thao tác; nếu một trong hai chữ cái nằm ở giữa thì cần hai thao tác; nếu không thì cần ba thao tác.
>
> Các trường hợp trên bao phủ đầy đủ và được kiểm tra trong thời gian tuyến tính.

<!-- thinking:end -->

Trước tiên, ta kiểm tra xem chuỗi đã được sắp xếp theo thứ tự tăng dần hay chưa; nếu đã được sắp xếp, ta trả về 0.

Nếu chưa, mà chuỗi có độ dài 2, vì không được chọn toàn bộ chuỗi để sắp xếp nên không thể sắp xếp chuỗi; do đó ta trả về -1.

Tiếp theo, ta tìm ký tự nhỏ nhất $mn$ và ký tự lớn nhất $mx$ trong chuỗi. Nếu ký tự đầu tiên của chuỗi bằng $mn$, hoặc ký tự cuối cùng bằng $mx$, thì chỉ cần thực hiện một thao tác trên chuỗi con còn lại là đủ để sắp xếp toàn bộ chuỗi, nên ta trả về 1.

Nếu không, mà có một ký tự ở giữa chuỗi bằng $mn$ hoặc $mx$, ta cần một thao tác để đưa ký tự đó về đầu hoặc cuối chuỗi, rồi thêm một thao tác nữa để sắp xếp phần còn lại, nên ta trả về 2.

Cuối cùng, nếu không trường hợp nào ở trên xảy ra, ta cần một thao tác trên chuỗi con chứa cả $mn$ và $mx$, sau đó thêm một thao tác nữa trên chuỗi con còn lại, nên ta trả về 3.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi $s$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minOperations(self, s: str) -> int:
        if all(a <= b for a, b in pairwise(s)):
            return 0
        if len(s) == 2:
            return -1
        mn, mx = min(s), max(s)
        if s[0] == mn or s[-1] == mx:
            return 1
        if any(c in [mn, mx] for c in s[1:-1]):
            return 2
        return 3
```

#### Java

```java
class Solution {
    public int minOperations(String s) {
        boolean isSorted = true;
        char[] cs = s.toCharArray();
        int n = cs.length;
        char mn = cs[0], mx = cs[0];
        for (int i = 1; i < n; ++i) {
            mn = (char) Math.min(mn, cs[i]);
            mx = (char) Math.max(mx, cs[i]);
            if (cs[i] < cs[i - 1]) {
                isSorted = false;
            }
        }
        if (isSorted) {
            return 0;
        }
        if (n == 2) {
            return -1;
        }
        if (cs[0] == mn || cs[n - 1] == mx) {
            return 1;
        }
        for (int i = 1; i < n - 1; ++i) {
            if (cs[i] == mn || cs[i] == mx) {
                return 2;
            }
        }
        return 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minOperations(string s) {
        int n = s.size();
        bool sorted = true;

        for (int i = 1; i < n; ++i) {
            if (s[i] < s[i - 1]) {
                sorted = false;
                break;
            }
        }

        if (sorted) {
            return 0;
        }

        if (n == 2) {
            return -1;
        }

        char mn = *min_element(s.begin(), s.end());
        char mx = *max_element(s.begin(), s.end());

        if (s[0] == mn || s[n - 1] == mx) {
            return 1;
        }

        for (int i = 1; i < n - 1; ++i) {
            if (s[i] == mn || s[i] == mx) {
                return 2;
            }
        }

        return 3;
    }
};
```

#### Go

```go
func minOperations(s string) int {
	n := len(s)

	sorted := true
	for i := 1; i < n; i++ {
		if s[i] < s[i-1] {
			sorted = false
			break
		}
	}

	if sorted {
		return 0
	}

	if n == 2 {
		return -1
	}

	mn, mx := s[0], s[0]
	for i := 1; i < n; i++ {
		if s[i] < mn {
			mn = s[i]
		}
		if s[i] > mx {
			mx = s[i]
		}
	}

	if s[0] == mn || s[n-1] == mx {
		return 1
	}

	for i := 1; i < n-1; i++ {
		if s[i] == mn || s[i] == mx {
			return 2
		}
	}

	return 3
}
```

#### TypeScript

```ts
function minOperations(s: string): number {
    const n = s.length;

    let sorted = true;
    for (let i = 1; i < n; i++) {
        if (s[i] < s[i - 1]) {
            sorted = false;
            break;
        }
    }

    if (sorted) {
        return 0;
    }

    if (n === 2) {
        return -1;
    }

    let mn = s[0];
    let mx = s[0];

    for (const c of s) {
        if (c < mn) {
            mn = c;
        }
        if (c > mx) {
            mx = c;
        }
    }

    if (s[0] === mn || s[n - 1] === mx) {
        return 1;
    }

    for (let i = 1; i < n - 1; i++) {
        if (s[i] === mn || s[i] === mx) {
            return 2;
        }
    }

    return 3;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
