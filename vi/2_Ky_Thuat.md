# CHƯƠNG 2: ĐẶC TẢ KỸ THUẬT

## PHẦN 1 - HỌC NHANH

### 1.1 Bảng thuật ngữ và ký hiệu

Toàn bộ tài liệu này sử dụng các thuật ngữ và ký hiệu sau với ý nghĩa thống nhất. Khi có sự khác biệt giữa cách hiểu thông thường và định nghĩa tại bảng này, định nghĩa trong bảng là chuẩn.

| **Thuật ngữ**                       | **Ký hiệu/cú pháp** | **Định nghĩa**                                                                                                                                                                                                                                                    |
| ----------------------------------- | ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vai trò (Role)                      | **`@N`**                  | Người hoặc bộ phận chịu trách nhiệm thực hiện một hoặc nhiều Giai đoạn. Khai báo Vai trò là tùy chọn: có khai báo thì sơ đồ vẽ dạng làn bơi (swimlane); không khai báo thì sơ đồ vẽ không có Làn. |
| Làn (làn bơi)                      | **`@N`**                  | Vùng riêng trên sơ đồ dành cho một Vai trò. Tên Vai trò được ghi làm nhãn của Làn. Các Nút của Vai trò đó được đặt bên trong Làn này.                                                                                                                             |
| Giai đoạn (Stage)                   | **`Gx`**                  | Đơn vị tổ chức lớn nhất trong nội dung quy trình. Mỗi Giai đoạn thuộc về một Vai trò duy nhất. Các Giai đoạn tạo thành trục thời gian chính: G1, G2, G3, ...                                                                                                      |
| Mã định danh (ID)                   | **`x.y`**                 | Chuỗi ký tự xác định duy nhất cho mọi Nút trong quy trình.                                                                                                                                                                                                        |
| Nút (Node)                          |                           | Từ dùng để gọi chung cho mọi đối tượng được gán ID trong quy trình, bao gồm: Tác vụ, Ngõ, Quy trình con, Quyết định, Cổng vào, Cổng ra, Cổng phụ và Điểm nối.                                                                                                     |
| Luồng (Flow)                        |                           | Quá trình thực hiện từ Nút này đến Nút khác.                                                                                                                                                                                                                      |
| Mũi tên/Đường luồng (Flowline)      | **`->`**                  | Mũi tên biểu diễn hướng chuyển tiếp giữa các Nút.                                                                                                                                                                                                                 |
| Nhãn Đường luồng (Label)            | **`'...'`**               | Là văn bản ngắn gắn kèm với một Đường luồng cụ thể. Không bắt buộc phải có.                                                                                                                                                                                       |
| Khối mô tả/nội dung/bọc văn bản     | **`"..."`**               | Dùng để bọc văn bản có nhiều dòng hoặc chứa ký tự cú pháp.                                                                                                                                                                                                        |
| Khối ghi chú (Annotation)           | **`~...~`**               | Dùng để bọc ghi chú. Không tham gia Luồng quy trình. Gắn với đối tượng xuất hiện ngay trước nó. Không bắt buộc phải có. Trên sơ đồ: hình chữ nhật nét đứt, nối bằng đường đứt.                                                                                    |
| Cổng (Terminal)                     |                           | Tên gọi chung cho Cổng vào, Cổng ra và Cổng phụ của quy trình.                                                                                                                                                                                                    |
| Cổng vào (Start)                    | **`\.`**                  | Nơi bắt đầu vào của quy trình. Phải khai báo tường minh tại Nút 1.1 nếu muốn hiển thị trên sơ đồ; không tự động tạo. Có thể có nhiều Cổng vào. Trên sơ đồ: hình elip hoặc bầu dục.                                                                               |
| Cổng ra (End)                       | **`./`**                  | Nơi thoát khỏi quy trình (kết thúc). `-> ./` trỏ đến Cổng ra Mặc định (ID ảo `z`); khai báo `x.y ./` tạo Cổng ra tường minh riêng biệt. Có thể có nhiều Cổng ra. Trên sơ đồ: hình elip hoặc bầu dục.                                                             |
| Cổng phụ (Off-page Connector)       | **`\/`**                  | Ký hiệu thay thế cho Đường luồng nối sang trang sơ đồ khác. Loại (đầu vào hay đầu ra) được xác định tự động: có `->` đi ra là đầu vào, không có là đầu ra. Mô tả nên ngắn gọn 1-2 ký tự (0-9 hoặc a-Z). Trên sơ đồ: hình ngũ giác ngược với cạnh nhọn quay xuống dưới. |
| Hành động (Action)                  |                           | Từ dùng để gọi chung mọi Tác vụ, Ngõ hoặc Quy trình con.                                                                                                                                                                                                          |
| Tác vụ (Process)                    |                           | Mô tả một việc cụ thể cần làm. Trên sơ đồ: hình chữ nhật.                                                                                                                                                                                                         |
| Ngõ (I/O)                           | **`/`**                   | Tên gọi chung cho Ngõ vào (Input) và Ngõ ra (Output). Dùng để đánh dấu điểm dữ liệu đi vào hoặc đi ra khỏi Luồng xử lý. Tham gia Luồng điều khiển giống Tác vụ. Trên sơ đồ: hình bình hành.                                                                       |
| Quy trình con (Predefined Process)  | **`!`**                   | Tác vụ gọi đến một quy trình hoặc thủ tục đã được đặt tên và mô tả chi tiết ở nơi khác. Khi Luồng đến đây, nó thực hiện sơ đồ con đó rồi quay lại Luồng chính. Tham gia Luồng điều khiển giống Tác vụ. Trên sơ đồ: hình chữ nhật có hai cạnh thẳng đứng gạch đôi. |
| Quyết định (Decision)               | **`?`**                   | Điểm rẽ nhánh, tại đây Luồng chỉ chọn một nhánh để đi tiếp. Trên sơ đồ: hình thoi.                                                                                                                                                                                |
| Điểm nối (On-page Connector)        | **`()`**                  | Cặp ký hiệu gồm một hoặc nhiều Điểm thu và một Điểm phát, dùng để thay thế Đường luồng dài trên cùng trang hoặc làm điểm gộp AND-join cho các nhánh. Mô tả của Điểm nối nên ngắn gọn 1-2 ký tự (chữ số 0-9 hoặc chữ cái a-Z). Trên sơ đồ: hình tròn nhỏ.          |
| Nhánh (Branch)                      |                           | Một Luồng đi ra từ một Nút (trừ Cổng ra, Cổng phụ đi ra). Một nhánh có thể bao gồm một hoặc nhiều Nút.                                                                                                                                                           |
| Phân tách/song song (Fork/Parallel) |                           | Từ một Nút (trừ Cổng ra, Cổng phụ đi ra) chia ra nhiều Luồng thực hiện đồng thời (AND-fork).                                                                                                                                                                     |
| Gộp/Hội tụ (Join)                   |                           | Nơi các Luồng song song gộp lại trước khi tiếp tục (AND-join).                                                                                                                                                                                                    |
| Ký hiệu phân loại                   | Ký_hiệu                   | Đại diện cho một ký hiệu nhằm phân biệt Nút thuộc loại nào (`?` `/` `!` `./`...)                                                                                                                                                                                  |
| Đường nối (Connect lines)           | **`__`**                  | Cách sử dụng giống Đường luồng, nhưng khi vẽ trên sơ đồ sẽ không có đầu mũi tên.                                                                                                                                                                                  |
| Bình luận (Comment)                 | **`#`**                   | Ghi chú dành cho người viết/đọc tài liệu, không có giá trị cú pháp và không xuất hiện trên sơ đồ.                                                                                                                                                                 |
| Tường minh                          |                           | Được khai báo rõ ràng, đúng cú pháp.                                                                                                                                                                                                                              |

### 1.2 Nguyên tắc tổng thể

Tài liệu quy trình hợp lệ tồn tại ở hai dạng:

- **Có khai báo Vai trò:** thứ tự bắt buộc là (1) khai báo Vai trò, (2) nội dung quy trình gồm các Giai đoạn và Nút.
- **Không khai báo Vai trò:** chỉ có nội dung quy trình, các Nút được đánh số dạng `1.y`.

Các nguyên tắc cốt lõi cần nắm ngay:

