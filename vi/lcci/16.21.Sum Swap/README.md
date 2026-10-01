---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.21. Sum Swap](https://leetcode.cn/problems/sum-swap-lcci)

[中文文档](/lcci/16.21.Sum%20Swap/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên, hãy tìm một cặp giá trị (mỗi giá trị thuộc một mảng) mà khi hoán đổi chúng, tổng của hai mảng sẽ bằng nhau.</p>

<p>Trả về một mảng, trong đó phần tử đầu tiên là phần tử trong mảng thứ nhất sẽ được hoán đổi, phần tử thứ hai là một phần tử trong mảng thứ hai. Nếu có nhiều đáp án, hãy trả về bất kỳ đáp án nào. Nếu không có đáp án, trả về một mảng rỗng.</p>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> array1 = [4, 1, 2, 1, 1, 2], array2 = [3, 6, 3, 3]

<strong>Đầu ra:</strong> [1, 3]

</pre>

<p><strong>Ví dụ:</strong></p>

<pre>

<strong>Đầu vào:</strong> array1 = <code>[1, 2, 3], array2 = [4, 5, 6]</code>

<strong>Đầu ra: </strong>[]</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>1 &lt;= array1.length, array2.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Hoán đổi một giá trị từ mỗi mảng để hai tổng bằng nhau. Thử mọi cặp sẽ có độ phức tạp $O(nm)$.
>
> Gọi khoảng chênh lệch giữa hai tổng là $diff$. Hoán đổi $a$ và $b$ làm khoảng chênh lệch thay đổi một lượng $2(a-b)$, nên $a-b=diff/2$. Nếu $diff$ là số lẻ thì không thể thực hiện được.
>
> Đưa $array2$ vào một set rồi tra cứu $a-diff/2$ khi duyệt $array1$. Một lượt duyệt bằng hash, thời gian tuyến tính.

<!-- thinking:end -->

Trước tiên, chúng ta tính tổng của hai mảng, sau đó tính hiệu $diff$ giữa hai tổng. Nếu $diff$ là số lẻ, điều đó có nghĩa là không thể làm cho tổng của hai mảng bằng nhau, nên chúng ta trực tiếp trả về một mảng rỗng.

Nếu $diff$ là số chẵn, chúng ta có thể duyệt một trong hai mảng. Giả sử phần tử hiện tại đang được duyệt là $a$, khi đó chúng ta cần tìm một phần tử $b$ trong mảng còn lại sao cho $a - b = diff / 2$, tức là $b = a - diff / 2$. Chúng ta có thể dùng hash table để nhanh chóng kiểm tra xem $b$ có tồn tại hay không. Nếu tồn tại, điều đó có nghĩa là chúng ta đã tìm được một cặp phần tử thỏa mãn điều kiện và có thể trả về ngay.

Độ phức tạp thời gian là $O(m + n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của hai mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findSwapValues(self, array1: List[int], array2: List[int]) -> List[int]:
        diff = sum(array1) - sum(array2)
        if diff & 1:
            return []
        diff >>= 1
        s = set(array2)
        for a in array1:
            if (b := (a - diff)) in s:
                return [a, b]
        return []
```

#### Java

```java
class Solution {
    public int[] findSwapValues(int[] array1, int[] array2) {
        long s1 = 0, s2 = 0;
        Set<Integer> s = new HashSet<>();
        for (int x : array1) {
            s1 += x;
        }
        for (int x : array2) {
            s2 += x;
            s.add(x);
        }
        long diff = s1 - s2;
        if (diff % 2 != 0) {
            return new int[0];
        }
        diff /= 2;
        for (int a : array1) {
            int b = (int) (a - diff);
            if (s.contains(b)) {
                return new int[] {a, b};
            }
        }
        return new int[0];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> findSwapValues(vector<int>& array1, vector<int>& array2) {
        long long s1 = accumulate(array1.begin(), array1.end(), 0LL);
        long long s2 = accumulate(array2.begin(), array2.end(), 0LL);
        long long diff = s1 - s2;
        if (diff & 1) {
            return {};
        }
        diff >>= 1;
        unordered_set<int> s(array2.begin(), array2.end());
        for (int x : array1) {
            int y = x - diff;
            if (s.count(y)) {
                return {x, y};
            }
        }
        return {};
    }
};
```

#### Go

```go
func findSwapValues(array1 []int, array2 []int) []int {
	s1, s2 := 0, 0
	s := map[int]bool{}
	for _, a := range array1 {
		s1 += a
	}
	for _, b := range array2 {
		s2 += b
		s[b] = true
	}
	diff := s1 - s2
	if (diff & 1) == 1 {
		return []int{}
	}
	diff >>= 1
	for _, a := range array1 {
		if b := a - diff; s[b] {
			return []int{a, b}
		}
	}
	return []int{}
}
```

#### TypeScript

```ts
function findSwapValues(array1: number[], array2: number[]): number[] {
    const s1 = array1.reduce((a, b) => a + b, 0);
    const s2 = array2.reduce((a, b) => a + b, 0);
    let diff = s1 - s2;
    if (diff & 1) {
        return [];
    }
    diff >>= 1;
    const s: Set<number> = new Set(array2);
    for (const x of array1) {
        const y = x - diff;
        if (s.has(y)) {
            return [x, y];
        }
    }
    return [];
}
```

#### Swift

```swift
class Solution {
    func findSwapValues(_ array1: [Int], _ array2: [Int]) -> [Int] {
        var s1 = 0, s2 = 0
        var set = Set<Int>()

        for x in array1 {
            s1 += x
        }
        for x in array2 {
            s2 += x
            set.insert(x)
        }

        let diff = s1 - s2
        if diff % 2 != 0 {
            return []
        }
        let target = diff / 2

        for a in array1 {
            let b = a - target
            if set.contains(b) {
                return [a, b]
            }
        }
        return []
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
