---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.06. Smallest Difference](https://leetcode.cn/problems/smallest-difference-lcci)

[中文文档](/lcci/16.06.Smallest%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên, hãy tìm cặp giá trị (mỗi giá trị thuộc một mảng) có hiệu không âm nhỏ nhất. Trả về hiệu đó.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>{1, 3, 15, 11, 2}, {23, 127, 235, 19, 8}

<strong>Đầu ra: </strong> 3, the pair (11, 8)

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>1 &lt;= a.length, b.length &lt;= 100000</code></li>
	<li><code>-2147483648 &lt;= a[i], b[i] &lt;= 2147483647</code></li>
	<li>Kết quả nằm trong khoảng [-2147483648, 2147483647]</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Chọn một giá trị từ mỗi mảng để tối thiểu hóa hiệu tuyệt đối. Xét tất cả các cặp sẽ có độ phức tạp $O(nm)$.
>
> Với một $x$ cố định trong $a$, chỉ giá trị gần nhất trong $b$ là cần thiết; mảng $b$ đã sắp xếp có thể trả lời bằng tìm kiếm nhị phân.
>
> Sắp xếp $b$, sau đó gọi `bisect_left` cho từng $x$ và kiểm tra vị trí chèn cùng phần tử đứng trước nó. Chi phí sắp xếp cộng với $n$ lần tìm kiếm theo logarit là phần chi phối.

<!-- thinking:end -->

Ta có thể sắp xếp mảng $b$, sau đó với mỗi phần tử $x$ trong mảng $a$, thực hiện tìm kiếm nhị phân trên mảng $b$ để tìm phần tử $y$ gần $x$ nhất. Khi đó, hiệu tuyệt đối giữa $x$ và $y$ chính là hiệu tuyệt đối giữa $x$ và phần tử gần nhất trong $b$.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của mảng $b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestDifference(self, a: List[int], b: List[int]) -> int:
        b.sort()
        ans = inf
        n = len(b)
        for x in a:
            j = bisect_left(b, x)
            if j < n:
                ans = min(ans, b[j] - x)
            if j:
                ans = min(ans, x - b[j - 1])
        return ans
```

#### Java

```java
class Solution {
    public int smallestDifference(int[] a, int[] b) {
        Arrays.sort(b);
        long ans = Long.MAX_VALUE;
        for (int x : a) {
            int j = search(b, x);
            if (j < b.length) {
                ans = Math.min(ans, (long) b[j] - x);
            }
            if (j > 0) {
                ans = Math.min(ans, (long) x - b[j - 1]);
            }
        }
        return (int) ans;
    }

    private int search(int[] nums, int x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestDifference(vector<int>& a, vector<int>& b) {
        sort(b.begin(), b.end());
        long long ans = LONG_LONG_MAX;
        for (int x : a) {
            auto it = lower_bound(b.begin(), b.end(), x);
            if (it != b.end()) {
                ans = min(ans, (long long) *it - x);
            }
            if (it != b.begin()) {
                ans = min(ans, x - (long long) *prev(it));
            }
        }
        return ans;
    }
};
```

#### Go

```go
func smallestDifference(a []int, b []int) int {
	sort.Ints(b)
	var ans int = 1e18
	for _, x := range a {
		i := sort.SearchInts(b, x)
		if i < len(b) {
			ans = min(ans, b[i]-x)
		}
		if i > 0 {
			ans = min(ans, x-b[i-1])
		}
	}
	return ans
}
```

#### TypeScript

```ts
function smallestDifference(a: number[], b: number[]): number {
    b.sort((a, b) => a - b);
    let ans = Infinity;
    const search = (nums: number[], x: number): number => {
        let [l, r] = [0, nums.length];
        while (l < r) {
            const mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    };
    for (const x of a) {
        const j = search(b, x);
        if (j < b.length) {
            ans = Math.min(ans, b[j] - x);
        }
        if (j > 0) {
            ans = Math.min(ans, x - b[j - 1]);
        }
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func smallestDifference(_ a: [Int], _ b: [Int]) -> Int {
        let sortedB = b.sorted()
        var ans = Int.max

        for x in a {
            let j = search(sortedB, x)
            if j < sortedB.count {
                ans = min(ans, abs(sortedB[j] - x))
            }
            if j > 0 {
                ans = min(ans, abs(x - sortedB[j - 1]))
            }
        }

        return ans
    }

    private func search(_ nums: [Int], _ x: Int) -> Int {
        var l = 0
        var r = nums.count
        while l < r {
            let mid = (l + r) / 2
            if nums[mid] >= x {
                r = mid
            } else {
                l = mid + 1
            }
        }
        return l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Sắp xếp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Tìm kiếm nhị phân cho từng phần tử vẫn phải trả thêm hệ số logarit $n$ lần.
>
> Sắp xếp cả hai mảng cho phép hai con trỏ tiến về phía trước ở phía nhỏ hơn, hoàn tất trong một lượt duyệt tuyến tính sau khi sắp xếp.

<!-- thinking:end -->

Ta có thể sắp xếp cả hai mảng $a$ và $b$, rồi dùng hai con trỏ $i$ và $j$ để duy trì vị trí hiện tại trong hai mảng. Ban đầu, $i$ và $j$ lần lượt trỏ đến đầu mảng $a$ và $b$. Ở mỗi bước, ta tính hiệu tuyệt đối giữa $a[i]$ và $b[j]$, rồi cập nhật đáp án. Nếu một trong hai phần tử được $i$ và $j$ trỏ đến nhỏ hơn phần tử còn lại, ta dịch con trỏ đang trỏ đến phần tử nhỏ hơn lên trước một bước. Việc duyệt kết thúc khi ít nhất một con trỏ vượt ra ngoài phạm vi mảng.

Độ phức tạp thời gian là $O(n \times \log n)$, và độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là độ dài của các mảng $a$ và $b$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestDifference(self, a: List[int], b: List[int]) -> int:
        a.sort()
        b.sort()
        i = j = 0
        ans = inf
        while i < len(a) and j < len(b):
            ans = min(ans, abs(a[i] - b[j]))
            if a[i] < b[j]:
                i += 1
            else:
                j += 1
        return ans
```

#### Java

```java
class Solution {
    public int smallestDifference(int[] a, int[] b) {
        Arrays.sort(a);
        Arrays.sort(b);
        int i = 0, j = 0;
        long ans = Long.MAX_VALUE;
        while (i < a.length && j < b.length) {
            ans = Math.min(ans, Math.abs((long) a[i] - (long) b[j]));
            if (a[i] < b[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int smallestDifference(vector<int>& a, vector<int>& b) {
        sort(a.begin(), a.end());
        sort(b.begin(), b.end());
        int i = 0, j = 0;
        long long ans = LONG_LONG_MAX;
        while (i < a.size() && j < b.size()) {
            ans = min(ans, abs(1LL * a[i] - 1LL * b[j]));
            if (a[i] < b[j]) {
                ++i;
            } else {
                ++j;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func smallestDifference(a []int, b []int) int {
	sort.Ints(a)
	sort.Ints(b)
	i, j := 0, 0
	var ans int = 1e18
	for i < len(a) && j < len(b) {
		ans = min(ans, abs(a[i]-b[j]))
		if a[i] < b[j] {
			i++
		} else {
			j++
		}
	}
	return ans
}

func abs(a int) int {
	if a < 0 {
		return -a
	}
	return a
}
```

#### TypeScript

```ts
function smallestDifference(a: number[], b: number[]): number {
    a.sort((a, b) => a - b);
    b.sort((a, b) => a - b);
    let [i, j] = [0, 0];
    let ans = Infinity;
    while (i < a.length && j < b.length) {
        ans = Math.min(ans, Math.abs(a[i] - b[j]));
        if (a[i] < b[j]) {
            ++i;
        } else {
            ++j;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
