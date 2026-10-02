---
comments: true
difficulty: Easy
rating: 1179
source: Biweekly Contest 15 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1287. Element Appearing More Than 25% In Sorted Array](https://leetcode.com/problems/element-appearing-more-than-25-in-sorted-array)

[中文文档](/solution/1200-1299/1287.Element%20Appearing%20More%20Than%2025%25%20In%20Sorted%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên được <strong>sắp xếp</strong> theo thứ tự không giảm. Có chính xác một số nguyên xuất hiện hơn 25% số lần trong mảng; hãy trả về số đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,2,2,6,6,6,6,7,10]
<strong>Đầu ra:</strong> 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,1]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Trong mảng đã sắp xếp, một giá trị xuất hiện hơn $25\%$ số lần, tức là chiếm ít nhất $\lfloor n/4 \rfloor$ vị trí. Nếu $arr[i]=arr[i+\lfloor n/4 \rfloor]$, giá trị đó đã xuất hiện đủ số lần cần thiết. Lần đầu tiên điều kiện này đúng khi duyệt từ trái sang phải chính là đáp án; thứ tự đã sắp xếp giúp thay việc đếm bằng phép so sánh chỉ số.

<!-- thinking:end -->

Ta duyệt mảng $\textit{arr}$ từ đầu. Với mỗi phần tử $\textit{arr}[i]$, ta kiểm tra xem nó có bằng $\textit{arr}[i + \left\lfloor \frac{n}{4} \right\rfloor]$ hay không, trong đó $n$ là độ dài mảng. Nếu hai giá trị bằng nhau thì $\textit{arr}[i]$ là phần tử cần tìm và ta trả về ngay.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSpecialInteger(self, arr: List[int]) -> int:
        n = len(arr)
        for i, x in enumerate(arr):
            if x == arr[(i + (n >> 2))]:
                return x
```

#### Java

```java
class Solution {
    public int findSpecialInteger(int[] arr) {
        for (int i = 0;; ++i) {
            if (arr[i] == (arr[i + (arr.length >> 2)])) {
                return arr[i];
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findSpecialInteger(vector<int>& arr) {
        for (int i = 0;; ++i) {
            if (arr[i] == (arr[i + (arr.size() >> 2)])) {
                return arr[i];
            }
        }
    }
};
```

#### Go

```go
func findSpecialInteger(arr []int) int {
	for i := 0; ; i++ {
		if arr[i] == arr[i+len(arr)/4] {
			return arr[i]
		}
	}
}
```

#### TypeScript

```ts
function findSpecialInteger(arr: number[]): number {
    const n = arr.length;
    for (let i = 0; ; ++i) {
        if (arr[i] === arr[i + (n >> 2)]) {
            return arr[i];
        }
    }
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {number}
 */
var findSpecialInteger = function (arr) {
    const n = arr.length;
    for (let i = 0; ; ++i) {
        if (arr[i] === arr[i + (n >> 2)]) {
            return arr[i];
        }
    }
};
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $arr
     * @return Integer
     */
    function findSpecialInteger($arr) {
        $n = count($arr);
        for ($i = 0; ; ++$i) {
            if ($arr[$i] == $arr[$i + ($n >> 2)]) {
                return $arr[$i];
            }
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
