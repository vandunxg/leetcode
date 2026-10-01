---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [16.15. Master Mind](https://leetcode.cn/problems/master-mind-lcci)

[中文文档](/lcci/16.15.Master%20Mind/README.md)

## Mô tả

<!-- description:start -->

<p>Trò chơi Master Mind được chơi như sau:</p>
<p>Máy tính có bốn ô, mỗi ô chứa một quả bóng màu đỏ (R), vàng (Y), xanh lá (G) hoặc xanh dương (B). Ví dụ, máy tính có thể có RGGB (ô số 1 màu đỏ, ô số 2 và 3 màu xanh lá, ô số 4 màu xanh dương).</p>
<p>Bạn đang cố gắng đoán đáp án. Chẳng hạn, bạn có thể đoán YRGB.</p>
<p>Khi bạn đoán đúng màu cho đúng ô, bạn nhận được một &quot;hit&quot;. Nếu bạn đoán một màu có tồn tại nhưng nằm sai ô, bạn nhận được một &quot;pseudo-hit&quot;. Lưu ý rằng một ô đã là hit thì không bao giờ được tính là pseudo-hit.</p>
<p>Ví dụ, nếu đáp án thực tế là RGBY và bạn đoán GGRR, bạn có một hit và một pseudo-hit. Hãy viết một phương thức nhận vào một guess và một solution, rồi trả về số lượng hit và pseudo-hit.</p>
<p>Cho một dãy màu <code>solution</code> và một <code>guess</code>, hãy viết một phương thức trả về số lượng hit và pseudo-hit trong <code>answer</code>, trong đó <code>answer[0]</code> là số lượng hit và <code>answer[1]</code> là số lượng pseudo-hit.</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong> solution=&quot;RGBY&quot;,guess=&quot;GGRR&quot;

<strong>Đầu ra: </strong> [1,1]

<strong>Giải thích: </strong> có một hit và một pseudo-hit.

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>len(solution) = len(guess) = 4</code></li>
	<li>Trong <code>solution</code>&nbsp;và&nbsp;<code>guess</code> chỉ có các ký tự <code>&quot;R&quot;</code>,<code>&quot;G&quot;</code>,<code>&quot;B&quot;</code>,<code>&quot;Y&quot;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hit là đúng màu và đúng ô; pseudo-hit chỉ cần đúng màu. Với bốn vị trí, không cần matching phức tạp hơn.
>
> Hit $x$ là số cặp bằng nhau; pseudo-hit là tổng các giá trị min theo từng màu trừ $x$.
>
> `zip` đếm $x$; phép giao của hai `Counter` là phần giao nhau về màu $y$; trả về $[x,y-x]$. Tập ký tự có kích thước $4$.

<!-- thinking:end -->

Chúng ta đồng thời duyệt qua hai chuỗi, đếm số ký tự tương ứng giống nhau và cộng dồn vào $x$. Sau đó, chúng ta ghi nhận các ký tự cùng tần suất xuất hiện của chúng trong hai chuỗi vào hai hash table $cnt1$ và $cnt2$ tương ứng.

Tiếp theo, chúng ta duyệt qua hai hash table, đếm số ký tự chung và cộng dồn vào $y$. Khi đó, đáp án là $[x, y - x]$.

Độ phức tạp thời gian là $O(C)$, còn độ phức tạp không gian là $O(C)$. Ở đây, $C=4$ đối với bài toán này.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def masterMind(self, solution: str, guess: str) -> List[int]:
        x = sum(a == b for a, b in zip(solution, guess))
        y = sum((Counter(solution) & Counter(guess)).values())
        return [x, y - x]
```

#### Java

```java
class Solution {
    public int[] masterMind(String solution, String guess) {
        int x = 0, y = 0;
        Map<Character, Integer> cnt1 = new HashMap<>();
        Map<Character, Integer> cnt2 = new HashMap<>();
        for (int i = 0; i < 4; ++i) {
            char a = solution.charAt(i), b = guess.charAt(i);
            x += a == b ? 1 : 0;
            cnt1.merge(a, 1, Integer::sum);
            cnt2.merge(b, 1, Integer::sum);
        }
        for (char c : "RYGB".toCharArray()) {
            y += Math.min(cnt1.getOrDefault(c, 0), cnt2.getOrDefault(c, 0));
        }
        return new int[] {x, y - x};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> masterMind(string solution, string guess) {
        int x = 0, y = 0;
        unordered_map<char, int> cnt1;
        unordered_map<char, int> cnt2;
        for (int i = 0; i < 4; ++i) {
            x += solution[i] == guess[i];
            cnt1[solution[i]]++;
            cnt2[guess[i]]++;
        }
        for (char c : "RYGB") y += min(cnt1[c], cnt2[c]);
        return vector<int>{x, y - x};
    }
};
```

#### Go

```go
func masterMind(solution string, guess string) []int {
	var x, y int
	cnt1 := map[byte]int{}
	cnt2 := map[byte]int{}
	for i := range solution {
		a, b := solution[i], guess[i]
		if a == b {
			x++
		}
		cnt1[a]++
		cnt2[b]++
	}
	for _, c := range []byte("RYGB") {
		y += min(cnt1[c], cnt2[c])
	}
	return []int{x, y - x}
}
```

#### JavaScript

```js
/**
 * @param {string} solution
 * @param {string} guess
 * @return {number[]}
 */
var masterMind = function (solution, guess) {
    let counts1 = { R: 0, G: 0, B: 0, Y: 0 };
    let counts2 = { R: 0, G: 0, B: 0, Y: 0 };
    let res1 = 0;
    for (let i = 0; i < solution.length; i++) {
        let s1 = solution[i],
            s2 = guess[i];
        if (s1 === s2) {
            res1++;
        } else {
            counts1[s1] += 1;
            counts2[s2] += 1;
        }
    }
    let res2 = Object.keys(counts1).reduce((a, c) => a + Math.min(counts1[c], counts2[c]), 0);
    return [res1, res2];
};
```

#### Swift

```swift
class Solution {
    func masterMind(_ solution: String, _ guess: String) -> [Int] {
        var x = 0
        var y = 0
        var cnt1: [Character: Int] = [:]
        var cnt2: [Character: Int] = [:]

        for i in solution.indices {
            let a = solution[i]
            let b = guess[i]
            if a == b {
                x += 1
            }
            cnt1[a, default: 0] += 1
            cnt2[b, default: 0] += 1
        }

        let colors = "RYGB"
        for c in colors {
            let minCount = min(cnt1[c, default: 0], cnt2[c, default: 0])
            y += minCount
        }

        return [x, y - x]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
