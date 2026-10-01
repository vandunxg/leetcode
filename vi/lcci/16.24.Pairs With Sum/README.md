---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [16.24. Pairs With Sum](https://leetcode.cn/problems/pairs-with-sum-lcci)

## Mô tả

<!-- description:start -->

<p>Thiết kế một thuật toán để tìm tất cả các cặp số nguyên trong một mảng có tổng bằng một giá trị cho trước.</p>
<p><strong>Ví dụ 1:</strong></p>
<pre>

<strong>Đầu vào:</strong> nums = [5,6,5], target = 11

<strong>Đầu ra: </strong>[[5,6]]</pre>

<p><strong>Ví dụ 2:</strong></p>
<pre>

<strong>Đầu vào:</strong> nums = [5,6,5,6], target = 11

<strong>Đầu ra: </strong>[[5,6],[5,6]]</pre>

<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>nums.length &lt;= 100000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Bảng băm

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm các cặp có tổng bằng $target$, trong đó mỗi phần tử chỉ được dùng nhiều nhất một lần. Hai con trỏ trên mảng đã sắp xếp có thể giải quyết, nhưng phải xử lý cẩn thận các phần tử trùng lặp và việc sử dụng phần tử.
>
> Một bảng đếm các giá trị chưa ghép là đủ: nếu $target-x$ vẫn còn, tạo cặp và giảm số lượng, nếu không thì ghi nhận $x$.
>
> Các số bù xuất hiện trước sẽ được sử dụng trước, và một chỉ số không bao giờ bị dùng hai lần. Chỉ cần duyệt một lần.

<!-- thinking:end -->

Chúng ta có thể sử dụng một bảng băm để lưu các phần tử trong mảng, với khóa là các phần tử trong mảng và giá trị là số lần phần tử đó xuất hiện.

Chúng ta duyệt qua mảng, với mỗi phần tử $x$, ta tính $y = target - x$. Nếu $y$ tồn tại trong bảng băm, điều đó có nghĩa là có một cặp số $(x, y)$ có tổng bằng target, nên ta thêm cặp này vào đáp án và giảm số lượng của $y$ đi $1$. Nếu $y$ không tồn tại trong bảng băm, điều đó có nghĩa là không có cặp số như vậy, nên ta tăng số lượng của $x$ lên $1$.

Sau khi duyệt xong, ta thu được đáp án.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def pairSums(self, nums: List[int], target: int) -> List[List[int]]:
        cnt = Counter()
        ans = []
        for x in nums:
            y = target - x
            if cnt[y]:
                cnt[y] -= 1
                ans.append([x, y])
            else:
                cnt[x] += 1
        return ans
```

#### Java

```java
class Solution {
    public List<List<Integer>> pairSums(int[] nums, int target) {
        Map<Integer, Integer> cnt = new HashMap<>();
        List<List<Integer>> ans = new ArrayList<>();
        for (int x : nums) {
            int y = target - x;
            if (cnt.containsKey(y)) {
                ans.add(List.of(x, y));
                if (cnt.merge(y, -1, Integer::sum) == 0) {
                    cnt.remove(y);
                }
            } else {
                cnt.merge(x, 1, Integer::sum);
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
    vector<vector<int>> pairSums(vector<int>& nums, int target) {
        unordered_map<int, int> cnt;
        vector<vector<int>> ans;
        for (int x : nums) {
            int y = target - x;
            if (cnt[y]) {
                --cnt[y];
                ans.push_back({x, y});
            } else {
                ++cnt[x];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func pairSums(nums []int, target int) (ans [][]int) {
	cnt := map[int]int{}
	for _, x := range nums {
		y := target - x
		if cnt[y] > 0 {
			cnt[y]--
			ans = append(ans, []int{x, y})
		} else {
			cnt[x]++
		}
	}
	return
}
```

#### TypeScript

```ts
function pairSums(nums: number[], target: number): number[][] {
    const cnt = new Map();
    const ans: number[][] = [];
    for (const x of nums) {
        const y = target - x;
        if (cnt.has(y)) {
            ans.push([x, y]);
            const yCount = cnt.get(y) - 1;
            if (yCount === 0) {
                cnt.delete(y);
            } else {
                cnt.set(y, yCount);
            }
        } else {
            cnt.set(x, (cnt.get(x) || 0) + 1);
        }
    }
    return ans;
}
```

#### Swift

```swift
class Solution {
    func pairSums(_ nums: [Int], _ target: Int) -> [[Int]] {
        var countMap = [Int: Int]()
        var ans = [[Int]]()

        for x in nums {
            let y = target - x
            if let yCount = countMap[y], yCount > 0 {
                ans.append([x, y])
                countMap[y] = yCount - 1
                if countMap[y] == 0 {
                    countMap.removeValue(forKey: y)
                }
            } else {
                countMap[x, default: 0] += 1
            }
        }
        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
