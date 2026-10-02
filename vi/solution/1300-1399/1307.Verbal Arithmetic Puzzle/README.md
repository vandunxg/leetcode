---
comments: true
difficulty: Hard
rating: 2250
source: Weekly Contest 169 Q4
tags:
    - Array
    - Math
    - String
    - Backtracking
---

<!-- problem:start -->

# [1307. Verbal Arithmetic Puzzle](https://leetcode.com/problems/verbal-arithmetic-puzzle)

[中文文档](/solution/1300-1399/1307.Verbal%20Arithmetic%20Puzzle/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một phương trình, với <code>words</code> ở vế trái và <code>result</code> ở vế phải.</p>

<p>Hãy kiểm tra xem phương trình có thể giải được theo các quy tắc sau hay không:</p>

<ul>
	<li>Mỗi ký tự được ánh xạ thành một chữ số (0 - 9).</li>
	<li>Hai ký tự khác nhau không thể ánh xạ thành cùng một chữ số.</li>
	<li>Mỗi <code>words[i]</code> và <code>result</code> được ánh xạ thành một số <strong>không có</strong> số 0 ở đầu.</li>
	<li>Tổng các số ở vế trái (<code>words</code>) bằng số ở vế phải (<code>result</code>).</li>
</ul>

<p>Trả về <code>true</code> <em>nếu phương trình giải được; nếu không, trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;SEND&quot;,&quot;MORE&quot;], result = &quot;MONEY&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ánh xạ &#39;S&#39;-&gt; 9, &#39;E&#39;-&gt;5, &#39;N&#39;-&gt;6, &#39;D&#39;-&gt;7, &#39;M&#39;-&gt;1, &#39;O&#39;-&gt;0, &#39;R&#39;-&gt;8, &#39;Y&#39;-&gt;&#39;2&#39;
Sao cho: &quot;SEND&quot; + &quot;MORE&quot; = &quot;MONEY&quot; ,  9567 + 1085 = 10652</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;SIX&quot;,&quot;SEVEN&quot;,&quot;SEVEN&quot;], result = &quot;TWENTY&quot;
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Ánh xạ &#39;S&#39;-&gt; 6, &#39;I&#39;-&gt;5, &#39;X&#39;-&gt;0, &#39;E&#39;-&gt;8, &#39;V&#39;-&gt;7, &#39;N&#39;-&gt;2, &#39;T&#39;-&gt;1, &#39;W&#39;-&gt;&#39;3&#39;, &#39;Y&#39;-&gt;4
Sao cho: &quot;SIX&quot; + &quot;SEVEN&quot; + &quot;SEVEN&quot; = &quot;TWENTY&quot; ,  650 + 68782 + 68782 = 138214</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> words = [&quot;LEET&quot;,&quot;CODE&quot;], result = &quot;POINT&quot;
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có ánh xạ nào thỏa mãn phương trình, nên ta trả về false.
Lưu ý rằng hai ký tự khác nhau không thể ánh xạ thành cùng một chữ số.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= words.length &lt;= 5</code></li>
	<li><code>1 &lt;= words[i].length, result.length &lt;= 7</code></li>
	<li><code>words[i], result</code> chỉ chứa chữ cái tiếng Anh viết hoa.</li>
	<li>Số ký tự khác nhau được dùng trong biểu thức không vượt quá <code>10</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ cái phải được ánh xạ thành những chữ số khác nhau để phương trình chữ-số đúng và không có số 0 ở đầu. Có tối đa $10$ chữ cái khác nhau, nên nếu thử hoán vị trước rồi mới kiểm tra toàn bộ phương trình thì có đến $10!$ cách gán.
