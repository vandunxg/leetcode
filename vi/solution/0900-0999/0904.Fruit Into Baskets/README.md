---
comments: true
difficulty: Medium
tags:
    - Array
    - Hash Table
    - Sliding Window
---

<!-- problem:start -->

# [904. Fruit Into Baskets](https://leetcode.com/problems/fruit-into-baskets)

[中文文档](/solution/0900-0999/0904.Fruit%20Into%20Baskets/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang đến một trang trại có một hàng cây ăn quả xếp từ trái sang phải. Các cây được biểu diễn bằng mảng số nguyên <code>fruits</code>, trong đó <code>fruits[i]</code> là <strong>loại</strong> quả mà cây thứ <code>i<sup>th</sup></code> cho ra.</p>

<p>Bạn muốn hái được nhiều quả nhất có thể, nhưng chủ trang trại đưa ra một số quy tắc nghiêm ngặt:</p>

<ul>
	<li>Bạn chỉ có <strong>hai</strong> giỏ, mỗi giỏ chỉ đựng được <strong>một loại</strong> quả. Mỗi giỏ có thể chứa số lượng quả không giới hạn.</li>
	<li>Bắt đầu từ cây bất kỳ do bạn chọn, khi di chuyển sang phải, bạn phải hái <strong>đúng một quả</strong> từ <strong>mọi</strong> cây (kể cả cây bắt đầu). Các quả đã hái phải vừa với một trong hai giỏ.</li>
	<li>Khi gặp cây có quả không thể cho vào giỏ, bạn phải dừng lại.</li>
</ul>

<p>Cho mảng số nguyên <code>fruits</code>, hãy trả về <em>số quả <strong>nhiều nhất</strong> mà bạn có thể hái</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> fruits = [<u>1,2,1</u>]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể hái quả ở cả 3 cây.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> fruits = [0,<u>1,2,2</u>]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Ta có thể hái quả ở các cây [1,2,2].
Nếu bắt đầu từ cây đầu tiên, ta chỉ hái được quả ở các cây [0,1].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> fruits = [1,<u>2,3,2,2</u>]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Ta có thể hái quả ở các cây [2,3,2,2].
Nếu bắt đầu từ cây đầu tiên, ta chỉ hái được quả ở các cây [1,2].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= fruits.length &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= fruits[i] &lt; fruits.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Sliding Window

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm mảng con dài nhất chứa không quá hai loại quả. Vì $n\le 10^5$, cách thử mọi cặp đầu mút rồi đếm lại số loại sẽ quá chậm. Khi thêm một quả vào đầu phải, nếu cửa sổ có hơn hai loại thì cần dịch đầu trái.
>
> Dùng counter lưu tần suất; số key chính là số loại quả. Khi số loại vượt quá $2$, thu hẹp cửa sổ từ bên trái rồi cập nhật độ dài lớn nhất.

<!-- thinking:end -->

Ta dùng hash table $cnt$ để lưu các loại quả và số lượng tương ứng trong cửa sổ hiện tại, cùng hai con trỏ $j$ và $i$ để xác định đầu trái và đầu phải của cửa sổ.

Ta duyệt mảng $\textit{fruits}$, thêm quả hiện tại $x$ vào cửa sổ, tức là $cnt[x]++$, rồi kiểm tra cửa sổ có nhiều hơn $2$ loại quả hay không. Nếu có, dịch đầu trái $j$ sang phải cho đến khi cửa sổ còn không quá $2$ loại. Sau đó cập nhật đáp án bằng $ans = \max(ans, i - j + 1)$.

Duyệt xong, ta thu được đáp án cuối cùng.

```
1 2 3 2 2 1 4
^   ^
j   i


1 2 3 2 2 1 4
  ^ ^
  j i


1 2 3 2 2 1 4
  ^     ^
  j     i
```

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, với $n$ là độ dài mảng $\textit{fruits}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        cnt = Counter()
        ans = j = 0
        for i, x in enumerate(fruits):
            cnt[x] += 1
            while len(cnt) > 2:
                y = fruits[j]
                cnt[y] -= 1
                if cnt[y] == 0:
                    cnt.pop(y)
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

#### Java

```java
class Solution {
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int ans = 0;
        for (int i = 0, j = 0; i < fruits.length; ++i) {
            int x = fruits[i];
            cnt.merge(x, 1, Integer::sum);
            while (cnt.size() > 2) {
                int y = fruits[j++];
                if (cnt.merge(y, -1, Integer::sum) == 0) {
                    cnt.remove(y);
                }
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> cnt;
        int ans = 0;
        for (int i = 0, j = 0; i < fruits.size(); ++i) {
            int x = fruits[i];
            ++cnt[x];
            while (cnt.size() > 2) {
                int y = fruits[j++];
                if (--cnt[y] == 0) {
                    cnt.erase(y);
                }
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func totalFruit(fruits []int) int {
	cnt := map[int]int{}
	ans, j := 0, 0
	for i, x := range fruits {
		cnt[x]++
		for ; len(cnt) > 2; j++ {
			y := fruits[j]
			cnt[y]--
			if cnt[y] == 0 {
				delete(cnt, y)
			}
		}
		ans = max(ans, i-j+1)
	}
	return ans
}
```

#### TypeScript

```ts
function totalFruit(fruits: number[]): number {
    const n = fruits.length;
    const cnt: Map<number, number> = new Map();
    let ans = 0;
    for (let i = 0, j = 0; i < n; ++i) {
        cnt.set(fruits[i], (cnt.get(fruits[i]) || 0) + 1);
        for (; cnt.size > 2; ++j) {
            cnt.set(fruits[j], cnt.get(fruits[j])! - 1);
            if (!cnt.get(fruits[j])) {
                cnt.delete(fruits[j]);
            }
        }
        ans = Math.max(ans, i - j + 1);
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn total_fruit(fruits: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        let mut ans = 0;
        let mut j = 0;

        for (i, &x) in fruits.iter().enumerate() {
            *cnt.entry(x).or_insert(0) += 1;

            while cnt.len() > 2 {
                let y = fruits[j];
                j += 1;
                *cnt.get_mut(&y).unwrap() -= 1;
                if cnt[&y] == 0 {
                    cnt.remove(&y);
                }
            }

            ans = ans.max(i - j + 1);
        }

        ans as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int TotalFruit(int[] fruits) {
        var cnt = new Dictionary<int, int>();
        int ans = 0;
        for (int i = 0, j = 0; i < fruits.Length; ++i) {
            int x = fruits[i];
            if (cnt.ContainsKey(x)) {
                cnt[x]++;
            } else {
                cnt[x] = 1;
            }
            while (cnt.Count > 2) {
                int y = fruits[j++];
                if (cnt.ContainsKey(y)) {
                    cnt[y]--;
                    if (cnt[y] == 0) {
                        cnt.Remove(y);
                    }
                }
            }
            ans = Math.Max(ans, i - j + 1);
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sliding Window có độ dài đơn điệu

<!-- thinking:start -->

> **Tư duy**
>
> Cách 1 thu hẹp cửa sổ và cập nhật đáp án ở mỗi bước. Nhưng vì chỉ cần độ dài lớn nhất, ta có thể bỏ qua các cửa sổ ngắn hơn đã gặp trước đó. Cứ mở rộng cửa sổ; khi xuất hiện loại thứ ba, chỉ cần dịch đầu trái một lần. Cuối cùng, $n-j$ là độ dài lớn nhất có thể.

<!-- thinking:end -->

Ở Lời giải 1, kích thước cửa sổ lúc tăng lúc giảm nên ta phải cập nhật đáp án ở mỗi bước.

Tuy nhiên, bài toán chỉ yêu cầu số quả tối đa, tức cửa sổ "lớn nhất". Ta không cần thu hẹp cửa sổ theo cách cũ mà chỉ cần để cửa sổ tăng đơn điệu. Vì vậy, code bỏ thao tác cập nhật đáp án mỗi lần; sau khi duyệt xong, chỉ cần trả về kích thước cửa sổ.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là độ dài mảng $\textit{fruits}$. 

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def totalFruit(self, fruits: List[int]) -> int:
        cnt = Counter()
        j = 0
        for x in fruits:
            cnt[x] += 1
            if len(cnt) > 2:
                y = fruits[j]
                cnt[y] -= 1
                if cnt[y] == 0:
                    cnt.pop(y)
                j += 1
        return len(fruits) - j
```

#### Java

```java
class Solution {
    public int totalFruit(int[] fruits) {
        Map<Integer, Integer> cnt = new HashMap<>();
        int j = 0, n = fruits.length;
        for (int x : fruits) {
            cnt.merge(x, 1, Integer::sum);
            if (cnt.size() > 2) {
                int y = fruits[j++];
                if (cnt.merge(y, -1, Integer::sum) == 0) {
                    cnt.remove(y);
                }
            }
        }
        return n - j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int totalFruit(vector<int>& fruits) {
        unordered_map<int, int> cnt;
        int j = 0, n = fruits.size();
        for (int& x : fruits) {
            ++cnt[x];
            if (cnt.size() > 2) {
                int y = fruits[j++];
                if (--cnt[y] == 0) {
                    cnt.erase(y);
                }
            }
        }
        return n - j;
    }
};
```

#### Go

```go
func totalFruit(fruits []int) int {
	cnt := map[int]int{}
	j := 0
	for _, x := range fruits {
		cnt[x]++
		if len(cnt) > 2 {
			y := fruits[j]
			cnt[y]--
			if cnt[y] == 0 {
				delete(cnt, y)
			}
			j++
		}
	}
	return len(fruits) - j
}
```

#### TypeScript

```ts
function totalFruit(fruits: number[]): number {
    const n = fruits.length;
    const cnt: Map<number, number> = new Map();
    let j = 0;
    for (const x of fruits) {
        cnt.set(x, (cnt.get(x) || 0) + 1);
        if (cnt.size > 2) {
            cnt.set(fruits[j], cnt.get(fruits[j])! - 1);
            if (!cnt.get(fruits[j])) {
                cnt.delete(fruits[j]);
            }
            ++j;
        }
    }
    return n - j;
}
```

#### Rust

```rust
use std::collections::HashMap;

impl Solution {
    pub fn total_fruit(fruits: Vec<i32>) -> i32 {
        let mut cnt = HashMap::new();
        let mut j = 0;
        let n = fruits.len();

        for &x in &fruits {
            *cnt.entry(x).or_insert(0) += 1;
            if cnt.len() > 2 {
                let y = fruits[j];
                j += 1;
                *cnt.get_mut(&y).unwrap() -= 1;
                if cnt[&y] == 0 {
                    cnt.remove(&y);
                }
            }
        }

        (n - j) as i32
    }
}
```

#### C#

```cs
public class Solution {
    public int TotalFruit(int[] fruits) {
        var cnt = new Dictionary<int, int>();
        int j = 0, n = fruits.Length;
        foreach (int x in fruits) {
            if (cnt.ContainsKey(x)) {
                cnt[x]++;
            } else {
                cnt[x] = 1;
            }

            if (cnt.Count > 2) {
                int y = fruits[j++];
                if (cnt.ContainsKey(y)) {
                    cnt[y]--;
                    if (cnt[y] == 0) {
                        cnt.Remove(y);
                    }
                }
            }
        }
        return n - j;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
