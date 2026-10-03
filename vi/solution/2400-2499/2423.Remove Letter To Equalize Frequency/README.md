---
comments: true
difficulty: Easy
rating: 1648
source: Biweekly Contest 88 Q1
tags:
    - Hash Table
    - String
    - Counting
---

<!-- problem:start -->

# [2423. Remove Letter To Equalize Frequency](https://leetcode.com/problems/remove-letter-to-equalize-frequency)

[中文文档](/solution/2400-2499/2423.Remove%20Letter%20To%20Equalize%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một chuỗi <strong>được đánh chỉ số từ 0</strong> <code>word</code>, gồm các chữ cái tiếng Anh viết thường. Bạn cần chọn <strong>một</strong> chỉ số và <strong>xóa</strong> chữ cái tại chỉ số đó khỏi <code>word</code> sao cho <strong>tần suất</strong> của mọi chữ cái xuất hiện trong <code>word</code> đều bằng nhau.</p>

<p>Trả về <em></em><code>true</code><em> nếu có thể xóa một chữ cái để tần suất của mọi chữ cái trong </em><code>word</code><em> bằng nhau, và </em><code>false</code><em> nếu không thể</em>.</p>

<p><strong>Lưu ý:</strong></p>

<ul>
	<li><b>Tần suất</b> của một chữ cái <code>x</code> là số lần nó xuất hiện trong chuỗi.</li>
	<li>Bạn <strong>phải</strong> xóa chính xác một chữ cái và không được chọn không làm gì cả.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;abcc&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Chọn chỉ số 3 và xóa chữ cái tại đó: word trở thành &quot;abc&quot; và mỗi ký tự có tần suất bằng 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> word = &quot;aazz&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Ta phải xóa một ký tự, nên tần suất của &quot;a&quot; và &quot;z&quot; lần lượt sẽ là 1 và 2, hoặc ngược lại. Không thể làm cho tần suất của mọi chữ cái còn lại bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= word.length &lt;= 100</code></li>
	<li><code>word</code> chỉ gồm các chữ cái tiếng Anh viết thường.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Vì $|word|\le 100$, ta có thể thử xóa từng chữ cái phân biệt. Việc các tần suất còn lại có bằng nhau hay không chỉ phụ thuộc vào $26$ bộ đếm.
>
> Đếm số lần xuất hiện của các chữ cái, sau đó với mỗi key giảm bộ đếm một lần và kiểm tra xem các tần suất dương có tạo thành một tập hợp chỉ gồm một giá trị hay không. Khôi phục bộ đếm rồi thử chữ cái tiếp theo.

<!-- thinking:end -->

Trước tiên, ta sử dụng một hash table hoặc một mảng có độ dài $26$ tên là $cnt$ để đếm số lần xuất hiện của mỗi chữ cái trong chuỗi.

Tiếp theo, ta duyệt qua $26$ chữ cái. Nếu chữ cái $c$ xuất hiện trong chuỗi, ta giảm bộ đếm của nó đi một, rồi kiểm tra xem các bộ đếm của những chữ cái còn lại có bằng nhau hay không. Nếu bằng nhau, trả về `true`. Nếu không, tăng bộ đếm của $c$ lên một và tiếp tục duyệt chữ cái tiếp theo.

Nếu kết thúc quá trình duyệt, điều đó có nghĩa là không thể làm cho các bộ đếm của những chữ cái còn lại bằng nhau bằng cách xóa một chữ cái, nên trả về `false`.

Độ phức tạp thời gian là $O(n + C^2)$, và độ phức tạp không gian là $O(C)$. Trong đó, $n$ là độ dài của chuỗi $word$, còn $C$ là kích thước của tập ký tự. Trong bài toán này, $C = 26$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def equalFrequency(self, word: str) -> bool:
        cnt = Counter(word)
        for c in cnt.keys():
            cnt[c] -= 1
            if len(set(v for v in cnt.values() if v)) == 1:
                return True
            cnt[c] += 1
        return False
```

#### Java

```java
class Solution {
    public boolean equalFrequency(String word) {
        int[] cnt = new int[26];
        for (int i = 0; i < word.length(); ++i) {
            ++cnt[word.charAt(i) - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[i] > 0) {
                --cnt[i];
                int x = 0;
                boolean ok = true;
                for (int v : cnt) {
                    if (v == 0) {
                        continue;
                    }
                    if (x > 0 && v != x) {
                        ok = false;
                        break;
                    }
                    x = v;
                }
                if (ok) {
                    return true;
                }
                ++cnt[i];
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool equalFrequency(string word) {
        int cnt[26]{};
        for (char& c : word) {
            ++cnt[c - 'a'];
        }
        for (int i = 0; i < 26; ++i) {
            if (cnt[i]) {
                --cnt[i];
                int x = 0;
                bool ok = true;
                for (int v : cnt) {
                    if (v == 0) {
                        continue;
                    }
                    if (x && v != x) {
                        ok = false;
                        break;
                    }
                    x = v;
                }
                if (ok) {
                    return true;
                }
                ++cnt[i];
            }
        }
        return false;
    }
};
```

#### Go

```go
func equalFrequency(word string) bool {
	cnt := [26]int{}
	for _, c := range word {
		cnt[c-'a']++
	}
	for i := range cnt {
		if cnt[i] > 0 {
			cnt[i]--
			x := 0
			ok := true
			for _, v := range cnt {
				if v == 0 {
					continue
				}
				if x > 0 && v != x {
					ok = false
					break
				}
				x = v
			}
			if ok {
				return true
			}
			cnt[i]++
		}
	}
	return false
}
```

#### TypeScript

```ts
function equalFrequency(word: string): boolean {
    const cnt: number[] = new Array(26).fill(0);
    for (const c of word) {
        cnt[c.charCodeAt(0) - 97]++;
    }
    for (let i = 0; i < 26; ++i) {
        if (cnt[i]) {
            cnt[i]--;
            let x = 0;
            let ok = true;
            for (const v of cnt) {
                if (v === 0) {
                    continue;
                }
                if (x && v !== x) {
                    ok = false;
                    break;
                }
                x = v;
            }
            if (ok) {
                return true;
            }
            cnt[i]++;
        }
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
