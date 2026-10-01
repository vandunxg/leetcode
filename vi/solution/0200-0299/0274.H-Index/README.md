---
comments: true
difficulty: Medium
tags:
    - Array
    - Counting Sort
    - Sorting
---

<!-- problem:start -->

# [274. H-Index](https://leetcode.com/problems/h-index)

[中文文档](/solution/0200-0299/0274.H-Index/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>citations</code>, trong đó <code>citations[i]</code> là số lượt trích dẫn bài báo thứ <code>i<sup>th</sup></code> mà nhà nghiên cứu nhận được. Hãy trả về <em>h-index của nhà nghiên cứu đó</em>.</p>

<p>Theo <a href="https://en.wikipedia.org/wiki/H-index" target="_blank">định nghĩa h-index trên Wikipedia</a>: h-index là giá trị lớn nhất của <code>h</code> sao cho nhà nghiên cứu đã công bố ít nhất <code>h</code> bài báo, mỗi bài được trích dẫn ít nhất <code>h</code> lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> citations = [3,0,6,1,5]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> [3,0,6,1,5] nghĩa là nhà nghiên cứu có tổng cộng 5 bài báo, lần lượt nhận được 3, 0, 6, 1, 5 lượt trích dẫn.
Nhà nghiên cứu có 3 bài báo, mỗi bài được trích dẫn ít nhất 3 lần; hai bài còn lại có không quá 3 lượt trích dẫn mỗi bài. Vì vậy, h-index của họ là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> citations = [1,3,1]
<strong>Đầu ra:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == citations.length</code></li>
	<li><code>1 &lt;= n &lt;= 5000</code></li>
	<li><code>0 &lt;= citations[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sorting

<!-- thinking:start -->

> **Tư duy**
>
> h-index là giá trị $h$ lớn nhất sao cho có ít nhất $h$ bài báo được trích dẫn $\ge h$ lần. Sau khi sắp xếp giảm dần, điều kiện kiểm tra là $citations[h-1]\ge h$; ta xét $h$ từ lớn xuống nhỏ.

<!-- thinking:end -->

Ta sắp xếp mảng `citations` theo thứ tự giảm dần, rồi lần lượt xét $h$ từ lớn xuống nhỏ. Nếu có giá trị $h$ thỏa mãn $citations[h-1] \geq h$, nghĩa là có ít nhất $h$ bài báo được trích dẫn ít nhất $h$ lần; khi đó trả về ngay $h$. Nếu không tìm được giá trị nào như vậy, nghĩa là không bài báo nào được trích dẫn, nên trả về $0$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng `citations`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        citations.sort(reverse=True)
        for h in range(len(citations), 0, -1):
            if citations[h - 1] >= h:
                return h
        return 0
```

#### Java

```java
class Solution {
    public int hIndex(int[] citations) {
        Arrays.sort(citations);
        int n = citations.length;
        for (int h = n; h > 0; --h) {
            if (citations[n - h] >= h) {
                return h;
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
    int hIndex(vector<int>& citations) {
        sort(citations.rbegin(), citations.rend());
        for (int h = citations.size(); h; --h) {
            if (citations[h - 1] >= h) {
                return h;
            }
        }
        return 0;
    }
};
```

#### Go

```go
func hIndex(citations []int) int {
	sort.Ints(citations)
	n := len(citations)
	for h := n; h > 0; h-- {
		if citations[n-h] >= h {
			return h
		}
	}
	return 0
}
```

#### TypeScript

```ts
function hIndex(citations: number[]): number {
    citations.sort((a, b) => b - a);
    for (let h = citations.length; h; --h) {
        if (citations[h - 1] >= h) {
            return h;
        }
    }
    return 0;
}
```

#### Rust

```rust
impl Solution {
    #[allow(dead_code)]
    pub fn h_index(citations: Vec<i32>) -> i32 {
        let mut citations = citations;
        citations.sort_by(|&lhs, &rhs| rhs.cmp(&lhs));

        let n = citations.len();

        for i in (1..=n).rev() {
            if citations[i - 1] >= (i as i32) {
                return i as i32;
            }
        }

        0
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting + Sum

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp tốn $O(n\log n)$. Vì $h\le n$, ta giới hạn số lượt trích dẫn ở mức $n$, đếm số bài ở mỗi mức rồi cộng dồn từ cao xuống thấp cho đến khi $s\ge h$.

<!-- thinking:end -->

Ta dùng mảng $cnt$ có độ dài $n+1$, trong đó $cnt[i]$ là số bài báo có số lượt trích dẫn bằng $i$. Ta duyệt mảng `citations` và xem các bài có số lượt trích dẫn lớn hơn $n$ như thể có đúng $n$ lượt. Với mỗi bài, lấy số lượt trích dẫn làm chỉ số rồi cộng $1$ vào phần tử tương ứng trong $cnt$. Như vậy, ta đã đếm số bài báo ở từng mức trích dẫn.

Tiếp theo, ta xét $h$ từ lớn xuống nhỏ và cộng $cnt[h]$ vào biến $s$. Biến $s$ biểu thị số bài báo có số lượt trích dẫn lớn hơn hoặc bằng $h$. Nếu $s \geq h$, nghĩa là có ít nhất $h$ bài báo được trích dẫn ít nhất $h$ lần, nên ta trả về ngay $h$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `citations`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        cnt = [0] * (n + 1)
        for x in citations:
            cnt[min(x, n)] += 1
        s = 0
        for h in range(n, -1, -1):
            s += cnt[h]
            if s >= h:
                return h
```

#### Java

```java
class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] cnt = new int[n + 1];
        for (int x : citations) {
            ++cnt[Math.min(x, n)];
        }
        for (int h = n, s = 0;; --h) {
            s += cnt[h];
            if (s >= h) {
                return h;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int hIndex(vector<int>& citations) {
        int n = citations.size();
        int cnt[n + 1];
        memset(cnt, 0, sizeof(cnt));
        for (int x : citations) {
            ++cnt[min(x, n)];
        }
        for (int h = n, s = 0;; --h) {
            s += cnt[h];
            if (s >= h) {
                return h;
            }
        }
    }
};
```

#### Go

```go
func hIndex(citations []int) int {
	n := len(citations)
	cnt := make([]int, n+1)
	for _, x := range citations {
		cnt[min(x, n)]++
	}
	for h, s := n, 0; ; h-- {
		s += cnt[h]
		if s >= h {
			return h
		}
	}
}
```

#### TypeScript

```ts
function hIndex(citations: number[]): number {
    const n: number = citations.length;
    const cnt: number[] = new Array(n + 1).fill(0);
    for (const x of citations) {
        ++cnt[Math.min(x, n)];
    }
    for (let h = n, s = 0; ; --h) {
        s += cnt[h];
        if (s >= h) {
            return h;
        }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Binary Search

<!-- thinking:start -->

> **Tư duy**
>
> Mệnh đề “có ít nhất $h$ bài báo được trích dẫn $\ge h$ lần” có tính đơn điệu, nên ta binary search giá trị $h$ lớn nhất thỏa mãn bằng cách đếm số giá trị $\ge mid$.

<!-- thinking:end -->

Ta nhận thấy nếu có giá trị $h$ sao cho ít nhất $h$ bài báo được trích dẫn ít nhất $h$ lần, thì với mọi $h'<h$, cũng có ít nhất $h' bài báo được trích dẫn ít nhất $h' lần. Do đó, ta có thể dùng binary search để tìm giá trị $h$ lớn nhất sao cho có ít nhất $h$ bài báo được trích dẫn ít nhất $h$ lần.

Ta đặt cận trái của binary search là $l=0$ và cận phải là $r=n$. Mỗi lượt, ta tính $mid = \lfloor \frac{l + r + 1}{2} \rfloor$, trong đó $\lfloor x \rfloor$ là phép lấy floor của $x$. Sau đó, ta đếm số phần tử trong mảng `citations` lớn hơn hoặc bằng $mid$ và ký hiệu số lượng đó là $s$. Nếu $s \geq mid$, nghĩa là có ít nhất $mid$ bài báo được trích dẫn ít nhất $mid$ lần; khi đó cập nhật cận trái $l$ thành $mid$. Nếu không, cập nhật cận phải $r$ thành $mid-1$. Khi $l=r$, ta tìm được giá trị $h$ lớn nhất; đó chính là $l$ hoặc $r$.

Độ phức tạp thời gian là $O(n \times \log n)$, trong đó $n$ là độ dài của mảng `citations`. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        l, r = 0, len(citations)
        while l < r:
            mid = (l + r + 1) >> 1
            if sum(x >= mid for x in citations) >= mid:
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class Solution {
    public int hIndex(int[] citations) {
        int l = 0, r = citations.length;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            int s = 0;
            for (int x : citations) {
                if (x >= mid) {
                    ++s;
                }
            }
            if (s >= mid) {
                l = mid;
            } else {
                r = mid - 1;
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
    int hIndex(vector<int>& citations) {
        int l = 0, r = citations.size();
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            int s = 0;
            for (int x : citations) {
                if (x >= mid) {
                    ++s;
                }
            }
            if (s >= mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return l;
    }
};
```

#### Go

```go
func hIndex(citations []int) int {
	l, r := 0, len(citations)
	for l < r {
		mid := (l + r + 1) >> 1
		s := 0
		for _, x := range citations {
			if x >= mid {
				s++
			}
		}
		if s >= mid {
			l = mid
		} else {
			r = mid - 1
		}
	}
	return l
}
```

#### TypeScript

```ts
function hIndex(citations: number[]): number {
    let l = 0;
    let r = citations.length;
    while (l < r) {
        const mid = (l + r + 1) >> 1;
        let s = 0;
        for (const x of citations) {
            if (x >= mid) {
                ++s;
            }
        }
        if (s >= mid) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }
    return l;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
