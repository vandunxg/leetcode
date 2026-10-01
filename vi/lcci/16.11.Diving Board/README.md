---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [16.11. Diving Board](https://leetcode.cn/problems/diving-board-lcci)

[中文文档](/lcci/16.11.Diving%20Board/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang xây dựng một ván cầu nhảy bằng cách đặt nhiều thanh gỗ nối tiếp nhau. Có hai loại thanh gỗ, một loại có độ dài <code>shorter</code> và một loại có độ dài <code>longer</code>. Bạn phải sử dụng chính xác <code>K</code> thanh gỗ. Hãy viết một method để tạo ra tất cả các độ dài có thể có của ván cầu.</p>

<p>Trả về tất cả các độ dài theo thứ tự không giảm.</p>

<p><strong>Ví dụ: </strong></p>

<pre>

<strong>Đầu vào: </strong>

shorter = 1

longer = 2

k = 3

<strong>Đầu ra: </strong> {3,4,5,6}

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li>0 &lt; shorter &lt;= longer</li>
	<li>0 &lt;= k &lt;= 100000</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Phân tích trường hợp

<!-- thinking:start -->

> **Tư duy**
>
> Có $k$ thanh gỗ, mỗi thanh ngắn hoặc dài. Đệ quy qua $2^k$ cách phân công sẽ đếm trùng; có nhiều nhất $k+1$ tổng khác nhau.
>
> Với $i$ thanh dài, tổng độ dài là $i\cdot longer+(k-i)\cdot shorter$. $k=0$ là trường hợp rỗng; nếu hai độ dài bằng nhau thì chúng gộp thành một giá trị.
>
> Khi hai độ dài khác nhau, $i=0\ldots k$ tăng nghiêm ngặt, nên vòng lặp không cần loại bỏ trùng lặp.

<!-- thinking:end -->

Nếu $k=0$, không có lời giải nào và ta có thể trả về trực tiếp một danh sách rỗng.

Nếu $shorter=longer$, ta chỉ có thể dùng một ván có độ dài $longer \times k$, nên trả về trực tiếp một danh sách chứa độ dài $longer \times k$.

Ngược lại, ta có thể dùng một ván có độ dài $shorter \times (k-i) + longer \times i$, trong đó $0 \leq i \leq k$. Ta duyệt $i$ trong đoạn $[0, k]$ và tính độ dài tương ứng. Với các giá trị khác nhau của $i$, ta sẽ không nhận được cùng một độ dài, bởi vì nếu $0 \leq i \lt j \leq k$, thì chênh lệch độ dài là $(i - j) \times (longer - shorter) \lt 0$. Do đó, các giá trị khác nhau của $i$ sẽ cho các độ dài khác nhau.

Độ phức tạp thời gian là $O(k)$, trong đó $k$ là số lượng ván. Bỏ qua phần không gian dùng cho đáp án, độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def divingBoard(self, shorter: int, longer: int, k: int) -> List[int]:
        if k == 0:
            return []
        if longer == shorter:
            return [longer * k]
        ans = []
        for i in range(k + 1):
            ans.append(longer * i + shorter * (k - i))
        return ans
```

#### Java

```java
class Solution {
    public int[] divingBoard(int shorter, int longer, int k) {
        if (k == 0) {
            return new int[0];
        }
        if (longer == shorter) {
            return new int[] {longer * k};
        }
        int[] ans = new int[k + 1];
        for (int i = 0; i < k + 1; ++i) {
            ans[i] = longer * i + shorter * (k - i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> divingBoard(int shorter, int longer, int k) {
        if (k == 0) return {};
        if (longer == shorter) return {longer * k};
        vector<int> ans;
        for (int i = 0; i < k + 1; ++i)
            ans.push_back(longer * i + shorter * (k - i));
        return ans;
    }
};
```

#### Go

```go
func divingBoard(shorter int, longer int, k int) []int {
	if k == 0 {
		return []int{}
	}
	if longer == shorter {
		return []int{longer * k}
	}
	var ans []int
	for i := 0; i < k+1; i++ {
		ans = append(ans, longer*i+shorter*(k-i))
	}
	return ans
}
```

#### TypeScript

```ts
function divingBoard(shorter: number, longer: number, k: number): number[] {
    if (k === 0) {
        return [];
    }
    if (longer === shorter) {
        return [longer * k];
    }
    const ans: number[] = [k + 1];
    for (let i = 0; i <= k; ++i) {
        ans[i] = longer * i + shorter * (k - i);
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func divingBoard(_ shorter: Int, _ longer: Int, _ k: Int) -> [Int] {
        if k == 0 {
            return []
        }
        if shorter == longer {
            return [shorter * k]
        }

        var ans = [Int](repeating: 0, count: k + 1)
        for i in 0...k {
            ans[i] = longer * i + shorter * (k - i)
        }
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
