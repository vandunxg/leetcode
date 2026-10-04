---
comments: true
difficulty: Easy
rating: 1348
source: Weekly Contest 444 Q1
tags:
    - Array
    - Hash Table
    - Linked List
    - Doubly-Linked List
    - Ordered Set
    - Simulation
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3507. Minimum Pair Removal to Sort Array I](https://leetcode.com/problems/minimum-pair-removal-to-sort-array-i)

[中文文档](/solution/3500-3599/3507.Minimum%20Pair%20Removal%20to%20Sort%20Array%20I/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng <code>nums</code>, bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
    <li>Chọn cặp phần tử <strong>liền kề</strong> có tổng <strong>nhỏ nhất</strong> trong <code>nums</code>. Nếu có nhiều cặp như vậy, chọn cặp nằm ngoài cùng bên trái.</li>
    <li>Thay cặp phần tử bằng tổng của chúng.</li>
</ul>

<p>Trả về <strong>số thao tác ít nhất</strong> cần thực hiện để mảng trở thành <strong>không giảm</strong>.</p>

<p>Một mảng được gọi là <strong>không giảm</strong> nếu mỗi phần tử lớn hơn hoặc bằng phần tử đứng trước nó (nếu có).</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [5,2,3,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Cặp <code>(3,1)</code> có tổng nhỏ nhất là 4. Sau khi thay thế, <code>nums = [5,2,4]</code>.</li>
    <li>Cặp <code>(2,4)</code> có tổng nhỏ nhất là 6. Sau khi thay thế, <code>nums = [5,6]</code>.</li>
</ul>

<p>Mảng <code>nums</code> trở thành không giảm sau hai thao tác.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,2]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p>Mảng <code>nums</code> đã được sắp xếp.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 50</code></li>
    <li><code>-1000 &lt;= nums[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Mô phỏng

<!-- thinking:start -->

> **Tư duy**
>
> $n \le 50$ và mỗi bước gộp cặp phần tử liền kề có tổng nhỏ nhất. Có nhiều nhất $n-1$ lần gộp, nên chỉ cần mô phỏng trực tiếp: duyệt các tổng của những cặp liền kề, thay giá trị bên trái và xóa giá trị bên phải.
>
> Không cần dùng cấu trúc dữ liệu phức tạp hơn; việc duyệt với độ phức tạp bậc hai đáp ứng giới hạn đề bài.

<!-- thinking:end -->

Ta định nghĩa hàm $\text{is\_non\_decreasing}(a)$ để xác định mảng $a$ có phải là mảng không giảm hay không.

Ta dùng một vòng lặp cho đến khi mảng $arr$ trở thành mảng không giảm. Trong mỗi lần lặp, ta tìm tổng nhỏ nhất của các cặp phần tử liền kề trong mảng $arr$ và lưu chỉ số $k$ của phần tử bên trái của cặp đó. Sau đó, ta thay phần tử bên trái bằng tổng của cặp và xóa phần tử bên phải. Cuối cùng, ta trả về số thao tác.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumPairRemoval(self, nums: List[int]) -> int:
        arr = nums[:]
        ans = 0

        def is_non_decreasing(a: List[int]) -> bool:
            for i in range(1, len(a)):
                if a[i] < a[i - 1]:
                    return False
            return True

        while not is_non_decreasing(arr):
            k = 0
            s = arr[0] + arr[1]
            for i in range(1, len(arr) - 1):
                t = arr[i] + arr[i + 1]
                if s > t:
                    s = t
                    k = i
            arr[k] = s
            arr.pop(k + 1)
            ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int minimumPairRemoval(int[] nums) {
        List<Integer> arr = new ArrayList<>();
        for (int x : nums) {
            arr.add(x);
        }
        int ans = 0;
        while (!isNonDecreasing(arr)) {
            int k = 0;
            int s = arr.get(0) + arr.get(1);
            for (int i = 1; i < arr.size() - 1; ++i) {
                int t = arr.get(i) + arr.get(i + 1);
                if (s > t) {
                    s = t;
                    k = i;
                }
            }
            arr.set(k, s);
            arr.remove(k + 1);
            ++ans;
        }
        return ans;
    }

    private boolean isNonDecreasing(List<Integer> arr) {
        for (int i = 1; i < arr.size(); ++i) {
            if (arr.get(i) < arr.get(i - 1)) {
                return false;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumPairRemoval(vector<int>& nums) {
        vector<int> arr = nums;
        int ans = 0;

        while (!isNonDecreasing(arr)) {
            int k = 0;
            int s = arr[0] + arr[1];

            for (int i = 1; i < arr.size() - 1; ++i) {
                int t = arr[i] + arr[i + 1];
                if (s > t) {
                    s = t;
                    k = i;
                }
            }

            arr[k] = s;
            arr.erase(arr.begin() + (k + 1));
            ++ans;
        }

        return ans;
    }

private:
    bool isNonDecreasing(const vector<int>& arr) {
        for (int i = 1; i < (int) arr.size(); ++i) {
            if (arr[i] < arr[i - 1]) {
                return false;
            }
        }
        return true;
    }
};
```

#### Go

```go
func minimumPairRemoval(nums []int) int {
    arr := append([]int(nil), nums...)
    ans := 0

    isNonDecreasing := func(a []int) bool {
        for i := 1; i < len(a); i++ {
            if a[i] < a[i-1] {
                return false
            }
        }
        return true
    }

    for !isNonDecreasing(arr) {
        k := 0
        s := arr[0] + arr[1]

        for i := 1; i < len(arr)-1; i++ {
            t := arr[i] + arr[i+1]
            if s > t {
                s = t
                k = i
            }
        }

        arr[k] = s
        copy(arr[k+1:], arr[k+2:])
        arr = arr[:len(arr)-1]
        ans++
    }

    return ans
}
```

#### TypeScript

```ts
function minimumPairRemoval(nums: number[]): number {
    const arr = nums.slice();
    let ans = 0;
    const isNonDecreasing = (a: number[]): boolean => {
        for (let i = 1; i < a.length; i++) {
            if (a[i] < a[i - 1]) {
                return false;
            }
        }
        return true;
    };
    while (!isNonDecreasing(arr)) {
        let k = 0;
        let s = arr[0] + arr[1];
        for (let i = 1; i < arr.length - 1; ++i) {
            const t = arr[i] + arr[i + 1];
            if (s > t) {
                s = t;
                k = i;
            }
        }
        arr[k] = s;
        arr.splice(k + 1, 1);
        ans++;
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_pair_removal(nums: Vec<i32>) -> i32 {
        let mut arr: Vec<i32> = nums.clone();
        let mut ans: i32 = 0;

        fn is_non_decreasing(a: &Vec<i32>) -> bool {
            for i in 1..a.len() {
                if a[i] < a[i - 1] {
                    return false;
                }
            }
            true
        }

        while !is_non_decreasing(&arr) {
            let mut k: usize = 0;
            let mut s: i32 = arr[0] + arr[1];
            for i in 1..arr.len() - 1 {
                let t: i32 = arr[i] + arr[i + 1];
                if s > t {
                    s = t;
                    k = i;
                }
            }
            arr[k] = s;
            arr.remove(k + 1);
            ans += 1;
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
