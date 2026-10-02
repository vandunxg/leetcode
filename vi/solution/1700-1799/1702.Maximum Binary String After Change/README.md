---
comments: true
difficulty: Medium
rating: 1825
source: Biweekly Contest 42 Q3
tags:
    - Greedy
    - String
---

<!-- problem:start -->

# [1702. Maximum Binary String After Change](https://leetcode.com/problems/maximum-binary-string-after-change)

[中文文档](/solution/1700-1799/1702.Maximum%20Binary%20String%20After%20Change/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi nhị phân <code>binary</code> chỉ gồm các ký tự <code>0</code> và <code>1</code>. Bạn có thể thực hiện mỗi phép biến đổi sau một số lần bất kỳ:</p>

<ul>
	<li>Phép biến đổi 1: Nếu chuỗi chứa chuỗi con <code>&quot;00&quot;</code>, bạn có thể thay nó bằng <code>&quot;10&quot;</code>.

    <ul>
      <li>Ví dụ, <code>&quot;<u>00</u>010&quot; -&gt; &quot;<u>10</u>010</code>&quot;</li>
    </ul>
    </li>
    <li>Phép biến đổi 2: Nếu chuỗi chứa chuỗi con <code>&quot;10&quot;</code>, bạn có thể thay nó bằng <code>&quot;01&quot;</code>.
    <ul>
      <li>Ví dụ, <code>&quot;000<u>10</u>&quot; -&gt; &quot;000<u>01</u>&quot;</code></li>
    </ul>
    </li>

</ul>

<p><em>Hãy trả về <strong>chuỗi nhị phân lớn nhất</strong> có thể nhận được sau một số phép biến đổi bất kỳ. Chuỗi nhị phân <code>x</code> lớn hơn chuỗi <code>y</code> nếu giá trị thập phân của <code>x</code> lớn hơn giá trị thập phân của <code>y</code>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> binary = &quot;000110&quot;
<strong>Đầu ra:</strong> &quot;111011&quot;
<strong>Giải thích:</strong> Một dãy biến đổi hợp lệ là:
&quot;0001<u>10</u>&quot; -&gt; &quot;0001<u>01</u>&quot;
&quot;<u>00</u>0101&quot; -&gt; &quot;<u>10</u>0101&quot;
&quot;1<u>00</u>101&quot; -&gt; &quot;1<u>10</u>101&quot;
&quot;110<u>10</u>1&quot; -&gt; &quot;110<u>01</u>1&quot;
&quot;11<u>00</u>11&quot; -&gt; &quot;11<u>10</u>11&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> binary = &quot;01&quot;
<strong>Đầu ra:</strong> &quot;01&quot;
<strong>Giải thích:</strong>&nbsp;&quot;01&quot; không thể biến đổi thêm.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= binary.length &lt;= 10<sup>5</sup></code></li>
	<li><code>binary</code> chỉ gồm <code>&#39;0&#39;</code> và <code>&#39;1&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Suy luận nhanh

<!-- thinking:start -->

> **Tư duy**
>
> Các phép biến đổi thay $00$ bằng $10$ và $10$ bằng $01$. Thử mọi cách biến đổi sẽ tăng theo cấp số nhân theo độ dài chuỗi và không thể hoàn thành trong giới hạn đề bài.
>
> Phép biến đổi 2 có thể đẩy mọi $1$ sang phải; phép biến đổi 1 biến một đoạn các số 0 thành các số 1 theo sau bởi một $0$. Vì vậy chuỗi tối ưu có nhiều nhất một $0$, nằm càng về bên phải càng tốt.
>
> Các số 1 đứng trước $0$ đầu tiên không thể thay đổi. Các số 0 phía sau có thể gom về một chỉ số: nếu $0$ đầu tiên ở vị trí $k$, cộng số lượng số 0 phía sau để tìm vị trí cuối của $0$, rồi điền các vị trí còn lại bằng số 1.

<!-- thinking:end -->

Ta nhận thấy phép biến đổi $2$ có thể đưa mọi $1$s về cuối chuỗi, còn phép biến đổi $1$ có thể đổi chuỗi `0000..000` thành `111..110`.

Do đó, để có chuỗi nhị phân lớn nhất, ta đưa mọi $1$s không nằm ở đầu về cuối chuỗi, tạo thành dạng `111..11...000..00..11`. Sau đó dùng phép biến đổi $1$ để đổi phần `000..00` ở giữa thành `111..10`. Cuối cùng chuỗi có nhiều nhất một $0$, chính là chuỗi lớn nhất cần tìm.

Trong code, trước hết ta kiểm tra chuỗi có chứa $0$ hay không. Nếu không, ta trả về chuỗi ban đầu. Nếu có, ta tìm vị trí $k$ của $0$ đầu tiên, cộng số lượng $0$ phía sau vị trí này để có vị trí của $0$ trong chuỗi mới. Các vị trí còn lại đều là $1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài chuỗi.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumBinaryString(self, binary: str) -> str:
        k = binary.find('0')
        if k == -1:
            return binary
        k += binary[k + 1 :].count('0')
        return '1' * k + '0' + '1' * (len(binary) - k - 1)
```

#### Java

```java
class Solution {
    public String maximumBinaryString(String binary) {
        int k = binary.indexOf('0');
        if (k == -1) {
            return binary;
        }
        int n = binary.length();
        for (int i = k + 1; i < n; ++i) {
            if (binary.charAt(i) == '0') {
                ++k;
            }
        }
        char[] ans = binary.toCharArray();
        Arrays.fill(ans, '1');
        ans[k] = '0';
        return String.valueOf(ans);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string maximumBinaryString(string binary) {
        int k = binary.find('0');
        if (k == binary.npos) {
            return binary;
        }
        int n = binary.size();
        for (int i = k + 1; i < n; ++i) {
            if (binary[i] == '0') {
                ++k;
            }
        }
        return string(k, '1') + '0' + string(n - k - 1, '1');
    }
};
```

#### Go

```go
func maximumBinaryString(binary string) string {
	k := strings.IndexByte(binary, '0')
	if k == -1 {
		return binary
	}
	for _, c := range binary[k+1:] {
		if c == '0' {
			k++
		}
	}
	ans := []byte(binary)
	for i := range ans {
		ans[i] = '1'
	}
	ans[k] = '0'
	return string(ans)
}
```

#### TypeScript

```ts
function maximumBinaryString(binary: string): string {
    let k = binary.indexOf('0');
    if (k === -1) {
        return binary;
    }
    k += binary.slice(k + 1).split('0').length - 1;
    return '1'.repeat(k) + '0' + '1'.repeat(binary.length - k - 1);
}
```

#### Rust

```rust
impl Solution {
    pub fn maximum_binary_string(binary: String) -> String {
        if let Some(k) = binary.find('0') {
            let k = k + binary[k + 1..].chars().filter(|&c| c == '0').count();
            return format!(
                "{}{}{}",
                "1".repeat(k),
                "0",
                "1".repeat(binary.len() - k - 1)
            );
        }
        binary
    }
}
```

#### C#

```cs
public class Solution {
    public string MaximumBinaryString(string binary) {
        int k = binary.IndexOf('0');
        if (k == -1) {
            return binary;
        }
        k += binary.Substring(k + 1).Count(c => c == '0');
        return new string('1', k) + '0' + new string('1', binary.Length - k - 1);
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