- **QT.1.1:** Không được viết bất kỳ Giai đoạn hoặc Nút nào trước khi khai báo xong toàn bộ Vai trò (nếu có khai báo Vai trò).
- **QT.1.2:** Mỗi Giai đoạn (`Gx`) thuộc về đúng một Vai trò.
- **QT.1.3:** Mỗi Nút phải có Mã định danh (ID), kể cả khi Giai đoạn chỉ có một Nút.
- **QT.1.4:** Trong chế độ làn bơi, không được có Nút nào đứng ngoài một Giai đoạn. Mọi dòng `x.y` phải nằm sau một dòng `@N Gx` và trước dòng `@M G(x+1)` hoặc hết tài liệu. Trong chế độ không làn bơi, mọi Nút dùng ID dạng `1.y` và không có Giai đoạn.
- **QT.1.5:** Các Nút trong cùng một Giai đoạn chạy tuần tự theo thứ tự tăng dần mà không cần viết `->` ra. Khi `x.y` có `->` tường minh, luồng tuần tự mặc định từ `x.y` sang `x.(y+1)` bị hủy; `x.y` chỉ đi đến đích được khai báo tường minh.
- **QT.1.6:** Quy trình bắt đầu tại Nút 1.1. Không có Cổng vào nào được tạo tự động; muốn có Cổng vào trên sơ đồ phải khai báo tường minh tại Nút 1.1. Không có yêu cầu bắt buộc về loại Nút kết thúc.
- **QT.1.7:** Rẽ nhánh từ Quyết định tạo ra N Đường luồng nhưng chỉ một được chọn (XOR). Rẽ nhánh từ các Nút khác tạo ra Phân tách song song (AND-fork). Cú pháp khai báo nhánh (dùng dấu `,` ngăn cách) là đồng nhất cho cả hai trường hợp; sự khác biệt chỉ do ký hiệu loại của Nút nguồn.
- **QT.1.8:** Để gộp các nhánh, dùng `->` đến cùng một đích.
- **QT.1.9:** Mô tả nhiều dòng hoặc có ký tự cú pháp phải bọc trong `"..."`.
- **QT.1.10:** Ghi chú không tham gia Luồng được viết trong `~...~`.
- **QT.1.11:** Lịch trình định kỳ và giới hạn thời gian nên đưa vào Mô tả hoặc Ghi chú.
- **QT.1.12:** Mọi nhánh đều phải có đích xác định: một ID Nút khác hoặc `./`.
- **QT.1.13:** Không bắt buộc phải sử dụng Ghi chú và Nhãn đường luồng.
- **QT.1.14:** Mọi văn bản đứng sau `#` thì đều là Bình luận và bị bỏ qua hoàn toàn về mặt cú pháp.
- **QT.1.15:** Thụt lề ở bất kỳ dòng nào trong tài liệu đều không có giá trị cú pháp.

### 1.3 Cú pháp thường dùng

Dưới đây là bốn cú pháp xuất hiện nhiều nhất trong mọi quy trình.

**1.3.1 Tác vụ:** ID đứng đầu dòng, theo sau là Mô tả. Nếu có Đường luồng, viết `->` và ID đích ở cuối.

```
    2.1 Lập hóa đơn -> 2.2
```

**1.3.2 Quyết định:** thêm dấu `?` sau ID, các nhánh đi ra được viết dạng nhiều dòng hoặc 1 dòng, ngăn cách nhau bằng dấu `,`.

```
    2.3 ? Khách đã thanh toán?
        'Rồi' -> 3.1,
        'Chưa' -> 2.4
```

**1.3.3 Đường luồng:** `->` dẫn đến ID đích, 'Nhãn' gắn nhãn cho mũi tên.

```
    2.1 Kiểm tra hồ sơ 'Hợp lệ' -> 2.2
```

**1.3.4 Cổng ra:** `->` dẫn đến `./` cho biết quy trình kết thúc.

```
    2.1 Nhập dữ liệu vào hồ sơ -> ./
```

### 1.4 Ví dụ hoàn chỉnh

```
    @1 Nhân viên kinh doanh (NVKD)
    @2 Trưởng phòng (TP)
    @3 Kế toán (KT)

    @1 G1
    1.1 Tiếp nhận đơn hàng từ khách
    1.2 ? Đơn hàng đầy đủ thông tin?
        'Không' -> ./,
        'Có' -> 2.1

    @2 G2
    2.1 Xem xét các điều kiện và điều khoản
    2.2 ? Đơn hàng được duyệt?
        'Không' -> ./,
        'Có' -> 3.1

    @3 G3
    3.1 Lập hóa đơn và gửi cho khách -> ./
```

- Luồng 1 (từ chối sớm): 1.1 -> 1.2 [Không]-> ./
- Luồng 2 (từ chối ở TP): 1.1 -> 1.2 [Có]-> 2.1 -> 2.2 [Không]-> ./
- Luồng 3 (hoàn tất): 1.1 -> 1.2 [Có]-> 2.1 -> 2.2 [Có]-> 3.1 -> ./

**Sơ đồ:**

```
Nhân viên kinh doanh (NVKD)  Trưởng phòng (TP)  Kế toán (KT)

    ┌───────────────────┐
    │ Tiếp nhận đơn     │
    │ hàng từ khách     │
    └─────────┬─────────┘
              │
     /────────▼────────\
    / Đơn hàng đầy đủ   \
    \ thông tin?        /
     \────┬───────┬────/
          │       │
        Không     Có
          │       │
      ┌───▼────┐  │
      │ Từ chối│  │     ┌─────────────────────┐
      └────────┘  └────►│ Xem xét các điều    │
                        │ kiện và điều khoản  │
                        └──────────┬──────────┘
                                   │
                         /─────────▼─────────\
                        / Đơn hàng được       \
                        \ duyệt?              /
                         \────┬───────┬──────/
                              │       │
                            Không     Có
                              │       │
                          ┌───▼───┐   │
                          │  Hủy  │   │    ┌─────────────────────┐
                          └───────┘   └───►│ Lập hóa đơn và      │
                                           │ gửi cho khách       │
                                           └──────────┬──────────┘
                                                      │
                                                  ┌───▼───┐
                                                  │ Xong  │
                                                  └───────┘
```

## PHẦN 2 - VAI TRÒ (ROLE)

### 2.1 Khái niệm

Một tài liệu quy trình có hai chế độ tùy theo việc có hay không khai báo Vai trò:

**Chế độ làn bơi (có khai báo Vai trò):** Dùng khi quy trình có nhiều bên tham gia. Mỗi Vai trò là người hoặc bộ phận chịu trách nhiệm thực hiện một hoặc nhiều Giai đoạn. Khi vẽ sơ đồ, mỗi Vai trò chiếm một Làn riêng biệt. Khai báo Vai trò và khai báo Giai đoạn đều bắt buộc trong chế độ này.

**Chế độ không làn bơi (không khai báo Vai trò):** Dùng khi quy trình không cần phân biệt bên thực hiện. Ví dụ: quy trình làm việc cá nhân, sơ đồ hướng dẫn từng bước. Không khai báo Vai trò, không khai báo Giai đoạn. Các Nút được đánh số từ 1.1 đến 1.N và vẽ thẳng trên sơ đồ, không có khung Làn bao quanh.

### 2.2 Cú pháp

```
    @N Tên đầy đủ (TÊN VIẾT TẮT)
```

**Trong đó:**

- `@N`: ký hiệu Vai trò, N là số nguyên dương bắt đầu từ 1, tăng dần. Ví dụ: @1, @2, @3, ...
- `Tên đầy đủ`: chuỗi văn bản mô tả Vai trò, ví dụ: Trưởng phòng, Kế toán.
- `(TÊN VIẾT TẮT)`: tên viết tắt, không bắt buộc, đặt trong ngoặc đơn ngay sau tên đầy đủ. Tên viết tắt phải viết HOA toàn bộ, không chứa khoảng trắng, và phải được dùng nhất quán trong toàn bộ quy trình. Hai Vai trò khác nhau không được trùng tên viết tắt.

### 2.3 Quy tắc

- **QT.2.1:** Nếu có khai báo Vai trò, tất cả dòng khai báo Vai trò phải xuất hiện ở đầu tài liệu, trước mọi Giai đoạn và Nút.
- **QT.2.2:** Mỗi Vai trò chiếm một dòng riêng.
- **QT.2.3:** Số `@N` phải tăng dần liên tục: `@1`, `@2`, `@3`, ... Không được viết `@1` rồi nhảy sang `@3`.
- **QT.2.4:** Khi có khai báo Vai trò (dù chỉ một Vai trò), sơ đồ luôn vẽ dạng làn bơi với Làn tương ứng.
- **QT.2.5:** Khi không có khai báo Vai trò, không được khai báo Giai đoạn. Mọi Nút dùng ID dạng `1.y`.

### 2.4 Ví dụ

Ví dụ 1 - Nhiều Vai trò có tên viết tắt (chế độ làn bơi):

```
    @1 Trưởng phòng (TP)
    @2 Phó giám đốc (PGD)
    @3 Kế toán (KT)
```

Ví dụ 2 - Nhiều Vai trò không có tên viết tắt (chế độ làn bơi):

```
    @1 Nhân viên kinh doanh
    @2 Quản lý kho
```

Ví dụ 3 - Một Vai trò duy nhất (chế độ làn bơi với một Làn):

```
    @1 Nhân viên kinh doanh

    @1 G1
    1.1 Tiếp nhận yêu cầu
    1.2 Xử lý yêu cầu -> ./
```

Ví dụ 4 - Không khai báo Vai trò (chế độ không làn bơi):

```
    1.1 Chuẩn bị tài liệu
    1.2 Nộp hồ sơ -> ./
```

Ví dụ 5 - Không hợp lệ vì thiếu `@2`:

```
    @1 Nhân viên kinh doanh
    @3 Kế toán

    Giải thích: Số @N phải tăng liên tục, không được nhảy số.
```

Ví dụ 6 - Không hợp lệ vì khai báo Giai đoạn khi không có Vai trò:

```
    @1 G1
    1.1 Làm việc A

    Giải thích: @1 G1 là khai báo Giai đoạn G1 thuộc Vai trò @1, nhưng không có dòng khai báo Vai trò @1 trước đó.
```

### 2.5 Cách vẽ sơ đồ

**Chế độ làn bơi:**

- Mỗi Vai trò được vẽ thành một Làn riêng biệt.
- Nhãn của Làn ghi tên đầy đủ, kèm tên viết tắt trong ngoặc đơn nếu có.
- Thứ tự các Làn từ trái sang phải hoặc từ trên xuống dưới theo thứ tự `@1`, `@2`, `@3`, ...
- Đối với Ví dụ 1, sơ đồ Làn bơi dạng cột dọc sẽ có các nhãn: Trưởng phòng (TP) | Phó giám đốc (PGD) | Kế toán (KT)

