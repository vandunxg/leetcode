---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [05.03. Reverse Bits](https://leetcode.cn/problems/reverse-bits-lcci)

[中文文档](/lcci/05.03.Reverse%20Bits/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn có một số nguyên và có thể lật chính xác một bit từ 0 thành 1. Hãy viết code để tìm độ dài của chuỗi các số 1 dài nhất có thể tạo ra.</p>
<p><strong>Ví dụ 1: </strong></p>
<pre>

<strong>Đầu vào:</strong> <code>num</code> = 1775(11011101111<sub>2</sub>)

<strong>Đầu ra:</strong> 8

</pre>
<p><strong>Ví dụ 2: </strong></p>
<pre>

<strong>Đầu vào:</strong> <code>num</code> = 7(0111<sub>2</sub>)

<strong>Đầu ra:</strong> 4

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Sau khi lật nhiều nhất một $0$ thành $1$, ta cần tối đa hóa một đoạn liên tiếp các số 1. Thử từng vị trí lật rồi mở rộng có độ phức tạp $O(32^2)$ và lặp lại nhiều thao tác.
>
> Đây chính là cửa sổ dài nhất chứa nhiều nhất một $0$, và sliding window có thể tìm được trong một lần duyệt.
>
> Đầu phải $i$ duyệt qua $32$ bit; $cnt$ là số lượng số 0. Khi $cnt>1$, tăng $j$. Biểu thức `num>>i & 1 ^ 1` chỉ tăng khi gặp một bit 0.

<!-- thinking:end -->

Ta có thể dùng hai con trỏ $i$ và $j$ để duy trì một sliding window, trong đó $i$ là con trỏ phải và $j$ là con trỏ trái. Mỗi khi con trỏ phải $i$ dịch sang phải một bit, nếu số lượng $0$ trong cửa sổ lớn hơn $1$, con trỏ trái $j$ sẽ dịch sang phải một bit cho đến khi số lượng $0$ trong cửa sổ không vượt quá $1$. Sau đó, tính độ dài cửa sổ hiện tại, so sánh với độ dài lớn nhất hiện tại và chọn giá trị lớn hơn làm độ dài lớn nhất mới.

Cuối cùng, trả về độ dài lớn nhất.

Độ phức tạp thời gian là $O(\log M)$, còn độ phức tạp không gian là $O(1)$. Trong đó, $M$ là giá trị lớn nhất của một số nguyên 32-bit.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reverseBits(self, num: int) -> int:
        ans = cnt = j = 0
        for i in range(32):
            cnt += num >> i & 1 ^ 1
            while cnt > 1:
                cnt -= num >> j & 1 ^ 1
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

#### Java

```java
class Solution {
    public int reverseBits(int num) {
        int ans = 0, cnt = 0;
        for (int i = 0, j = 0; i < 32; ++i) {
            cnt += num >> i & 1 ^ 1;
            while (cnt > 1) {
                cnt -= num >> j & 1 ^ 1;
                ++j;
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reverseBits(int num) {
        int ans = 0, cnt = 0;
        for (int i = 0, j = 0; i < 32; ++i) {
            cnt += num >> i & 1 ^ 1;
            while (cnt > 1) {
                cnt -= num >> j & 1 ^ 1;
                ++j;
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```

#### Go

```go
func reverseBits(num int) (ans int) {
	var cnt, j int
	for i := 0; i < 32; i++ {
		cnt += num>>i&1 ^ 1
		for cnt > 1 {
			cnt -= num>>j&1 ^ 1
			j++
		}
		ans = max(ans, i-j+1)
	}
	return
}
```

#### TypeScript

```ts
function reverseBits(num: number): number {
    let ans = 0;
    let cnt = 0;
    for (let i = 0, j = 0; i < 32; ++i) {
        cnt += ((num >> i) & 1) ^ 1;
        for (; cnt > 1; ++j) {
            cnt -= ((num >> j) & 1) ^ 1;
        }
        ans = Math.max(ans, i - j + 1);
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func reverseBits(_ num: Int) -> Int {
        var ans = 0
        var cnt = 0
        var j = 0

        for i in 0..<32 {
            cnt += (num >> i & 1 ^ 1)
            while cnt > 1 {
                cnt -= (num >> j & 1 ^ 1)
                j += 1
            }
            ans = max(ans, i - j + 1)
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
