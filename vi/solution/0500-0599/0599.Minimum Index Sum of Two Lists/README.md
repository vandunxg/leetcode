---
comments: true
difficulty: Easy
tags:
    - Array
    - Hash Table
    - String
---

<!-- problem:start -->

# [599. Minimum Index Sum of Two Lists](https://leetcode.com/problems/minimum-index-sum-of-two-lists)

[中文文档](/solution/0500-0599/0599.Minimum%20Index%20Sum%20of%20Two%20Lists/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng chuỗi <code>list1</code> và <code>list2</code>, hãy tìm các <strong>chuỗi chung có tổng chỉ số nhỏ nhất</strong>.</p>

<p><strong>Chuỗi chung</strong> là chuỗi xuất hiện trong cả <code>list1</code> và <code>list2</code>.</p>

<p><strong>Chuỗi chung có tổng chỉ số nhỏ nhất</strong> là chuỗi chung xuất hiện tại <code>list1[i]</code> và <code>list2[j]</code> sao cho <code>i + j</code> là giá trị nhỏ nhất trong số tất cả các <strong>chuỗi chung</strong>.</p>

<p>Hãy trả về <em>tất cả <strong>chuỗi chung có tổng chỉ số nhỏ nhất</strong></em>. Kết quả có thể theo <strong>bất kỳ thứ tự nào</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> list1 = [&quot;Shogun&quot;,&quot;Tapioca Express&quot;,&quot;Burger King&quot;,&quot;KFC&quot;], list2 = [&quot;Piatti&quot;,&quot;The Grill at Torrey Pines&quot;,&quot;Hungry Hunter Steakhouse&quot;,&quot;Shogun&quot;]
<strong>Đầu ra:</strong> [&quot;Shogun&quot;]
<strong>Giải thích:</strong> Chuỗi chung duy nhất là &quot;Shogun&quot;.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> list1 = [&quot;Shogun&quot;,&quot;Tapioca Express&quot;,&quot;Burger King&quot;,&quot;KFC&quot;], list2 = [&quot;KFC&quot;,&quot;Shogun&quot;,&quot;Burger King&quot;]
<strong>Đầu ra:</strong> [&quot;Shogun&quot;]
<strong>Giải thích:</strong> Chuỗi chung có tổng chỉ số nhỏ nhất là &quot;Shogun&quot;, với tổng chỉ số = (0 + 1) = 1.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> list1 = [&quot;happy&quot;,&quot;sad&quot;,&quot;good&quot;], list2 = [&quot;sad&quot;,&quot;happy&quot;,&quot;good&quot;]
<strong>Đầu ra:</strong> [&quot;sad&quot;,&quot;happy&quot;]
<strong>Giải thích:</strong> Có ba chuỗi chung:
&quot;happy&quot; có tổng chỉ số = (0 + 1) = 1.
&quot;sad&quot; có tổng chỉ số = (1 + 0) = 1.
&quot;good&quot; có tổng chỉ số = (2 + 2) = 4.
Các chuỗi có tổng chỉ số nhỏ nhất là &quot;sad&quot; và &quot;happy&quot;.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= list1.length, list2.length &lt;= 1000</code></li>
	<li><code>1 &lt;= list1[i].length, list2[i].length &lt;= 30</code></li>
	<li><code>list1[i]</code> và <code>list2[i]</code> chỉ gồm dấu cách <code>&#39; &#39;</code> và các chữ cái tiếng Anh.</li>
	<li>Tất cả chuỗi trong <code>list1</code> đều <strong>khác nhau</strong>.</li>
	<li>Tất cả chuỗi trong <code>list2</code> đều <strong>khác nhau</strong>.</li>
	<li><code>list1</code> và <code>list2</code> có ít nhất một chuỗi chung.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash table

<!-- thinking:start -->

> **Tư duy**
>
> Trong các chuỗi chung, giữ lại những chuỗi có tổng chỉ số nhỏ nhất. Quét danh sách còn lại cho từng chuỗi chung sẽ tốn $O(nm)$.
>
> Tạo map từ chuỗi sang chỉ số trong `list2`, rồi duyệt `list1` để tính tổng chỉ số. Theo dõi giá trị nhỏ nhất hiện tại: thêm vào kết quả nếu bằng nhau, tạo lại danh sách khi tìm được tổng nhỏ hơn. Chỉ cần tạo hash một lần và duyệt tuyến tính một lần.

<!-- thinking:end -->

Ta dùng hash table $\textit{d}$ để lưu các chuỗi trong $\textit{list2}$ cùng chỉ số của chúng, và biến $\textit{mi}$ để lưu tổng chỉ số nhỏ nhất.

Tiếp theo, ta duyệt $\textit{list1}$. Với mỗi chuỗi $\textit{s}$, nếu $\textit{s}$ xuất hiện trong $\textit{list2}$, ta xác định chỉ số $\textit{i}$ của $\textit{s}$ trong $\textit{list1}$ và chỉ số $\textit{j}$ trong $\textit{list2}$. Nếu $\textit{i} + \textit{j} < \textit{mi}$, cập nhật mảng đáp án $\textit{ans}$ thành $\textit{s}$ và cập nhật $\textit{mi}$ thành $\textit{i} + \textit{j}$. Nếu $\textit{i} + \textit{j} = \textit{mi}$, thêm $\textit{s}$ vào mảng đáp án $\textit{ans}$.

Sau khi duyệt xong, trả về mảng đáp án $\textit{ans}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRestaurant(self, list1: List[str], list2: List[str]) -> List[str]:
        d = {s: i for i, s in enumerate(list2)}
        ans = []
        mi = inf
        for i, s in enumerate(list1):
            if s in d:
                j = d[s]
                if i + j < mi:
                    mi = i + j
                    ans = [s]
                elif i + j == mi:
                    ans.append(s)
        return ans
```

#### Java

```java
class Solution {
    public String[] findRestaurant(String[] list1, String[] list2) {
        Map<String, Integer> d = new HashMap<>();
        for (int i = 0; i < list2.length; ++i) {
            d.put(list2[i], i);
        }
        List<String> ans = new ArrayList<>();
        int mi = 1 << 30;
        for (int i = 0; i < list1.length; ++i) {
            if (d.containsKey(list1[i])) {
                int j = d.get(list1[i]);
                if (i + j < mi) {
                    mi = i + j;
                    ans.clear();
                    ans.add(list1[i]);
                } else if (i + j == mi) {
                    ans.add(list1[i]);
                }
            }
        }
        return ans.toArray(new String[0]);
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findRestaurant(vector<string>& list1, vector<string>& list2) {
        unordered_map<string, int> d;
        for (int i = 0; i < list2.size(); ++i) {
            d[list2[i]] = i;
        }
        vector<string> ans;
        int mi = INT_MAX;
        for (int i = 0; i < list1.size(); ++i) {
            if (d.contains(list1[i])) {
                int j = d[list1[i]];
                if (i + j < mi) {
                    mi = i + j;
                    ans.clear();
                    ans.push_back(list1[i]);
                } else if (i + j == mi) {
                    ans.push_back(list1[i]);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findRestaurant(list1 []string, list2 []string) []string {
	d := map[string]int{}
	for i, s := range list2 {
		d[s] = i
	}
	ans := []string{}
	mi := 1 << 30
	for i, s := range list1 {
		if j, ok := d[s]; ok {
			if i+j < mi {
				mi = i + j
				ans = []string{s}
			} else if i+j == mi {
				ans = append(ans, s)
			}
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findRestaurant(list1: string[], list2: string[]): string[] {
    const d = new Map<string, number>(list2.map((s, i) => [s, i]));
    let mi = Infinity;
    const ans: string[] = [];
    list1.forEach((s, i) => {
        if (d.has(s)) {
            const j = d.get(s)!;
            if (i + j < mi) {
                mi = i + j;
                ans.length = 0;
                ans.push(s);
            } else if (i + j === mi) {
                ans.push(s);
            }
        }
    });
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn find_restaurant(list1: Vec<String>, list2: Vec<String>) -> Vec<String> {
        let mut d = HashMap::new();
        for (i, s) in list2.iter().enumerate() {
            d.insert(s, i);
        }

        let mut ans = Vec::new();
        let mut mi = std::i32::MAX;

        for (i, s) in list1.iter().enumerate() {
            if let Some(&j) = d.get(s) {
                if (i as i32 + j as i32) < mi {
                    mi = i as i32 + j as i32;
                    ans = vec![s.clone()];
                } else if (i as i32 + j as i32) == mi {
                    ans.push(s.clone());
                }
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
