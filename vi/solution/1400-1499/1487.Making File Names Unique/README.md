---
comments: true
difficulty: Medium
rating: 1696
source: Weekly Contest 194 Q2
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [1487. Making File Names Unique](https://leetcode.com/problems/making-file-names-unique)

[中文文档](/solution/1400-1499/1487.Making%20File%20Names%20Unique/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi <code>names</code> có kích thước <code>n</code>. Bạn sẽ tạo <code>n</code> thư mục trong file system <strong>sao cho</strong>, vào <code>phút thứ i</code>, bạn tạo một thư mục có tên <code>names[i]</code>.</p>

<p>Vì hai file <strong>không thể</strong> có cùng tên, nếu bạn nhập một tên thư mục đã được sử dụng trước đó, system sẽ thêm hậu tố vào tên dưới dạng <code>(k)</code>, trong đó <code>k</code> là <strong>số nguyên dương nhỏ nhất</strong> sao cho tên nhận được vẫn là duy nhất.</p>

<p>Trả về <em>một mảng chuỗi có độ dài </em><code>n</code>, trong đó <code>ans[i]</code> là tên thực tế mà system gán cho thư mục thứ <code>i</code> khi bạn tạo thư mục đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> names = [&quot;pes&quot;,&quot;fifa&quot;,&quot;gta&quot;,&quot;pes(2019)&quot;]
<strong>Output:</strong> [&quot;pes&quot;,&quot;fifa&quot;,&quot;gta&quot;,&quot;pes(2019)&quot;]
<strong>Giải thích:</strong> Hãy xem file system tạo tên thư mục như thế nào:
&quot;pes&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;pes&quot;
&quot;fifa&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;fifa&quot;
&quot;gta&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;gta&quot;
&quot;pes(2019)&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;pes(2019)&quot;
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> names = [&quot;gta&quot;,&quot;gta(1)&quot;,&quot;gta&quot;,&quot;avalon&quot;]
<strong>Output:</strong> [&quot;gta&quot;,&quot;gta(1)&quot;,&quot;gta(2)&quot;,&quot;avalon&quot;]
<strong>Giải thích:</strong> Hãy xem file system tạo tên thư mục như thế nào:
&quot;gta&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;gta&quot;
&quot;gta(1)&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;gta(1)&quot;
&quot;gta&quot; --&gt; tên này đã được sử dụng, system thêm (k); vì &quot;gta(1)&quot; cũng đã được sử dụng, system đặt k = 2. Tên trở thành &quot;gta(2)&quot;
&quot;avalon&quot; --&gt; chưa được gán trước đó, giữ nguyên là &quot;avalon&quot;
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> names = [&quot;onepiece&quot;,&quot;onepiece(1)&quot;,&quot;onepiece(2)&quot;,&quot;onepiece(3)&quot;,&quot;onepiece&quot;]
<strong>Output:</strong> [&quot;onepiece&quot;,&quot;onepiece(1)&quot;,&quot;onepiece(2)&quot;,&quot;onepiece(3)&quot;,&quot;onepiece(4)&quot;]
<strong>Giải thích:</strong> Khi thư mục cuối cùng được tạo, k nhỏ nhất hợp lệ là 4, nên tên thư mục trở thành &quot;onepiece(4)&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= names.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= names[i].length &lt;= 20</code></li>
	<li><code>names[i]</code> chỉ gồm các chữ cái tiếng Anh viết thường, chữ số và/hoặc dấu ngoặc tròn.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 5\times 10^4$. Gán tên theo thứ tự mà không để xảy ra trùng lặp. Một map lưu hậu tố $k$ tiếp theo cần thử; khi bị trùng, lần lượt kiểm tra $name(k),name(k+1),\ldots$ cho đến khi tìm được tên chưa được sử dụng, sau đó tăng $k$.

<!-- thinking:end -->

Ta có thể dùng một hash table $d$ để ghi lại chỉ số nhỏ nhất còn có thể dùng cho mỗi tên thư mục, trong đó $d[name] = k$ nghĩa là chỉ số nhỏ nhất còn có thể dùng cho thư mục $name$ là $k$. Ban đầu, $d$ rỗng vì chưa có thư mục nào.

Tiếp theo, ta duyệt qua mảng tên thư mục. Với mỗi tên file $name$:

- Nếu $name$ đã có trong $d$, nghĩa là thư mục $name$ đã tồn tại và ta cần tìm một tên thư mục mới. Ta có thể lần lượt thử $name(k)$, với $k$ bắt đầu từ $d[name]$, cho đến khi tìm được một tên thư mục $name(k)$ chưa có trong $d$. Ta thêm $name(k)$ vào $d$, cập nhật $d[name]$ thành $k + 1$, rồi cập nhật $name$ thành $name(k)$.
- Nếu $name$ chưa có trong $d$, ta có thể thêm trực tiếp $name$ vào $d$ và đặt $d[name]$ bằng $1$.
- Sau đó, ta thêm $name$ vào mảng kết quả và chuyển sang tên file tiếp theo.

Sau khi duyệt qua tất cả tên file, ta thu được mảng kết quả.

> Trong phần cài đặt bên dưới, ta sửa trực tiếp mảng $names$ mà không dùng thêm một mảng kết quả.

Độ phức tạp là $O(L)$, và độ phức tạp không gian là $O(L)$, trong đó $L$ là tổng độ dài của tất cả tên file trong mảng $names$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getFolderNames(self, names: List[str]) -> List[str]:
        d = defaultdict(int)
        for i, name in enumerate(names):
            if name in d:
                k = d[name]
                while f'{name}({k})' in d:
                    k += 1
                d[name] = k + 1
                names[i] = f'{name}({k})'
            d[names[i]] = 1
        return names
```

#### Java

```java
class Solution {
    public String[] getFolderNames(String[] names) {
        Map<String, Integer> d = new HashMap<>();
        for (int i = 0; i < names.length; ++i) {
            if (d.containsKey(names[i])) {
                int k = d.get(names[i]);
                while (d.containsKey(names[i] + "(" + k + ")")) {
                    ++k;
                }
                d.put(names[i], k);
                names[i] += "(" + k + ")";
            }
            d.put(names[i], 1);
        }
        return names;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> getFolderNames(vector<string>& names) {
        unordered_map<string, int> d;
        for (auto& name : names) {
            int k = d[name];
            if (k) {
                while (d[name + "(" + to_string(k) + ")"]) {
                    k++;
                }
                d[name] = k;
                name += "(" + to_string(k) + ")";
            }
            d[name] = 1;
        }
        return names;
    }
};
```

#### Go

```go
func getFolderNames(names []string) []string {
	d := map[string]int{}
	for i, name := range names {
		if k, ok := d[name]; ok {
			for {
				newName := fmt.Sprintf("%s(%d)", name, k)
				if d[newName] == 0 {
					d[name] = k + 1
					names[i] = newName
					break
				}
				k++
			}
		}
		d[names[i]] = 1
	}
	return names
}
```

#### TypeScript

```ts
function getFolderNames(names: string[]): string[] {
    let d: Map<string, number> = new Map();
    for (let i = 0; i < names.length; ++i) {
        if (d.has(names[i])) {
            let k: number = d.get(names[i]) || 0;
            while (d.has(names[i] + '(' + k + ')')) {
                ++k;
            }
            d.set(names[i], k);
            names[i] += '(' + k + ')';
        }
        d.set(names[i], 1);
    }
    return names;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