>
> Ta xử lý các cột từ hàng đơn vị trở lên và dùng carry để cắt nhánh: sau khi cộng các chữ số ở vế trái, trừ chữ số ở vế phải và cộng carry cũ, chữ số hàng đơn vị phải bằng $0$ thì mới chuyển sang cột tiếp theo. Nếu ký tự đã có ánh xạ thì dùng lại; nếu chưa, thử các chữ số chưa dùng và không cho phép số 0 ở đầu một từ nhiều chữ số. Sau khi xử lý hết các cột, carry còn lại phải bằng $0$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def isAnyMapping(
        self, words, row, col, bal, letToDig, digToLet, totalRows, totalCols
    ):
        # If traversed all columns.
        if col == totalCols:
            return bal == 0

        # At the end of a particular column.
        if row == totalRows:
            return bal % 10 == 0 and self.isAnyMapping(
                words, 0, col + 1, bal // 10, letToDig, digToLet, totalRows, totalCols
            )

        w = words[row]

        # If the current string 'w' has no character in the ('col')th index.
        if col >= len(w):
            return self.isAnyMapping(
                words, row + 1, col, bal, letToDig, digToLet, totalRows, totalCols
            )

        # Take the current character in the variable letter.
        letter = w[len(w) - 1 - col]

        # Create a variable 'sign' to check whether we have to add it or subtract it.
        if row < totalRows - 1:
            sign = 1
        else:
            sign = -1

        # If we have a prior valid mapping, then use that mapping.
        # The second condition is for the leading zeros.
        if letter in letToDig and (
            letToDig[letter] != 0
            or (letToDig[letter] == 0 and len(w) == 1)
            or col != len(w) - 1
        ):

            return self.isAnyMapping(
                words,
                row + 1,
                col,
                bal + sign * letToDig[letter],
                letToDig,
                digToLet,
                totalRows,
                totalCols,
            )

        # Choose a new mapping.
        else:
            for i in range(10):
                # If 'i'th mapping is valid then select it.
                if digToLet[i] == "-" and (
                    i != 0 or (i == 0 and len(w) == 1) or col != len(w) - 1
                ):
                    digToLet[i] = letter
                    letToDig[letter] = i

                    # Call the function again with the new mapping.
                    if self.isAnyMapping(
                        words,
                        row + 1,
                        col,
                        bal + sign * letToDig[letter],
                        letToDig,
                        digToLet,
                        totalRows,
                        totalCols,
                    ):
                        return True

                    # Unselect the mapping.
                    digToLet[i] = "-"
                    if letter in letToDig:
                        del letToDig[letter]

        # If nothing is correct then just return false.
        return False

    def isSolvable(self, words, result):
        # Add the string 'result' in the list 'words'.
        words.append(result)

        # Initialize 'totalRows' with the size of the list.
        totalRows = len(words)

        # Find the longest string in the list and set 'totalCols' with the size of that string.
        totalCols = max(len(word) for word in words)

        # Create a HashMap for the letter to digit mapping.
        letToDig = {}

        # Create a list for the digit to letter mapping.
        digToLet = ["-"] * 10

        return self.isAnyMapping(
            words, 0, 0, 0, letToDig, digToLet, totalRows, totalCols
        )
```

#### Java

```java
class Solution {
    private boolean isAnyMapping(List<String> words, int row, int col, int bal,
        HashMap<Character, Integer> letToDig, char[] digToLet, int totalRows, int totalCols) {
        // If traversed all columns.
        if (col == totalCols) {
            return bal == 0;
        }

        // At the end of a particular column.
        if (row == totalRows) {
            return (bal % 10 == 0
                && isAnyMapping(
                    words, 0, col + 1, bal / 10, letToDig, digToLet, totalRows, totalCols));
        }

        String w = words.get(row);

        // If the current string 'w' has no character in the ('col')th index.
        if (col >= w.length()) {
            return isAnyMapping(words, row + 1, col, bal, letToDig, digToLet, totalRows, totalCols);
        }

        // Take the current character in the variable letter.
        char letter = w.charAt(w.length() - 1 - col);

        // Create a variable 'sign' to check whether we have to add it or subtract it.
        int sign = (row < totalRows - 1) ? 1 : -1;

        // If we have a prior valid mapping, then use that mapping.
        // The second condition is for the leading zeros.
        if (letToDig.containsKey(letter)
            && (letToDig.get(letter) != 0 || (letToDig.get(letter) == 0 && w.length() == 1)
                || col != w.length() - 1)) {

            return isAnyMapping(words, row + 1, col, bal + sign * letToDig.get(letter), letToDig,
                digToLet, totalRows, totalCols);

        } else {
            // Choose a new mapping.
            for (int i = 0; i < 10; i++) {
                // If 'i'th mapping is valid then select it.
                if (digToLet[i] == '-'
                    && (i != 0 || (i == 0 && w.length() == 1) || col != w.length() - 1)) {
                    digToLet[i] = letter;
                    letToDig.put(letter, i);

                    // Call the function again with the new mapping.
                    if (isAnyMapping(words, row + 1, col, bal + sign * letToDig.get(letter),
                            letToDig, digToLet, totalRows, totalCols)) {
                        return true;
                    }

                    // Unselect the mapping.
                    digToLet[i] = '-';
                    letToDig.remove(letter);
                }
            }
        }

        // If nothing is correct then just return false.
        return false;
    }