**Chế độ không làn bơi:** Các Nút được vẽ thẳng trên sơ đồ, không có khung Làn bao quanh.

## PHẦN 3 - GIAI ĐOẠN (STAGE)

### 3.1 Khái niệm

Giai đoạn là đơn vị tổ chức lớn nhất trong nội dung quy trình. Mỗi Giai đoạn thuộc về một Vai trò duy nhất và chứa một hoặc nhiều Nút. Các Giai đoạn tạo thành trục thời gian chính: G1 xảy ra trước G2, G2 trước G3. Khai báo một Giai đoạn không tương đương với một hình vẽ cụ thể trên sơ đồ, nó chỉ xác định nhóm Nút tiếp theo thuộc Làn của Vai trò nào.

### 3.2 Cú pháp

```
    @N Gx
```

**Trong đó:**

- `@N`: Vai trò chịu trách nhiệm cho toàn bộ Giai đoạn này.
- `Gx`: chữ G (viết tắt của Giai đoạn) cố định kết hợp với số thứ tự Giai đoạn `x`. `x` là số nguyên dương, bắt đầu từ 1, tăng dần.

### 3.3 Quy tắc

- **QT.3.1:** Mỗi Giai đoạn bắt buộc phải có một dòng khai báo `@N` `Gx`.
- **QT.3.2:** Dòng khai báo Giai đoạn `@N` `Gx` phải đứng trên một dòng riêng biệt. Không được có thêm bất kỳ ký tự nào sau `@N` `Gx` trên cùng dòng đó. Ngoại lệ duy nhất: Bình luận bắt đầu bằng `#` được phép đứng sau `@N` `Gx` trên cùng dòng.
- **QT.3.3:** Trong cú pháp khai báo Giai đoạn, bắt buộc phải dùng `@N`, không dùng tên viết tắt.
- **QT.3.4:** Các Giai đoạn phải theo thứ tự tăng dần: G1, G2, G3, ... Không được phép nhảy số.
- **QT.3.5:** Một Giai đoạn chỉ có một Vai trò chịu trách nhiệm. Không thể có hai Vai trò khác nhau cùng chịu trách nhiệm một Giai đoạn.
- **QT.3.6:** Sau khi hoàn thành Nút cuối cùng của Giai đoạn `Gx`, nếu Nút đó chưa có `->` tường minh đến một đích khác, quy trình sẽ chuyển sang Giai đoạn `G(x+1)` và bắt đầu từ Nút đầu tiên của Giai đoạn đó. Nếu Nút cuối cùng đã có `->` tường minh, luồng mặc định sang `G(x+1)` bị hủy và quy trình đi theo đích được khai báo.
- **QT.3.7:** Một Vai trò có thể chịu trách nhiệm cho nhiều Giai đoạn không liên tiếp. Ví dụ: `@1` có thể thực hiện G1 rồi `@2` thực hiện G2, sau đó `@1` thực hiện tiếp G3.
- **QT.3.8:** Khai báo Giai đoạn chỉ có hiệu lực khi có khai báo Vai trò. Nếu không có khai báo Vai trò, không được phép khai báo Giai đoạn, nếu không sẽ được coi là lỗi cú pháp.

### 3.4 Ví dụ

Ví dụ 1 - Khai báo Giai đoạn đúng cú pháp:

```
    @1 G1
    1.1 A
    1.2 B
    @2 G2
    2.1 C
```

Ví dụ 2 - Sai vì có ký tự khác sau khai báo:

```
    @1 G1 Giai đoạn xử lý hồ sơ

    Giải thích: Không hợp lệ vì có ký tự sau @N Gx.
```

Ví dụ 3 - Ngoại lệ hợp lệ (Bình luận):

```
    @1 G1 # Giai đoạn xử lý hồ sơ đầu vào

    Giải thích: Hợp lệ vì phần sau @1 G1 là Bình luận bắt đầu bằng #.
```

Ví dụ 4 - Sai (thiếu G2):

```
    @1 G1
    1.1
    ...
    @3 G3

    Giải thích: Không hợp lệ vì thiếu Giai đoạn G2. Số Giai đoạn phải tăng liên tục.
```

### 3.5 Cách vẽ sơ đồ

- Bản thân dòng khai báo `@N` `Gx` không tạo ra hình vẽ riêng trên sơ đồ.
- Toàn bộ các Nút bên trong Giai đoạn được vẽ trong Làn của Vai trò `@N`.
- Các Giai đoạn nối tiếp nhau theo trục thời gian (từ trên xuống dưới hoặc từ trái sang phải tùy bố cục sơ đồ).

## PHẦN 4 - NÚT: TỔNG QUAN VÀ ĐÁNH SỐ

### 4.1 Khái niệm

Nút (Node) là từ dùng để gọi chung cho mọi đối tượng được gán ID trong quy trình, bao gồm: Tác vụ, Ngõ, Quy trình con, Quyết định, Cổng vào, Cổng ra, Cổng phụ và Điểm nối.

Trong đó:

- Hành động là nhóm gồm ba loại: Tác vụ, Ngõ và Quy trình con. Ba loại này có mọi quy tắc về luồng hoàn toàn giống nhau, chỉ khác ở ký hiệu khai báo và hình vẽ trên sơ đồ.
- Quyết định là điểm rẽ nhánh trong quy trình. Tại đây, Luồng chỉ chọn một nhánh trong số các nhánh được liệt kê (XOR).
- Cổng, Cổng phụ và Điểm nối là các Nút đặc biệt đánh dấu ranh giới hoặc kết nối của quy trình.

### 4.2 Cú pháp khai báo Nút

Mọi Nút đều bắt đầu bằng ID theo dạng `x.y`, tiếp theo là Ký hiệu loại (nếu có) và Mô tả. Cú pháp tổng quát:

```
    x.y Ký_hiệu "Mô tả" ~Ghi chú~
```

**Trong đó:**

- `x.y`: Mã định danh (ID) của Nút:
Mỗi Nút có một ID duy nhất trong tài liệu, theo dạng `x.y` trong đó `x` là số thứ tự Giai đoạn và `y` là số thứ tự của Nút trong Giai đoạn đó.

| **Giai đoạn** | **ID Nút**               |
| ---       | ---                  |
| G1        | 1.1, 1.2, 1.3, ...   |
| G2        | 2.1, 2.2, 2.3, ...   |
| Gx        | x.1, x.2, x.3, ...   |

- `Ký_hiệu` xác định loại Nút:

| **Loại Nút** | **Cú pháp khai báo** | **Ghi chú** |
| --- | --- | --- |
| Tác vụ | `x.y "Mô tả" ~Ghi chú~` | Không có ký hiệu riêng. |
| Ngõ | `x.y / "Mô tả" ~Ghi chú~` | |
| Quy trình con | `x.y ! "Mô tả" ~Ghi chú~` | |
| Quyết định | `x.y ? "Mô tả" ~Ghi chú~` | |
| Cổng vào | `x.y \. "Mô tả" ~Ghi chú~` | |
| Cổng ra | `x.y ./ "Mô tả" ~Ghi chú~` | |
| Cổng phụ | `x.y \/ "Mô tả" ~Ghi chú~` | Mô tả nên ngắn gọn 1-2 ký tự (0-9 hoặc a-Z). |
| Điểm nối | `x.y () "Mô tả" ~Ghi chú~` | Mô tả nên ngắn gọn 1-2 ký tự (0-9 hoặc a-Z). |

- Mô tả: văn bản Mô tả của Nút. Nếu mô tả chứa ký tự cú pháp, phải bọc trong `"..."`.
- ~Ghi chú~: văn bản ghi chú của Nút.

### 4.3 Quy tắc

- **QT.4.1:** Mọi Nút đều phải có ID, kể cả khi Giai đoạn chỉ có một Nút.
- **QT.4.2:** Trong cùng một Giai đoạn, các Nút được đánh số y = 1, 2, 3, ... theo thứ tự thực hiện mặc định.
- **QT.4.3:** Không được phép có hai Nút cùng ID trong một tài liệu.
- **QT.4.4:** ID phải luôn được viết ở đầu một dòng, không dính liền với bất kỳ ký tự nào khác.
- **QT.4.5:** Các thành phần của một Nút phải xuất hiện theo đúng thứ tự sau:

 1. ID (`x.y`): bắt buộc, đứng đầu.
 2. Ký hiệu loại Nút: ngay sau ID, nếu có.
 3. Mô tả: văn bản mô tả của Nút, nếu có.
 4. Ghi chú: nếu có, đứng sau Mô tả và trước Đường luồng.
 5. Đường luồng (`'Nhãn' -> Đích`): nếu có, đứng cuối cùng.

- **QT.4.6:** Mô tả, Khối ghi chú (`~...~`) và Đường luồng (`->`) đứng sau ID của Nút thì dù nằm cùng dòng hoặc ở dòng riêng bên dưới đều gắn với Nút đó. Điều này có nghĩa là một Nút có thể được viết trải ra nhiều dòng.

### 4.4 Ví dụ

Ví dụ 1 - Giai đoạn có một Nút (vẫn phải đánh số 1.1):

```
    @1 G1
    1.1 Họp giao ban
```

Ví dụ 2 - Sai (thiếu ID):

```
    @1 G1
    Họp giao ban

    Giải thích: Không hợp lệ. Mọi Nút đều phải có ID.
```

Ví dụ 3 - Một nút được viết trải ra nhiều dòng:

