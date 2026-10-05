---
comments: true
difficulty: Medium
rating: 1853
source: Weekly Contest 485 Q3
---

<!-- problem:start -->

# [3815. Design Auction System](https://leetcode.com/problems/design-auction-system)

[中文文档](/solution/3800-3899/3815.Design%20Auction%20System/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được yêu cầu thiết kế một hệ thống đấu giá quản lý các lượt đặt giá từ nhiều người dùng theo thời gian thực.</p>

<p>Mỗi lượt đặt giá gắn với một <code>userId</code>, một <code>itemId</code> và một <code>bidAmount</code>.</p>

<p>Hãy triển khai lớp <code>AuctionSystem</code>:​​​​​​​</p>

<ul>
	<li><code>AuctionSystem()</code>: Khởi tạo đối tượng <code>AuctionSystem</code>.</li>
	<li><code>void addBid(int userId, int itemId, int bidAmount)</code>: Thêm một lượt đặt giá mới cho <code>itemId</code> từ <code>userId</code> với <code>bidAmount</code>. Nếu <code>userId</code> đó <strong>đã có</strong> lượt đặt giá cho <code>itemId</code>, <strong>thay thế</strong> lượt đặt giá đó bằng <code>bidAmount</code> mới.</li>
	<li><code>void updateBid(int userId, int itemId, int newAmount)</code>: Cập nhật lượt đặt giá hiện có của <code>userId</code> cho <code>itemId</code> thành <code>newAmount</code>. <strong>Đảm bảo</strong> rằng lượt đặt giá này <em>tồn tại</em>.</li>
	<li><code>void removeBid(int userId, int itemId)</code>: Xóa lượt đặt giá của <code>userId</code> cho <code>itemId</code>. <strong>Đảm bảo</strong> rằng lượt đặt giá này <em>tồn tại</em>.</li>
	<li><code>int getHighestBidder(int itemId)</code>: Trả về <code>userId</code> của người đặt giá <strong>cao nhất</strong> cho <code>itemId</code>. Nếu có nhiều người dùng cùng có <code>bidAmount</code> <strong>cao nhất</strong>, trả về người dùng có <code>userId</code> <strong>lớn nhất</strong>. Nếu không có lượt đặt giá nào cho item đó, trả về -1.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong><br />
<span class="example-io">[&quot;AuctionSystem&quot;, &quot;addBid&quot;, &quot;addBid&quot;, &quot;getHighestBidder&quot;, &quot;updateBid&quot;, &quot;getHighestBidder&quot;, &quot;removeBid&quot;, &quot;getHighestBidder&quot;, &quot;getHighestBidder&quot;]<br />
[[], [1, 7, 5], [2, 7, 6], [7], [1, 7, 8], [7], [2, 7], [7], [3]]</span></p>

<p><strong>Đầu ra:</strong><br />
<span class="example-io">[null, null, null, 2, null, 1, null, 1, -1] </span></p>

<p><strong>Giải thích</strong></p>
AuctionSystem auctionSystem = new AuctionSystem(); // Khởi tạo hệ thống đấu giá<br />
auctionSystem.addBid(1, 7, 5); // Người dùng 1 đặt giá 5 cho item 7<br />
auctionSystem.addBid(2, 7, 6); // Người dùng 2 đặt giá 6 cho item 7<br />
auctionSystem.getHighestBidder(7); // trả về 2 vì người dùng 2 có lượt đặt giá cao nhất<br />
auctionSystem.updateBid(1, 7, 8); // Người dùng 1 cập nhật lượt đặt giá thành 8 cho item 7<br />
auctionSystem.getHighestBidder(7); // trả về 1 vì người dùng 1 hiện có lượt đặt giá cao nhất<br />
auctionSystem.removeBid(2, 7); // Xóa lượt đặt giá của người dùng 2 cho item 7<br />
auctionSystem.getHighestBidder(7); // trả về 1 vì người dùng 1 đang là người đặt giá cao nhất<br />
auctionSystem.getHighestBidder(3); // trả về -1 vì không có lượt đặt giá nào cho item 3</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= userId, itemId &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= bidAmount, newAmount &lt;= 10<sup>9</sup></code></li>
	<li>Có nhiều nhất <code>5 * 10<sup>4</sup></code> lời gọi đến <code>addBid</code>, <code>updateBid</code>, <code>removeBid</code> và <code>getHighestBidder</code>.</li>
	<li>Dữ liệu đầu vào được tạo sao cho với <code>updateBid</code> và <code>removeBid</code>, lượt đặt giá từ <code>userId</code> đã cho cho <code>itemId</code> đã cho luôn hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table + Ordered Set

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần thêm, cập nhật và xóa các lượt đặt giá, đồng thời truy vấn người đặt giá cao nhất của một item, với quy tắc phá hòa bằng $\textit{userId}$ lớn hơn. Với tối đa $5 \times 10^4$ lời gọi, ta không thể duyệt qua các lượt đặt giá của một item.
>
> Truy vấn cực đại cần một đầu mút đã được sắp xếp; các thao tác cập nhật và xóa phải tìm rồi loại bỏ lượt đặt giá trước đó của người dùng.
>
> Lưu $(\textit{bidAmount},\textit{userId})$ trong một ordered set cho mỗi item, đồng thời lưu $\textit{users}$ là bid hiện tại để thao tác xóa có độ phức tạp $O(\log m)$.
>
> Khi thêm, trước tiên xóa lượt đặt giá đã tồn tại; người đặt giá cao nhất là user của cặp cuối cùng. Hai map luôn đồng bộ nên mọi thao tác đều có độ phức tạp logarithmic.

<!-- thinking:end -->

Ta định nghĩa hai hash table. `items` dùng để lưu toàn bộ thông tin đặt giá của từng item, trong đó `items[itemId]` lưu một ordered set. Mỗi phần tử trong set là một tuple `(bidAmount, userId)`, biểu diễn mức giá của một người dùng cho item đó. Vì cần nhanh chóng lấy người dùng có mức giá cao nhất, ordered set này phải được sắp xếp theo mức giá tăng dần. Nếu các mức giá giống nhau, chúng được sắp xếp theo user ID tăng dần. Hash table còn lại `users` dùng để lưu thông tin đặt giá của từng người dùng cho mỗi item, trong đó `users[userId][itemId]` lưu mức giá của người dùng cho item đó.

Với thao tác `addBid(userId, itemId, bidAmount)`, trước tiên ta kiểm tra xem người dùng đã đặt giá cho item này chưa. Nếu rồi, ta gọi phương thức `removeBid(userId, itemId)` để xóa lượt đặt giá ban đầu; sau đó thêm thông tin đặt giá mới vào `users` và `items`.

Với thao tác `updateBid(userId, itemId, newAmount)`, trước tiên ta lấy mức giá ban đầu của người dùng cho item từ `users`, sau đó xóa tuple `(oldAmount, userId)` tương ứng khỏi `items`, thêm thông tin đặt giá mới vào `items` và cập nhật mức giá trong `users`.

Với thao tác `removeBid(userId, itemId)`, trước tiên ta lấy mức giá ban đầu của người dùng cho item từ `users`, sau đó xóa tuple `(oldAmount, userId)` tương ứng khỏi `items`, cuối cùng xóa thông tin đặt giá của người dùng cho item đó khỏi `users`.

Với thao tác `getHighestBidder(itemId)`, trước tiên ta kiểm tra xem `items[itemId]` có rỗng hay không. Nếu rỗng, ta trả về -1; ngược lại, ta trả về user ID của phần tử cuối cùng trong ordered set, tương ứng với người đặt giá cao nhất.

Độ phức tạp thời gian của mỗi thao tác là $O(\log m)$, trong đó $m$ là số lượt đặt giá của item hiện tại. Độ phức tạp không gian là $O(n)$, trong đó $n$ là tổng số lượt đặt giá.

<!-- tabs:start -->

#### Python3

```python
class AuctionSystem:

    def __init__(self):
        self.items = defaultdict(SortedList)
        self.users = {}

    def addBid(self, userId: int, itemId: int, bidAmount: int) -> None:
        if userId not in self.users:
            self.users[userId] = {}
        if itemId in self.users[userId]:
            self.removeBid(userId, itemId)
        self.users[userId][itemId] = bidAmount
        self.items[itemId].add((bidAmount, userId))

    def updateBid(self, userId: int, itemId: int, newAmount: int) -> None:
        oldAmount = self.users[userId][itemId]
        self.items[itemId].remove((oldAmount, userId))
        self.items[itemId].add((newAmount, userId))
        self.users[userId][itemId] = newAmount

    def removeBid(self, userId: int, itemId: int) -> None:
        oldAmount = self.users[userId][itemId]
        self.items[itemId].remove((oldAmount, userId))
        self.users[userId].pop(itemId)

    def getHighestBidder(self, itemId: int) -> int:
        ls = self.items[itemId]
        return -1 if not ls else ls[-1][1]


# Your AuctionSystem object will be instantiated and called as such:
# obj = AuctionSystem()
# obj.addBid(userId,itemId,bidAmount)
# obj.updateBid(userId,itemId,newAmount)
# obj.removeBid(userId,itemId)
# param_4 = obj.getHighestBidder(itemId)
```

#### Java

```java
class AuctionSystem {
    private final Map<Integer, TreeSet<int[]>> items = new HashMap<>();
    private final Map<Integer, Map<Integer, Integer>> users = new HashMap<>();

    public AuctionSystem() {
    }

    public void addBid(int userId, int itemId, int bidAmount) {
        users.computeIfAbsent(userId, k -> new HashMap<>());

        if (users.get(userId).containsKey(itemId)) {
            removeBid(userId, itemId);
        }

        users.get(userId).put(itemId, bidAmount);

        items.computeIfAbsent(itemId, k -> new TreeSet<>(
            (a, b) -> a[0] != b[0] ? Integer.compare(a[0], b[0]) : Integer.compare(a[1], b[1])
        ));

        items.get(itemId).add(new int[]{bidAmount, userId});
    }

    public void updateBid(int userId, int itemId, int newAmount) {
        int oldAmount = users.get(userId).get(itemId);
        TreeSet<int[]> set = items.get(itemId);

        set.remove(new int[]{oldAmount, userId});
        set.add(new int[]{newAmount, userId});

        users.get(userId).put(itemId, newAmount);
    }

    public void removeBid(int userId, int itemId) {
        int oldAmount = users.get(userId).get(itemId);
        TreeSet<int[]> set = items.get(itemId);

        set.remove(new int[]{oldAmount, userId});
        users.get(userId).remove(itemId);
    }

    public int getHighestBidder(int itemId) {
        TreeSet<int[]> set = items.get(itemId);
        if (set == null || set.isEmpty()) {
            return -1;
        }
        return set.last()[1];
    }
}

/**
 * Your AuctionSystem object will be instantiated and called as such:
 * AuctionSystem obj = new AuctionSystem();
 * obj.addBid(userId,itemId,bidAmount);
 * obj.updateBid(userId,itemId,newAmount);
 * obj.removeBid(userId,itemId);
 * int param_4 = obj.getHighestBidder(itemId);
 */
```

#### C++

```cpp
class AuctionSystem {
    unordered_map<int, set<pair<int, int>>> items;
    unordered_map<int, unordered_map<int, int>> users;

public:
    AuctionSystem() {
    }

    void addBid(int userId, int itemId, int bidAmount) {
        if (users[userId].count(itemId)) {
            removeBid(userId, itemId);
        }
        users[userId][itemId] = bidAmount;
        items[itemId].insert({bidAmount, userId});
    }

    void updateBid(int userId, int itemId, int newAmount) {
        int oldAmount = users[userId][itemId];
        auto& s = items[itemId];
        s.erase({oldAmount, userId});
        s.insert({newAmount, userId});
        users[userId][itemId] = newAmount;
    }

    void removeBid(int userId, int itemId) {
        int oldAmount = users[userId][itemId];
        auto& s = items[itemId];
        s.erase({oldAmount, userId});
        users[userId].erase(itemId);
    }

    int getHighestBidder(int itemId) {
        auto it = items.find(itemId);
        if (it == items.end() || it->second.empty()) {
            return -1;
        }
        return it->second.rbegin()->second;
    }
};

/**
 * Your AuctionSystem object will be instantiated and called as such:
 * AuctionSystem* obj = new AuctionSystem();
 * obj->addBid(userId,itemId,bidAmount);
 * obj->updateBid(userId,itemId,newAmount);
 * obj->removeBid(userId,itemId);
 * int param_4 = obj->getHighestBidder(itemId);
 */
```

#### Go

```go

```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
