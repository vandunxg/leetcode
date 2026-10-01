---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.05. Find Longest Subarray](https://leetcode.cn/problems/find-longest-subarray-lcci)

[中文文档](/lcci/17.05.Find%20Longest%20Subarray/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng gồm các chữ cái và chữ số, hãy tìm mảng con dài nhất có số lượng chữ cái và chữ số bằng nhau.</p>

<p>Trả về mảng con đó. Nếu có nhiều đáp án, trả về đáp án có chỉ số điểm đầu bên trái nhỏ nhất. Nếu không có đáp án, trả về một mảng rỗng.</p>

<p><strong>Ví dụ 1:</strong></p>

<pre>

<strong>Đầu vào: </strong>[&quot;A&quot;,&quot;1&quot;,&quot;B&quot;,&quot;C&quot;,&quot;D&quot;,&quot;2&quot;,&quot;3&quot;,&quot;4&quot;,&quot;E&quot;,&quot;5&quot;,&quot;F&quot;,&quot;G&quot;,&quot;6&quot;,&quot;7&quot;,&quot;H&quot;,&quot;I&quot;,&quot;J&quot;,&quot;K&quot;,&quot;L&quot;,&quot;M&quot;]



<strong>Đầu ra: </strong>[&quot;A&quot;,&quot;1&quot;,&quot;B&quot;,&quot;C&quot;,&quot;D&quot;,&quot;2&quot;,&quot;3&quot;,&quot;4&quot;,&quot;E&quot;,&quot;5&quot;,&quot;F&quot;,&quot;G&quot;,&quot;6&quot;,&quot;7&quot;]

</pre>

<p><strong>Ví dụ 2:</strong></p>

<pre>

<strong>Đầu vào: </strong>[&quot;A&quot;,&quot;A&quot;]



<strong>Đầu ra: </strong>[]

</pre>

<p><strong>Lưu ý: </strong></p>

<ul>
	<li><code>array.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Prefix Sum + Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Bài toán yêu cầu tìm mảng con dài nhất có số chữ cái và chữ số bằng nhau. Thử mọi cặp điểm đầu cuối sẽ có độ phức tạp bậc hai.
>
> Gán chữ cái là $+1$ và chữ số là $-1$ để biến bài toán thành tìm mảng con có tổng bằng 0, tức là tìm cặp tổng tiền tố bằng nhau ở xa nhất.
>
> $vis$ lưu chỉ số đầu tiên của mỗi tổng tiền tố, bao gồm cả $0\mapsto -1$. Khi $s$ lặp lại, $(j,i]$ là một mảng con có tổng bằng 0; giữ lại chỉ số đầu tiên để span đạt độ dài lớn nhất.

<!-- thinking:end -->

Bài toán yêu cầu tìm mảng con dài nhất có số lượng chữ cái và chữ số bằng nhau. Ta có thể coi chữ cái là $1$ và chữ số là $-1$, biến bài toán thành tìm mảng con có tổng bằng $0$.

Ta có thể sử dụng ý tưởng tổng tiền tố và hash table `vis` để ghi nhận lần xuất hiện đầu tiên của mỗi tổng tiền tố. Ta dùng các biến `mx` và `k` để lần lượt ghi nhận độ dài và điểm đầu bên trái của mảng con dài nhất thỏa mãn điều kiện.

Tiếp theo, ta duyệt qua mảng, tính tổng tiền tố `s` tại vị trí hiện tại `i`:

- Nếu tổng tiền tố `s` tại vị trí hiện tại đã tồn tại trong hash table `vis`, gọi lần xuất hiện đầu tiên của `s` là `j`, khi đó tổng của mảng con trong khoảng $[j + 1,..,i]$ là $0$. Nếu độ dài của mảng con hiện tại lớn hơn độ dài của mảng con dài nhất đã tìm được, tức là $mx < i - j$, ta cập nhật `mx = i - j` và `k = j + 1`.
- Nếu không, ta lưu tổng tiền tố hiện tại `s` làm key và vị trí hiện tại `i` làm value trong hash table `vis`.

Sau khi duyệt xong, ta trả về mảng con có điểm đầu bên trái là `k` và độ dài là `mx`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findLongestSubarray(self, array: List[str]) -> List[str]:
        vis = {0: -1}
        s = mx = k = 0
        for i, x in enumerate(array):
            s += 1 if x.isalpha() else -1
            if s in vis:
                if mx < i - (j := vis[s]):
                    mx = i - j
                    k = j + 1
            else:
                vis[s] = i
        return array[k : k + mx]
```

#### Java

```java
class Solution {
    public String[] findLongestSubarray(String[] array) {
        Map<Integer, Integer> vis = new HashMap<>();
        vis.put(0, -1);
        int s = 0, mx = 0, k = 0;
        for (int i = 0; i < array.length; ++i) {
            s += array[i].charAt(0) >= 'A' ? 1 : -1;
            if (vis.containsKey(s)) {
                int j = vis.get(s);
                if (mx < i - j) {
                    mx = i - j;
                    k = j + 1;
                }
            } else {
                vis.put(s, i);
            }
        }
        String[] ans = new String[mx];
        System.arraycopy(array, k, ans, 0, mx);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<string> findLongestSubarray(vector<string>& array) {
        unordered_map<int, int> vis{{0, -1}};
        int s = 0, mx = 0, k = 0;
        for (int i = 0; i < array.size(); ++i) {
            s += array[i][0] >= 'A' ? 1 : -1;
            if (vis.count(s)) {
                int j = vis[s];
                if (mx < i - j) {
                    mx = i - j;
                    k = j + 1;
                }
            } else {
                vis[s] = i;
            }
        }
        return vector<string>(array.begin() + k, array.begin() + k + mx);
    }
};
```

#### Go

```go
func findLongestSubarray(array []string) []string {
	vis := map[int]int{0: -1}
	var s, mx, k int
	for i, x := range array {
		if x[0] >= 'A' {
			s++
		} else {
			s--
		}
		if j, ok := vis[s]; ok {
			if mx < i-j {
				mx = i - j
				k = j + 1
			}
		} else {
			vis[s] = i
		}
	}
	return array[k : k+mx]
}
```

#### TypeScript

```ts
function findLongestSubarray(array: string[]): string[] {
    const vis = new Map();
    vis.set(0, -1);
    let s = 0,
        mx = 0,
        k = 0;
    for (let i = 0; i < array.length; ++i) {
        s += array[i] >= 'A' ? 1 : -1;
        if (vis.has(s)) {
            const j = vis.get(s);
            if (mx < i - j) {
                mx = i - j;
                k = j + 1;
            }
        } else {
            vis.set(s, i);
        }
    }
    return array.slice(k, k + mx);
}
```

#### Swift

```swift
class Solution {
    func findLongestSubarray(_ array: [String]) -> [String] {
        var vis: [Int: Int] = [0: -1]
        var s = 0, mx = 0, k = 0

        for i in 0..<array.count {
            s += array[i].first!.isLetter ? 1 : -1
            if let j = vis[s] {
                if mx < i - j {
                    mx = i - j
                    k = j + 1
                }
            } else {
                vis[s] = i
            }
        }

        return Array(array[k..<(k + mx)])
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
