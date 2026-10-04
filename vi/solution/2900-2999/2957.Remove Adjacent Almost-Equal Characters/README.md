---
comments: true
difficulty: Medium
rating: 1429
source: Biweekly Contest 119 Q2
tags:
    - Greedy
    - String
    - Dynamic Programming
---

<!-- problem:start -->

# [2957. Remove Adjacent Almost-Equal Characters](https://leetcode.com/problems/remove-adjacent-almost-equal-characters)

[中文文档](/solution/2900-2999/2957.Remove%20Adjacent%20Almost-Equal%20Characters/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>0-indexed</strong> <code>word</code>.</p>

<p>Trong một phép toán, bạn có thể chọn bất kỳ chỉ số <code>i</code> nào của <code>word</code> và đổi <code>word[i]</code> thành một chữ cái tiếng Anh viết thường bất kỳ.</p>

<p>Hãy trả về <em>số phép toán <strong>nhỏ nhất</strong> cần thực hiện để loại bỏ tất cả các cặp ký tự liền kề <strong>gần bằng nhau</strong> khỏi</em> <code>word</code>.</p>

<p>Hai ký tự <code>a</code> và <code>b</code> được gọi là <strong>gần bằng nhau</strong> nếu <code>a == b</code> hoặc <code>a</code> và <code>b</code> đứng liền nhau trong bảng chữ cái.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aaaaa&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể đổi word thành &quot;a<strong><u>c</u></strong>a<u><strong>c</strong></u>a&quot;, khi đó không còn cặp ký tự liền kề nào gần bằng nhau.
Có thể chứng minh rằng số phép toán nhỏ nhất cần thực hiện để loại bỏ tất cả các cặp ký tự liền kề gần bằng nhau khỏi word là 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abddez&quot;
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Ta có thể đổi word thành &quot;<strong><u>y</u></strong>bd<u><strong>o</strong></u>ez&quot;, khi đó không còn cặp ký tự liền kề nào gần bằng nhau.
Có thể chứng minh rằng số phép toán nhỏ nhất cần thực hiện để loại bỏ tất cả các cặp ký tự liền kề gần bằng nhau khỏi word là 2.</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;zyxyxyz&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể đổi word thành &quot;z<u><strong>a</strong></u>x<u><strong>a</strong></u>x<strong><u>a</u></strong>z&quot;, khi đó không còn cặp ký tự liền kề nào gần bằng nhau.
Có thể chứng minh rằng số phép toán nhỏ nhất cần thực hiện để loại bỏ tất cả các cặp ký tự liền kề gần bằng nhau khỏi word là 3.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= word.length &lt;= 100</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái liền kề có mã ký tự chênh lệch nhỏ hơn $2$ buộc ta phải thay đổi. Thay đổi ký tự ở vị trí hiện tại sẽ tách nó khỏi ký tự tiếp theo, nên tăng chỉ số tại $i$ rồi bỏ qua $i+1$ là tối ưu. $n \le 100$; bắt đầu từ chỉ số $1$.
>
> Không cần viết lại chuỗi; chỉ cần đếm số phép toán.

<!-- thinking:end -->

Chúng ta bắt đầu duyệt chuỗi `word` từ chỉ số $1$. Nếu `word[i]` và `word[i - 1]` gần bằng nhau, ta tham lam thay `word[i]` bằng một ký tự không đồng thời bằng `word[i - 1]` và `word[i + 1]` (ta không cần thực sự thực hiện phép thay thế, chỉ cần ghi nhận số phép toán). Sau đó, bỏ qua `word[i + 1]` và tiếp tục duyệt chuỗi `word`.

Cuối cùng, trả về số phép toán đã ghi nhận.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi `word`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def removeAlmostEqualCharacters(self, word: str) -> int:
        ans = 0
        i, n = 1, len(word)
        while i < n:
            if abs(ord(word[i]) - ord(word[i - 1])) < 2:
                ans += 1
                i += 2
            else:
                i += 1
        return ans
```

#### Java

```java
class Solution {
    public int removeAlmostEqualCharacters(String word) {
        int ans = 0, n = word.length();
        for (int i = 1; i < n; ++i) {
            if (Math.abs(word.charAt(i) - word.charAt(i - 1)) < 2) {
                ++ans;
                ++i;
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
    int removeAlmostEqualCharacters(string word) {
        int ans = 0, n = word.size();
        for (int i = 1; i < n; ++i) {
            if (abs(word[i] - word[i - 1]) < 2) {
                ++ans;
                ++i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func removeAlmostEqualCharacters(word string) (ans int) {
	for i := 1; i < len(word); i++ {
		if abs(int(word[i])-int(word[i-1])) < 2 {
			ans++
			i++
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function removeAlmostEqualCharacters(word: string): number {
    let ans = 0;
    for (let i = 1; i < word.length; ++i) {
        if (Math.abs(word.charCodeAt(i) - word.charCodeAt(i - 1)) < 2) {
            ++ans;
            ++i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