```
    2.1 Nhận đơn hàng
    ~Vào: Phiếu yêu cầu từ khách~
    -> 2.2

    Tương đương với:
    2.1 Nhận đơn hàng ~Vào: Phiếu yêu cầu từ khách~ -> 2.2
```

### 4.5 Cách vẽ sơ đồ

- Mỗi Nút `x.y` được vẽ theo hình dạng tương ứng với loại của nó: hình chữ nhật (Tác vụ), hình bình hành (Ngõ), hình chữ nhật cạnh gạch đôi (Quy trình con), hình thoi (Quyết định), hình elip (Cổng vào/ra), hình ngũ giác ngược (Cổng phụ), hình tròn nhỏ (Điểm nối). Mô tả được đặt bên trong hình vẽ tương ứng. Các Nút được nối với nhau bằng Đường luồng.
- Nội dung Mô tả được đặt bên trong khối.
- Các khối được nối với nhau bằng Mũi tên theo thứ tự mặc định (từ trên xuống dưới hoặc từ trái sang phải).

## PHẦN 5 - ĐƯỜNG LUỒNG (FLOWLINE)

### 5.1 Khái niệm

Đường luồng (flowline) biểu diễn hướng đi của quy trình từ một Nút này đến một Nút khác. Trong sơ đồ, Đường luồng được vẽ bằng Mũi tên. Ký hiệu của Đường luồng là `->` (dấu gạch ngang và dấu lớn hơn).

### 5.2 Cú pháp

```
    'Nhãn' -> Đích
```

**Trong đó:**

- 'Nhãn' là nhãn của Đường luồng. Hiếm dùng, thường bị bỏ qua. Chỉ bắt buộc ghi nhãn với các nhánh của Quyết định.
- `->` mũi tên Đường luồng.
- Đích: `x.y` (ID đầy đủ của một Nút khác) hoặc `./` (kết thúc quy trình tại đây).

### 5.3 Quy tắc

- **QT.5.1:** Đường luồng `->` có thể đặt ở ngay cuối dòng của Nút (viết cùng dòng) hoặc trên một dòng riêng. Nếu `->` đứng trên một dòng riêng, nó chỉ hợp lệ khi dòng ngay phía trên (bỏ qua dòng trống và Bình luận) là khai báo ID (x.y) của Nút. Khi đó `->` gắn với `x.y`.
- **QT.5.2:** Sau `->` bắt buộc phải là đích hợp lệ.
- **QT.5.3:** Luồng mặc định đi tuần tự từ `x.y` đến `x.(y+1)` mà không cần viết `->` ra. Tuy nhiên, Luồng mặc định bị dừng tại `x.y` trong hai trường hợp:

    1. `x.y` có `->` tường minh. Khi đó `x.y` chỉ đi đến các đích được khai báo tường minh;
    2. `x.(y+1)` là Cổng vào/ra, Cổng phụ hoặc Điểm nối. Khi đó luồng mặc định không tự chạy vào Nút, người viết phải khai báo tường minh `-> x.(y+1)` nếu muốn dẫn luồng vào đó.

- **QT.5.4:** Khi một Nút có bất kỳ đường luồng nào trỏ đến đích `0` (số không), toàn bộ luồng đi ra của Nút đó bị hủy, bao gồm các đường luồng trỏ đến đích khác được khai báo cùng lúc. Trên sơ đồ, Nút đó được vẽ bình thường nhưng không có mũi tên nào xuất phát từ nó. Quyết định không được sử dụng đích `0` ở bất kỳ nhánh nào.
- **QT.5.5:** Nội dung nhãn Đường luồng phải được bọc trong cặp dấu nháy đơn `'...'`.
- **QT.5.6:** `->` không nhãn và có nhãn có thể xen kẽ trong cùng một dòng.
- **QT.5.7:** Có thể sử dụng `__` thay cho `->` để tạo Đường nối. Đường nối có cùng ngữ nghĩa điều khiển luồng như Đường luồng và tham gia đầy đủ mọi kiểm tra logic (đích hợp lệ, gộp AND-join, phát hiện Nút không thể đến được, v.v.), nhưng trên sơ đồ được vẽ không có đầu mũi tên.

### 5.4 Ví dụ

Ví dụ 1 - Gắn nhãn cho Đường luồng giữa hai Nút:

```
    2.3 Duyệt hồ sơ 'Đã duyệt' -> 3.1
```

Ví dụ 2 - Gắn nhãn khi khai báo Đường luồng đến các nhánh:

```
    2.1 ? Kiểm tra
    'Không đạt' -> 2.2
    'Đạt' -> 2.3
```

Ví dụ 3 - Luồng tuần tự Cấp 1:

```
    2.1 A
    2.2 B
    2.3 C

    Giải thích: Luồng đi từ 2.1 -> 2.2 -> 2.3
```

Ví dụ 4 - Luồng bị ngắt bởi `->`:

```
    2.1 A
    2.2 B -> 3.5
    2.3 C

 Hoặc:
    2.1 A
    2.2 B 
    -> 3.5
    2.3 C

    Giải thích: Luồng đi từ 2.1 -> 2.2 -> 3.5. Nút 2.3 không được vẽ vì không có Mũi tên nào dẫn đến nó.
```

Ví dụ 5 - Tác vụ có đích đến là 0:

```
    1.1 Mua hàng
        -> 0
        -> 1.2

Hoặc
    1.1 Mua hàng -> 1.2, -> 0

    Giải thích: Khối 1.1 được vẽ trên sơ đồ, nhưng không có luồng nào đi ra, kể cả luồng đến 1.2
```

### 5.5 Cách vẽ sơ đồ

- Vẽ Mũi tên (Đường luồng) từ Nút nguồn đến Nút đích.
- Nếu Đường luồng có nhãn, ghi nội dung nhãn lên Mũi tên (thường đặt ở giữa hoặc cạnh Mũi tên).
- Nếu Mũi tên vượt qua ranh giới Làn (sang Vai trò khác), vẫn vẽ bình thường.
- Khi `->` đặt trên dòng riêng, về mặt sơ đồ không có gì thay đổi, vẫn vẽ Mũi tên từ Nút đó đến đích chỉ định.

## PHẦN 6 - MÔ TẢ

### 6.1 Khái niệm

Mô tả là nội dung chính của một Nút, ghi bên trong hình vẽ trên sơ đồ.

Ví dụ: nội dung bên trong hình chữ nhật của Tác vụ, bên trong hình thoi của Quyết định, bên trong hình elip của Cổng vào/ra.

### 6.2 Cú pháp

```
    Dạng một dòng:    "Mô tả"
    Dạng nhiều dòng:  "
                      Dòng 1
                      Dòng 2
                      "
```

### 6.3 Quy tắc