    public boolean isSolvable(String[] wordsArr, String result) {
        // Add the string 'result' in the list 'words'.
        List<String> words = new ArrayList<>();
        for (String word : wordsArr) {
            words.add(word);
        }
        words.add(result);

        int totalRows = words.size();

        // Find the longest string in the list and set 'totalCols' with the size of that string.
        int totalCols = 0;
        for (String word : words) {
            if (totalCols < word.length()) {
                totalCols = word.length();
            }
        }

        // Create a HashMap for the letter to digit mapping.
        HashMap<Character, Integer> letToDig = new HashMap<>();

        // Create a char array for the digit to letter mapping.
        char[] digToLet = new char[10];
        for (int i = 0; i < 10; i++) {
            digToLet[i] = '-';
        }

        return isAnyMapping(words, 0, 0, 0, letToDig, digToLet, totalRows, totalCols);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isAnyMapping(vector<string>& words, int row, int col, int bal, unordered_map<char, int>& letToDig,
        vector<char>& digToLet, int totalRows, int totalCols) {
        // If traversed all columns.
        if (col == totalCols) {
            return bal == 0;
        }

        // At the end of a particular column.
        if (row == totalRows) {
            return (bal % 10 == 0 && isAnyMapping(words, 0, col + 1, bal / 10, letToDig, digToLet, totalRows, totalCols));
        }

        string w = words[row];

        // If the current string 'W' has no character in the ('COL')th index.
        if (col >= w.length()) {
            return isAnyMapping(words, row + 1, col, bal, letToDig, digToLet, totalRows, totalCols);
        }

        // Take the current character in the variable letter.
        char letter = w[w.length() - 1 - col];

        // Create a variable 'SIGN' to check whether we have to add it or subtract it.
        int sign;

        if (row < totalRows - 1) {
            sign = 1;
        } else {
            sign = -1;
        }

        /*
            If we have a prior valid mapping, then use that mapping.
            The second condition is for the leading zeros.
        */
        if (letToDig.count(letter) && (letToDig[letter] != 0 || (letToDig[letter] == 0 && w.length() == 1) || col != w.length() - 1)) {

            return isAnyMapping(words, row + 1, col, bal + sign * letToDig[letter],
                letToDig, digToLet, totalRows, totalCols);

        }
        // Choose a new mapping.
        else {
            for (int i = 0; i < 10; i++) {

                // If 'i'th mapping is valid then select it.
                if (digToLet[i] == '-' && (i != 0 || (i == 0 && w.length() == 1) || col != w.length() - 1)) {
                    digToLet[i] = letter;
                    letToDig[letter] = i;

                    // Call the function again with the new mapping.
                    bool x = isAnyMapping(words, row + 1, col, bal + sign * letToDig[letter],
                        letToDig, digToLet, totalRows, totalCols);

                    if (x == true) {
                        return true;
                    }

                    // Unselect the mapping.
                    digToLet[i] = '-';
                    if (letToDig.find(letter) != letToDig.end()) {
                        letToDig.erase(letter);
                    }
                }
            }
        }

        // If nothing is correct then just return false.
        return false;
    }

    bool isSolvable(vector<string>& words, string result) {
        // Add the string 'RESULT' in the vector 'WORDS'.
        words.push_back(result);

        int totalRows;
        int totalCols;

        // Initialize 'TOTALROWS' with the size of the vector.
        totalRows = words.size();

        // Find the longest string in the vector and set 'TOTALCOLS' with the size of that string.
        totalCols = 0;

        for (int i = 0; i < words.size(); i++) {

            // If the current string is the longest then update 'TOTALCOLS' with its length.
            if (totalCols < words[i].size()) {
                totalCols = words[i].size();
            }
        }

        // Create a HashMap for the letter to digit mapping.
        unordered_map<char, int> letToDig;

        // Create a vector for the digit to letter mapping.
        vector<char> digToLet(10, '-');

        return isAnyMapping(words, 0, 0, 0, letToDig, digToLet, totalRows, totalCols);
    }
};
```

#### Go

```go
func isSolvable(words []string, result string) bool {
	words = append(words, result)
	totalRows := len(words)
	totalCols := 0
	for _, w := range words {
		if len(w) > totalCols {
			totalCols = len(w)
		}
	}
	letToDig := map[byte]int{}
	digToLet := [10]byte{}
	for i := range digToLet {
		digToLet[i] = '-'
	}
	var isAnyMapping func(row, col, bal int) bool
	isAnyMapping = func(row, col, bal int) bool {
		if col == totalCols {
			return bal == 0
		}
		if row == totalRows {
			return bal%10 == 0 && isAnyMapping(0, col+1, bal/10)
		}
		w := words[row]
		if col >= len(w) {
			return isAnyMapping(row+1, col, bal)
		}
		letter := w[len(w)-1-col]
		sign := 1
		if row == totalRows-1 {
			sign = -1
		}
		if d, ok := letToDig[letter]; ok && (d != 0 || (d == 0 && len(w) == 1) || col != len(w)-1) {
			return isAnyMapping(row+1, col, bal+sign*d)
		}
		for i := 0; i < 10; i++ {
			if digToLet[i] == '-' && (i != 0 || (i == 0 && len(w) == 1) || col != len(w)-1) {
				digToLet[i] = letter
				letToDig[letter] = i
				if isAnyMapping(row+1, col, bal+sign*letToDig[letter]) {
					return true
				}
				digToLet[i] = '-'
				delete(letToDig, letter)
			}
		}
		return false
	}
	return isAnyMapping(0, 0, 0)
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
