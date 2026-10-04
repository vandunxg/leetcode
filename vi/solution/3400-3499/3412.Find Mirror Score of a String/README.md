---
comments: true
difficulty: Medium
rating: 1578
source: Weekly Contest 431 Q2
tags:
    - Stack
    - Hash Table
    - String
    - Simulation
---

<!-- problem:start -->

# [3412. Find Mirror Score of a String](https://leetcode.com/problems/find-mirror-score-of-a-string)

[中文文档](/solution/3400-3499/3412.Find%20Mirror%20Score%20of%20a%20String/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một chuỗi <code>s</code>.</p>

<p>Ta định nghĩa <strong>ký tự đối xứng</strong> của một chữ cái trong bảng chữ cái tiếng Anh là chữ cái tương ứng khi đảo ngược bảng chữ cái. Ví dụ, ký tự đối xứng của <code>&#39;a&#39;</code> là <code>&#39;z&#39;</code>, còn ký tự đối xứng của <code>&#39;y&#39;</code> là <code>&#39;b&#39;</code>.</p>

<p>Ban đầu, tất cả ký tự trong chuỗi <code>s</code> đều <strong>chưa được đánh dấu</strong>.</p>

<p>Bạn bắt đầu với điểm số bằng 0 và thực hiện quy trình sau trên chuỗi <code>s</code>:</p>

<ul>
	<li>Duyệt chuỗi từ trái sang phải.</li>
	<li>Tại mỗi chỉ số <code>i</code>, tìm chỉ số <strong>chưa được đánh dấu</strong> <code>j</code> gần nhất sao cho <code>j &lt; i</code> và <code>s[j]</code> là ký tự đối xứng của <code>s[i]</code>. Sau đó, <strong>đánh dấu</strong> cả hai chỉ số <code>i</code> và <code>j</code>, rồi cộng giá trị <code>i - j</code> vào tổng điểm.</li>
	<li>Nếu không tồn tại chỉ số <code>j</code> như vậy với chỉ số <code>i</code>, chuyển sang chỉ số tiếp theo mà không thay đổi gì.</li>
</ul>

<p>Trả về tổng điểm sau khi kết thúc quy trình.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;aczzx&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li><code>i = 0</code>. Không có chỉ số <code>j</code> nào thỏa mãn điều kiện, nên ta bỏ qua.</li>
	<li><code>i = 1</code>. Không có chỉ số <code>j</code> nào thỏa mãn điều kiện, nên ta bỏ qua.</li>
	<li><code>i = 2</code>. Chỉ số <code>j</code> gần nhất thỏa mãn điều kiện là <code>j = 0</code>, nên ta đánh dấu cả hai chỉ số 0 và 2, sau đó cộng <code>2 - 0 = 2</code> vào điểm số.</li>
	<li><code>i = 3</code>. Không có chỉ số <code>j</code> nào thỏa mãn điều kiện, nên ta bỏ qua.</li>
	<li><code>i = 4</code>. Chỉ số <code>j</code> gần nhất thỏa mãn điều kiện là <code>j = 1</code>, nên ta đánh dấu cả hai chỉ số 1 và 4, sau đó cộng <code>4 - 1 = 3</code> vào điểm số.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">s = &quot;abcdef&quot;</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Với mỗi chỉ số <code>i</code>, không có chỉ số <code>j</code> nào thỏa mãn điều kiện.</p>
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

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Một ký tự chưa được đánh dấu sẽ ghép với ký tự đối xứng chưa được đánh dấu gần nhất ở bên trái; điểm số là khoảng cách giữa hai chỉ số. Nếu duyệt sang trái tại mỗi vị trí thì độ phức tạp là bậc hai với $n\le 10^5$.
>
> Phép đối xứng là một phép involution. Việc ghép với ký tự đối xứng chưa dùng gần nhất tương đương với thao tác pop trên một stack riêng cho mỗi ký tự.
>
> Ta duy trì một stack các chỉ số chưa dùng cho mỗi chữ cái. Khi gặp $x$, nếu stack của ký tự đối xứng $y$ không rỗng thì ta pop $j$ và cộng $i-j$; nếu không, ta đẩy $i$ vào stack của $x$.

<!-- thinking:end -->

Ta có thể dùng một bảng băm $\textit{d}$ để lưu danh sách chỉ số của từng ký tự chưa được đánh dấu, trong đó key là ký tự và value là danh sách các chỉ số.

Ta duyệt chuỗi $\textit{s}$; với mỗi ký tự $\textit{x}$, ta tìm ký tự đối xứng $\textit{y}$. Nếu $\textit{d}$ chứa $\textit{y}$, ta lấy danh sách chỉ số $\textit{ls}$ tương ứng với $\textit{y}$, lấy phần tử cuối cùng $\textit{j}$ khỏi $\textit{ls}$, rồi xóa $\textit{j}$ khỏi $\textit{ls}$. Nếu $\textit{ls}$ trở thành rỗng, ta xóa $\textit{y}$ khỏi $\textit{d}$. Khi đó, ta đã tìm được một cặp chỉ số $(\textit{j}, \textit{i})$ thỏa mãn điều kiện, và cộng $\textit{i} - \textit{j}$ vào đáp án. Ngược lại, ta thêm $\textit{x}$ vào $\textit{d}$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của chuỗi $\textit{s}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def calculateScore(self, s: str) -> int:
        d = defaultdict(list)
        ans = 0
        for i, x in enumerate(s):
            y = chr(ord("a") + ord("z") - ord(x))
            if d[y]:
                j = d[y].pop()
                ans += i - j
            else:
                d[x].append(i)
        return ans
```

#### Java

```java
class Solution {
    public long calculateScore(String s) {
        Map<Character, List<Integer>> d = new HashMap<>(26);
        int n = s.length();
        long ans = 0;
        for (int i = 0; i < n; ++i) {
            char x = s.charAt(i);
            char y = (char) ('a' + 'z' - x);
            if (d.containsKey(y)) {
                var ls = d.get(y);
                int j = ls.remove(ls.size() - 1);
                if (ls.isEmpty()) {
                    d.remove(y);
                }
                ans += i - j;
            } else {
                d.computeIfAbsent(x, k -> new ArrayList<>()).add(i);
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
    long long calculateScore(string s) {
        unordered_map<char, vector<int>> d;
        int n = s.length();
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            char x = s[i];
            char y = 'a' + 'z' - x;
            if (d.contains(y)) {
                vector<int>& ls = d[y];
                int j = ls.back();
                ls.pop_back();
                if (ls.empty()) {
                    d.erase(y);
                }
                ans += i - j;
            } else {
                d[x].push_back(i);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func calculateScore(s string) (ans int64) {
	d := make(map[rune][]int)
	for i, x := range s {
		y := 'a' + 'z' - x
		if ls, ok := d[y]; ok {
			j := ls[len(ls)-1]
			d[y] = ls[:len(ls)-1]
			if len(d[y]) == 0 {
				delete(d, y)
			}
			ans += int64(i - j)
		} else {
			d[x] = append(d[x], i)
		}
	}
	return
}
```

#### TypeScript

```ts
function calculateScore(s: string): number {
    const d: Map<string, number[]> = new Map();
    const n = s.length;
    let ans = 0;
    for (let i = 0; i < n; i++) {
        const x = s[i];
        const y = String.fromCharCode('a'.charCodeAt(0) + 'z'.charCodeAt(0) - x.charCodeAt(0));

        if (d.has(y)) {
            const ls = d.get(y)!;
            const j = ls.pop()!;
            if (ls.length === 0) {
                d.delete(y);
            }
            ans += i - j;
        } else {
            if (!d.has(x)) {
                d.set(x, []);
            }
            d.get(x)!.push(i);
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
