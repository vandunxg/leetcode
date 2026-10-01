---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.16. Sub Sort](https://leetcode.cn/problems/sub-sort-lcci)

[中文文档](/lcci/16.15.Master%20Mind/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên, hãy viết một method để tìm hai chỉ số m và n sao cho nếu sắp xếp các phần tử từ m đến n thì toàn bộ mảng sẽ được sắp xếp. Tối thiểu hóa <code>n - m</code> (nghĩa là tìm dãy ngắn nhất như vậy).</p>
<p>Trả về <code>[m,n]</code>. Nếu không có m và n nào thỏa mãn (ví dụ: mảng đã được sắp xếp), hãy trả về [-1, -1].</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong> [1,2,4,7,10,11,7,12,6,7,16,18,19]

<strong>Đầu ra: </strong> [3,9]

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>0 &lt;= len(array) &lt;= 1000000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Đây là mảng con ngắn nhất mà khi được sắp xếp sẽ khiến toàn bộ mảng được sắp xếp. Sắp xếp một bản sao rồi so sánh các đầu mút sẽ thực hiện được, nhưng cần thêm không gian tuyến tính.
>
> Biên phải là chỉ số ngoài cùng bên phải có giá trị nhỏ hơn một giá trị lớn nhất ở bên trái; biên trái là chỉ số ngoài cùng bên trái có giá trị lớn hơn một giá trị nhỏ nhất ở bên phải.
>
> Quét từ trái sang phải với $mx$ cập nhật $right$; quét từ phải sang trái với $mi$ cập nhật $left$. Hai lượt quét tuyến tính, dùng không gian phụ hằng số.

<!-- thinking:end -->

Trước hết, chúng ta duyệt mảng $array$ từ trái sang phải và dùng $mx$ để ghi nhận giá trị lớn nhất đã gặp. Nếu giá trị hiện tại $x$ nhỏ hơn $mx$, điều đó có nghĩa là $x$ cần được sắp xếp, và chúng ta ghi nhận chỉ số $i$ của $x$ là $right$; ngược lại, cập nhật $mx$.

Tương tự, chúng ta duyệt mảng $array$ từ phải sang trái và dùng $mi$ để ghi nhận giá trị nhỏ nhất đã gặp. Nếu giá trị hiện tại $x$ lớn hơn $mi$, điều đó có nghĩa là $x$ cần được sắp xếp, và chúng ta ghi nhận chỉ số $i$ của $x$ là $left$; ngược lại, cập nhật $mi$.

Cuối cùng, trả về $[left, right]$.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của mảng $array$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subSort(self, array: List[int]) -> List[int]:
        n = len(array)
        mi, mx = inf, -inf
        left = right = -1
        for i, x in enumerate(array):
            if x < mx:
                right = i
            else:
                mx = x
        for i in range(n - 1, -1, -1):
            if array[i] > mi:
                left = i
            else:
                mi = array[i]
        return [left, right]
```

#### Java

```java
class Solution {
    public int[] subSort(int[] array) {
        int n = array.length;
        int mi = Integer.MAX_VALUE, mx = Integer.MIN_VALUE;
        int left = -1, right = -1;
        for (int i = 0; i < n; ++i) {
            if (array[i] < mx) {
                right = i;
            } else {
                mx = array[i];
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            if (array[i] > mi) {
                left = i;
            } else {
                mi = array[i];
            }
        }
        return new int[] {left, right};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> subSort(vector<int>& array) {
        int n = array.size();
        int mi = INT_MAX, mx = INT_MIN;
        int left = -1, right = -1;
        for (int i = 0; i < n; ++i) {
            if (array[i] < mx) {
                right = i;
            } else {
                mx = array[i];
            }
        }
        for (int i = n - 1; ~i; --i) {
            if (array[i] > mi) {
                left = i;
            } else {
                mi = array[i];
            }
        }
        return {left, right};
    }
};
```

#### Go

```go
func subSort(array []int) []int {
	n := len(array)
	mi, mx := math.MaxInt32, math.MinInt32
	left, right := -1, -1
	for i, x := range array {
		if x < mx {
			right = i
		} else {
			mx = x
		}
	}
	for i := n - 1; i >= 0; i-- {
		if array[i] > mi {
			left = i
		} else {
			mi = array[i]
		}
	}
	return []int{left, right}
}
```

#### TypeScript

```ts
function subSort(array: number[]): number[] {
    const n = array.length;
    let [mi, mx] = [Infinity, -Infinity];
    let [left, right] = [-1, -1];
    for (let i = 0; i < n; ++i) {
        if (array[i] < mx) {
            right = i;
        } else {
            mx = array[i];
        }
    }
    for (let i = n - 1; ~i; --i) {
        if (array[i] > mi) {
            left = i;
        } else {
            mi = array[i];
        }
    }
    return [left, right];
}
```

#### Swift

```swift
class Solution {
    func subSort(_ array: [Int]) -> [Int] {
        let n = array.count
        var mi = Int.max, mx = Int.min
        var left = -1, right = -1

        for i in 0..<n {
            if array[i] < mx {
                right = i
            } else {
                mx = array[i]
            }
        }

        for i in stride(from: n - 1, through: 0, by: -1) {
            if array[i] > mi {
                left = i
            } else {
                mi = array[i]
            }
        }

        return [left, right]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
