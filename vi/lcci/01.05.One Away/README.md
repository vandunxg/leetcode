---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [01.05. One Away](https://leetcode.cn/problems/one-away-lcci)

[中文文档](/lcci/01.05.One%20Away/README.md)

## Mô tả

<!-- description:start -->

<p>Có ba loại chỉnh sửa có thể thực hiện trên chuỗi: chèn một ký tự, xóa một ký tự hoặc thay thế một ký tự. Cho hai chuỗi, hãy viết một hàm kiểm tra xem chúng có cách nhau không quá một lần chỉnh sửa (hoặc không có chỉnh sửa nào) hay không.</p>

<p><strong>Ví dụ&nbsp;1:</strong></p>

<pre>

<strong>Đầu vào:</strong>
first = &quot;pale&quot;
second = &quot;ple&quot;

<strong>Đầu ra:</strong> True

</pre>

<p><strong>Ví dụ&nbsp;2:</strong></p>

<pre>

<strong>Đầu vào:</strong>
first = &quot;pales&quot;
second = &quot;pal&quot;

<strong>Đầu ra:</strong> False

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp + Hai con trỏ

<!-- thinking:start -->

> **Tư duy**
>
> Một lần chỉnh sửa là thay thế, chèn hoặc xóa. Việc liệt kê mọi chuỗi có thể nhận được từ chuỗi ngắn hơn bằng một lần chỉnh sửa có độ phức tạp tuyến tính theo độ dài, nhưng triển khai khá rườm rà.
>
> Chênh lệch độ dài lớn hơn $1$ không thể được bù đắp bằng một lần chỉnh sửa. Giả sử $m \ge n$: nếu độ dài bằng nhau thì cho phép đúng một lần thay thế; nếu chênh lệch là $1$, chuỗi dài hơn có đúng một ký tự thừa.
>
> Khi độ dài bằng nhau, đếm số vị trí khác nhau; nếu không, dùng hai con trỏ để bỏ qua nhiều nhất một điểm không khớp trên chuỗi dài hơn. Hoán đổi để đối số đầu tiên là chuỗi dài hơn giúp triển khai trường hợp xóa.

<!-- thinking:end -->

Gọi độ dài của hai chuỗi $\textit{first}$ và $\textit{second}$ lần lượt là $m$ và $n$. Giả sử $m \geq n$.

Tiếp theo, chúng ta xét các trường hợp sau:

- Khi $m - n \gt 1$, không thể biến $\textit{first}$ và $\textit{second}$ thành giống nhau bằng một lần chỉnh sửa, vì vậy trả về `false`;
- Khi $m = n$, có thể biến $\textit{first}$ và $\textit{second}$ thành giống nhau bằng một lần chỉnh sửa khi và chỉ khi có đúng một ký tự khác nhau;
- Khi $m - n = 1$, có thể biến $\textit{first}$ và $\textit{second}$ thành giống nhau bằng một lần chỉnh sửa khi và chỉ khi $\textit{second}$ nhận được bằng cách xóa một ký tự khỏi $\textit{first}$. Ta có thể dùng hai con trỏ để thực hiện việc này.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của chuỗi. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def oneEditAway(self, first: str, second: str) -> bool:
        m, n = len(first), len(second)
        if m < n:
            return self.oneEditAway(second, first)
        if m - n > 1:
            return False
        if m == n:
            return sum(a != b for a, b in zip(first, second)) < 2
        i = j = cnt = 0
        while i < m:
            if j == n or (j < n and first[i] != second[j]):
                cnt += 1
            else:
                j += 1
            i += 1
        return cnt < 2
```

#### Java

```java
class Solution {
    public boolean oneEditAway(String first, String second) {
        int m = first.length(), n = second.length();
        if (m < n) {
            return oneEditAway(second, first);
        }
        if (m - n > 1) {
            return false;
        }
        int cnt = 0;
        if (m == n) {
            for (int i = 0; i < n; ++i) {
                if (first.charAt(i) != second.charAt(i)) {
                    if (++cnt > 1) {
                        return false;
                    }
                }
            }
            return true;
        }
        for (int i = 0, j = 0; i < m; ++i) {
            if (j == n || (j < n && first.charAt(i) != second.charAt(j))) {
                ++cnt;
            } else {
                ++j;
            }
        }
        return cnt < 2;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool oneEditAway(std::string first, std::string second) {
        int m = first.length(), n = second.length();
        if (m < n) {
            return oneEditAway(second, first);
        }
        if (m - n > 1) {
            return false;
        }
        int cnt = 0;
        if (m == n) {
            for (int i = 0; i < n; ++i) {
                if (first[i] != second[i]) {
                    if (++cnt > 1) {
                        return false;
                    }
                }
            }
            return true;
        }
        for (int i = 0, j = 0; i < m; ++i) {
            if (j == n || (j < n && first[i] != second[j])) {
                ++cnt;
            } else {
                ++j;
            }
        }
        return cnt < 2;
    }
};
```

#### Go

```go
func oneEditAway(first string, second string) bool {
	m, n := len(first), len(second)
	if m < n {
		return oneEditAway(second, first)
	}
	if m-n > 1 {
		return false
	}
	cnt := 0
	if m == n {
		for i := 0; i < n; i++ {
			if first[i] != second[i] {
				if cnt++; cnt > 1 {
					return false
				}
			}
		}
		return true
	}
	for i, j := 0, 0; i < m; i++ {
		if j == n || (j < n && first[i] != second[j]) {
			cnt++
		} else {
			j++
		}
	}
	return cnt < 2
}
```

#### TypeScript

```ts
function oneEditAway(first: string, second: string): boolean {
    let m: number = first.length;
    let n: number = second.length;
    if (m < n) {
        return oneEditAway(second, first);
    }
    if (m - n > 1) {
        return false;
    }

    let cnt: number = 0;
    if (m === n) {
        for (let i: number = 0; i < n; ++i) {
            if (first[i] !== second[i]) {
                if (++cnt > 1) {
                    return false;
                }
            }
        }
        return true;
    }

    for (let i: number = 0, j: number = 0; i < m; ++i) {
        if (j === n || (j < n && first[i] !== second[j])) {
            ++cnt;
        } else {
            ++j;
        }
    }
    return cnt < 2;
}
```

#### Rust

```rust
impl Solution {
    pub fn one_edit_away(first: String, second: String) -> bool {
        let (f_len, s_len) = (first.len(), second.len());
        let (first, second) = (first.as_bytes(), second.as_bytes());
        let (mut i, mut j) = (0, 0);
        let mut count = 0;
        while i < f_len && j < s_len {
            if first[i] != second[j] {
                if count > 0 {
                    return false;
                }

                count += 1;
                if i + 1 < f_len && first[i + 1] == second[j] {
                    i += 1;
                } else if j + 1 < s_len && first[i] == second[j + 1] {
                    j += 1;
                }
            }
            i += 1;
            j += 1;
        }
        count += f_len - i + s_len - j;
        count <= 1
    }
}
```

#### Swift

```swift
class Solution {
    func oneEditAway(_ first: String, _ second: String) -> Bool {
        let m = first.count, n = second.count
        if m < n {
            return oneEditAway(second, first)
        }
        if m - n > 1 {
            return false
        }

        var cnt = 0
        var firstIndex = first.startIndex
        var secondIndex = second.startIndex

        if m == n {
            while secondIndex != second.endIndex {
                if first[firstIndex] != second[secondIndex] {
                    cnt += 1
                    if cnt > 1 {
                        return false
                    }
                }
                firstIndex = first.index(after: firstIndex)
                secondIndex = second.index(after: secondIndex)
            }
            return true
        } else {
            while firstIndex != first.endIndex {
                if secondIndex == second.endIndex || (secondIndex != second.endIndex && first[firstIndex] != second[secondIndex]) {
                    cnt += 1
                } else {
                    secondIndex = second.index(after: secondIndex)
                }
                firstIndex = first.index(after: firstIndex)
            }
        }
        return cnt < 2
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
