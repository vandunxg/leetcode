---
comments: true
difficulty: Medium
rating: 1410
source: Weekly Contest 147 Q2
tags:
    - Hash Table
    - String
---

<!-- problem:start -->

# [1138. Alphabet Board Path](https://leetcode.com/problems/alphabet-board-path)

[中文文档](/solution/1100-1199/1138.Alphabet%20Board%20Path/README.md)

## Mô tả

<!-- description:start -->

<p>Trên bảng chữ cái, ta bắt đầu tại vị trí <code>(0, 0)</code>, tương ứng với ký tự&nbsp;<code>board[0][0]</code>.</p>

<p>Bảng là <code>board = [&quot;abcde&quot;, &quot;fghij&quot;, &quot;klmno&quot;, &quot;pqrst&quot;, &quot;uvwxy&quot;, &quot;z&quot;]</code>, như minh họa bên dưới.</p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1138.Alphabet%20Board%20Path/images/azboard.png" style="width: 250px; height: 317px;" /></p>

<p>Ta có thể thực hiện các bước sau:</p>

<ul>
	<li><code>&#39;U&#39;</code> di chuyển lên một hàng nếu vị trí đó tồn tại trên bảng;</li>
	<li><code>&#39;D&#39;</code> di chuyển xuống một hàng nếu vị trí đó tồn tại trên bảng;</li>
	<li><code>&#39;L&#39;</code> di chuyển sang trái một cột nếu vị trí đó tồn tại trên bảng;</li>
	<li><code>&#39;R&#39;</code> di chuyển sang phải một cột nếu vị trí đó tồn tại trên bảng;</li>
	<li><code>&#39;!&#39;</code>&nbsp;thêm ký tự <code>board[r][c]</code> tại vị trí hiện tại <code>(r, c)</code>&nbsp;vào&nbsp;đáp án.</li>
</ul>

<p>(Chỉ những vị trí có chữ cái mới được xem là tồn tại trên bảng.)</p>

<p>Trả về chuỗi bước di chuyển giúp tạo ra <code>target</code>&nbsp;với số bước ít nhất.&nbsp; Có thể trả về bất kỳ đường đi nào thỏa mãn yêu cầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> target = "leet"
<strong>Output:</strong> "DDR!UURRR!!DDD!"
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> target = "code"
<strong>Output:</strong> "RR!DDRR!UUL!R!"
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= target.length &lt;= 100</code></li>
	<li><code>target</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái nằm trên bảng có $5$ cột; ta di chuyển từ chữ này đến chữ tiếp theo. Các ô thông thường có bốn ô kề, nhưng `z` chỉ có ô kề phía trên, nên nếu đi xuống hoặc sang phải trước thì có thể ra khỏi bảng.
>
> Với mỗi chữ cái, di chuyển theo thứ tự trái, lên, phải, xuống: hoàn tất hướng trái/lên trước hướng phải/xuống để không bước ra ngoài tại `z`, rồi thêm `!`.

<!-- thinking:end -->

Bắt đầu từ gốc tọa độ $(0, 0)$, mô phỏng từng bước di chuyển và thêm mỗi bước vào đáp án. Lưu ý thứ tự di chuyển là "trái, lên, phải, xuống".

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài chuỗi target, vì cần duyệt từng ký tự trong chuỗi này. Không tính bộ nhớ của đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def alphabetBoardPath(self, target: str) -> str:
        i = j = 0
        ans = []
        for c in target:
            v = ord(c) - ord("a")
            x, y = v // 5, v % 5
            while j > y:
                j -= 1
                ans.append("L")
            while i > x:
                i -= 1
                ans.append("U")
            while j < y:
                j += 1
                ans.append("R")
            while i < x:
                i += 1
                ans.append("D")
            ans.append("!")
        return "".join(ans)
```

#### Java

```java
class Solution {
    public String alphabetBoardPath(String target) {
        StringBuilder ans = new StringBuilder();
        int i = 0, j = 0;
        for (int k = 0; k < target.length(); ++k) {
            int v = target.charAt(k) - 'a';
            int x = v / 5, y = v % 5;
            while (j > y) {
                --j;
                ans.append('L');
            }
            while (i > x) {
                --i;
                ans.append('U');
            }
            while (j < y) {
                ++j;
                ans.append('R');
            }
            while (i < x) {
                ++i;
                ans.append('D');
            }
            ans.append("!");
        }
        return ans.toString();
    }
}
```

#### C++

```cpp
class Solution {
public:
    string alphabetBoardPath(string target) {
        string ans;
        int i = 0, j = 0;
        for (char& c : target) {
            int v = c - 'a';
            int x = v / 5, y = v % 5;
            while (j > y) {
                --j;
                ans += 'L';
            }
            while (i > x) {
                --i;
                ans += 'U';
            }
            while (j < y) {
                ++j;
                ans += 'R';
            }
            while (i < x) {
                ++i;
                ans += 'D';
            }
            ans += '!';
        }
        return ans;
    }
};
```

#### Go

```go
func alphabetBoardPath(target string) string {
	ans := []byte{}
	var i, j int
	for _, c := range target {
		v := int(c - 'a')
		x, y := v/5, v%5
		for j > y {
			j--
			ans = append(ans, 'L')
		}
		for i > x {
			i--
			ans = append(ans, 'U')
		}
		for j < y {
			j++
			ans = append(ans, 'R')
		}
		for i < x {
			i++
			ans = append(ans, 'D')
		}
		ans = append(ans, '!')
	}
	return string(ans)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
