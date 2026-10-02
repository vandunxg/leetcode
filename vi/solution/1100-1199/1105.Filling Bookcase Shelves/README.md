---
comments: true
difficulty: Medium
rating: 2014
source: Weekly Contest 143 Q3
tags:
    - Array
    - Dynamic Programming
---

<!-- problem:start -->

# [1105. Filling Bookcase Shelves](https://leetcode.com/problems/filling-bookcase-shelves)

[中文文档](/solution/1100-1199/1105.Filling%20Bookcase%20Shelves/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>books</code>, trong đó <code>books[i] = [thickness<sub>i</sub>, height<sub>i</sub>]</code> lần lượt biểu thị độ dày và chiều cao của quyển sách thứ <code>i<sup>th</sup></code>. Bạn cũng được cho số nguyên <code>shelfWidth</code>.</p>

<p>Ta muốn xếp các quyển sách theo đúng thứ tự lên những tầng kệ có tổng chiều rộng là <code>shelfWidth</code>.</p>

<p>Ta chọn một số quyển sách để đặt lên tầng kệ hiện tại sao cho tổng độ dày không vượt quá <code>shelfWidth</code>, sau đó tạo tầng kệ tiếp theo. Chiều cao tổng thể của tủ sách tăng thêm bằng chiều cao lớn nhất trong các quyển vừa xếp. Lặp lại quá trình này cho đến khi xếp hết sách.</p>

<p>Lưu ý rằng ở mỗi bước, thứ tự các quyển sách được xếp phải giữ nguyên như trong dãy đã cho.</p>

<ul>
	<li>Ví dụ, với danh sách gồm <code>5</code> quyển sách theo thứ tự, ta có thể đặt quyển thứ nhất và thứ hai lên tầng đầu tiên, quyển thứ ba lên tầng thứ hai, rồi quyển thứ tư và thứ năm lên tầng cuối cùng.</li>
</ul>

<p>Hãy trả về <em>chiều cao nhỏ nhất có thể của toàn bộ tủ sách sau khi xếp sách theo cách này</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1100-1199/1105.Filling%20Bookcase%20Shelves/images/shelves.png" style="height: 500px; width: 337px;" />
<pre>
<strong>Đầu vào:</strong> books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelfWidth = 4
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong>
Tổng chiều cao của 3 tầng kệ là 1 + 3 + 2 = 6.
Lưu ý rằng quyển sách thứ 2 không nhất thiết phải nằm trên tầng kệ đầu tiên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> books = [[1,3],[2,4],[3,2]], shelfWidth = 6
<strong>Đầu ra:</strong> 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= books.length &lt;= 1000</code></li>
	<li><code>1 &lt;= thickness<sub>i</sub> &lt;= shelfWidth &lt;= 1000</code></li>
	<li><code>1 &lt;= height<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dynamic Programming

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi quyển sách có thể bắt đầu một tầng kệ mới hoặc được xếp chung tầng với một đoạn liên tiếp các quyển trước đó; số cách phân chia tăng theo hàm mũ. Với $n\le 1000$, quy hoạch động $O(n^2)$ là phù hợp.
>
> Gọi $f[i]$ là chiều cao nhỏ nhất khi xếp $i$ quyển đầu tiên. Tầng kệ cuối kết thúc ở $books[i-1]$; ta mở rộng tầng này ngược về trước, cộng dồn độ dày và dừng khi tổng vượt quá $shelfWidth$. Chiều cao của tầng là chiều cao lớn nhất trong các quyển trên tầng đó, cộng với $f[j-1]$. Khi duyệt ngược, tổng độ dày không giảm.

<!-- thinking:end -->

Ta định nghĩa $f[i]$ là chiều cao nhỏ nhất để xếp $i$ quyển sách đầu tiên. Ban đầu, $f[0] = 0$ và đáp án là $f[n]$.

Xét $f[i]$. Quyển sách cuối là $books[i - 1]$, có độ dày $w$ và chiều cao $h$.

- Nếu quyển sách này được đặt một mình trên tầng mới, thì $f[i] = f[i - 1] + h$;
- Nếu có thể xếp quyển này cùng tầng với một số quyển ngay trước đó, ta lần lượt xét quyển đầu tiên $books[j-1]$ của tầng từ sau ra trước, với $j \in [1, i - 1]$, đồng thời cộng độ dày vào $w$. Nếu $w > shelfWidth$, nghĩa là không thể xếp $books[j-1]$ cùng tầng với $books[i-1]$, nên dừng; nếu không, cập nhật chiều cao lớn nhất của tầng hiện tại bằng $h = \max(h, books[j-1][1])$, rồi cập nhật $f[i] = \min(f[i], f[j - 1] + h)$.

Đáp án cuối cùng là $f[n]$.

Độ phức tạp thời gian là $O(n^2)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $books$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minHeightShelves(self, books: List[List[int]], shelfWidth: int) -> int:
        n = len(books)
        f = [0] * (n + 1)
        for i, (w, h) in enumerate(books, 1):
            f[i] = f[i - 1] + h
            for j in range(i - 1, 0, -1):
                w += books[j - 1][0]
                if w > shelfWidth:
                    break
                h = max(h, books[j - 1][1])
                f[i] = min(f[i], f[j - 1] + h)
        return f[n]
```

#### Java

```java
class Solution {
    public int minHeightShelves(int[][] books, int shelfWidth) {
        int n = books.length;
        int[] f = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            int w = books[i - 1][0], h = books[i - 1][1];
            f[i] = f[i - 1] + h;
            for (int j = i - 1; j > 0; --j) {
                w += books[j - 1][0];
                if (w > shelfWidth) {
                    break;
                }
                h = Math.max(h, books[j - 1][1]);
                f[i] = Math.min(f[i], f[j - 1] + h);
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        int f[n + 1];
        f[0] = 0;
        for (int i = 1; i <= n; ++i) {
            int w = books[i - 1][0], h = books[i - 1][1];
            f[i] = f[i - 1] + h;
            for (int j = i - 1; j > 0; --j) {
                w += books[j - 1][0];
                if (w > shelfWidth) {
                    break;
                }
                h = max(h, books[j - 1][1]);
                f[i] = min(f[i], f[j - 1] + h);
            }
        }
        return f[n];
    }
};
```

#### Go

```go
func minHeightShelves(books [][]int, shelfWidth int) int {
	n := len(books)
	f := make([]int, n+1)
	for i := 1; i <= n; i++ {
		w, h := books[i-1][0], books[i-1][1]
		f[i] = f[i-1] + h
		for j := i - 1; j > 0; j-- {
			w += books[j-1][0]
			if w > shelfWidth {
				break
			}
			h = max(h, books[j-1][1])
			f[i] = min(f[i], f[j-1]+h)
		}
	}
	return f[n]
}
```

#### TypeScript

```ts
function minHeightShelves(books: number[][], shelfWidth: number): number {
    const n = books.length;
    const f = new Array(n + 1).fill(0);
    for (let i = 1; i <= n; ++i) {
        let [w, h] = books[i - 1];
        f[i] = f[i - 1] + h;
        for (let j = i - 1; j > 0; --j) {
            w += books[j - 1][0];
            if (w > shelfWidth) {
                break;
            }
            h = Math.max(h, books[j - 1][1]);
            f[i] = Math.min(f[i], f[j - 1] + h);
        }
    }
    return f[n];
}
```

#### C#

```cs
public class Solution {
    public int MinHeightShelves(int[][] books, int shelfWidth) {
        int n = books.Length;
        int[] f = new int[n + 1];
        for (int i = 1; i <= n; ++i) {
            int w = books[i - 1][0], h = books[i - 1][1];
            f[i] = f[i - 1] + h;
            for (int j = i - 1; j > 0; --j) {
                w += books[j - 1][0];
                if (w > shelfWidth) {
                    break;
                }
                h = Math.Max(h, books[j - 1][1]);
                f[i] = Math.Min(f[i], f[j - 1] + h);
            }
        }
        return f[n];
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
