---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Queue
    - String
---

<!-- problem:start -->

# [649. Dota2 Senate](https://leetcode.com/problems/dota2-senate)

[中文文档](/solution/0600-0699/0649.Dota2%20Senate/README.md)

## Mô tả

<!-- description:start -->

<p>Trong thế giới Dota2 có hai phe: Radiant và Dire.</p>

<p>Thượng viện Dota2 gồm các thượng nghị sĩ thuộc hai phe. Thượng viện muốn quyết định một thay đổi trong trò chơi Dota2. Việc bỏ phiếu diễn ra theo từng vòng. Trong mỗi vòng, mỗi thượng nghị sĩ có thể thực hiện <strong>một</strong> trong hai quyền sau:</p>

<ul>
	<li><strong>Cấm quyền của một thượng nghị sĩ:</strong> Một thượng nghị sĩ có thể khiến một thượng nghị sĩ khác mất toàn bộ quyền trong vòng này và các vòng tiếp theo.</li>
	<li><strong>Tuyên bố chiến thắng:</strong> Nếu thượng nghị sĩ này nhận thấy tất cả thượng nghị sĩ còn quyền bỏ phiếu đều thuộc cùng một phe, người đó có thể tuyên bố chiến thắng và quyết định thay đổi trong trò chơi.</li>
</ul>

<p>Cho chuỗi <code>senate</code> biểu thị phe của từng thượng nghị sĩ. Ký tự <code>&#39;R&#39;</code> và <code>&#39;D&#39;</code> lần lượt biểu thị phe Radiant và Dire. Nếu có <code>n</code> thượng nghị sĩ thì độ dài chuỗi đã cho là <code>n</code>.</p>

<p>Quy trình theo vòng bắt đầu từ thượng nghị sĩ đầu tiên đến người cuối cùng theo thứ tự đã cho và tiếp tục cho đến khi kết thúc cuộc bỏ phiếu. Trong quá trình này, các thượng nghị sĩ đã mất quyền sẽ bị bỏ qua.</p>

<p>Giả sử mỗi thượng nghị sĩ đều đủ thông minh và sẽ chọn chiến lược tốt nhất cho phe mình. Hãy dự đoán phe nào sẽ tuyên bố chiến thắng và quyết định thay đổi trong trò chơi Dota2. Kết quả phải là <code>&quot;Radiant&quot;</code> hoặc <code>&quot;Dire&quot;</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> senate = &quot;RD&quot;
<strong>Đầu ra:</strong> &quot;Radiant&quot;
<strong>Giải thích:</strong> 
Thượng nghị sĩ đầu tiên thuộc phe Radiant và có thể cấm quyền của thượng nghị sĩ tiếp theo ngay trong vòng 1. 
Thượng nghị sĩ thứ hai không thể thực hiện quyền nào nữa vì quyền của người đó đã bị cấm. 
Đến vòng 2, thượng nghị sĩ đầu tiên có thể tuyên bố chiến thắng vì là người duy nhất trong thượng viện còn quyền bỏ phiếu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> senate = &quot;RDD&quot;
<strong>Đầu ra:</strong> &quot;Dire&quot;
<strong>Giải thích:</strong> 
Thượng nghị sĩ đầu tiên thuộc phe Radiant và có thể cấm quyền của thượng nghị sĩ tiếp theo ngay trong vòng 1. 
Thượng nghị sĩ thứ hai không thể thực hiện quyền nào nữa vì quyền của người đó đã bị cấm. 
Thượng nghị sĩ thứ ba thuộc phe Dire và có thể cấm quyền của thượng nghị sĩ đầu tiên trong vòng 1. 
Đến vòng 2, thượng nghị sĩ thứ ba có thể tuyên bố chiến thắng vì là người duy nhất trong thượng viện còn quyền bỏ phiếu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == senate.length</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>senate[i]</code> là <code>&#39;R&#39;</code> hoặc <code>&#39;D&#39;</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Queue + Simulation

<!-- thinking:start -->

> **Tư duy**
>
> Các thượng nghị sĩ lần lượt cấm quyền của đối thủ tiếp theo cho đến khi một phe bị loại hết. Xóa liên tục khỏi chuỗi sẽ có độ phức tạp bậc hai.
>
> Lưu chỉ số của các thượng nghị sĩ mỗi phe trong queue. Người có chỉ số đầu queue nhỏ hơn sẽ hành động trước, loại đối thủ khỏi queue và được đưa trở lại ở chỉ số $i+n$ cho vòng tiếp theo. Queue rỗng là phe thua.

<!-- thinking:end -->

Ta tạo hai queue $qr$ và $qd$ để lưu chỉ số của các thượng nghị sĩ thuộc Radiant và Dire. Sau đó, ta mô phỏng cuộc bỏ phiếu: trong mỗi vòng, lấy một thượng nghị sĩ từ mỗi queue và xử lý tùy theo phe của họ:

