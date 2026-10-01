---
comments: true
difficulty: Easy
---

<!-- problem:start -->

# [17.16. The Masseuse](https://leetcode.cn/problems/the-masseuse-lcci)

[中文文档](/lcci/17.16.The%20Masseuse/README.md)

## Mô tả

<!-- description:start -->

<p>Một chuyên viên massage nổi tiếng nhận được một chuỗi yêu cầu đặt lịch liên tiếp và đang cân nhắc nên nhận những yêu cầu nào. Cô ấy cần nghỉ giữa các cuộc hẹn, do đó không thể nhận hai yêu cầu liền kề. Cho một chuỗi các yêu cầu đặt lịch liên tiếp, hãy tìm tập hợp tối ưu (có tổng số phút được đặt lịch cao nhất) mà chuyên viên massage có thể nhận. Trả về số phút.</p>

<p><b>Lưu ý:&nbsp;</b>Bài toán này hơi khác so với phiên bản gốc trong sách.</p>

<p>&nbsp;</p>

<p><strong>Ví dụ 1: </strong></p>

<pre>

<strong>Đầu vào: </strong> [1,2,3,1]

<strong>Đầu ra: </strong> 4

<strong>Giải thích: </strong> Nhận yêu cầu 1 và 3, tổng số phút = 1 + 3 = 4

</pre>

<p><strong>Ví dụ 2: </strong></p>

<pre>

<strong>Đầu vào: </strong> [2,7,9,3,1]

<strong>Đầu ra: </strong> 12

<strong>Giải thích: </strong> Nhận yêu cầu 1, 3 và 5, tổng số phút = 2 + 9 + 1 = 12

</pre>

<p><strong>Ví dụ 3: </strong></p>

<pre>

<strong>Đầu vào: </strong> [2,1,4,5,3,1,1,3]

<strong>Đầu ra: </strong> 12

<strong>Giải thích: </strong> Nhận yêu cầu 1, 3, 5 và 8, tổng số phút = 2 + 4 + 3 + 3 = 12

</pre>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cuộc hẹn không liền kề, mục tiêu là tối đa hóa tổng thời gian — đây là bài toán House Robber. Đệ quy không có memo sẽ tính lặp lại các hậu tố.
>
> Tại $i$ chỉ có hai trạng thái đáng quan tâm: nhận yêu cầu đó (nên bỏ qua $i-1$) hoặc bỏ qua nó.
>
> $f$ chọn yêu cầu hiện tại, $g$ bỏ qua yêu cầu đó: cập nhật $f,g = g+x, \max(f,g)$. Đáp án là giá trị lớn hơn, với không gian hằng số.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def massage(self, nums: List[int]) -> int:
        f = g = 0
        for x in nums:
            f, g = g + x, max(f, g)
        return max(f, g)
```

#### Java

```java
class Solution {
    public int massage(int[] nums) {
        int f = 0, g = 0;
        for (int x : nums) {
            int ff = g + x;
            int gg = Math.max(f, g);
            f = ff;
            g = gg;
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int massage(vector<int>& nums) {
        int f = 0, g = 0;
        for (int& x : nums) {
            int ff = g + x;
            int gg = max(f, g);
            f = ff;
            g = gg;
        }
        return max(f, g);
    }
};
```

#### Go

```go
func massage(nums []int) int {
	f, g := 0, 0
	for _, x := range nums {
		f, g = g+x, max(f, g)
	}
	return max(f, g)
}
```

#### TypeScript

```ts
function massage(nums: number[]): number {
    let f = 0,
        g = 0;
    for (const x of nums) {
        const ff = g + x;
        const gg = Math.max(f, g);
        f = ff;
        g = gg;
    }
    return Math.max(f, g);
}
```

#### Swift

```swift
class Solution {
    func massage(_ nums: [Int]) -> Int {
        var f = 0
        var g = 0

        for x in nums {
            let ff = g + x
            let gg = max(f, g)
            f = ff
            g = gg
        }

        return max(f, g)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
