---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.02. Words Frequency](https://leetcode.cn/problems/words-frequency-lcci)

[中文文档](/lcci/16.02.Words%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Thiết kế một phương thức để tìm tần suất xuất hiện của bất kỳ từ nào trong một cuốn sách. Nếu chạy thuật toán này nhiều lần thì sao?</p>

<p>Bạn cần triển khai các phương thức sau:</p>

<ul>
	<li>constructor <code>WordsFrequency(book)</code>, tham số là một mảng các chuỗi, đại diện cho cuốn sách.</li>
	<li><code>get(word)</code>&nbsp;lấy tần suất xuất hiện của <code>word</code> trong cuốn sách.&nbsp;</li>
</ul>

<p><strong>Ví dụ: </strong></p>

<pre>

WordsFrequency wordsFrequency = new WordsFrequency({&quot;i&quot;, &quot;have&quot;, &quot;an&quot;, &quot;apple&quot;, &quot;he&quot;, &quot;have&quot;, &quot;a&quot;, &quot;pen&quot;});

wordsFrequency.get(&quot;you&quot;); //returns 0，&quot;you&quot; is not in the book

wordsFrequency.get(&quot;have&quot;); //returns 2，&quot;have&quot; occurs twice in the book

wordsFrequency.get(&quot;an&quot;); //returns 1

wordsFrequency.get(&quot;apple&quot;); //returns 1

wordsFrequency.get(&quot;pen&quot;); //returns 1

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
    <li><code>There are only lowercase letters in book[i].</code></li>
    <li><code>1 &lt;= book.length &lt;= 100000</code></li>
    <li><code>1 &lt;= book[i].length &lt;= 10</code></li>
    <li>Hàm <code>get</code> sẽ không được gọi quá&nbsp;100000 lần.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Có nhiều truy vấn yêu cầu biết một từ xuất hiện bao nhiêu lần. Mỗi lần quét lại cuốn sách sẽ lặp lại công việc tuyến tính.
>
> Chỉ cần một lượt tiền xử lý là có thể thực hiện các lần tra cứu với độ phức tạp kỳ vọng $O(1)$.
>
> `Counter(book)` tạo bảng; `get` trả về $cnt[word]$. Dung lượng phụ thuộc vào số từ phân biệt.

<!-- thinking:end -->

Ta sử dụng một hash table $cnt$ để đếm số lần xuất hiện của mỗi từ trong $book$.

Khi gọi hàm `get`, ta chỉ cần trả về số lần xuất hiện của từ tương ứng trong $cnt$.

Về độ phức tạp thời gian, độ phức tạp thời gian khởi tạo hash table $cnt$ là $O(n)$, trong đó $n$ là độ dài của $book$. Độ phức tạp thời gian của hàm `get` là $O(1)$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class WordsFrequency:
    def __init__(self, book: List[str]):
        self.cnt = Counter(book)

    def get(self, word: str) -> int:
        return self.cnt[word]


# Your WordsFrequency object will be instantiated and called as such:
# obj = WordsFrequency(book)
# param_1 = obj.get(word)
```

#### Java

```java
class WordsFrequency {
    private Map<String, Integer> cnt = new HashMap<>();

    public WordsFrequency(String[] book) {
        for (String x : book) {
            cnt.merge(x, 1, Integer::sum);
        }
    }

    public int get(String word) {
        return cnt.getOrDefault(word, 0);
    }
}

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * WordsFrequency obj = new WordsFrequency(book);
 * int param_1 = obj.get(word);
 */
```

#### C++

```cpp
class WordsFrequency {
public:
    WordsFrequency(vector<string>& book) {
        for (auto& x : book) {
            ++cnt[x];
        }
    }

    int get(string word) {
        return cnt[word];
    }

private:
    unordered_map<string, int> cnt;
};

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * WordsFrequency* obj = new WordsFrequency(book);
 * int param_1 = obj->get(word);
 */
```

#### Go

```go
type WordsFrequency struct {
	cnt map[string]int
}

func Constructor(book []string) WordsFrequency {
	cnt := map[string]int{}
	for _, x := range book {
		cnt[x]++
	}
	return WordsFrequency{cnt}
}

func (this *WordsFrequency) Get(word string) int {
	return this.cnt[word]
}

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * obj := Constructor(book);
 * param_1 := obj.Get(word);
 */
```

#### TypeScript

```ts
class WordsFrequency {
    private cnt: Map<string, number>;

    constructor(book: string[]) {
        const cnt = new Map<string, number>();
        for (const word of book) {
            cnt.set(word, (cnt.get(word) ?? 0) + 1);
        }
        this.cnt = cnt;
    }

    get(word: string): number {
        return this.cnt.get(word) ?? 0;
    }
}

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * var obj = new WordsFrequency(book)
 * var param_1 = obj.get(word)
 */
```

#### Rust

```rust
use std::collections::HashMap;
struct WordsFrequency {
    cnt: HashMap<String, i32>,
}

/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl WordsFrequency {
    fn new(book: Vec<String>) -> Self {
        let mut cnt = HashMap::new();
        for word in book.into_iter() {
            *cnt.entry(word).or_insert(0) += 1;
        }
        Self { cnt }
    }

    fn get(&self, word: String) -> i32 {
        *self.cnt.get(&word).unwrap_or(&0)
    }
}
```

#### JavaScript

```js
/**
 * @param {string[]} book
 */
var WordsFrequency = function (book) {
    this.cnt = new Map();
    for (const x of book) {
        this.cnt.set(x, (this.cnt.get(x) || 0) + 1);
    }
};

/**
 * @param {string} word
 * @return {number}
 */
WordsFrequency.prototype.get = function (word) {
    return this.cnt.get(word) || 0;
};

/**
 * Your WordsFrequency object will be instantiated and called as such:
 * var obj = new WordsFrequency(book)
 * var param_1 = obj.get(word)
 */
```

#### Swift

```swift
class WordsFrequency {
    private var cnt: [String: Int] = [:]

    init(_ book: [String]) {
        for word in book {
            cnt[word, default: 0] += 1
        }
    }

    func get(_ word: String) -> Int {
        return cnt[word, default: 0]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
