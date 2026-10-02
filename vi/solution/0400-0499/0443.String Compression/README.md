---
comments: true
difficulty: Medium
tags:
    - Two Pointers
    - String
---

<!-- problem:start -->

# [443. String Compression](https://leetcode.com/problems/string-compression)

[中文文档](/solution/0400-0499/0443.String%20Compression/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng ký tự <code>chars</code>, hãy nén mảng theo thuật toán sau:</p>

<p>Bắt đầu với chuỗi rỗng <code>s</code>. Với mỗi nhóm ký tự <strong>lặp lại liên tiếp</strong> trong <code>chars</code>:</p>

<ul>
	<li>Nếu độ dài nhóm là <code>1</code>, nối ký tự đó vào <code>s</code>.</li>
	<li>Nếu không, nối ký tự đó rồi đến độ dài của nhóm vào <code>s</code>.</li>
</ul>

<p>Chuỗi đã nén <code>s</code> <strong>không được trả về riêng</strong>; thay vào đó, hãy lưu nó <strong>ngay trong mảng ký tự đầu vào <code>chars</code></strong>. Lưu ý rằng độ dài nhóm từ <code>10</code> trở lên sẽ được ghi thành nhiều ký tự trong <code>chars</code>.</p>

<p>Sau khi <strong>chỉnh sửa mảng đầu vào</strong>, hãy trả về <em>độ dài mới của mảng</em>.</p>

<p>Thuật toán chỉ được sử dụng lượng bộ nhớ phụ hằng số.</p>

<p><strong>Lưu ý: </strong>Các ký tự trong mảng nằm sau độ dài được trả về không quan trọng và nên được bỏ qua.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> chars = [&quot;a&quot;,&quot;a&quot;,&quot;b&quot;,&quot;b&quot;,&quot;c&quot;,&quot;c&quot;,&quot;c&quot;]
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Các nhóm là <code>&quot;aa&quot;</code>, <code>&quot;bb&quot;</code> và <code>&quot;ccc&quot;</code>. Sau khi nén, ta được <code>&quot;a2b2c3&quot;</code>.
Sau khi chỉnh sửa trực tiếp mảng đầu vào, 6 ký tự đầu tiên của <code>chars</code> phải là <code>[&quot;a&quot;,&quot;2&quot;,&quot;b&quot;,&quot;2&quot;,&quot;c&quot;,&quot;3&quot;]</code>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> chars = [&quot;a&quot;]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Nhóm duy nhất là <code>&quot;a&quot;</code>; nhóm này không bị nén vì chỉ có một ký tự.
Sau khi chỉnh sửa trực tiếp mảng đầu vào, ký tự đầu tiên của <code>chars</code> phải là <code>[&quot;a&quot;]</code>.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> chars = [&quot;a&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;,&quot;b&quot;]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Các nhóm là <code>&quot;a&quot;</code> và <code>&quot;bbbbbbbbbbbb&quot;</code>. Sau khi nén, ta được <code>&quot;ab12&quot;</code>.
Sau khi chỉnh sửa trực tiếp mảng đầu vào, 4 ký tự đầu tiên của <code>chars</code> phải là <code>[&quot;a&quot;,&quot;b&quot;,&quot;1&quot;,&quot;2&quot;]</code>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= chars.length &lt;= 2000</code></li>
	<li><code>chars[i]</code> là chữ cái tiếng Anh viết thường, chữ hoa, chữ số hoặc ký hiệu.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi nhóm được thay bằng ký tự đó kèm số lần lặp (nếu số lần lặp lớn hơn $1$), ghi trực tiếp tại chỗ. Dùng buffer thứ hai sẽ không đáp ứng giới hạn bộ nhớ.
>
> Con trỏ đọc $i$ tìm từng nhóm; con trỏ ghi $k$ lưu ký tự và các chữ số biểu diễn số lần lặp nếu cần. Sau đó, $i$ chuyển đến nhóm tiếp theo.
>
> Con trỏ ghi không bao giờ vượt con trỏ đọc: đoạn đã nén không dài hơn đoạn ban đầu, nên ghi đè từ trái sang phải là an toàn. Trả về $k$ làm độ dài mới.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def compress(self, chars: List[str]) -> int:
        i, k, n = 0, 0, len(chars)
        while i < n:
            j = i + 1
            while j < n and chars[j] == chars[i]:
                j += 1
            chars[k] = chars[i]
            k += 1
            if j - i > 1:
                cnt = str(j - i)
                for c in cnt:
                    chars[k] = c
                    k += 1
            i = j
        return k
```

#### Java

```java
class Solution {
    public int compress(char[] chars) {
        int k = 0, n = chars.length;
        for (int i = 0, j = i + 1; i < n;) {
            while (j < n && chars[j] == chars[i]) {
                ++j;
            }
            chars[k++] = chars[i];
            if (j - i > 1) {
                String cnt = String.valueOf(j - i);
                for (char c : cnt.toCharArray()) {
                    chars[k++] = c;
                }
            }
            i = j;
        }
        return k;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int compress(vector<char>& chars) {
        int k = 0, n = chars.size();
        for (int i = 0, j = i + 1; i < n;) {
            while (j < n && chars[j] == chars[i])
                ++j;
            chars[k++] = chars[i];
            if (j - i > 1) {
                for (char c : to_string(j - i)) {
                    chars[k++] = c;
                }
            }
            i = j;
        }
        return k;
    }
};
```

#### Go

```go
func compress(chars []byte) int {
	i, k, n := 0, 0, len(chars)
	for i < n {
		j := i + 1
		for j < n && chars[j] == chars[i] {
			j++
		}
		chars[k] = chars[i]
		k++
		if j-i > 1 {
			cnt := strconv.Itoa(j - i)
			for _, c := range cnt {
				chars[k] = byte(c)
				k++
			}
		}
		i = j
	}
	return k
}
```

#### Rust

```rust
impl Solution {
    pub fn compress(chars: &mut Vec<char>) -> i32 {
        let (mut i, mut k, n) = (0, 0, chars.len());
        while i < n {
            let mut j = i + 1;
            while j < n && chars[j] == chars[i] {
                j += 1;
            }
            chars[k] = chars[i];
            k += 1;

            if j - i > 1 {
                let cnt = (j - i).to_string();
                for c in cnt.chars() {
                    chars[k] = c;
                    k += 1;
                }
            }
            i = j;
        }
        k as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
