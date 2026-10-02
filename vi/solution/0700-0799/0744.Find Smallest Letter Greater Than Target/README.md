---
comments: true
difficulty: Easy
tags:
    - Array
    - Binary Search
---

<!-- problem:start -->

# [744. Find Smallest Letter Greater Than Target](https://leetcode.com/problems/find-smallest-letter-greater-than-target)

[中文文档](/solution/0700-0799/0744.Find%20Smallest%20Letter%20Greater%20Than%20Target/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng ký tự <code>letters</code> được sắp xếp theo thứ tự <strong>không giảm</strong> và ký tự <code>target</code>. Trong <code>letters</code> có <strong>ít nhất hai ký tự khác nhau</strong>.</p>

<p>Hãy trả về <em>ký tự nhỏ nhất trong </em><code>letters</code><em> lớn hơn </em><code>target</code> <em>theo thứ tự từ điển</em>. Nếu không có ký tự nào như vậy, hãy trả về ký tự đầu tiên trong <code>letters</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> letters = [&quot;c&quot;,&quot;f&quot;,&quot;j&quot;], target = &quot;a&quot;
<strong>Đầu ra:</strong> &quot;c&quot;
<strong>Giải thích:</strong> Ký tự nhỏ nhất trong letters lớn hơn &#39;a&#39; theo thứ tự từ điển là &#39;c&#39;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> letters = [&quot;c&quot;,&quot;f&quot;,&quot;j&quot;], target = &quot;c&quot;
<strong>Đầu ra:</strong> &quot;f&quot;
<strong>Giải thích:</strong> Ký tự nhỏ nhất trong letters lớn hơn &#39;c&#39; theo thứ tự từ điển là &#39;f&#39;.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> letters = [&quot;x&quot;,&quot;x&quot;,&quot;y&quot;,&quot;y&quot;], target = &quot;z&quot;
<strong>Đầu ra:</strong> &quot;x&quot;
<strong>Giải thích:</strong> Không có ký tự nào trong letters lớn hơn &#39;z&#39; theo thứ tự từ điển, nên ta trả về letters[0].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= letters.length &lt;= 10<sup>4</sup></code></li>
	<li><code>letters[i]</code> là chữ cái tiếng Anh viết thường.</li>
	<li><code>letters</code> được sắp xếp theo thứ tự <strong>không giảm</strong>.</li>
	<li><code>letters</code> có ít nhất hai ký tự khác nhau.</li>
	<li><code>target</code> là chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Trong mảng ký tự không giảm, tìm ký tự nhỏ nhất lớn hơn nghiêm ngặt $\textit{target}$; nếu không có thì quay lại đầu mảng. Vì mảng đã sắp xếp nên ta dùng tìm kiếm nhị phân.
>
> Đây là vị trí upper bound. Nếu vị trí đó bằng $n$, đáp án là $letters[0]$; có thể lấy chỉ số modulo $n$.
>
> Dùng $\textit{bisect\_right}$ trên mã ký tự, sau đó lấy $letters[i\bmod n]$. Độ phức tạp $O(\log n)$.

<!-- thinking:end -->

Vì `letters` được sắp xếp theo thứ tự không giảm, ta có thể dùng tìm kiếm nhị phân để tìm ký tự nhỏ nhất lớn hơn `target`.

Ta đặt biên trái của tìm kiếm nhị phân là $l = 0$ và biên phải là $r = n$. Ở mỗi bước, tính vị trí giữa $mid = (l + r) / 2$. Nếu $letters[mid] > \textit{target}$, ta tiếp tục tìm ở nửa trái và đặt $r = mid$. Ngược lại, ta tìm ở nửa phải và đặt $l = mid + 1$.

Cuối cùng, trả về $letters[l \mod n]$.

Độ phức tạp thời gian là $O(\log n)$, với $n$ là độ dài của `letters`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        i = bisect_right(letters, ord(target), key=lambda c: ord(c))
        return letters[i % len(letters)]
```

#### Java

```java
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int i = Arrays.binarySearch(letters, (char) (target + 1));
        i = i < 0 ? -i - 1 : i;
        return letters[i % letters.length];
    }
}
```

#### C++

```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int i = upper_bound(letters.begin(), letters.end(), target) - letters.begin();
        return letters[i % letters.size()];
    }
};
```

#### Go

```go
func nextGreatestLetter(letters []byte, target byte) byte {
	i := sort.Search(len(letters), func(i int) bool { return letters[i] > target })
	return letters[i%len(letters)]
}
```

#### TypeScript

```ts
function nextGreatestLetter(letters: string[], target: string): string {
    const idx = _.sortedIndex(letters, target + '\0');
    return letters[idx % letters.length];
}
```

#### Rust

```rust
impl Solution {
    pub fn next_greatest_letter(letters: Vec<char>, target: char) -> char {
        let mut l = 0;
        let mut r = letters.len();
        while l < r {
            let mid = l + (r - l) / 2;
            if letters[mid] > target {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        letters[l % letters.len()]
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param String[] $letters
     * @param String $target
     * @return String
     */
    function nextGreatestLetter($letters, $target) {
        $l = 0;
        $r = count($letters);
        while ($l < $r) {
            $mid = ($l + $r) >> 1;
            if ($letters[$mid] > $target) {
                $r = $mid;
            } else {
                $l = $mid + 1;
            }
        }
        return $letters[$l % count($letters)];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