- Nếu chỉ số của thượng nghị sĩ Radiant nhỏ hơn chỉ số của thượng nghị sĩ Dire, người của Radiant có thể cấm vĩnh viễn quyền bỏ phiếu của người Dire. Ta cộng $n$ vào chỉ số của thượng nghị sĩ Radiant rồi đưa người đó về cuối queue, biểu thị rằng người này sẽ tham gia vòng bỏ phiếu tiếp theo.
- Nếu chỉ số của thượng nghị sĩ Dire nhỏ hơn chỉ số của thượng nghị sĩ Radiant, người của Dire có thể cấm vĩnh viễn quyền bỏ phiếu của người Radiant. Ta cộng $n$ vào chỉ số của thượng nghị sĩ Dire rồi đưa người đó về cuối queue, biểu thị rằng người này sẽ tham gia vòng bỏ phiếu tiếp theo.

Cuối cùng, khi trong các queue chỉ còn thượng nghị sĩ của một phe, phe đó chiến thắng.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số thượng nghị sĩ.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def predictPartyVictory(self, senate: str) -> str:
        qr = deque()
        qd = deque()
        for i, c in enumerate(senate):
            if c == "R":
                qr.append(i)
            else:
                qd.append(i)
        n = len(senate)
        while qr and qd:
            if qr[0] < qd[0]:
                qr.append(qr[0] + n)
            else:
                qd.append(qd[0] + n)
            qr.popleft()
            qd.popleft()
        return "Radiant" if qr else "Dire"
```

#### Java

```java
class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        Deque<Integer> qr = new ArrayDeque<>();
        Deque<Integer> qd = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (senate.charAt(i) == 'R') {
                qr.offer(i);
            } else {
                qd.offer(i);
            }
        }
        while (!qr.isEmpty() && !qd.isEmpty()) {
            if (qr.peek() < qd.peek()) {
                qr.offer(qr.peek() + n);
            } else {
                qd.offer(qd.peek() + n);
            }
            qr.poll();
            qd.poll();
        }
        return qr.isEmpty() ? "Dire" : "Radiant";
    }
}
```

#### C++

```cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        int n = senate.size();
        queue<int> qr;
        queue<int> qd;
        for (int i = 0; i < n; ++i) {
            if (senate[i] == 'R') {
                qr.push(i);
            } else {
                qd.push(i);
            }
        }
        while (!qr.empty() && !qd.empty()) {
            int r = qr.front();
            int d = qd.front();
            qr.pop();
            qd.pop();
            if (r < d) {
                qr.push(r + n);
            } else {
                qd.push(d + n);
            }
        }
        return qr.empty() ? "Dire" : "Radiant";
    }
};
```

#### Go

```go
func predictPartyVictory(senate string) string {
	n := len(senate)
	qr := []int{}
	qd := []int{}
	for i, c := range senate {
		if c == 'R' {
			qr = append(qr, i)
		} else {
			qd = append(qd, i)
		}
	}
	for len(qr) > 0 && len(qd) > 0 {
		r, d := qr[0], qd[0]
		qr, qd = qr[1:], qd[1:]
		if r < d {
			qr = append(qr, r+n)
		} else {
			qd = append(qd, d+n)
		}
	}
	if len(qr) > 0 {
		return "Radiant"
	}
	return "Dire"
}
```

#### TypeScript

```ts
function predictPartyVictory(senate: string): string {
    const n = senate.length;
    const qr: number[] = [];
    const qd: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (senate[i] === 'R') {
            qr.push(i);
        } else {
            qd.push(i);
        }
    }
    while (qr.length > 0 && qd.length > 0) {
        const r = qr.shift()!;
        const d = qd.shift()!;
        if (r < d) {
            qr.push(r + n);
        } else {
            qd.push(d + n);
        }
    }
    return qr.length > 0 ? 'Radiant' : 'Dire';
}
```

#### Rust

```rust
impl Solution {
    pub fn predict_party_victory(senate: String) -> String {
        let mut qr = std::collections::VecDeque::new();
        let mut qd = std::collections::VecDeque::new();
        let n = senate.len();
        for i in 0..n {
            if let Some(char) = senate.chars().nth(i) {
                if char == 'R' {
                    qr.push_back(i);
                } else {
                    qd.push_back(i);
                }
            }
        }

        while !qr.is_empty() && !qd.is_empty() {
            let front_qr = qr.pop_front().unwrap();
            let front_qd = qd.pop_front().unwrap();
            if front_qr < front_qd {
                qr.push_back(front_qr + n);
            } else {
                qd.push_back(front_qd + n);
            }
        }
        if qr.is_empty() {
            return "Dire".to_string();
        }
        "Radiant".to_string()
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
