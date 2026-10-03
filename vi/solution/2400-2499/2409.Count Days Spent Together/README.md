---
comments: true
difficulty: Easy
rating: 1562
source: Biweekly Contest 87 Q1
tags:
    - Math
    - String
---

<!-- problem:start -->

# [2409. Count Days Spent Together](https://leetcode.com/problems/count-days-spent-together)

[中文文档](/solution/2400-2499/2409.Count%20Days%20Spent%20Together/README.md)

## Mô tả

<!-- description:start -->

<p>Alice và Bob đến Rome để tham dự các cuộc họp công việc riêng biệt.</p>

<p>Cho 4 chuỗi <code>arriveAlice</code>, <code>leaveAlice</code>, <code>arriveBob</code> và <code>leaveBob</code>. Alice sẽ ở trong thành phố từ ngày <code>arriveAlice</code> đến ngày <code>leaveAlice</code> (<strong>bao gồm cả hai ngày</strong>), còn Bob sẽ ở trong thành phố từ ngày <code>arriveBob</code> đến ngày <code>leaveBob</code> (<strong>bao gồm cả hai ngày</strong>). Mỗi chuỗi có 5 ký tự theo định dạng <code>&quot;MM-DD&quot;</code>, tương ứng với tháng và ngày.</p>

<p>Hãy trả về <em>tổng số ngày Alice và Bob cùng ở Rome</em>.</p>

<p>Bạn có thể giả sử tất cả ngày đều thuộc <strong>cùng một</strong> năm dương lịch, và năm đó <strong>không phải</strong> năm nhuận. Lưu ý rằng số ngày trong mỗi tháng có thể được biểu diễn bằng: <code>[31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arriveAlice = &quot;08-15&quot;, leaveAlice = &quot;08-18&quot;, arriveBob = &quot;08-16&quot;, leaveBob = &quot;08-19&quot;
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Alice ở Rome từ ngày 15 đến ngày 18 tháng 8. Bob ở Rome từ ngày 16 đến ngày 19 tháng 8. Cả hai cùng ở Rome vào các ngày 16, 17 và 18 tháng 8, nên đáp án là 3.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arriveAlice = &quot;10-01&quot;, leaveAlice = &quot;10-31&quot;, arriveBob = &quot;11-01&quot;, leaveBob = &quot;12-31&quot;
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có ngày nào Alice và Bob cùng ở Rome, nên ta trả về 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li>Tất cả ngày đều được cung cấp theo định dạng <code>&quot;MM-DD&quot;</code>.</li>
	<li>Ngày đến của Alice và Bob <strong>nhỏ hơn hoặc bằng</strong> ngày rời đi tương ứng.</li>
	<li>Các ngày đã cho đều hợp lệ trong một năm <strong>không nhuận</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Số ngày ở cùng nhau chính là độ dài phần giao của hai khoảng thời gian. Các ngày có dạng $\texttt{MM-DD}$, nên thứ tự chuỗi trùng với thứ tự trên lịch: phần giao bắt đầu ở ngày đến muộn hơn và kết thúc ở ngày rời đi sớm hơn.
>
> Năm không phải năm nhuận. Ta chuyển mỗi ngày thành số thứ tự trong năm bằng bảng số ngày của các tháng, sau đó lấy hiệu và cộng thêm một. Nếu hai khoảng không giao nhau thì kết quả là 0.

<!-- thinking:end -->

Ta chuyển đổi các ngày thành số thứ tự trong năm, sau đó tính số ngày cả hai người cùng ở Rome.

Độ phức tạp thời gian là $O(C)$ và độ phức tạp không gian là $O(C)$. Trong đó, $C$ là một hằng số.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countDaysTogether(
        self, arriveAlice: str, leaveAlice: str, arriveBob: str, leaveBob: str
    ) -> int:
        a = max(arriveAlice, arriveBob)
        b = min(leaveAlice, leaveBob)
        days = (31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31)
        x = sum(days[: int(a[:2]) - 1]) + int(a[3:])
        y = sum(days[: int(b[:2]) - 1]) + int(b[3:])
        return max(y - x + 1, 0)
```

#### Java

```java
class Solution {
    private int[] days = new int[] {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    public int countDaysTogether(
        String arriveAlice, String leaveAlice, String arriveBob, String leaveBob) {
        String a = arriveAlice.compareTo(arriveBob) < 0 ? arriveBob : arriveAlice;
        String b = leaveAlice.compareTo(leaveBob) < 0 ? leaveAlice : leaveBob;
        int x = f(a), y = f(b);
        return Math.max(y - x + 1, 0);
    }

    private int f(String s) {
        int i = Integer.parseInt(s.substring(0, 2)) - 1;
        int res = 0;
        for (int j = 0; j < i; ++j) {
            res += days[j];
        }
        res += Integer.parseInt(s.substring(3));
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> days = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    int countDaysTogether(string arriveAlice, string leaveAlice, string arriveBob, string leaveBob) {
        string a = arriveAlice < arriveBob ? arriveBob : arriveAlice;
        string b = leaveAlice < leaveBob ? leaveAlice : leaveBob;
        int x = f(a), y = f(b);
        return max(0, y - x + 1);
    }

    int f(string s) {
        int m, d;
        sscanf(s.c_str(), "%d-%d", &m, &d);
        int res = 0;
        for (int i = 0; i < m - 1; ++i) {
            res += days[i];
        }
        res += d;
        return res;
    }
};
```

#### Go

```go
func countDaysTogether(arriveAlice string, leaveAlice string, arriveBob string, leaveBob string) int {
	days := []int{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31}
	f := func(s string) int {
		m, _ := strconv.Atoi(s[:2])
		d, _ := strconv.Atoi(s[3:])
		res := 0
		for i := 0; i < m-1; i++ {
			res += days[i]
		}
		res += d
		return res
	}
	a, b := arriveAlice, leaveBob
	if arriveAlice < arriveBob {
		a = arriveBob
	}
	if leaveAlice < leaveBob {
		b = leaveAlice
	}
	x, y := f(a), f(b)
	ans := y - x + 1
	if ans < 0 {
		return 0
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
