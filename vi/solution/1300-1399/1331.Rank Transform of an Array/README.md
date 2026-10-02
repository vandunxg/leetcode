---
comments: true
difficulty: Easy
rating: 1355
source: Biweekly Contest 18 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [1331. Rank Transform of an Array](https://leetcode.com/problems/rank-transform-of-an-array)

[中文文档](/solution/1300-1399/1331.Rank%20Transform%20of%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên&nbsp;<code>arr</code>, hãy thay mỗi phần tử bằng thứ hạng của nó.</p>

<p>Thứ hạng thể hiện độ lớn của phần tử và tuân theo các quy tắc sau:</p>

<ul>
	<li>Thứ hạng là số nguyên bắt đầu từ 1.</li>
	<li>Phần tử càng lớn thì thứ hạng càng cao. Nếu hai phần tử bằng nhau, chúng phải có cùng thứ hạng.</li>
	<li>Thứ hạng phải nhỏ nhất có thể.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [40,10,20,30]
<strong>Đầu ra:</strong> [4,1,2,3]
<strong>Giải thích</strong>: 40 là phần tử lớn nhất. 10 là phần tử nhỏ nhất. 20 là phần tử nhỏ thứ hai. 30 là phần tử nhỏ thứ ba.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [100,100,100]
<strong>Đầu ra:</strong> [1,1,1]
<strong>Giải thích</strong>: Các phần tử bằng nhau có cùng thứ hạng.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [37,12,28,9,100,56,80,5,12]
<strong>Đầu ra:</strong> [5,3,4,2,8,6,7,1,3]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>9</sup>&nbsp;&lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Nén tọa độ

<!-- thinking:start -->

> **Tư duy**
>
> Thay mỗi giá trị bằng thứ hạng bắt đầu từ $1$, các giá trị bằng nhau dùng chung một thứ hạng. So sánh từng cặp sẽ có độ phức tạp bậc hai với $n \le 10^5$. Thứ hạng chỉ phụ thuộc vào vị trí trong danh sách đã sắp xếp và loại bỏ trùng lặp $t$; dùng $\mathrm{bisect\_right}$ trên $t$ để tìm thứ hạng của mỗi $x$.

<!-- thinking:end -->

Đầu tiên, ta sao chép mảng thành $t$, sau đó sắp xếp và loại bỏ các phần tử trùng lặp để thu được mảng có độ dài $m$ và tăng nghiêm ngặt.

Tiếp theo, ta duyệt mảng ban đầu $arr$. Với mỗi phần tử $x$, dùng tìm kiếm nhị phân để tìm vị trí của $x$ trong $t$. Vị trí cộng một chính là thứ hạng của $x$.

Độ phức tạp thời gian là $O(n \times \log n)$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là độ dài của mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arrayRankTransform(self, arr: List[int]) -> List[int]:
        t = sorted(set(arr))
        return [bisect_right(t, x) for x in arr]
```

#### Java

```java
class Solution {
    public int[] arrayRankTransform(int[] arr) {
        int n = arr.length;
        int[] t = arr.clone();
        Arrays.sort(t);
        int m = 0;
        for (int i = 0; i < n; ++i) {
            if (i == 0 || t[i] != t[i - 1]) {
                t[m++] = t[i];
            }
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = Arrays.binarySearch(t, 0, m, arr[i]) + 1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<int> t = arr;
        sort(t.begin(), t.end());
        t.erase(unique(t.begin(), t.end()), t.end());
        vector<int> ans;
        for (int x : arr) {
            ans.push_back(upper_bound(t.begin(), t.end(), x) - t.begin());
        }
        return ans;
    }
};
```

#### Go

```go
func arrayRankTransform(arr []int) (ans []int) {
	t := make([]int, len(arr))
	copy(t, arr)
	sort.Ints(t)
	m := 0
	for i, x := range t {
		if i == 0 || x != t[i-1] {
			t[m] = x
			m++
		}
	}
	t = t[:m]
	for _, x := range arr {
		ans = append(ans, sort.SearchInts(t, x)+1)
	}
	return
}
```

#### TypeScript

```ts
function arrayRankTransform(arr: number[]): number[] {
    const t = [...arr].sort((a, b) => a - b);
    let m = 0;
    for (let i = 0; i < t.length; ++i) {
        if (i === 0 || t[i] !== t[i - 1]) {
            t[m++] = t[i];
        }
    }
    const search = (t: number[], right: number, x: number) => {
        let left = 0;
        while (left < right) {
            const mid = (left + right) >> 1;
            if (t[mid] > x) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    };
    const ans: number[] = [];
    for (const x of arr) {
        ans.push(search(t, m, x));
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn array_rank_transform(arr: Vec<i32>) -> Vec<i32> {
        let mut q1 = arr.clone();
        q1.sort_unstable();
        q1.dedup();
        arr.iter()
            .map(|q2| q1.binary_search(q2).unwrap() as i32 + 1)
            .collect()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Hash Map

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân cho từng phần tử sẽ lặp lại công việc. Sau khi sắp xếp các giá trị duy nhất, ta ánh xạ giá trị sang thứ hạng bằng hash map để tra cứu mỗi phần tử trong thời gian hằng số kỳ vọng.

<!-- thinking:end -->

<!-- tabs:start -->

#### TypeScript

```ts
function arrayRankTransform(arr: number[]): number[] {
    const sorted = [...new Set(arr)].sort((a, b) => a - b);
    const map = new Map<number, number>();
    let c = 1;

    for (const x of sorted) {
        map.set(x, c++);
    }

    return arr.map(x => map.get(x)!);
}
```

#### JavaScript

```js
function arrayRankTransform(arr) {
    const sorted = [...new Set(arr)].sort((a, b) => a - b);
    const map = new Map();
    let c = 1;

    for (const x of sorted) {
        map.set(x, c++);
    }

    return arr.map(x => map.get(x));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
