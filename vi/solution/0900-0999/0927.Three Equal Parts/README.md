---
comments: true
difficulty: Hard
tags:
    - Array
    - Math
---

<!-- problem:start -->

# [927. Three Equal Parts](https://leetcode.com/problems/three-equal-parts)

[中文文档](/solution/0900-0999/0927.Three%20Equal%20Parts/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng <code>arr</code> chỉ gồm các số 0 và 1. Hãy chia mảng thành <strong>ba phần không rỗng</strong> sao cho cả ba phần biểu diễn cùng một giá trị nhị phân.</p>

<p>Nếu có thể, hãy trả về một cặp <code>[i, j]</code> bất kỳ thỏa mãn <code>i + 1 &lt; j</code>, sao cho:</p>

<ul>
	<li><code>arr[0], arr[1], ..., arr[i]</code> là phần thứ nhất,</li>
	<li><code>arr[i + 1], arr[i + 2], ..., arr[j - 1]</code> là phần thứ hai, và</li>
	<li><code>arr[j], arr[j + 1], ..., arr[arr.length - 1]</code> là phần thứ ba.</li>
	<li>Cả ba phần có cùng giá trị nhị phân.</li>
</ul>

<p>Nếu không thể chia như vậy, hãy trả về <code>[-1, -1]</code>.</p>

<p>Lưu ý, cần xét toàn bộ phần khi xác định giá trị nhị phân mà phần đó biểu diễn. Ví dụ, <code>[1,1,0]</code> biểu diễn số <code>6</code> ở hệ thập phân, không phải <code>3</code>. Ngoài ra, <strong>được phép</strong> có các số 0 ở đầu, nên <code>[0,1,1]</code> và <code>[1,1]</code> biểu diễn cùng một giá trị.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> arr = [1,0,1,0,1]
<strong>Đầu ra:</strong> [0,3]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> arr = [1,1,0,1,1]
<strong>Đầu ra:</strong> [-1,-1]
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Đầu vào:</strong> arr = [1,1,0,0,1]
<strong>Đầu ra:</strong> [0,2]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>arr[i]</code> là <code>0</code> hoặc <code>1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + ba con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Chia mảng nhị phân thành ba phần có cùng giá trị. Các số 0 ở đầu không làm thay đổi giá trị; điều cần khớp là số lượng số 1 và các bit theo sau. Nếu tổng số bit 1 không chia hết cho $3$, không có đáp án; nếu mảng chỉ gồm số 0 thì có thể chia ở bất kỳ vị trí nào.
>
> Mỗi phần cần có $cnt/3$ bit 1. Tìm vị trí bit 1 đầu tiên của mỗi phần ba, rồi đồng thời tiến ba con trỏ cho đến khi phần cuối kết thúc. Cách chia hợp lệ khi mọi bit tương ứng đều giống nhau.

<!-- thinking:end -->

Gọi độ dài mảng là $n$ và số lượng bit 1 trong mảng là $cnt$.

Hiển nhiên, $cnt$ phải chia hết cho $3$; nếu không, không thể chia mảng thành ba phần bằng nhau và ta có thể trả về $[-1, -1]$ ngay. Nếu $cnt$ bằng $0$, nghĩa là mọi phần tử trong mảng đều là 0, khi đó ta có thể trả về trực tiếp $[0, n - 1]$.

Chia $cnt$ cho $3$ để tìm số bit 1 trong mỗi phần, rồi tìm vị trí bit 1 đầu tiên của từng phần trong mảng `arr` (xem hàm $find(x)$ trong code bên dưới), lần lượt ký hiệu là $i$, $j$, $k$.

```bash
0 1 1 0 0 0 1 1 0 0 0 0 0 1 1 0 0
  ^         ^             ^
  i         j             k
```

Sau đó, bắt đầu từ $i$, $j$, $k$ và đồng thời duyệt qua từng phần để kiểm tra các giá trị tương ứng có bằng nhau hay không. Nếu bằng nhau, tiếp tục duyệt cho đến khi $k$ đến cuối mảng `arr`.

```bash
0 1 1 0 0 0 1 1 0 0 0 0 0 1 1 0 0
          ^         ^             ^
          i         j             k
```

Sau khi duyệt xong, nếu $k=n$ thì cách chia thành ba phần bằng nhau hợp lệ, ta trả về $[i - 1, j]$; nếu không, trả về $[-1, -1]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của `arr`. Độ phức tạp không gian là $O(1)$.

Bài tương tự:

- [1573. Number of Ways to Split a String](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1573.Number%20of%20Ways%20to%20Split%20a%20String/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def threeEqualParts(self, arr: List[int]) -> List[int]:
        def find(x):
            s = 0
            for i, v in enumerate(arr):
                s += v
                if s == x:
                    return i

        n = len(arr)
        cnt, mod = divmod(sum(arr), 3)
        if mod:
            return [-1, -1]
        if cnt == 0:
            return [0, n - 1]

        i, j, k = find(1), find(cnt + 1), find(cnt * 2 + 1)
        while k < n and arr[i] == arr[j] == arr[k]:
            i, j, k = i + 1, j + 1, k + 1
        return [i - 1, j] if k == n else [-1, -1]
```

#### Java

```java
class Solution {
    private int[] arr;

    public int[] threeEqualParts(int[] arr) {
        this.arr = arr;
        int cnt = 0;
        int n = arr.length;
        for (int v : arr) {
            cnt += v;
        }
        if (cnt % 3 != 0) {
            return new int[] {-1, -1};
        }
        if (cnt == 0) {
            return new int[] {0, n - 1};
        }
        cnt /= 3;

        int i = find(1), j = find(cnt + 1), k = find(cnt * 2 + 1);
        for (; k < n && arr[i] == arr[j] && arr[j] == arr[k]; ++i, ++j, ++k) {
        }
        return k == n ? new int[] {i - 1, j} : new int[] {-1, -1};
    }

    private int find(int x) {
        int s = 0;
        for (int i = 0; i < arr.length; ++i) {
            s += arr[i];
            if (s == x) {
                return i;
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> threeEqualParts(vector<int>& arr) {
        int n = arr.size();
        int cnt = accumulate(arr.begin(), arr.end(), 0);
        if (cnt % 3) return {-1, -1};
        if (!cnt) return {0, n - 1};
        cnt /= 3;

        auto find = [&](int x) {
            int s = 0;
            for (int i = 0; i < n; ++i) {
                s += arr[i];
                if (s == x) return i;
            }
            return 0;
        };
        int i = find(1), j = find(cnt + 1), k = find(cnt * 2 + 1);
        for (; k < n && arr[i] == arr[j] && arr[j] == arr[k]; ++i, ++j, ++k) {}
        return k == n ? vector<int>{i - 1, j} : vector<int>{-1, -1};
    }
};
```

#### Go

```go
func threeEqualParts(arr []int) []int {
	find := func(x int) int {
		s := 0
		for i, v := range arr {
			s += v
			if s == x {
				return i
			}
		}
		return 0
	}
	n := len(arr)
	cnt := 0
	for _, v := range arr {
		cnt += v
	}
	if cnt%3 != 0 {
		return []int{-1, -1}
	}
	if cnt == 0 {
		return []int{0, n - 1}
	}
	cnt /= 3
	i, j, k := find(1), find(cnt+1), find(cnt*2+1)
	for ; k < n && arr[i] == arr[j] && arr[j] == arr[k]; i, j, k = i+1, j+1, k+1 {
	}
	if k == n {
		return []int{i - 1, j}
	}
	return []int{-1, -1}
}
```

#### JavaScript

```js
/**
 * @param {number[]} arr
 * @return {number[]}
 */
var threeEqualParts = function (arr) {
    function find(x) {
        let s = 0;
        for (let i = 0; i < n; ++i) {
            s += arr[i];
            if (s == x) {
                return i;
            }
        }
        return 0;
    }
    const n = arr.length;
    let cnt = 0;
    for (const v of arr) {
        cnt += v;
    }
    if (cnt % 3) {
        return [-1, -1];
    }
    if (cnt == 0) {
        return [0, n - 1];
    }
    cnt = Math.floor(cnt / 3);
    let [i, j, k] = [find(1), find(cnt + 1), find(cnt * 2 + 1)];
    for (; k < n && arr[i] == arr[j] && arr[j] == arr[k]; ++i, ++j, ++k) {}
    return k == n ? [i - 1, j] : [-1, -1];
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
