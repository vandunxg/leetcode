---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [10.05. Sparse Array Search](https://leetcode.cn/problems/sparse-array-search-lcci)

## Mô tả

<!-- description:start -->

<p>Cho một mảng chuỗi đã được sắp xếp, trong đó xen kẽ các chuỗi rỗng, hãy viết một phương thức để tìm vị trí của một chuỗi cho trước.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong> Đầu vào</strong>: words = [&quot;at&quot;, &quot;&quot;, &quot;&quot;, &quot;&quot;, &quot;ball&quot;, &quot;&quot;, &quot;&quot;, &quot;car&quot;, &quot;&quot;, &quot;&quot;,&quot;dad&quot;, &quot;&quot;, &quot;&quot;], s = &quot;ta&quot;

<strong> Đầu ra</strong>: -1

<strong> Giải thích</strong>: Trả về -1 nếu <code>s</code> không có trong <code>words</code>.

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong> Đầu vào</strong>: words = [&quot;at&quot;, &quot;&quot;, &quot;&quot;, &quot;&quot;, &quot;ball&quot;, &quot;&quot;, &quot;&quot;, &quot;car&quot;, &quot;&quot;, &quot;&quot;,&quot;dad&quot;, &quot;&quot;, &quot;&quot;], s = &quot;ball&quot;

<strong> Đầu ra</strong>: 4

</pre>

<p><strong>Lưu ý:</strong></p>

<ol>
	<li><code>1 &lt;= words.length &lt;= 1000000</code></li>
</ol>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Một mảng chuỗi đã sắp xếp được ngắt quãng bởi các chuỗi rỗng. Tìm kiếm nhị phân vẫn hoạt động sau khi bỏ qua các chuỗi rỗng, nhưng khi điểm giữa là chuỗi rỗng thì cần thăm dò thêm.
>
> Mã nguồn dùng cùng cách chia để trị như bài toán chỉ số ma thuật: tìm nửa bên trái trước (để lấy kết quả khớp bên trái nhất), sau đó xét mid rồi nửa bên phải. Các chuỗi rỗng đơn giản là không vượt qua phép kiểm tra bằng nhau.
>
> Trường hợp xấu nhất là tuyến tính, nhưng thứ tự ưu tiên bên trái rồi đến mid trả về kết quả khớp bên trái nhất mà không cần xử lý riêng chuỗi rỗng.

<!-- thinking:end -->

Chúng ta thiết kế một hàm $dfs(i, j)$ để tìm chuỗi đích trong mảng $nums[i, j]$. Nếu tìm thấy, trả về chỉ số của chuỗi đích, nếu không thì trả về $-1$. Vì vậy, đáp án là $dfs(0, n-1)$.

Cách triển khai hàm $dfs(i, j)$ như sau:

1. Nếu $i > j$, trả về $-1$.
2. Nếu không, lấy vị trí giữa $mid = (i + j) / 2$, sau đó gọi đệ quy $dfs(i, mid-1)$. Nếu giá trị trả về khác $-1$, nghĩa là đã tìm thấy chuỗi đích ở nửa bên trái, nên trả về ngay. Nếu không, nếu $words[mid] = s$, nghĩa là đã tìm thấy chuỗi đích, nên trả về ngay. Nếu không, gọi đệ quy $dfs(mid+1, j)$ và trả về kết quả.

Trong trường hợp xấu nhất, độ phức tạp thời gian là $O(n \times m)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là độ dài của mảng chuỗi và độ dài của chuỗi $s$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findString(self, words: List[str], s: str) -> int:
        def dfs(i: int, j: int) -> int:
            if i > j:
                return -1
            mid = (i + j) >> 1
            l = dfs(i, mid - 1)
            if l != -1:
                return l
            if words[mid] == s:
                return mid
            return dfs(mid + 1, j)

        return dfs(0, len(words) - 1)
```

#### Java

```java
class Solution {
    public int findString(String[] words, String s) {
        return dfs(words, s, 0, words.length - 1);
    }

    private int dfs(String[] words, String s, int i, int j) {
        if (i > j) {
            return -1;
        }
        int mid = (i + j) >> 1;
        int l = dfs(words, s, i, mid - 1);
        if (l != -1) {
            return l;
        }
        if (words[mid].equals(s)) {
            return mid;
        }
        return dfs(words, s, mid + 1, j);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findString(vector<string>& words, string s) {
        function<int(int, int)> dfs = [&](int i, int j) {
            if (i > j) {
                return -1;
            }
            int mid = (i + j) >> 1;
            int l = dfs(i, mid - 1);
            if (l != -1) {
                return l;
            }
            if (words[mid] == s) {
                return mid;
            }
            return dfs(mid + 1, j);
        };
        return dfs(0, words.size() - 1);
    }
};
```

#### Go

```go
func findString(words []string, s string) int {
	var dfs func(i, j int) int
	dfs = func(i, j int) int {
		if i > j {
			return -1
		}
		mid := (i + j) >> 1
		if l := dfs(i, mid-1); l != -1 {
			return l
		}
		if words[mid] == s {
			return mid
		}
		return dfs(mid+1, j)
	}
	return dfs(0, len(words)-1)
}
```

#### TypeScript

```ts
function findString(words: string[], s: string): number {
    const dfs = (i: number, j: number): number => {
        if (i > j) {
            return -1;
        }
        const mid = (i + j) >> 1;
        const l = dfs(i, mid - 1);
        if (l !== -1) {
            return l;
        }
        if (words[mid] === s) {
            return mid;
        }
        return dfs(mid + 1, j);
    };
    return dfs(0, words.length - 1);
}
```

#### Swift

```swift
class Solution {
    func findString(_ words: [String], _ s: String) -> Int {
        return dfs(words, s, 0, words.count - 1)
    }

    private func dfs(_ words: [String], _ s: String, _ i: Int, _ j: Int) -> Int {
        if i > j {
            return -1
        }
        let mid = (i + j) >> 1
        let left = dfs(words, s, i, mid - 1)
        if left != -1 {
            return left
        }
        if words[mid] == s {
            return mid
        }
        return dfs(words, s, mid + 1, j)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
