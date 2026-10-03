---
comments: true
difficulty: Easy
rating: 1454
source: Weekly Contest 270 Q1
tags:
    - Recursion
    - Array
    - Hash Table
    - Enumeration
    - Sorting
---

<!-- problem:start -->

# [2094. Finding 3-Digit Even Numbers](https://leetcode.com/problems/finding-3-digit-even-numbers)

[中文文档](/solution/2000-2099/2094.Finding%203-Digit%20Even%20Numbers/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>digits</code>, trong đó mỗi phần tử là một chữ số. Mảng có thể chứa các phần tử trùng lặp.</p>

<p>Bạn cần tìm <strong>tất cả</strong> các số nguyên <strong>duy nhất</strong> thỏa mãn các yêu cầu sau:</p>

<ul>
	<li>Số nguyên được tạo bởi phép <strong>nối</strong> <strong>ba</strong> phần tử từ <code>digits</code> theo <strong>bất kỳ</strong> thứ tự nào.</li>
	<li>Số nguyên không có <strong>chữ số 0 ở đầu</strong>.</li>
	<li>Số nguyên là <strong>số chẵn</strong>.</li>
</ul>

<p>Ví dụ, nếu <code>digits</code> là <code>[1, 2, 3]</code>, các số nguyên <code>132</code> và <code>312</code> thỏa mãn các yêu cầu.</p>

<p>Hãy trả về <em>một mảng <strong>đã sắp xếp</strong> gồm các số nguyên duy nhất.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [2,1,3,0]
<strong>Đầu ra:</strong> [102,120,130,132,210,230,302,310,312,320]
<strong>Giải thích:</strong> Tất cả các số nguyên có thể tạo thành và thỏa mãn các yêu cầu đều nằm trong mảng đầu ra.
Lưu ý rằng không có <strong>số lẻ</strong> hoặc số nguyên có <strong>chữ số 0 ở đầu</strong>.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [2,2,8,8,2]
<strong>Đầu ra:</strong> [222,228,282,288,822,828,882]
<strong>Giải thích:</strong> Một chữ số có thể được sử dụng số lần đúng bằng số lần nó xuất hiện trong digits.
Trong ví dụ này, chữ số 8 được sử dụng hai lần trong mỗi số 288, 828 và 882.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> digits = [3,7,5]
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Không thể tạo được số nguyên <strong>chẵn</strong> nào từ các chữ số đã cho.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= digits.length &lt;= 100</code></li>
	<li><code>0 &lt;= digits[i] &lt;= 9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Hãy tạo các số chẵn có ba chữ số khác nhau từ `digits` mà không có chữ số 0 ở đầu. Chỉ có $450$ số chẵn như vậy, nên ta liệt kê chúng và kiểm tra tần suất thay vì hoán vị các phần tử đầu vào.
>
> Đếm các chữ số $0..9$, sau đó tách từng số chẵn trong $[100,998]$ và so sánh số lần xuất hiện.

<!-- thinking:end -->

Trước tiên, ta đếm số lần xuất hiện của mỗi chữ số trong $\textit{digits}$ và lưu kết quả vào một mảng hoặc hash table $\textit{cnt}$.

Sau đó, ta liệt kê tất cả các số chẵn trong khoảng $[100, 1000)$, kiểm tra xem số lần xuất hiện của mỗi chữ số trong số chẵn đó có vượt quá số lần xuất hiện tương ứng trong $\textit{cnt}$ hay không. Nếu không, ta thêm số chẵn này vào mảng kết quả.

Cuối cùng, ta trả về mảng kết quả.

Độ phức tạp thời gian là $O(k \times 10^k)$, trong đó $k$ là số chữ số của số chẵn cần tìm, bằng $3$ trong bài toán này. Bỏ qua phần không gian mà đáp án chiếm dụng, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findEvenNumbers(self, digits: List[int]) -> List[int]:
        cnt = Counter(digits)
        ans = []
        for x in range(100, 1000, 2):
            cnt1 = Counter()
            y = x
            while y:
                y, v = divmod(y, 10)
                cnt1[v] += 1
            if all(cnt[i] >= cnt1[i] for i in range(10)):
                ans.append(x)
        return ans
```

#### Java

```java
class Solution {
    public int[] findEvenNumbers(int[] digits) {
        int[] cnt = new int[10];
        for (int x : digits) {
            ++cnt[x];
        }
        List<Integer> ans = new ArrayList<>();
        for (int x = 100; x < 1000; x += 2) {
            int[] cnt1 = new int[10];
            for (int y = x; y > 0; y /= 10) {
                ++cnt1[y % 10];
            }
            boolean ok = true;
            for (int i = 0; i < 10 && ok; ++i) {
                ok = cnt[i] >= cnt1[i];
            }
            if (ok) {
                ans.add(x);
            }
        }
        return ans.stream().mapToInt(i -> i).toArray();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findEvenNumbers(vector<int>& digits) {
        int cnt[10]{};
        for (int x : digits) {
            ++cnt[x];
        }
        vector<int> ans;
        for (int x = 100; x < 1000; x += 2) {
            int cnt1[10]{};
            for (int y = x; y; y /= 10) {
                ++cnt1[y % 10];
            }
            bool ok = true;
            for (int i = 0; i < 10 && ok; ++i) {
                ok = cnt[i] >= cnt1[i];
            }
            if (ok) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findEvenNumbers(digits []int) (ans []int) {
	cnt := [10]int{}
	for _, x := range digits {
		cnt[x]++
	}
	for x := 100; x < 1000; x += 2 {
		cnt1 := [10]int{}
		for y := x; y > 0; y /= 10 {
			cnt1[y%10]++
		}
		ok := true
		for i := 0; i < 10 && ok; i++ {
			ok = cnt[i] >= cnt1[i]
		}
		if ok {
			ans = append(ans, x)
		}
	}
	return
}
```

#### TypeScript

```ts
function findEvenNumbers(digits: number[]): number[] {
    const cnt: number[] = Array(10).fill(0);
    for (const x of digits) {
        ++cnt[x];
    }
    const ans: number[] = [];
    for (let x = 100; x < 1000; x += 2) {
        const cnt1: number[] = Array(10).fill(0);
        for (let y = x; y; y = Math.floor(y / 10)) {
            ++cnt1[y % 10];
        }
        let ok = true;
        for (let i = 0; i < 10 && ok; ++i) {
            ok = cnt[i] >= cnt1[i];
        }
        if (ok) {
            ans.push(x);
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[]} digits
 * @return {number[]}
 */
var findEvenNumbers = function (digits) {
    const cnt = Array(10).fill(0);
    for (const x of digits) {
        ++cnt[x];
    }
    const ans = [];
    for (let x = 100; x < 1000; x += 2) {
        const cnt1 = Array(10).fill(0);
        for (let y = x; y; y = Math.floor(y / 10)) {
            ++cnt1[y % 10];
        }
        let ok = true;
        for (let i = 0; i < 10 && ok; ++i) {
            ok = cnt[i] >= cnt1[i];
        }
        if (ok) {
            ans.push(x);
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
