---
comments: true
difficulty: Easy
rating: 1365
source: Weekly Contest 288 Q1
tags:
    - Sorting
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2231. Largest Number After Digit Swaps by Parity](https://leetcode.com/problems/largest-number-after-digit-swaps-by-parity)

[中文文档](/solution/2200-2299/2231.Largest%20Number%20After%20Digit%20Swaps%20by%20Parity/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>num</code>. Bạn có thể hoán đổi hai chữ số bất kỳ của <code>num</code> có cùng <strong>tính chẵn lẻ</strong> (tức là cả hai đều là chữ số lẻ hoặc cả hai đều là chữ số chẵn).</p>

<p>Trả về<em> giá trị <strong>lớn nhất</strong> có thể của </em><code>num</code><em> sau <strong>bất kỳ</strong> số lần hoán đổi nào.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 1234
<strong>Đầu ra:</strong> 3412
<strong>Giải thích:</strong> Hoán đổi chữ số 3 với chữ số 1, ta được số 3214.
Hoán đổi chữ số 2 với chữ số 4, ta được số 3412.
Lưu ý rằng có thể có những chuỗi phép hoán đổi khác, nhưng có thể chứng minh 3412 là số lớn nhất có thể tạo ra.
Ngoài ra, ta không thể hoán đổi chữ số 4 với chữ số 1 vì chúng có tính chẵn lẻ khác nhau.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> num = 65875
<strong>Đầu ra:</strong> 87655
<strong>Giải thích:</strong> Hoán đổi chữ số 8 với chữ số 6, ta được số 85675.
Hoán đổi chữ số 5 đầu tiên với chữ số 7, ta được số 87655.
Lưu ý rằng có thể có những chuỗi phép hoán đổi khác, nhưng có thể chứng minh 87655 là số lớn nhất có thể tạo ra.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= num &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Ta có thể hoán đổi các chữ số có cùng tính chẵn lẻ và muốn thu được giá trị thập phân lớn nhất. Vì $num \le 10^9$ có ít chữ số, không cần duyệt các hoán vị: các chữ số cùng tính chẵn lẻ có thể được sắp xếp tự do, nên tại mỗi vị trí ta chỉ cần chọn chữ số lớn nhất còn lại có cùng tính chẵn lẻ.
>
> Đếm số lần xuất hiện của các chữ số từ $0$ đến $9$. Duyệt số ban đầu từ trái sang phải, chọn chữ số chẵn hoặc lẻ chưa dùng tiếp theo, bắt đầu lần lượt từ $8$ hoặc $9$.

<!-- thinking:end -->

Ta có thể dùng một mảng $\textit{cnt}$ có độ dài $10$ để đếm số lần xuất hiện của mỗi chữ số trong số nguyên $\textit{num}$. Đồng thời, ta dùng một mảng chỉ số $\textit{idx}$ để ghi nhận chữ số chẵn và chữ số lẻ lớn nhất hiện còn, ban đầu được gán là $[8, 9]$.

Tiếp theo, ta duyệt qua từng chữ số của số nguyên $\textit{num}$. Nếu chữ số đó là số lẻ, ta lấy chữ số tương ứng với chỉ số $1$ trong $\textit{idx}$; nếu không, ta lấy chữ số tương ứng với chỉ số $0$. Nếu số lần xuất hiện của chữ số đó bằng $0$, ta giảm chữ số đi $2$ và tiếp tục kiểm tra cho đến khi tìm được chữ số thỏa mãn. Sau đó, ta cập nhật đáp án và số lần xuất hiện của chữ số đó, rồi tiếp tục duyệt cho đến khi đã xử lý tất cả chữ số của số nguyên $\textit{num}$.

Độ phức tạp thời gian là $O(\log \textit{num})$, và độ phức tạp không gian là $O(\log \textit{num})$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def largestInteger(self, num: int) -> int:
        nums = [int(c) for c in str(num)]
        cnt = Counter(nums)
        idx = [8, 9]
        ans = 0
        for x in nums:
            while cnt[idx[x & 1]] == 0:
                idx[x & 1] -= 2
            ans = ans * 10 + idx[x & 1]
            cnt[idx[x & 1]] -= 1
        return ans
```

#### Java

```java
class Solution {
    public int largestInteger(int num) {
        char[] s = String.valueOf(num).toCharArray();
        int[] cnt = new int[10];
        for (char c : s) {
            ++cnt[c - '0'];
        }
        int[] idx = {8, 9};
        int ans = 0;
        for (char c : s) {
            int x = c - '0';
            while (cnt[idx[x & 1]] == 0) {
                idx[x & 1] -= 2;
            }
            ans = ans * 10 + idx[x & 1];
            cnt[idx[x & 1]]--;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int largestInteger(int num) {
        string s = to_string(num);
        int cnt[10] = {0};
        for (char c : s) {
            cnt[c - '0']++;
        }
        int idx[2] = {8, 9};
        int ans = 0;
        for (char c : s) {
            int x = c - '0';
            while (cnt[idx[x & 1]] == 0) {
                idx[x & 1] -= 2;
            }
            ans = ans * 10 + idx[x & 1];
            cnt[idx[x & 1]]--;
        }
        return ans;
    }
};
```

#### Go

```go
func largestInteger(num int) int {
	s := []byte(fmt.Sprint(num))
	cnt := [10]int{}

	for _, c := range s {
		cnt[c-'0']++
	}

	idx := [2]int{8, 9}
	ans := 0

	for _, c := range s {
		x := int(c - '0')
		for cnt[idx[x&1]] == 0 {
			idx[x&1] -= 2
		}
		ans = ans*10 + idx[x&1]
		cnt[idx[x&1]]--
	}

	return ans
}
```

#### TypeScript

```ts
function largestInteger(num: number): number {
    const s = num.toString().split('');
    const cnt = Array(10).fill(0);

    for (const c of s) {
        cnt[+c]++;
    }

    const idx = [8, 9];
    let ans = 0;

    for (const c of s) {
        const x = +c;
        while (cnt[idx[x % 2]] === 0) {
            idx[x % 2] -= 2;
        }
        ans = ans * 10 + idx[x % 2];
        cnt[idx[x % 2]]--;
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
