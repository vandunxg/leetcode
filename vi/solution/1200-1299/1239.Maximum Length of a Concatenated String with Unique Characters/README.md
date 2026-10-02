---
comments: true
difficulty: Medium
rating: 1719
source: Weekly Contest 160 Q3
tags:
    - Bit Manipulation
    - Array
    - String
    - Backtracking
---

<!-- problem:start -->

# [1239. Maximum Length of a Concatenated String with Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters)

[中文文档](/solution/1200-1299/1239.Maximum%20Length%20of%20a%20Concatenated%20String%20with%20Unique%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng chuỗi <code>arr</code>. Chuỗi <code>s</code> được tạo bằng cách <strong>nối</strong> một <strong>dãy con</strong> của <code>arr</code> sao cho các ký tự trong chuỗi tạo thành đều <strong>không trùng lặp</strong>.</p>

<p>Trả về <em>độ dài lớn nhất có thể</em> của <code>s</code>.</p>

<p><strong>Dãy con</strong> là mảng có thể tạo từ một mảng khác bằng cách xóa một số phần tử (hoặc không xóa phần tử nào) mà vẫn giữ nguyên thứ tự các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [&quot;un&quot;,&quot;iq&quot;,&quot;ue&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các cách nối hợp lệ gồm:
- &quot;&quot;
- &quot;un&quot;
- &quot;iq&quot;
- &quot;ue&quot;
- &quot;uniq&quot; (&quot;un&quot; + &quot;iq&quot;)
- &quot;ique&quot; (&quot;iq&quot; + &quot;ue&quot;)
Độ dài lớn nhất là 4.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [&quot;cha&quot;,&quot;r&quot;,&quot;act&quot;,&quot;ers&quot;]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các chuỗi nối hợp lệ dài nhất có thể là &quot;chaers&quot; (&quot;cha&quot; + &quot;ers&quot;) và &quot;acters&quot; (&quot;act&quot; + &quot;ers&quot;).
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [&quot;abcdefghijklmnopqrstuvwxyz&quot;]
<strong>Đầu ra:</strong> 26
<strong>Giải thích:</strong> Chuỗi duy nhất trong arr chứa đủ 26 ký tự.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 16</code></li>
	<li><code>1 &lt;= arr[i].length &lt;= 26</code></li>
	<li><code>arr[i]</code> chỉ chứa các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén trạng thái + Thao tác bit

<!-- thinking:start -->

> **Tư duy**
>
> Các dãy con được nối phải có các chữ cái không trùng lặp. Vì $|arr|\le 16$, có thể duyệt hết $2^{16}$ tập con. Hai mươi sáu chữ cái vừa trong một mask số nguyên.
>
> Bắt đầu từ mask rỗng, chỉ ghép một chuỗi có ký tự duy nhất với mask hiện có khi phép AND bitwise giữa chúng bằng 0. Đáp án là popcount lớn nhất trong các mask có thể đạt được. Thao tác bit giúp các phép toán trên tập hợp chạy trong thời gian hằng số.

<!-- thinking:end -->

Vì các ký tự trong dãy con không được trùng lặp và đều là chữ cái viết thường, ta có thể dùng một số nhị phân dài $26$ bit để biểu diễn dãy con. Bit thứ $i$ bằng $1$ nghĩa là dãy con có chứa ký tự thứ $i$, còn bằng $0$ nghĩa là không chứa ký tự đó.

Dùng mảng $s$ để lưu trạng thái của mọi dãy con thỏa điều kiện. Ban đầu, $s$ chỉ chứa một phần tử là $0$.

Sau đó, duyệt mảng $\textit{arr}$. Với mỗi chuỗi $t$, dùng số nguyên $x$ để biểu diễn trạng thái của $t$. Tiếp tục duyệt mảng $s$. Với mỗi trạng thái $y$, nếu $x$ và $y$ không có ký tự chung, thêm hợp của $x$ và $y$ vào $s$ rồi cập nhật đáp án.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(2^n + L)$ và độ phức tạp không gian là $O(2^n)$, trong đó $n$ là số chuỗi trong mảng và $L$ là tổng độ dài của tất cả chuỗi trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxLength(self, arr: List[str]) -> int:
        s = [0]
        for t in arr:
            x = 0
            for b in map(lambda c: ord(c) - 97, t):
                if x >> b & 1:
                    x = 0
                    break
                x |= 1 << b
            if x:
                s.extend((x | y) for y in s if (x & y) == 0)
        return max(x.bit_count() for x in s)
```

#### Java

```java
class Solution {
    public int maxLength(List<String> arr) {
        List<Integer> s = new ArrayList<>();
        s.add(0);
        int ans = 0;
        for (var t : arr) {
            int x = 0;
            for (char c : t.toCharArray()) {
                int b = c - 'a';
                if ((x >> b & 1) == 1) {
                    x = 0;
                    break;
                }
                x |= 1 << b;
            }
            if (x > 0) {
                for (int i = s.size() - 1; i >= 0; --i) {
                    int y = s.get(i);
                    if ((x & y) == 0) {
                        s.add(x | y);
                        ans = Math.max(ans, Integer.bitCount(x | y));
                    }
                }
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
    int maxLength(vector<string>& arr) {
        vector<int> s = {0};
        int ans = 0;
        for (const string& t : arr) {
            int x = 0;
            for (char c : t) {
                int b = c - 'a';
                if ((x >> b & 1) == 1) {
                    x = 0;
                    break;
                }
                x |= 1 << b;
            }
            if (x > 0) {
                for (int i = s.size() - 1; i >= 0; --i) {
                    int y = s[i];
                    if ((x & y) == 0) {
                        s.push_back(x | y);
                        ans = max(ans, __builtin_popcount(x | y));
                    }
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func maxLength(arr []string) (ans int) {
	s := []int{0}
	for _, t := range arr {
		x := 0
		for _, c := range t {
			b := int(c - 'a')
			if (x>>b)&1 == 1 {
				x = 0
				break
			}
			x |= 1 << b
		}
		if x > 0 {
			for i := len(s) - 1; i >= 0; i-- {
				y := s[i]
				if (x & y) == 0 {
					s = append(s, x|y)
					ans = max(ans, bits.OnesCount(uint(x|y)))
				}
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxLength(arr: string[]): number {
    const s: number[] = [0];
    let ans = 0;
    for (const t of arr) {
        let x = 0;
        for (const c of t) {
            const b = c.charCodeAt(0) - 97;
            if ((x >> b) & 1) {
                x = 0;
                break;
            }
            x |= 1 << b;
        }

        if (x > 0) {
            for (let i = s.length - 1; ~i; --i) {
                const y = s[i];
                if ((x & y) === 0) {
                    s.push(x | y);
                    ans = Math.max(ans, bitCount(x | y));
                }
            }
        }
    }

    return ans;
}

function bitCount(i: number): number {
    i = i - ((i >>> 1) & 0x55555555);
    i = (i & 0x33333333) + ((i >>> 2) & 0x33333333);
    i = (i + (i >>> 4)) & 0x0f0f0f0f;
    i = i + (i >>> 8);
    i = i + (i >>> 16);
    return i & 0x3f;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
