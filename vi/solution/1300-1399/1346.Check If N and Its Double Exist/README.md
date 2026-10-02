---
comments: true
difficulty: Easy
rating: 1225
source: Weekly Contest 175 Q1
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [1346. Check If N and Its Double Exist](https://leetcode.com/problems/check-if-n-and-its-double-exist)

[中文文档](/solution/1300-1399/1346.Check%20If%20N%20and%20Its%20Double%20Exist/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, hãy kiểm tra xem có tồn tại hai chỉ số <code>i</code> và <code>j</code> sao cho:</p>

<ul>
	<li><code>i != j</code></li>
	<li><code>0 &lt;= i, j &lt; arr.length</code></li>
	<li><code>arr[i] == 2 * arr[j]</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [10,2,5,3]
<strong>Output:</strong> true
<strong>Giải thích:</strong> Với i = 0 và j = 2, arr[i] == 10 == 2 * 5 == 2 * arr[j]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,1,7,11]
<strong>Output:</strong> false
<strong>Giải thích:</strong> Không tồn tại i và j nào thỏa mãn các điều kiện.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 500</code></li>
	<li><code>-10<sup>3</sup> &lt;= arr[i] &lt;= 10<sup>3</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Kiểm tra xem có cặp $i \neq j$ nào thỏa mãn $arr[i]=2\,arr[j]$ không. Duyệt hai vòng lặp vẫn ổn với $n \le 500$, nhưng chỉ cần một lượt với hash set: nếu $2x$ đã xuất hiện trước đó, hoặc $x$ chẵn và $x/2$ đã xuất hiện, thì tìm thấy đáp án; nếu không, thêm $x$ vào set. Set chỉ chứa các phần tử đã duyệt trước đó nên hai chỉ số luôn khác nhau.

<!-- thinking:end -->

Ta dùng hash table $s$ để lưu các phần tử đã duyệt.

Duyệt mảng $arr$. Với mỗi phần tử $x$, nếu $2x$ có trong hash table $s$, hoặc $x$ chẵn và $x/2$ có trong hash table $s$, thì trả về `true`. Nếu không, thêm $x$ vào hash table $s$.

Nếu duyệt hết mảng mà không tìm thấy phần tử nào thỏa điều kiện, trả về `false`.

Độ phức tạp thời gian và không gian đều là $O(n)$, trong đó $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def checkIfExist(self, arr: List[int]) -> bool:
        s = set()
        for x in arr:
            if x * 2 in s or (x % 2 == 0 and x // 2 in s):
                return True
            s.add(x)
        return False
```

#### Java

```java
class Solution {
    public boolean checkIfExist(int[] arr) {
        Set<Integer> s = new HashSet<>();
        for (int x : arr) {
            if (s.contains(x * 2) || ((x % 2 == 0 && s.contains(x / 2)))) {
                return true;
            }
            s.add(x);
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool checkIfExist(vector<int>& arr) {
        unordered_set<int> s;
        for (int x : arr) {
            if (s.contains(x * 2) || (x % 2 == 0 && s.contains(x / 2))) {
                return true;
            }
            s.insert(x);
        }
        return false;
    }
};
```

#### Go

```go
func checkIfExist(arr []int) bool {
	s := map[int]bool{}
	for _, x := range arr {
		if s[x*2] || (x%2 == 0 && s[x/2]) {
			return true
		}
		s[x] = true
	}
	return false
}
```

#### TypeScript

```ts
function checkIfExist(arr: number[]): boolean {
    const s: Set<number> = new Set();
    for (const x of arr) {
        if (s.has(x * 2) || (x % 2 === 0 && s.has((x / 2) | 0))) {
            return true;
        }
        s.add(x);
    }
    return false;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