- **QT.6.1:** Không bắt buộc dùng dấu nháy kép (") để bọc nếu Mô tả chỉ gồm một dòng, không chứa ký tự cú pháp, được viết ngay sau và trên cùng dòng với ID. Tuy nhiên, có thể sử dụng để nội dung được chặt chẽ.
- **QT.6.2:** Bắt buộc phải dùng `"..."` trong các trường hợp sau:
  - Mô tả có nhiều dòng;
  - Mô tả được viết xuống dòng riêng (ID ở dòng trên, mô tả ở dòng dưới);
  - Mô tả chứa bất kỳ ký tự hoặc chuỗi nào trùng với ký hiệu cú pháp: (`->`, `./`, `#`, `,`, `'`, `~`);
- **QT.6.3:** Khi gặp `"` mở đầu, dấu `"` đóng là ký tự `"` cuối cùng trong cùng dòng hoặc khối nhiều dòng. Mọi nội dung nằm giữa hai dấu đó đều là văn bản thuần túy, kể cả các dấu `"` ở giữa.
- **QT.6.4:** Đối với Mô tả nhiều dòng, dấu `"` mở và `"` đóng phải nằm trên một dòng riêng, không chứa ký tự khác.
- **QT.6.5:** Nội dung bên trong `"..."` được giữ nguyên toàn bộ khoảng trắng và xuống dòng.
- **QT.6.6:** `@N` có thể xuất hiện bên Trong Mô tả của bất kì Nút nào như một ký hiệu tham chiếu. Khi vẽ sơ đồ, mỗi @N trong Mô tả sẽ được thay thế tự động bằng tên viết tắt của Vai trò đó (nếu có khai báo tên viết tắt), hoặc bằng tên đầy đủ (nếu không có tên viết tắt).

### 6.4 Ví dụ

Ví dụ 1 - Không cần `"..."` (một dòng, không ký tự đặc biệt):

```
    1.1 Tạo đơn hàng mới
```

Ví dụ 2 - Bắt buộc dùng `"..."` (viết xuống dòng riêng):

```
    1.2
    "Kiểm tra tồn kho"
```

Ví dụ 3 - Bắt buộc dùng `"..."` (nhiều dòng):

```
    1.3
    "
    Phân tích dữ liệu khách hàng:
    - Tổng hợp từ CRM
    - Xác định nhóm rời bỏ
    "
```

Ví dụ 4 - Bắt buộc dùng `"..."` (nội dung chứa ký tự `->`):

```
    1.4 "Nếu đạt -> ghi nhận, nếu không -> báo cáo"
```

Ví dụ 5 - Dùng trong Quyết định (mô tả gồm nhiều dòng):

```
    2.2 ?
    "
    Kiểm tra điều kiện thanh toán:
    - Đã có chứng từ chưa?
    - Hạn mức tín dụng?
    "
```

Ví dụ 6 - Ghi ký hiệu Vai trò trực tiếp trong Mô tả:

```
    @1 Nhân viên kinh doanh (NVKD)
    @2 Trưởng phòng kinh doanh (TPKD)

    @1 G1
    1.1 Gửi @2 đề xuất phát triển khách hàng mới

    Khi vẽ sơ đồ, nội dung Tác vụ 1.1 hiển thị là: "Gửi TPKD đề xuất phát triển khách hàng mới"
```

### 6.5 Cách vẽ sơ đồ

- Toàn bộ nội dung Mô tả được đặt nguyên văn vào bên trong hình vẽ tương ứng.
- Xuống dòng và khoảng trắng được giữ nguyên.
- Không ghi dấu nháy kép (") vào trong sơ đồ.

## PHẦN 7 - GHI CHÚ (ANNOTATION)

### 7.1 Khái niệm

Ghi chú (annotation) là thông tin bổ sung gắn với một đối tượng nhưng không nằm bên trong hình vẽ của đối tượng đó. Trên sơ đồ, ghi chú được vẽ thành một khung riêng (thường là hình chữ nhật nét đứt) nối với đối tượng bằng đường nét đứt. Ghi chú không tham gia vào Luồng điều khiển.

### 7.2 Cú pháp

```
    Dạng một dòng:    ~Ghi chú~
    Dạng nhiều dòng:  ~
                      Dòng 1
                      Dòng 2
                      ~
```

### 7.3 Quy tắc

- **QT.7.1:** Ghi chú chỉ được đặt sau ID và Mô tả của một Nút, nghĩa là sau phần khai báo loại và nội dung Mô tả, trên cùng dòng hoặc trên các dòng ngay bên dưới, nhưng phải xuất hiện trước bất kỳ `->` nào của Nút đó.
- **QT.7.2:** Khi gặp `~` mở đầu, dấu `~` đóng là ký tự `~` cuối cùng trên cùng dòng hoặc khối nhiều dòng. Mọi nội dung nằm giữa hai dấu đó đều là văn bản thuần túy, kể cả các dấu `~` ở giữa.
- **QT.7.3:** Đối với Ghi chú nhiều dòng, dấu `~` mở và `~` đóng phải nằm trên một dòng riêng, không chứa ký tự khác.
- **QT.7.4:** `@N` có thể xuất hiện bên Trong Ghi chú của bất kì Nút nào như một ký hiệu tham chiếu. Khi vẽ sơ đồ, mỗi @N trong Ghi chú sẽ được thay thế tự động bằng tên viết tắt của Vai trò đó (nếu có khai báo tên viết tắt), hoặc bằng tên đầy đủ (nếu không có tên viết tắt).

### 7.4 Ví dụ

Ví dụ 1 - Ghi chú của Tác vụ:

```
    1.3 Kiểm tra chứng từ
    ~
    Bao gồm:
    - Hợp đồng
    - Phụ lục hợp đồng
    ~
```

Ví dụ 2 - Tác vụ có Mô tả và Ghi chú nhiều dòng:

```
    1.3 
    "
    - Kiểm tra chứng từ
    - Kiểm tra chữ ký
    "
    ~
    Bao gồm:
    - Hợp đồng
    - Phụ lục hợp đồng
    ~
```

Ví dụ 3 - Ghi chú của Quyết định:

```
 2.1 ? Duyệt đơn hàng?
  ~
  Từ chối khi:
  - Thiếu chứng từ
  - Sai thông tin khách hàng
  ~
  'Từ chối' -> ./
  'Đồng ý' -> 2.2
```

Ví dụ 4 - Không hợp lệ:

```
 2.1 Kiểm tra hồ sơ -> 2.2
 ~Bao gồm kiểm tra chữ ký~
 
 Giải thích: Ghi chú đặt sau -> của Nút 2.1 là không hợp lệ.
```

Ví dụ 5 - Tham chiếu @N trong Ghi chú:

```
 @1 Nhân viên kinh doanh (NVKD) 
 @2 Trưởng phòng kinh doanh (TPKD) 
 
 @1 G1 
 1.1 Kiểm tra hồ sơ ~Nếu thiếu thông tin, liên hệ @2 để xác nhận~ 
 
 Khi vẽ sơ đồ, nội dung Ghi chú hiển thị là: "Nếu thiếu thông tin, liên hệ TPKD để xác nhận"
```

Ví dụ 6 - Dấu ~ bên trong ~...~:

```
    1.1 Nhận yêu cầu
    ~
    Điểm ~đầu~
    và ~cuối~
    ~

    Giải thích: Ghi chú là toàn bộ nội dung giữa ~ mở đầu và ~ cuối cùng của khối.
```

### 7.5 Cách vẽ sơ đồ

- Vẽ một hình ghi chú (hình chữ nhật, thường có một cạnh gấp khúc hoặc khung đứt nét).
- Ghi toàn bộ nội dung bên trong `~...~` vào hình ghi chú đó.
- Vẽ đường nét đứt từ hình ghi chú đến đối tượng mà nó gắn kết.

## PHẦN 8 - CỔNG VÀO/CỔNG RA

### 8.1 Khái niệm

Cổng vào là điểm bắt đầu của quy trình, Cổng ra là điểm kết thúc. Chúng không mô tả một công việc cụ thể mà chỉ đánh dấu ranh giới của quy trình: nơi luồng đi vào và nơi luồng thoát ra. Một quy trình có thể có nhiều Cổng vào và nhiều Cổng ra, ví dụ khi quy trình có thể được khởi động từ nhiều tình huống khác nhau, hoặc kết thúc theo nhiều kết quả khác nhau.

Trên sơ đồ cả hai đều được vẽ bằng hình elip. Điểm khác biệt với Tác vụ là Cổng vào không có mũi tên đi vào và Cổng ra không có mũi tên đi ra.

### 8.2 Quy tắc

- **QT.8.1:** Cổng vào được ký hiệu bằng `\.`, Cổng ra được ký hiệu bằng `./`. Trên sơ đồ, cả hai đều được vẽ bằng hình elip.
- **QT.8.2:** Muốn hiển thị Cổng vào trên sơ đồ, phải khai báo tường minh Cổng vào (ký hiệu `\.`) hoặc Cổng phụ đi vào (ký hiệu `\/`) tại Nút 1.1 của Giai đoạn G1. Không có Cổng vào nào được tạo tự động.
- **QT.8.3:** Sau Cổng vào hoặc Cổng phụ đi vào đầu tiên tại 1.1, có thể khai báo thêm các Cổng vào/Cổng phụ ở các ID hợp lệ khác; mỗi Cổng là một Nút riêng và phải tuân thủ quy tắc đánh số Nút.
- **QT.8.4:** Ký hiệu `./` dùng trong Đường luồng (`-> ./`) trỏ đến Cổng ra Mặc định, là một Nút ảo với ID `z`, không cần khai báo. Khi khai báo tường minh `x.y ./`, ID của Cổng ra đó là `x.y`, khác hoàn toàn với Cổng ra Mặc định `z`. Đây là hai Cổng ra riêng biệt, được vẽ thành hai hình elip riêng.
- **QT.8.5:** `./` có thể xuất hiện nhiều lần trong cùng một tài liệu. Tất cả các `-> ./` đều trỏ đến cùng một Cổng ra Mặc định `z`. Cổng ra Mặc định được vẽ trong Làn của Giai đoạn cuối cùng chứa `-> ./`; Mô tả của nó là `Kết thúc`.
- **QT.8.6:** Không có yêu cầu bắt buộc về loại Nút cuối cùng của quy trình. Nút cuối có thể là Cổng ra, Cổng phụ đi ra, hoặc bất kỳ loại Nút nào khác tùy người viết.
- **QT.8.7:** Khai báo tường minh Cổng ra chỉ cần thiết khi:
  - Có nhiều Cổng ra;
  - Cần tùy chỉnh làn hoặc Mô tả;
  - Cần thêm Ghi chú.

### 8.3 Khai báo Cổng vào

Cú pháp:

```
    x.y \. Mô tả ~Ghi chú~ 'Nhãn' -> Đích
```

**Trong đó:**

- x.y: mã định danh của Cổng vào, theo dạng `x.y`.
- `\.`: ký hiệu Cổng vào.
- Mô tả: bắt buộc khi khai báo tường minh. Sẽ ghi bên trong hình elip.
- ~Ghi chú~: không bắt buộc.
- 'Nhãn' `->` Đích: đường luồng đầu tiên từ Cổng. Không bắt buộc nếu Cổng vào là Nút 1.1 và luồng tuần tự mặc định dẫn sang 1.2.

Ví dụ 1:

```
    1.1 \. Start1 -> 1.3
    1.2 \. Start2 -> 1.4

    Giải thích: Sẽ vẽ 2 Cổng vào, Start1 dẫn đến 1.3, Start2 dẫn đến 1.4.
```

### 8.4 Khai báo Cổng ra

Cú pháp:

```
    x.y ./ Mô tả ~Ghi chú~
```

**Trong đó:**

- x.y: mã định danh của Cổng ra.
- `./`: ký hiệu Cổng ra.
- Mô tả: bắt buộc khi khai báo tường minh.
- ~Ghi chú~: không bắt buộc.

Cổng ra là đích cuối cùng, không có 'Nhãn' hoặc `->` trong khai báo.

Ví dụ 2:

```
    2.1 Lấy mẫu -> 3.5
    ...
    3.1 Gửi đơn -> 4.3
    ...
    3.5 ./ Hoàn tất nhánh A
    ...
    4.3 ./ Hủy đơn

    Từ 2.1 có đường luồng dẫn đến Cổng ra 3.5; từ 3.1 có đường luồng dẫn đến Cổng ra 4.3.
```

Ví dụ 3:

```
    @1 G1
    1.1 A -> 1.2, -> 2.1
    1.2 B -> 1.3
    1.3 ./ Kết thúc nhánh A        # Cổng ra x.y = 1.3, vẽ trong Làn của @1

    @2 G2
    2.1 C -> 2.2
    2.2 D -> ./                     # Trỏ đến Cổng ra Mặc định (ID ảo z), vẽ trong Làn của @2

    Giải thích: Nút 1.3 là một Cổng ra tường minh với ID = 1.3, vẽ trong Làn @1 vì thuộc G1.
    Cổng ra Mặc định (z) được vẽ trong Làn @2 vì G2 là Giai đoạn cuối chứa -> ./
    Đây là hai hình elip riêng biệt trên sơ đồ.
```

### 8.5 Cách vẽ sơ đồ

Đối với Cổng vào (`\.` hoặc khai báo tường minh):

- Vẽ hình elip. Bên trong ghi Mô tả.
- Vẽ Mũi tên từ hình elip đến Nút đầu tiên.
- Hình elip nằm trong Làn của Vai trò tương ứng.
- Nếu có nhiều Cổng vào, mỗi Cổng có một hình elip riêng, độc lập.

Đối với Cổng ra (`./` hoặc khai báo tường minh):

- Vẽ hình elip. Bên trong ghi Mô tả.
- Vẽ Mũi tên từ Nút hoặc nhánh cuối cùng trỏ vào hình elip.
- Hình elip nằm trong Làn tương ứng.
- Nếu có nhiều Cổng ra, mỗi Cổng có một hình elip riêng.

## PHẦN 9 - CỔNG PHỤ

### 9.1 Khái niệm

Cổng phụ thay thế cho Đường luồng nối sang trang sơ đồ khác. Có hai dạng:

- **Cổng phụ đi vào:** là Cổng phụ mà từ nó có Đường luồng `->` đi ra, và không có Đường luồng nào trỏ vào nó. Dùng để thay thế Cổng vào trên trang tiếp theo.
- **Cổng phụ đi ra:** là Cổng phụ mà có Đường luồng `->` trỏ vào nó, và từ nó không có Đường luồng nào đi ra. Dùng để thay thế Cổng ra ở cuối trang.

Cho phép có nhiều Cổng phụ đi vào và nhiều Cổng phụ đi ra. Không có Cổng phụ nào vừa nhận luồng vào vừa có luồng đi ra; nếu có, đây là lỗi. Mọi luồng đến hoặc đi từ Cổng phụ đều phải khai báo tường minh bằng `->`.

Trên sơ đồ: hình ngũ giác ngược với cạnh nhọn quay xuống dưới. Ghi Mô tả bên trong.

### 9.2 Cú pháp

```
Cổng phụ đi vào:

    x.y \/ Mô tả ~Ghi chú~ 'Nhãn' -> Đích

Cổng phụ đi ra:

    x.y \/ Mô tả ~Ghi chú~
```

**Trong đó:**

- x.y: mã định danh của Cổng phụ.
- `\/`: ký hiệu Cổng phụ.
- Mô tả: nhãn ngắn gọn 1-2 ký tự (chữ số 0-9 hoặc chữ cái a-Z), dùng để ghép cặp Cổng phụ đi vào và đầu ra tương ứng với nhau trên các trang sơ đồ khác nhau. Bắt buộc phải có.
- ~Ghi chú~ và 'Nhãn': không bắt buộc.
- Loại Cổng phụ (đầu vào hay đầu ra) được xác định tự động dựa trên việc có hay không có `->` trong khai báo của nó và trong các khai báo khác trỏ vào nó.

### 9.3 Quy tắc

- **QT.9.1:** Không bắt buộc Mô tả của các Cổng phụ cùng loại trong một quy trình phải khác nhau.
- **QT.9.2:** Mô tả của Cổng phụ đi vào bắt buộc phải khác với Mô tả của Cổng phụ đi ra trong cùng một quy trình.
- **QT.9.3:** Mô tả của Cổng phụ và Mô tả của Điểm nối thuộc hai không gian độc lập: một Cổng phụ và một Điểm nối có cùng Mô tả vẫn là hai đối tượng hoàn toàn riêng biệt, không liên quan đến nhau.
- **QT.9.4:** Mọi luồng đi vào Cổng phụ đi ra và mọi luồng đi ra từ Cổng phụ đi vào đều phải khai báo tường minh bằng `->`. Không có luồng tuần tự mặc định vào hoặc ra từ Cổng phụ.

### 9.4 Ví dụ

Ví dụ 1:

```
    1.1 \/ A -> 1.3
    1.2 \/ B -> 1.4
```

Ví dụ 2:

```
    2.1 Lấy mẫu -> 3.5
    ...
    3.1 Gửi đơn -> 4.3
    ...
    3.5 \/ A
    ...
    4.3 \/ B
```

### 9.5 Cách vẽ sơ đồ

- Dạng đầu ra: vẽ Mũi tên từ Nút cuối vào hình ngũ giác ngược, đặt ở cuối trang, trong Làn tương ứng.
- Dạng đầu vào: vẽ hình ngũ giác ngược ở đầu trang tiếp theo, trong Làn tương ứng, Mũi tên từ đó dẫn đến Nút đầu tiên.

## PHẦN 10 - HÀNH ĐỘNG (TÁC VỤ, NGÕ, QUY TRÌNH CON)

### 10.1 Khái niệm

Tác vụ, Ngõ và Quy trình con là ba loại Hành động. Mọi quy tắc về luồng đi đều giống nhau. Điểm duy nhất khác nhau là ký hiệu khai báo và hình vẽ trên sơ đồ:

| **Loại**          | **Cú pháp khai báo**  | **Hình vẽ**                                       |
| ---           | ---               | ---                                           |
| Tác vụ        | x.y Mô tả          | Hình chữ nhật                                 |
| Ngõ           | x.y `/` Mô tả      | Hình bình hành                                |
| Quy trình con | x.y `!` Mô tả      | Hình chữ nhật có hai cạnh thẳng đứng gạch đôi |

### 10.2 Cú pháp

```
    x.y Ký_hiệu Mô tả ~Ghi chú~ 'Nhãn' -> Đích
```

**Trong đó:**

- x.y: ID của Hành động.
- Ký_hiệu: để phân biệt giữa Tác vụ, Ngõ và Quy trình con.
- Mô tả: văn bản ghi bên trong hình vẽ.
- ~Ghi chú~: ghi chú của Hành động, không bắt buộc.
- 'Nhãn': nhãn của đường luồng, không bắt buộc.
- Đích: điểm đến.

### 10.3 Quy tắc

- **QT.10.1:** Ngõ vào và Ngõ ra không cần khai báo riêng bằng cú pháp khác nhau. Ngữ nghĩa vào hay ra được suy ra từ nội dung Mô tả hoặc Ghi chú.
- **QT.10.2:** Trong Mô tả của Quy trình con chỉ cần ghi tên hoặc mã để tham chiếu.

### 10.4 Ví dụ

Ví dụ 1 - Hành động có nhiều đường luồng đi ra (mỗi luồng có nhãn):

```
    1.1 Lấy mẫu 'A' -> 2.1, 'B' -> 2.2
```

Ví dụ 2 - Hành động có một đường luồng đi ra:

```
    2.1 Thử mẫu -> 3.1
```

Ví dụ 3 - Hành động phân tách thành 3 nhánh, nhánh thứ 3 có nhãn:

```
    3.1 A ~Note~
    -> 3.2 B
    -> 3.3 C
    'X' -> 3.4 D

    Giải thích: Từ Tác vụ 3.1 phân tách thành 3 nhánh: 3.2, 3.3 và 3.4. Đường luồng sang nhánh 3.4 có nhãn ''X''.
```

Ví dụ 4 - Ngõ có Đường luồng:

```
    2.1 / Nhận yêu cầu qua email -> 2.2
```

Ví dụ 5 - Ngõ có ghi chú:

```
    2.1 / Nhận báo cáo tài chính
    ~Định dạng: Excel. Thời hạn nộp: trước 17h ngày 5 hàng tháng.~
```

Ví dụ 6 - Quy trình con theo mã:

```
    2.3 ! QT.KD.01 -> 3.1
```

### 10.5 Cách vẽ sơ đồ

- Tác vụ: hình chữ nhật. Ngõ: hình bình hành. Quy trình con: hình chữ nhật có hai đường kẻ dọc sát hai cạnh trái và phải bên trong.
- Ghi Mô tả bên trong hình. Không ghi ký hiệu `/`, `!` vào trong sơ đồ.
- Các quy tắc vẽ còn lại (Đường luồng vào/ra, vị trí trong Làn, ghi chú bằng nét đứt) hoàn toàn giống nhau giữa ba loại.

## PHẦN 11 - QUYẾT ĐỊNH (DECISION)

### 11.1 Khái niệm

Quyết định (decision) là điểm rẽ nhánh trong quy trình. Tại đây, Luồng chọn đúng một nhánh duy nhất trong số các nhánh được liệt kê (XOR - exclusive choice). Trong sơ đồ, Quyết định được biểu diễn bằng hình thoi (decision diamond).

### 11.2 Cú pháp

```
    Cú pháp 1 dòng:
    x.y ? Mô tả ~Ghi chú~ 'Nhãn' -> Đích 1, 'Nhãn' -> Đích 2, 'Nhãn' -> Đích 3, ...

    Cú pháp nhiều dòng:
    x.y ? Mô tả ~Ghi chú~
    'Nhãn' -> ID đích 1,
    'Nhãn' -> ID đích 2,
    'Nhãn' -> ID đích 3
```

Cú pháp khai báo các nhánh của Quyết định hoàn toàn giống với cú pháp Phân tách (xem Phần 13): các nhánh ngăn cách nhau bằng dấu `,`. Sự khác biệt giữa Quyết định (XOR) và Phân tách (AND-fork) chỉ được xác định bằng ký hiệu loại của Nút nguồn: `?` là Quyết định, không có `?` là Phân tách.

**Trong đó:**

- x.y: Mã định danh của Quyết định.
- `?`: dấu hỏi, bắt buộc, đặt ngay sau ID (có thể có khoảng trắng).
- Mô tả: văn bản ghi bên trong hình thoi. Tính từ sau dấu `?` cho đến hết dòng, hoặc cho đến khi gặp khối ghi chú `~...~`. Không bắt buộc.
- ~Ghi chú~: ghi chú của Quyết định, không bắt buộc.
- 'Nhãn': nhãn của đường luồng, bắt buộc với mọi nhánh của Quyết định.
- Đích: đích của nhánh đó.

### 11.3 Quy tắc

- **QT.11.1:** Một Quyết định phải có ít nhất hai nhánh.
- **QT.11.2:** Mọi nhánh của Quyết định đều bắt buộc phải có nhãn. Nhãn của các nhánh không được trùng nhau.
- **QT.11.3:** Các nhánh của Quyết định được ngăn cách với nhau bằng dấu `,`. Dấu `,` có thể đứng ngay sau đích hoặc cách một khoảng trắng. Tại nhánh cuối cùng, dấu `,` không bắt buộc nhưng nếu có cũng không bị coi là lỗi.
- **QT.11.4:** Quyết định có thể lồng nhau: một nhánh có thể trỏ đến một Quyết định khác bằng cách trỏ đến ID của Quyết định đó.
- **QT.11.5:** Trong cùng một Quyết định, không được có hai nhánh cùng trỏ đến một đích. Mỗi nhánh phải trỏ đến một đích riêng biệt.
- **QT.11.6:** Không được dùng đích `0` trong bất kỳ nhánh nào của Quyết định.

### 11.4 Ví dụ

Ví dụ 1 - Quyết định nhị phân, viết nhiều dòng:

```
    2.1 ? Đơn hàng hợp lệ?
        'Không' -> ./,
        'Có' -> 2.2
```

Ví dụ 2 - Quyết định nhị phân, viết 1 dòng:

```
    2.1 ? Đơn hàng hợp lệ? 'Không' -> ./, 'Có' -> 2.2
```

Ví dụ 3 - Quyết định ba nhánh:

```
    2.1 ? Kiểm tra
        'Đạt' -> 2.3,
        'Không đạt' -> 2.2,
        'Gần đạt' -> 2.4
```

Ví dụ 4 - Quyết định lồng nhau:

```
    2.1 ? Kiểm tra
        'Không đạt' -> 2.2,
        'Đạt' -> 2.3
    2.2 ? Tại sao không?
        'Thiếu X' -> 2.5,
        'Thiếu Y' -> 2.6

    Giải thích: Từ hình thoi ''Kiểm tra'' có 2 đường luồng đi ra. Đường luồng ''Không đạt'' dẫn đến hình thoi ''Tại sao không?''. Đây là cách tạo Quyết định lồng nhau.
```

Ví dụ 5 - Quyết định với ghi chú định nghĩa nhãn:

```
    3.1 ? Phân loại khách hàng
    ~
    VIP: Khách hàng có doanh thu trên 500 triệu/năm
    Thường: Doanh thu dưới 500 triệu
    Mới: Chưa có lịch sử giao dịch
    ~
        'VIP' -> 3.1,
        'Thường' -> 4.1,
        'Mới' -> 4.2
```

Ví dụ 6 - Quyết định với nội dung nhiều dòng:

```
    2.2 ?
    "
    Điều kiện phê duyệt:
    - Số tiền dưới 100 triệu
    - Khách hàng có hợp đồng gốc
    "
        'Không đạt' -> 1./,
        'Đạt' -> 2.3
```

Ví dụ 7 - Không hợp lệ (hai nhánh cùng đích):

```
    2.2 ? Kiểm tra
        'Đạt' -> 2.3,
        'Không' -> 2.4,
        'Gần đạt' -> 2.3

    Giải thích: Không hợp lệ vì hai nhánh 'Đạt' và 'Gần đạt' cùng trỏ đến 2.3. Cần dùng hai đích khác nhau hoặc gộp hai điều kiện thành một nhãn.
```

### 11.5 Cách vẽ sơ đồ

Vẽ hình thoi (decision diamond) với Mô tả (nếu có) ghi bên trong.

Từ hình thoi, vẽ một Mũi tên cho mỗi nhánh. Ghi nhãn Mũi tên (nếu có). Mỗi Mũi tên dẫn đến đích tương ứng.

Nếu có `~...~` gắn với Quyết định, vẽ hình ghi chú nối bằng nét đứt đến hình thoi.

## PHẦN 12 - ĐIỂM NỐI

### 12.1 Khái niệm

Điểm nối dùng để thay thế Đường luồng dài trên cùng trang. Thay vì các Đường luồng phải dẫn đến một Nút ở xa, có thể cắt ngang những Đường luồng khác gây rối cho sơ đồ, thì chúng có thể chạy đến Điểm thu gần nhất, sau đó đi ra từ Điểm phát nằm bên cạnh Nút đích.

Trên sơ đồ: hình tròn nhỏ, ghi Mô tả bên trong.

### 12.2 Cú pháp

```
    x.y () Mô tả ~Ghi chú~ 'Nhãn' -> Đích
```

**Trong đó:**

- x.y: ID của Điểm nối.
- `()`: ký hiệu Điểm nối.
- Mô tả: ngắn gọn 1-2 ký tự (chữ số 0-9 hoặc chữ cái a-Z), dùng để ghép cặp các Điểm thu và Điểm phát của Điểm nối. Bắt buộc phải có.
- `~Ghi chú~`, `'Nhãn'`, `-> Đích`: các thành phần không bắt buộc. Nếu có `->` thì đây là Điểm phát; nếu không có `->` thì là Điểm thu.

### 12.3 Quy tắc

- **QT.12.1:** Không bắt buộc các Mô tả của Điểm nối trong cùng một quy trình phải khác nhau.
- **QT.12.2:** Mỗi nhóm Điểm nối cùng Mô tả chỉ được có một Điểm phát.
- **QT.12.3:** Giữa các Điểm nối cùng Mô tả không được có Đường luồng dẫn đến nhau.
- **QT.12.4:** Trong một nhóm Điểm nối có cùng Mô tả, tất cả các Điểm thu phải hoàn thành trước khi Điểm phát kích hoạt (AND-join).
- **QT.12.5:** Điểm thu và Điểm phát độc lập về mặt sơ đồ. Mối liên hệ giữa chúng được xác định ngầm thông qua Đường luồng: Điểm thu chỉ có luồng đi vào, Điểm phát chỉ có luồng đi ra.
- **QT.12.6:** Luồng tuần tự mặc định không áp dụng cho Điểm nối theo cả hai chiều. Mọi luồng đi vào Điểm thu và mọi luồng đi ra từ Điểm phát đều phải khai báo tường minh bằng `->`.

### 12.4 Ví dụ

Ví dụ 1:

```
    1.1 X -> 2.1
    2.1 () A
    ...
    4.1 () A -> 5.2

    Giải thích: 1.1 có Đường luồng đến Điểm thu ''A'' tại làn 2. Điểm phát ''A'' tại làn 4 có Đường luồng dẫn đến 5.2. Trên sơ đồ không vẽ Đường luồng từ Điểm thu đến Điểm phát. Luồng ''biến mất'' tại Điểm thu và ''xuất hiện lại'' tại Điểm phát nhờ cùng Mô tả ''A''.
```

Ví dụ 2 - Nhiều Điểm thu cùng Mô tả khác Làn:

```
    2.1 () A
    ...
    3.1 () A
    ...
    4.1 () A -> 5.2

    Giải thích: Hai Điểm thu ''A'' - một tại làn 2, một tại làn 3 - là hai hình tròn riêng biệt trên sơ đồ. Điểm phát ''A'' tại làn 4 dẫn đến 5.2. Luồng chỉ tiếp tục sang 5.2 khi cả hai Điểm thu hoàn thành (AND-join).
```

Ví dụ 3 - Không khai báo, không có luồng vào hoặc ra:

```
    1.1 () A
    1.2 B

    Giải thích: Không có luồng nào từ 1.1 đến 1.2
```

### 12.5 Cách vẽ sơ đồ

- Điểm nối được khai báo ở Làn nào thì vẽ hình tròn tại Làn đó.
- Hai Điểm nối cùng Mô tả nhưng khác Làn được vẽ thành hai hình tròn riêng biệt, mỗi hình tròn nằm trong Làn tương ứng.
- Khi có nhiều Đường luồng cùng trỏ vào một Điểm thu: vẽ thanh ngang hội tụ (join bar), các Đường luồng nhập vào thanh đó, rồi từ thanh có một Đường luồng duy nhất đi vào hình tròn Điểm nối.
- Nếu Điểm nối không có `->` đi ra: đây là Điểm thu, chỉ vẽ hình tròn với các Đường luồng đi vào. Không vẽ bất kỳ đường luồng nào đi ra từ hình tròn này.
- Nếu Điểm nối có `->` đi ra: đây là Điểm phát, vẽ một Đường luồng đến đích.

## PHẦN 13 - PHÂN TÁCH (FORK/PARALLEL)

### 13.1 Khái niệm

Phân tách cho phép từ một Nút nguồn tạo ra nhiều Luồng thực hiện đồng thời độc lập. Nút nguồn có thể là Hành động, Cổng vào, Cổng phụ đi vào hoặc Điểm phát.

### 13.2 Cú pháp

```
    Cú pháp 1 dòng:
    x.y Ký_hiệu Mô tả ~Ghi chú~ 'Nhãn' -> Đích 1, 'Nhãn' -> Đích 2, 'Nhãn' -> Đích 3

    Cú pháp nhiều dòng:
    x.y Ký_hiệu Mô tả ~Ghi chú~
    'Nhãn' -> Đích 1,
    'Nhãn' -> Đích 2,
    'Nhãn' -> Đích 3
```

### 13.3 Quy tắc

- **QT.13.1:** Phân tách chỉ có thể xuất phát từ một Hành động, Cổng vào, Cổng phụ đi vào hoặc Điểm nối Điểm phát. Quyết định không thể là gốc của phân tách.
- **QT.13.2:** Từ một Nút nguồn, không được có hai nhánh phân tách cùng trỏ đến một đích. Mỗi nhánh phải dẫn đến một đích riêng biệt.
- **QT.13.3:** Các luồng trong cùng một phân tách được phân cách nhau bằng dấu phẩy (`,`). Dấu `,` có thể đứng ngay sau đích hoặc cách một khoảng trắng. Tại đích cuối cùng, dấu `,` không bắt buộc nhưng nếu có cũng không bị coi là lỗi cú pháp.
- **QT.13.4:** Khi viết dạng nhiều dòng, không bắt buộc có dấu `,` ở cuối mỗi dòng.
- **QT.13.5:** Có thể dùng thụt lề để trình bày các Nút nằm trong cùng một nhánh, giúp sơ đồ dễ đọc hơn nhưng không ảnh hưởng đến cú pháp.

### 13.4 Ví dụ

Ví dụ 1:

```
    2.1 Sau khi nhận yêu cầu -> 2.2, -> 3.1
    2.2 Gửi email xác nhận cho khách -> ./
    3.1 Gửi yêu cầu xuất kho

    Giải thích: Từ 2.1 có một Mũi tên đến thanh fork, từ thanh fork có hai Mũi tên đến 2.2 và 3.1. 
```

Ví dụ 2:

```
    2.1 Xử lý đồng thời ~Thời hạn 3h~
    'Kho' -> 2.2,
    'Kế toán' -> 3.1,
    'Giao vận' -> 3.2
    2.2 Lấy hàng từ kho
    3.1 Lập hóa đơn
    3.2 Chuẩn bị vận chuyển

    Giải thích: Từ 2.1 phân tách thành ba nhánh qua thanh fork với nhãn tương ứng.
```

Ví dụ 5 - Sử dụng thụt lề để dễ đọc hơn:

```
    2.1 Việc 1
        -> 2.2,
        -> 2.5,
        -> 2.7
        2.2 Việc 1a1
            2.3 Việc 1a2
                2.4 Việc 1a3
        2.5 Việc 1b1
            2.6 Việc 1b2
        2.7 Việc 1c1
            2.8 Việc 1c2

    Giải thích: Các Nút đứng phía sau một nhánh được thụt lề so với Nút phía trước, giúp dễ quan sát và hình dung về các nhánh hơn. Thụt lề không có giá trị cú pháp.
```

### 13.5 Cách vẽ sơ đồ

Khi một Nút nguồn phân tách ra nhiều Đường luồng, người vẽ có thể chọn một trong hai cách tùy theo bố cục sơ đồ:

1. Vẽ thanh fork (thanh ngang), từ Nút nguồn có một Mũi tên đến thanh fork, từ thanh fork có nhiều Mũi tên đi ra đến từng đích;
2. Vẽ nhiều Mũi tên xuất phát thẳng từ Nút nguồn đến từng đích, không có thanh fork.

Cả hai cách đều có ngữ nghĩa như nhau.

## PHẦN 14 - GỘP (JOIN)

### 14.1 Khái niệm

Gộp là nơi các Luồng hội tụ lại trước khi tiếp tục. Ngữ nghĩa luôn là AND-join: tất cả các nhánh phải hoàn thành trước khi đích được kích hoạt. Gộp cho phép hội tụ từ bất kỳ nguồn nào, không phân biệt cùng gốc hay khác gốc.

### 14.2 Cú pháp

Dùng -> trỏ từ cuối mỗi nhánh về cùng một Đích.

```
    x.y Ký_hiệu Mô tả -> Đích
    x.z Ký_hiệu Mô tả -> Đích
```

Chú ý: Có thể thêm Ghi chú, Nhãn đường luồng nếu cần.

### 14.3 Quy tắc

- **QT.14.1:** Khi nhiều Đường luồng cùng trỏ vào một đích, đích đó chỉ được kích hoạt khi tất cả các nhánh đã hoàn thành (AND-join).
- **QT.14.2:** Không bắt buộc phải gộp các luồng song song. Các nhánh có thể tự đi đến Cổng ra riêng hoặc các đích khác nhau.
- **QT.14.3:** Khi một nhánh trong Phân tách song song kết thúc bằng `-> ./` (Cổng ra), chỉ nhánh đó được kết thúc. Các nhánh song song còn lại tiếp tục thực hiện độc lập và không bị ảnh hưởng. Toàn bộ quy trình chỉ kết thúc khi tất cả các nhánh song song đang hoạt động đều đã đến Cổng ra hoặc hội tụ tại một đích (AND-join).

### 14.4 Ví dụ

Ví dụ 1:

```
    2.1 Xử lý đơn hàng -> 2.2, -> 3.1
    2.2 Gửi email xác nhận -> 3.2
    3.1 Gửi yêu cầu xuất kho -> 3.2

    Giải thích: Cả hai nhánh 2.2 và 3.1 đều trỏ về 3.2. 3.2 chỉ bắt đầu khi cả hai nhánh hoàn thành.
```

Ví dụ 2:

```
    2.2 Lấy hàng từ kho -> 5.1
    3.1 Lập hóa đơn -> 5.1
    4.1 Chuẩn bị vận chuyển -> 5.1
    5.1 Hoàn tất đơn hàng

    Giải thích: 2.2, 3.1, 4.1 thuộc ba Giai đoạn khác nhau nhưng đều trỏ về 5.1. 5.1 chỉ bắt đầu khi cả ba nhánh hoàn thành.
```

Ví dụ 3 - Phân tán không gộp:

```
    2.1 Sau khi nhận yêu cầu -> 2.2, -> 3.1
    2.2 Gửi email xác nhận -> ./
    3.1 Gửi yêu cầu xuất kho -> 4.1

    Giải thích: Nhánh 2.2 kết thúc tại Cổng ra, nhánh 3.1 tiếp tục đến 4.1. Hai nhánh không gộp với nhau. Toàn bộ quy trình chỉ kết thúc khi nhánh 3.1 và các nhánh tiếp theo từ 4.1 cũng đã hoàn thành.
```

### 14.5 Cách vẽ sơ đồ

Khi nhiều Đường luồng hội tụ về cùng một đích, người vẽ có thể chọn một trong hai cách tùy theo bố cục sơ đồ:

1. Vẽ thanh join (thanh ngang), các Mũi tên từ mọi nhánh hội tụ vào thanh join, từ thanh join có một Mũi tên duy nhất đến đích;
2. Vẽ các Mũi tên trỏ thẳng vào đích, không có thanh join.

Cả hai cách đều có ngữ nghĩa AND-join như nhau.

## PHẦN 15 - BÌNH LUẬN TÀI LIỆU (COMMENT)

### 15.1 Khái niệm

Bình luận là ghi chú dành riêng cho người viết hoặc người đọc tài liệu quy trình, không liên quan đến quy trình. Bình luận không có giá trị cú pháp, không hiển thị trên sơ đồ và bị bỏ qua hoàn toàn khi xử lý tài liệu.

Điểm khác biệt so với Ghi chú (`~...~`): Ghi chú là một phần của quy trình, gắn với một đối tượng cụ thể và hiển thị trên sơ đồ. Bình luận hoàn toàn nằm ngoài quy trình, chỉ tồn tại trong văn bản.

### 15.2 Cú pháp

Mọi dòng bắt đầu bằng dấu `#` hoặc có `#` xuất hiện ở giữa/cuối dòng đều được coi là bình luận. Bình luận bị cắt từ vị trí `#` đến cuối dòng đó, bất kể trước `#` có ký tự khác hay khoảng trắng.

Ví dụ:

```
    # Đây là một dòng bình luận hoàn toàn
    2.1 Nhận đơn hàng # Đây là bình luận cuối dòng
```

### 15.3 Quy tắc

- **QT.15.1:** `#` có thể nằm ở bất cứ đâu trong tài liệu: ở dòng riêng, hoặc cuối một dòng ghi chép quy trình.
- **QT.15.2:** Mọi ký tự nằm sau `#` (đến cuối dòng) đều là văn bản thuần túy, không có giá trị cú pháp.
- **QT.15.3:** Bình luận không được thể hiện trong sơ đồ dưới bất kỳ hình thức nào.
- **QT.15.4:** Ký tự `#` xuất hiện bên trong cặp `"..."` hoặc `~...~` không được coi là bình luận và được giữ nguyên.
