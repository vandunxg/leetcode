---
comments: true
difficulty: Medium
rating: 1323
source: Weekly Contest 322 Q2
tags:
    - Array
    - Hash Table
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [2491. Divide Players Into Teams of Equal Skill](https://leetcode.com/problems/divide-players-into-teams-of-equal-skill)

[中文文档](/solution/2400-2499/2491.Divide%20Players%20Into%20Teams%20of%20Equal%20Skill/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên dương <code>skill</code> có độ dài <strong>chẵn</strong> <code>n</code>, trong đó <code>skill[i]</code> biểu thị kỹ năng của người chơi thứ <code>i<sup>th</sup></code>. Hãy chia người chơi thành <code>n / 2</code> đội, mỗi đội gồm <code>2</code> người, sao cho tổng kỹ năng của mỗi đội <strong>bằng nhau</strong>.</p>

<p><strong>Độ ăn ý</strong> của một đội bằng <strong>tích</strong> kỹ năng của hai người chơi trong đội đó.</p>

<p>Hãy trả về <em>tổng <strong>độ ăn ý</strong> của tất cả các đội, hoặc trả về </em><code>-1</code><em> nếu không thể chia người chơi thành các đội sao cho tổng kỹ năng của mỗi đội bằng nhau.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> skill = [3,2,5,1,3,4]
<strong>Đầu ra:</strong> 22
<strong>Giải thích:</strong>
Chia người chơi thành các đội sau: (1, 5), (2, 4), (3, 3), trong đó tổng kỹ năng của mỗi đội là 6.
Tổng độ ăn ý của tất cả các đội là: 1 * 5 + 2 * 4 + 3 * 3 = 5 + 8 + 9 = 22.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> skill = [3,4]
<strong>Đầu ra:</strong> 12
<strong>Giải thích:</strong>
Hai người chơi tạo thành một đội có tổng kỹ năng là 7.
Độ ăn ý của đội là 3 * 4 = 12.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> skill = [1,1,2,3]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Không thể chia người chơi thành các đội sao cho tổng kỹ năng của mỗi đội bằng nhau.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= skill.length &lt;= 10<sup>5</sup></code></li>
    <li><code>skill.length</code> là số chẵn.</li>
    <li><code>1 &lt;= skill[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cặp đều phải có cùng tổng kỹ năng, và tổng đó bắt buộc là giá trị nhỏ nhất cộng với giá trị lớn nhất của toàn bộ mảng. Với $n\le 10^5$, ta sắp xếp mảng rồi ghép các phần tử ở hai đầu; nếu có cặp không đạt đúng tổng thì không thể chia thành các đội hợp lệ, ngược lại cộng các tích vào đáp án.

<!-- thinking:end -->

Để tất cả các đội gồm 2 người có tổng điểm kỹ năng bằng nhau, giá trị nhỏ nhất phải được ghép với giá trị lớn nhất. Vì vậy, ta sắp xếp mảng `skill`, sau đó dùng hai con trỏ $i$ và $j$ lần lượt trỏ đến đầu và cuối mảng, ghép chúng thành từng cặp, rồi kiểm tra xem tổng của mỗi cặp có giống nhau không.

Nếu không, nghĩa là không thể chia các người chơi sao cho tổng điểm kỹ năng bằng nhau, nên ta trả về $-1$. Ngược lại, ta cộng độ ăn ý vào đáp án.

Sau khi duyệt xong, ta trả về đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng `skill`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dividePlayers(self, skill: List[int]) -> int:
        skill.sort()
        t = skill[0] + skill[-1]
        i, j = 0, len(skill) - 1
        ans = 0
        while i < j:
            if skill[i] + skill[j] != t:
                return -1
            ans += skill[i] * skill[j]
            i, j = i + 1, j - 1
        return ans
```

#### Java

```java
class Solution {
    public long dividePlayers(int[] skill) {
        Arrays.sort(skill);
        int n = skill.length;
        int t = skill[0] + skill[n - 1];
        long ans = 0;
        for (int i = 0, j = n - 1; i < j; ++i, --j) {
            if (skill[i] + skill[j] != t) {
                return -1;
            }
            ans += (long) skill[i] * skill[j];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        sort(skill.begin(), skill.end());
        int n = skill.size();
        int t = skill[0] + skill[n - 1];
        long long ans = 0;
        for (int i = 0, j = n - 1; i < j; ++i, --j) {
            if (skill[i] + skill[j] != t) return -1;
            ans += 1ll * skill[i] * skill[j];
        }
        return ans;
    }
};
```

#### Go

```go
func dividePlayers(skill []int) (ans int64) {
	sort.Ints(skill)
	n := len(skill)
	t := skill[0] + skill[n-1]
	for i, j := 0, n-1; i < j; i, j = i+1, j-1 {
		if skill[i]+skill[j] != t {
			return -1
		}
		ans += int64(skill[i] * skill[j])
	}
	return
}
```

#### TypeScript

```ts
function dividePlayers(skill: number[]): number {
    const n = skill.length;
    skill.sort((a, b) => a - b);
    const target = skill[0] + skill[n - 1];
    let ans = 0;
    for (let i = 0; i < n >> 1; i++) {
        if (target !== skill[i] + skill[n - 1 - i]) {
            return -1;
        }
        ans += skill[i] * skill[n - 1 - i];
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn divide_players(mut skill: Vec<i32>) -> i64 {
        let n = skill.len();
        skill.sort();
        let target = skill[0] + skill[n - 1];
        let mut ans = 0;
        for i in 0..n >> 1 {
            if skill[i] + skill[n - 1 - i] != target {
                return -1;
            }
            ans += (skill[i] * skill[n - 1 - i]) as i64;
        }
        ans
    }
}
```

#### JavaScript

```js
var dividePlayers = function (skill) {
    const n = skill.length,
        m = n / 2;
    skill.sort((a, b) => a - b);
    const sum = skill[0] + skill[n - 1];
    let ans = 0;
    for (let i = 0; i < m; i++) {
        const x = skill[i],
            y = skill[n - 1 - i];
        if (x + y != sum) return -1;
        ans += x * y;
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 cần sắp xếp. Nếu tổng kỹ năng không chia hết cho số đội thì không thể chia; tổng của mỗi cặp là $t=s/m$. Ta dùng hash map để ghép $v$ với $t-v$ trong thời gian tuyến tính.

<!-- thinking:end -->

Độ phức tạp thời gian là $O(n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng `skill`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def dividePlayers(self, skill: List[int]) -> int:
        s = sum(skill)
        m = len(skill) >> 1
        if s % m:
            return -1
        t = s // m
        d = defaultdict(int)
        ans = 0
        for v in skill:
            if d[t - v]:
                ans += v * (t - v)
                m -= 1
                d[t - v] -= 1
            else:
                d[v] += 1
        return -1 if m else ans
```

#### Java

```java
class Solution {
    public long dividePlayers(int[] skill) {
        int s = Arrays.stream(skill).sum();
        int m = skill.length >> 1;
        if (s % m != 0) {
            return -1;
        }
        int t = s / m;
        int[] d = new int[1010];
        long ans = 0;
        for (int v : skill) {
            if (d[t - v] > 0) {
                ans += (long) v * (t - v);
                --d[t - v];
                --m;
            } else {
                ++d[v];
            }
        }
        return m == 0 ? ans : -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        int s = accumulate(skill.begin(), skill.end(), 0);
        int m = skill.size() / 2;
        if (s % m) return -1;
        int t = s / m;
        int d[1010] = {0};
        long long ans = 0;
        for (int& v : skill) {
            if (d[t - v]) {
                ans += 1ll * v * (t - v);
                --d[t - v];
                --m;
            } else {
                ++d[v];
            }
        }
        return m == 0 ? ans : -1;
    }
};
```

#### Go

```go
func dividePlayers(skill []int) int64 {
	s := 0
	for _, v := range skill {
		s += v
	}
	m := len(skill) >> 1
	if s%m != 0 {
		return -1
	}
	t := s / m
	d := [1010]int{}
	ans := 0
	for _, v := range skill {
		if d[t-v] > 0 {
			ans += v * (t - v)
			d[t-v]--
			m--
		} else {
			d[v]++
		}
	}
	if m == 0 {
		return int64(ans)
	}
	return -1
}
```

#### TypeScript

```ts
function dividePlayers(skill: number[]): number {
    let [sum, res, map] = [0, 0, new Map<number, number>()];

    for (const x of skill) {
        sum += x;
        map.set(x, (map.get(x) || 0) + 1);
    }
    sum /= skill.length / 2;

    for (let [x, c] of map) {
        const complement = sum - x;
        if ((map.get(complement) ?? 0) !== c) return -1;
        if (x === complement) c /= 2;

        res += x * complement * c;
        map.delete(x);
        map.delete(complement);
    }

    return res;
}
```

#### JavaScript

```js
function dividePlayers(skill) {
    let [sum, res, map] = [0, 0, new Map()];

    for (const x of skill) {
        sum += x;
        map.set(x, (map.get(x) || 0) + 1);
    }
    sum /= skill.length / 2;

    for (let [x, c] of map) {
        const complement = sum - x;
        if ((map.get(complement) ?? 0) !== c) return -1;
        if (x === complement) c /= 2;

        res += x * complement * c;
        map.delete(x);
        map.delete(complement);
    }

    return res;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
