---
comments: true
difficulty: Medium
rating: 1745
source: Weekly Contest 434 Q2
tags:
    - Array
    - Math
    - Sorting
    - Simulation
---

<!-- problem:start -->

# [3433. Count Mentions Per User](https://leetcode.com/problems/count-mentions-per-user)

[中文文档](/solution/3400-3499/3433.Count%20Mentions%20Per%20User/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>numberOfUsers</code> biểu thị tổng số người dùng và một mảng <code>events</code> có kích thước <code>n x 3</code>.</p>

<p>Mỗi <code inline="">events[i]</code> có thể thuộc một trong hai loại sau:</p>

<ol>
    <li><strong>Sự kiện tin nhắn:</strong> <code>[&quot;MESSAGE&quot;, &quot;timestamp<sub>i</sub>&quot;, &quot;mentions_string<sub>i</sub>&quot;]</code>

    <ul>
        <li>Sự kiện này cho biết một nhóm người dùng được nhắc đến trong một tin nhắn tại <code>timestamp<sub>i</sub></code>.</li>
        <li>Chuỗi <code>mentions_string<sub>i</sub></code> có thể chứa một trong các token sau:
        <ul>
            <li><code>id&lt;number&gt;</code>: trong đó <code>&lt;number&gt;</code> là một số nguyên trong khoảng <code>[0,numberOfUsers - 1]</code>. Có thể có <strong>nhiều</strong> id, được phân tách bằng một khoảng trắng duy nhất, và có thể chứa các id trùng lặp. Token này vẫn có thể nhắc đến người dùng đang offline.</li>
            <li><code>ALL</code>: nhắc đến <strong>tất cả</strong> người dùng.</li>
            <li><code>HERE</code>: nhắc đến tất cả người dùng đang <strong>online</strong>.</li>
        </ul>
        </li>
    </ul>
    </li>
    <li><strong>Sự kiện offline:</strong> <code>[&quot;OFFLINE&quot;, &quot;timestamp<sub>i</sub>&quot;, &quot;id<sub>i</sub>&quot;]</code>
    <ul>
        <li>Sự kiện này cho biết người dùng <code>id<sub>i</sub></code> đã chuyển sang trạng thái offline tại <code>timestamp<sub>i</sub></code> trong <strong>60 đơn vị thời gian</strong>. Người dùng sẽ tự động online trở lại tại thời điểm <code>timestamp<sub>i</sub> + 60</code>.</li>
    </ul>
    </li>

</ol>

<p>Trả về một mảng <code>mentions</code>, trong đó <code>mentions[i]</code> biểu thị số lần người dùng có id <code>i</code> được nhắc đến trong tất cả các sự kiện <code>MESSAGE</code>.</p>

<p>Ban đầu, tất cả người dùng đều online. Khi một người dùng chuyển sang offline hoặc online trở lại, thay đổi trạng thái của họ được xử lý <em>trước</em> khi xử lý bất kỳ sự kiện tin nhắn nào xảy ra cùng timestamp.</p>

<p><strong>Lưu ý </strong>rằng một người dùng có thể được nhắc đến <strong>nhiều lần</strong> trong <strong>một</strong> sự kiện tin nhắn, và mỗi lần nhắc đến phải được đếm <strong>riêng</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numberOfUsers = 2, events = [[&quot;MESSAGE&quot;,&quot;10&quot;,&quot;id1 id0&quot;],[&quot;OFFLINE&quot;,&quot;11&quot;,&quot;0&quot;],[&quot;MESSAGE&quot;,&quot;71&quot;,&quot;HERE&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, tất cả người dùng đều online.</p>

<p>Tại timestamp 10, <code>id1</code> và <code>id0</code> được nhắc đến. <code>mentions = [1,1]</code></p>

<p>Tại timestamp 11, <code>id0</code> chuyển sang trạng thái <strong>offline.</strong></p>

<p>Tại timestamp 71, <code>id0</code> <strong>online</strong> trở lại và <code>&quot;HERE&quot;</code> được nhắc đến. <code>mentions = [2,2]</code></p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numberOfUsers = 2, events = [[&quot;MESSAGE&quot;,&quot;10&quot;,&quot;id1 id0&quot;],[&quot;OFFLINE&quot;,&quot;11&quot;,&quot;0&quot;],[&quot;MESSAGE&quot;,&quot;12&quot;,&quot;ALL&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, tất cả người dùng đều online.</p>

<p>Tại timestamp 10, <code>id1</code> và <code>id0</code> được nhắc đến. <code>mentions = [1,1]</code></p>

<p>Tại timestamp 11, <code>id0</code> chuyển sang trạng thái <strong>offline.</strong></p>

<p>Tại timestamp 12, <code>&quot;ALL&quot;</code> được nhắc đến. Điều này bao gồm cả những người dùng offline, vì vậy cả <code>id0</code> và <code>id1</code> đều được nhắc đến. <code>mentions = [2,2]</code></p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">numberOfUsers = 2, events = [[&quot;OFFLINE&quot;,&quot;10&quot;,&quot;0&quot;],[&quot;MESSAGE&quot;,&quot;12&quot;,&quot;HERE&quot;]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ban đầu, tất cả người dùng đều online.</p>

<p>Tại timestamp 10, <code>id0</code> chuyển sang trạng thái <strong>offline.</strong></p>

<p>Tại timestamp 12, <code>&quot;HERE&quot;</code> được nhắc đến. Vì <code>id0</code> vẫn đang offline nên người dùng này sẽ không được nhắc đến. <code>mentions = [0,1]</code></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= numberOfUsers &lt;= 100</code></li>
    <li><code>1 &lt;= events.length &lt;= 100</code></li>
    <li><code>events[i].length == 3</code></li>
    <li><code>events[i][0]</code> sẽ là một trong hai giá trị <code>MESSAGE</code> hoặc <code>OFFLINE</code>.</li>
    <li><code>1 &lt;= int(events[i][1]) &lt;= 10<sup>5</sup></code></li>
    <li>Số lần nhắc đến dạng <code>id&lt;number&gt;</code> trong bất kỳ sự kiện <code>&quot;MESSAGE&quot;</code> nào nằm trong khoảng từ <code>1</code> đến <code>100</code>.</li>
    <li><code>0 &lt;= &lt;number&gt; &lt;= numberOfUsers - 1</code></li>
    <li><strong>Đảm bảo</strong> rằng id người dùng được tham chiếu trong sự kiện <code>OFFLINE</code> đang <strong>online</strong> tại thời điểm sự kiện xảy ra.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> Các sự kiện có timestamp, và một người dùng offline sẽ không được HERE nhìn thấy trong $60$ giây. Nếu xử lý theo thứ tự đầu vào, ta có thể xử lý MESSAGE trước OFFLINE khi chúng có cùng timestamp.
>
> Hãy sắp xếp theo thời gian, đồng thời đặt các sự kiện OFFLINE trước MESSAGE khi timestamp trùng nhau, sau đó cập nhật theo loại sự kiện.
>
> $\textit{online\_t}[i]$ là thời điểm người dùng $i$ online trở lại. ALL làm tăng một biến lazy được áp dụng cho mọi người dùng ở cuối; HERE duyệt qua những người dùng đang online; các lần nhắc đích danh tăng trực tiếp một đơn vị.

<!-- thinking:end -->

Ta sắp xếp các sự kiện theo thứ tự tăng dần của timestamp. Nếu timestamp giống nhau, ta đặt các sự kiện OFFLINE trước các sự kiện MESSAGE.

Sau đó, ta mô phỏng các sự kiện xảy ra, sử dụng mảng `online_t` để ghi lại thời điểm online tiếp theo của mỗi người dùng và một biến `lazy` để ghi lại số lần nhắc cần áp dụng cho tất cả người dùng.

Ta duyệt qua danh sách sự kiện và xử lý từng sự kiện dựa trên loại của nó:

- Nếu là sự kiện ONLINE, ta cập nhật mảng `online_t`.
- Nếu là sự kiện ALL, ta tăng `lazy` lên một.
- Nếu là sự kiện HERE, ta duyệt qua mảng `online_t`. Nếu thời điểm online tiếp theo của người dùng nhỏ hơn hoặc bằng thời điểm hiện tại, ta tăng số lần người dùng đó được nhắc đến lên một.
- Nếu là sự kiện MESSAGE, ta tăng số lần được nhắc đến của người dùng được đề cập lên một.

Cuối cùng, nếu `lazy` lớn hơn 0, ta cộng `lazy` vào số lần được nhắc đến của tất cả người dùng.

Độ phức tạp thời gian là $O(n + m \times \log m \log M + L)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là tổng số người dùng và số sự kiện, còn $M$ và $L$ lần lượt là giá trị lớn nhất của timestamp và tổng độ dài của tất cả chuỗi nhắc đến.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countMentions(self, numberOfUsers: int, events: List[List[str]]) -> List[int]:
        events.sort(key=lambda e: (int(e[1]), e[0][2]))
        ans = [0] * numberOfUsers
        online_t = [0] * numberOfUsers
        lazy = 0
        for etype, ts, s in events:
            cur = int(ts)
            if etype[0] == "O":
                online_t[int(s)] = cur + 60
            elif s[0] == "A":
                lazy += 1
            elif s[0] == "H":
                for i, t in enumerate(online_t):
                    if t <= cur:
                        ans[i] += 1
            else:
                for a in s.split():
                    ans[int(a[2:])] += 1
        if lazy:
            for i in range(numberOfUsers):
                ans[i] += lazy
        return ans
```

#### Java

```java
class Solution {
    public int[] countMentions(int numberOfUsers, List<List<String>> events) {
        events.sort((a, b) -> {
            int x = Integer.parseInt(a.get(1));
            int y = Integer.parseInt(b.get(1));
            if (x == y) {
                return a.get(0).charAt(2) - b.get(0).charAt(2);
            }
            return x - y;
        });
        int[] ans = new int[numberOfUsers];
        int[] onlineT = new int[numberOfUsers];
        int lazy = 0;
        for (var e : events) {
            String etype = e.get(0);
            int cur = Integer.parseInt(e.get(1));
            String s = e.get(2);
            if (etype.charAt(0) == 'O') {
                onlineT[Integer.parseInt(s)] = cur + 60;
            } else if (s.charAt(0) == 'A') {
                ++lazy;
            } else if (s.charAt(0) == 'H') {
                for (int i = 0; i < numberOfUsers; ++i) {
                    if (onlineT[i] <= cur) {
                        ++ans[i];
                    }
                }
            } else {
                for (var a : s.split(" ")) {
                    ++ans[Integer.parseInt(a.substring(2))];
                }
            }
        }
        if (lazy > 0) {
            for (int i = 0; i < numberOfUsers; ++i) {
                ans[i] += lazy;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> countMentions(int numberOfUsers, vector<vector<string>>& events) {
        ranges::sort(events, [](const vector<string>& a, const vector<string>& b) {
            int x = stoi(a[1]);
            int y = stoi(b[1]);
            if (x == y) {
                return a[0][2] < b[0][2];
            }
            return x < y;
        });

        vector<int> ans(numberOfUsers, 0);
        vector<int> onlineT(numberOfUsers, 0);
        int lazy = 0;

        for (const auto& e : events) {
            string etype = e[0];
            int cur = stoi(e[1]);
            string s = e[2];

            if (etype[0] == 'O') {
                onlineT[stoi(s)] = cur + 60;
            } else if (s[0] == 'A') {
                lazy++;
            } else if (s[0] == 'H') {
                for (int i = 0; i < numberOfUsers; ++i) {
                    if (onlineT[i] <= cur) {
                        ++ans[i];
                    }
                }
            } else {
                stringstream ss(s);
                string token;
                while (ss >> token) {
                    ans[stoi(token.substr(2))]++;
                }
            }
        }

        if (lazy > 0) {
            for (int i = 0; i < numberOfUsers; ++i) {
                ans[i] += lazy;
            }
        }

        return ans;
    }
};
```

#### Go

```go
func countMentions(numberOfUsers int, events [][]string) []int {
    sort.Slice(events, func(i, j int) bool {
        x, _ := strconv.Atoi(events[i][1])
        y, _ := strconv.Atoi(events[j][1])
        if x == y {
            return events[i][0][2] < events[j][0][2]
        }
        return x < y
    })

    ans := make([]int, numberOfUsers)
    onlineT := make([]int, numberOfUsers)
    lazy := 0

    for _, e := range events {
        etype := e[0]
        cur, _ := strconv.Atoi(e[1])
        s := e[2]

        if etype[0] == 'O' {
            userID, _ := strconv.Atoi(s)
            onlineT[userID] = cur + 60
        } else if s[0] == 'A' {
            lazy++
        } else if s[0] == 'H' {
            for i := 0; i < numberOfUsers; i++ {
                if onlineT[i] <= cur {
                    ans[i]++
                }
            }
        } else {
            mentions := strings.Split(s, " ")
            for _, m := range mentions {
                userID, _ := strconv.Atoi(m[2:])
                ans[userID]++
            }
        }
    }

    if lazy > 0 {
        for i := 0; i < numberOfUsers; i++ {
            ans[i] += lazy
        }
    }

    return ans
}
```

#### TypeScript

```ts
function countMentions(numberOfUsers: number, events: string[][]): number[] {
    events.sort((a, b) => {
        const x = +a[1];
        const y = +b[1];
        if (x === y) {
            return a[0].charAt(2) < b[0].charAt(2) ? -1 : 1;
        }
        return x - y;
    });

    const ans: number[] = Array(numberOfUsers).fill(0);
    const onlineT: number[] = Array(numberOfUsers).fill(0);
    let lazy = 0;

    for (const [etype, ts, s] of events) {
        const cur = +ts;
        if (etype.charAt(0) === 'O') {
            const userID = +s;
            onlineT[userID] = cur + 60;
        } else if (s.charAt(0) === 'A') {
            lazy++;
        } else if (s.charAt(0) === 'H') {
            for (let i = 0; i < numberOfUsers; i++) {
                if (onlineT[i] <= cur) {
                    ans[i]++;
                }
            }
        } else {
            const mentions = s.split(' ');
            for (const m of mentions) {
                const userID = +m.slice(2);
                ans[userID]++;
            }
        }
    }

    if (lazy > 0) {
        for (let i = 0; i < numberOfUsers; i++) {
            ans[i] += lazy;
        }
    }

    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn count_mentions(number_of_users: i32, mut events: Vec<Vec<String>>) -> Vec<i32> {
        let n = number_of_users as usize;

        events.sort_by(|a, b| {
            let x: i32 = a[1].parse().unwrap();
            let y: i32 = b[1].parse().unwrap();
            if x == y {
                a[0].as_bytes()[2].cmp(&b[0].as_bytes()[2])
            } else {
                x.cmp(&y)
            }
        });

        let mut ans = vec![0_i32; n];
        let mut online_t = vec![0_i32; n];
        let mut lazy = 0_i32;

        for e in events {
            let etype = &e[0];
            let cur: i32 = e[1].parse().unwrap();
            let s = &e[2];

            let c0 = etype.as_bytes()[0] as char;

            if c0 == 'O' {
                let uid: usize = s.parse().unwrap();
                online_t[uid] = cur + 60;

            } else if s.as_bytes()[0] as char == 'A' {
                lazy += 1;

            } else if s.as_bytes()[0] as char == 'H' {
                for i in 0..n {
                    if online_t[i] <= cur {
                        ans[i] += 1;
                    }
                }

            } else {
                for a in s.split(' ') {
                    let uid: usize = a[2..].parse().unwrap();
                    ans[uid] += 1;
                }
            }
        }

        if lazy > 0 {
            for i in 0..n {
                ans[i] += lazy;
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
