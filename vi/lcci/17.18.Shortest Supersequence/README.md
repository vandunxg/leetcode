---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.18. Shortest Supersequence](https://leetcode.cn/problems/shortest-supersequence-lcci)

[中文文档](/lcci/17.18.Shortest%20Supersequence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng, một mảng ngắn hơn (có các phần tử đôi một khác nhau) và một mảng dài hơn. Hãy tìm mảng con ngắn nhất trong mảng dài hơn chứa tất cả phần tử của mảng ngắn hơn. Các phần tử có thể xuất hiện theo bất kỳ thứ tự nào.</p>
<p>Trả về chỉ số của phần tử ngoài cùng bên trái và ngoài cùng bên phải của mảng. Nếu có nhiều hơn một đáp án, trả về đáp án có chỉ số bên trái nhỏ nhất. Nếu không có đáp án, trả về một mảng rỗng.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:</strong>

big = [7,5,9,0,2,1,3,<strong>5,7,9,1</strong>,1,5,8,8,9,7]

small = [1,5,9]

<strong>Đầu ra: </strong>[7,10]</pre>

<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:</strong>

big = [1,2,3]

small = [4]

<strong>Đầu ra: </strong>[]</pre>

<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>big.length&nbsp;&lt;= 100000</code></li>
	<li><code>1 &lt;= small.length&nbsp;&lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mảng con ngắn nhất của $big$ bao phủ mọi giá trị của $small$ (tính cả số lần xuất hiện). Kiểm tra mọi cặp đầu mút có độ phức tạp bậc hai.
>
> Đây là bài toán bao phủ cửa sổ tối thiểu: mở rộng đầu phải cho đến khi $cnt=0$, rồi thu hẹp đầu trái.
>
> $need$ lưu số lần xuất hiện của các phần tử trong $small$, còn $window$ lưu số lần xuất hiện trong cửa sổ. Khi mở rộng, giảm $cnt$ nếu phần tử vẫn còn thiếu; khi thu hẹp, tăng bộ đếm nếu điều kiện bao phủ bị phá vỡ. Lưu lại đoạn tốt nhất $[k,k+mi-1]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestSeq(self, big: List[int], small: List[int]) -> List[int]:
        need = Counter(small)
        window = Counter()
        cnt, j, k, mi = len(small), 0, -1, inf
        for i, x in enumerate(big):
            window[x] += 1
            if need[x] >= window[x]:
                cnt -= 1
            while cnt == 0:
                if i - j + 1 < mi:
                    mi = i - j + 1
                    k = j
                if need[big[j]] >= window[big[j]]:
                    cnt += 1
                window[big[j]] -= 1
                j += 1
        return [] if k < 0 else [k, k + mi - 1]
```

#### Java

```java
class Solution {
    public int[] shortestSeq(int[] big, int[] small) {
        int cnt = small.length;
        Map<Integer, Integer> need = new HashMap<>(cnt);
        Map<Integer, Integer> window = new HashMap<>(cnt);
        for (int x : small) {
            need.put(x, 1);
        }
        int k = -1, mi = 1 << 30;
        for (int i = 0, j = 0; i < big.length; ++i) {
            window.merge(big[i], 1, Integer::sum);
            if (need.getOrDefault(big[i], 0) >= window.get(big[i])) {
                --cnt;
            }
            while (cnt == 0) {
                if (i - j + 1 < mi) {
                    mi = i - j + 1;
                    k = j;
                }
                if (need.getOrDefault(big[j], 0) >= window.get(big[j])) {
                    ++cnt;
                }
                window.merge(big[j++], -1, Integer::sum);
            }
        }
        return k < 0 ? new int[0] : new int[] {k, k + mi - 1};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> shortestSeq(vector<int>& big, vector<int>& small) {
        int cnt = small.size();
        unordered_map<int, int> need;
        unordered_map<int, int> window;
        for (int x : small) {
            need[x] = 1;
        }
        int k = -1, mi = 1 << 30;
        for (int i = 0, j = 0; i < big.size(); ++i) {
            window[big[i]]++;
            if (need[big[i]] >= window[big[i]]) {
                --cnt;
            }
            while (cnt == 0) {
                if (i - j + 1 < mi) {
                    mi = i - j + 1;
                    k = j;
                }
                if (need[big[j]] >= window[big[j]]) {
                    ++cnt;
                }
                window[big[j++]]--;
            }
        }
        if (k < 0) {
            return {};
        }
        return {k, k + mi - 1};
    }
};
```

#### Go

```go
func shortestSeq(big []int, small []int) []int {
	cnt := len(small)
	need := map[int]int{}
	window := map[int]int{}
	for _, x := range small {
		need[x] = 1
	}
	j, k, mi := 0, -1, 1<<30
	for i, x := range big {
		window[x]++
		if need[x] >= window[x] {
			cnt--
		}
		for cnt == 0 {
			if t := i - j + 1; t < mi {
				mi = t
				k = j
			}
			if need[big[j]] >= window[big[j]] {
				cnt++
			}
			window[big[j]]--
			j++
		}
	}
	if k < 0 {
		return []int{}
	}
	return []int{k, k + mi - 1}
}
```

#### TypeScript

```ts
function shortestSeq(big: number[], small: number[]): number[] {
    let cnt = small.length;
    const need: Map<number, number> = new Map();
    const window: Map<number, number> = new Map();
    for (const x of small) {
        need.set(x, 1);
    }
    let k = -1;
    let mi = 1 << 30;
    for (let i = 0, j = 0; i < big.length; ++i) {
        window.set(big[i], (window.get(big[i]) ?? 0) + 1);
        if ((need.get(big[i]) ?? 0) >= window.get(big[i])!) {
            --cnt;
        }
        while (cnt === 0) {
            if (i - j + 1 < mi) {
                mi = i - j + 1;
                k = j;
            }
            if ((need.get(big[j]) ?? 0) >= window.get(big[j])!) {
                ++cnt;
            }
            window.set(big[j], window.get(big[j])! - 1);
            ++j;
        }
    }
    return k < 0 ? [] : [k, k + mi - 1];
}
```

#### Swift

```swift
class Solution {
    func shortestSeq(_ big: [Int], _ small: [Int]) -> [Int] {
        let needCount = small.count
        var need = [Int: Int]()
        var window = [Int: Int]()
        small.forEach { need[$0, default: 0] += 1 }

        var count = needCount
        var minLength = Int.max
        var result = (-1, -1)

        var left = 0
        for right in 0..<big.count {
            let element = big[right]
            if need[element] != nil {
                window[element, default: 0] += 1
                if window[element]! <= need[element]! {
                    count -= 1
                }
            }

            while count == 0 {
                if right - left + 1 < minLength {
                    minLength = right - left + 1
                    result = (left, right)
                }

                let leftElement = big[left]
                if need[leftElement] != nil {
                    window[leftElement]! -= 1
                    if window[leftElement]! < need[leftElement]! {
                        count += 1
                    }
                }
                left += 1
            }
        }

        return result.0 == -1 ? [] : [result.0, result.1]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
