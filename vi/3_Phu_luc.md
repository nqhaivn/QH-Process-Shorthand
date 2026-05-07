# PHỤ LỤC: TÀI LIỆU THAM CHIẾU NHANH

## 1. HỌC NHANH

### 1.1 Bảng tóm tắt ký hiệu cú pháp

| **Ký hiệu** | **Ý nghĩa** |
|---------|---------|
| **`@N`** | Khai báo Vai trò thứ N |
| **`@N Gx`** | Khai báo Giai đoạn x thuộc Vai trò N |
| **`x.y`** | Mã định danh Nút |
| **`?`** | Quyết định (hình thoi) |
| **`/`** | Ngõ vào/ra (hình bình hành) |
| **`!`** | Quy trình con (hình chữ nhật gạch đôi) |
| **`\.`** | Cổng vào (hình elip) |
| **`./`** | Cổng ra (hình elip) |
| **`\/`** | Cổng phụ (hình ngũ giác ngược) |
| **`()`** | Điểm nối (hình tròn nhỏ) |
| **`->`** | Đường luồng (mũi tên) |
| **`__`** | Đường nối (không có đầu mũi tên) |
| **`'...'`** | Nhãn Đường luồng |
| **`"..."`** | Khối mô tả nhiều dòng hoặc chứa ký tự đặc biệt |
| **`~...~`** | Ghi chú (annotation), hiển thị trên sơ đồ |
| **`#`** | Bình luận tài liệu, không hiển thị trên sơ đồ |

### 1.2 Tổng quan cấu trúc tài liệu quy trình

Một tài liệu quy trình hợp lệ luôn bao gồm hai phần theo đúng thứ tự:

1. **Khai báo Vai trò** (`@1`, `@2`, `@3`, ...): xác định các bên tham gia, phải viết trước tất cả nội dung khác.
2. **Nội dung quy trình**: gồm các Giai đoạn và Nút bên trong mỗi Giai đoạn.

Quy trình bắt đầu tại Nút `1.1` (mặc định) và kết thúc khi gặp `./`. Luồng chạy tuần tự từ `x.y` sang `x.(y+1)` mà không cần viết `->`, trừ khi có `->` tường minh thay thế.

## 2. VAI TRÒ (ROLE)

Vai trò xác định ai chịu trách nhiệm thực hiện công việc. Khi vẽ sơ đồ, mỗi Vai trò chiếm một Làn (swimlane) riêng.

**Cú pháp:**

```
    @N Tên đầy đủ (TÊN VIẾT TẮT)
```

- `@N`: số thứ tự Vai trò, bắt đầu từ `@1`, tăng liên tục (không được nhảy số).
- Tên viết tắt: không bắt buộc, phải viết HOA toàn bộ, không khoảng trắng, phải dùng nhất quán.

**Ví dụ:**

```
    @1 Trưởng phòng (TP)
    @2 Phó giám đốc (PGD)
    @3 Kế toán (KT)
```

Nếu quy trình chỉ có một Vai trò và không cần hiển thị Làn bơi, dùng tên đặc biệt `0`:

```
    @1 0
```

## 3 GIAI ĐOẠN (STAGE)

Giai đoạn là đơn vị tổ chức lớn nhất trong nội dung quy trình. Mỗi Giai đoạn thuộc về đúng một Vai trò và chứa một hoặc nhiều Nút.

**Cú pháp:**

```
    @N Gx
```

- `x` là số thứ tự Giai đoạn, bắt đầu từ `1`, tăng liên tục (G1, G2, G3, ...).
- Dòng khai báo Giai đoạn phải đứng trên dòng riêng. Không được có ký tự nào sau `@N Gx` (ngoại lệ: bình luận bắt đầu bằng `#`).
- Một Vai trò có thể chịu trách nhiệm nhiều Giai đoạn không liên tiếp.

**Ví dụ:**

```
    @1 G1
    1.1 Tiếp nhận yêu cầu
    1.2 Kiểm tra hồ sơ

    @2 G2
    2.1 Phê duyệt

    @1 G3   # @1 thực hiện tiếp G3 sau khi @2 xong G2
    3.1 Thông báo kết quả
```

## 4. NÚT (NODE) VÀ MÃ ĐỊNH DANH (ID)

### 4.1 Khái niệm

Nút là từ gọi chung cho mọi đối tượng được gán ID trong quy trình: Tác vụ, Ngõ, Quy trình con, Quyết định, Cổng vào, Cổng ra, Cổng phụ và Điểm nối. Mọi Nút đều phải có ID, kể cả khi Giai đoạn chỉ có một Nút duy nhất.

### 4.2 Mã định danh

ID có dạng `x.y`, trong đó `x` là số thứ tự Giai đoạn, `y` là số thứ tự Nút trong Giai đoạn đó (bắt đầu từ 1, tăng dần). Không được có hai Nút cùng ID trong một tài liệu.

| Giai đoạn | Các ID Nút tương ứng |
|-----------|----------------------|
| G1 | 1.1, 1.2, 1.3, ... |
| G2 | 2.1, 2.2, 2.3, ... |
| Gx | x.1, x.2, x.3, ... |

### 4.3 Ký hiệu phân loại Nút

| Loại Nút | Ký hiệu | Hình vẽ trên sơ đồ |
|----------|---------|---------------------|
| Tác vụ | *(không có)* | Hình chữ nhật |
| Ngõ vào/ra | `/` | Hình bình hành |
| Quy trình con | `!` | Hình chữ nhật, hai cạnh dọc gạch đôi |
| Quyết định | `?` | Hình thoi |
| Cổng vào | `\.` | Hình elip |
| Cổng ra | `./` | Hình elip |
| Cổng phụ | `\/` | Hình ngũ giác ngược |
| Điểm nối | `()` | Hình tròn nhỏ |

**Cú pháp tổng quát:**

```
    x.y Ký_hiệu "Mô tả" ~Ghi chú~
```

## 5. ĐƯỜNG LUỒNG (FLOWLINE)

### 5.1 Khái niệm

Đường luồng biểu diễn hướng đi của quy trình từ Nút này sang Nút khác, ký hiệu là `->`. Trên sơ đồ, Đường luồng được vẽ bằng mũi tên.

### 5.2 Cú pháp và cách dùng

```
    'Nhãn' -> Đích
```

- `Đích` là một ID Nút hợp lệ (`x.y`) hoặc `./` (kết thúc quy trình).
- Nhãn là văn bản ngắn mô tả điều kiện của đường đi, bọc trong dấu nháy đơn `'...'`. Không bắt buộc, trừ các nhánh của Quyết định.
- `->` có thể đặt cuối dòng của Nút hoặc trên dòng riêng bên dưới.

### 5.3 Luồng mặc định và luồng tường minh

Các Nút trong cùng một Giai đoạn chạy tuần tự theo thứ tự tăng dần mà **không cần viết `->` ra**. Tuy nhiên, khi một Nút có `->` tường minh, luồng mặc định sang Nút kế tiếp bị hủy - Nút đó chỉ đi đến đích được khai báo tường minh.

Nếu đích là 0, toàn bộ luồng đi ra của Nút đó bị hủy, kể cả các đích khác được khai báo cùng lúc.

**Ví dụ luồng mặc định:**

```
    2.1 Nhận yêu cầu
    2.2 Xử lý yêu cầu
    2.3 Gửi kết quả -> ./
    # Luồng: 2.1 -> 2.2 -> 2.3 -> ./
```

**Ví dụ luồng tường minh (2.2 bỏ qua 2.3):**

```
    2.1 Nhận yêu cầu
    2.2 Xử lý yêu cầu -> 3.1
    2.3 Bước bị bỏ qua   # không có mũi tên nào đến 2.3
```

**Dùng `__` thay `->` khi không muốn vẽ đầu mũi tên trên sơ đồ:**

```
    2.1 Tác vụ A __ 2.2
```

## 6. MÔ TẢ

Mô tả là nội dung chính của Nút, được ghi bên trong hình vẽ trên sơ đồ.

- Nếu Mô tả chỉ gồm một dòng, không chứa ký tự cú pháp và viết ngay sau ID trên cùng dòng: **không cần** bọc `"..."`.
- **Bắt buộc dùng `"..."`** khi: mô tả nhiều dòng, viết xuống dòng riêng, hoặc chứa ký tự cú pháp như `->`, `./`, `#`, `,`, `~`.

**Ví dụ:**

```
    1.1 Tạo đơn hàng mới               # Không cần ""

    1.2 
    "Kiểm tra tồn kho"              # Bắt buộc vì viết xuống dòng riêng

    1.3
    "
    Phân tích dữ liệu:
    - Tổng hợp từ CRM
    - Xác định nhóm rời bỏ
    "

    1.4 "Nếu đạt -> ghi nhận, nếu không -> báo cáo"   # Bắt buộc vì chứa ->
```

## 7 GHI CHÚ

Ghi chú là thông tin bổ sung, hiển thị trên sơ đồ dưới dạng khung nét đứt nối với Nút, nhưng **không tham gia Luồng**.

**Cú pháp:**

```
    ~Ghi chú một dòng~

    ~
    Ghi chú
    nhiều dòng
    ~
```

Ghi chú phải đứng ngay sau ID và Mô tả của Nút mà nó gắn vào. Bên trong `~...~` không được chứa ký tự `~`.

**Ví dụ:**

```
    1.3 Kiểm tra chứng từ
    ~
    Bao gồm:
    - Hợp đồng
    - Phụ lục hợp đồng
    ~
```

## 8. CỔNG VÀO VÀ CỔNG RA

### 8.1 Khái niệm

Cổng vào (`\.`) đánh dấu nơi bắt đầu, Cổng ra (`./`) đánh dấu nơi kết thúc quy trình. Trên sơ đồ cả hai đều được vẽ bằng hình elip. Một quy trình có thể có nhiều Cổng vào và nhiều Cổng ra.

### 8.2 Quy tắc mặc định

- Khi chỉ có một Cổng vào: mặc định nằm trước Nút `1.1`, mô tả là `Bắt đầu`, **không cần khai báo**.
- Khi chỉ có một Cổng ra: mặc định nằm cùng Làn với Giai đoạn cuối chứa `-> ./`, mô tả là `Kết thúc`. Chỉ cần viết `./` trong nội dung.
- `./` có thể xuất hiện nhiều lần trong tài liệu.

### 8.3 Khai báo tường minh (khi cần nhiều Cổng hoặc tùy chỉnh)

```
    # Cổng vào tường minh:
    x.y \. Mô tả ~Ghi chú~ 'Nhãn' -> Đích

    # Cổng ra tường minh:
    x.y ./ Mô tả ~Ghi chú~
```

**Ví dụ nhiều Cổng vào:**

```
    1.1 \. Khởi động thủ công -> 1.3
    1.2 \. Khởi động tự động -> 1.4
```

**Ví dụ nhiều Cổng ra:**

```
    3.5 ./ Hoàn tất nhánh A
    4.3 ./ Hủy đơn
```

## 9. CỔNG PHỤ (OFF-PAGE CONNECTOR)

### 9.1 Khái niệm

Cổng phụ thay thế cho Đường luồng nối sang trang sơ đồ khác. Có hai dạng: đầu ra (thay Cổng ra) và đầu vào (thay Cổng vào). Trên sơ đồ vẽ bằng hình ngũ giác ngược, cạnh nhọn quay xuống dưới.

### 9.2 Cú pháp

```
    # Cổng phụ đầu vào (có -> để chỉ Nút tiếp theo):
    x.y \/ Mô tả 'Nhãn' -> Đích

    # Cổng phụ đầu ra (không có ->):
    x.y \/ Mô tả
```

Mô tả nên ngắn gọn 1–2 ký tự (0–9 hoặc a–Z) để ghép cặp đầu vào/đầu ra tương ứng trên các trang sơ đồ.

**Ví dụ:**

```
    # Trang 1 - đầu ra:
    3.5 \/ A

    # Trang 2 - đầu vào tiếp tục từ A:
    4.1 \/ A -> 4.2
```

## 10. HÀNH ĐỘNG: TÁC VỤ, NGÕ VÀ QUY TRÌNH CON

### 10.1 Khái niệm

Ba loại Hành động có mọi quy tắc về Luồng giống nhau, chỉ khác ký hiệu và hình vẽ.

| Loại | Ký hiệu | Hình vẽ |
|------|---------|---------|
| Tác vụ | *(không có)* | Hình chữ nhật |
| Ngõ (Input/Output) | `/` | Hình bình hành |
| Quy trình con | `!` | Hình chữ nhật cạnh dọc gạch đôi |

**Cú pháp:**

```
    x.y Ký_hiệu Mô tả ~Ghi chú~ 'Nhãn' -> Đích
```

### 10.2 Ví dụ

```
    2.1 Thẩm định hồ sơ -> 2.2              # Tác vụ
    2.2 / Nhận báo cáo tài chính            # Ngõ vào
    ~Định dạng: Excel. Thời hạn: trước 17h ngày 5 hàng tháng.~
    2.3 ! QT.KD.01 -> 3.1                   # Quy trình con tham chiếu theo mã
```

## 11. QUYẾT ĐỊNH (DECISION)

### 11.1 Khái niệm

Quyết định là điểm rẽ nhánh, tại đây Luồng **chọn đúng một nhánh** trong số các nhánh được liệt kê (XOR). Trên sơ đồ được vẽ bằng hình thoi. Mỗi Quyết định phải có **ít nhất hai nhánh**, và các nhánh không được trùng nhãn.

### 11.2 Cú pháp

```
    x.y ? Mô tả ~Ghi chú~
        'Nhãn nhánh 1' -> Đích 1
        'Nhãn nhánh 2' -> Đích 2
        ...
```

### 11.3 Ví dụ

**Quyết định nhị phân:**
```
    2.1 ? Đơn hàng hợp lệ?
        'Không' -> ./
        'Có' -> 2.2
```

**Quyết định ba nhánh với ghi chú định nghĩa:**

```
    3.1 ? Phân loại khách hàng
    ~
    VIP: Doanh thu trên 500 triệu/năm
    Thường: Dưới 500 triệu
    Mới: Chưa có lịch sử giao dịch
    ~
        'VIP' -> 3.2
        'Thường' -> 4.1
        'Mới' -> 4.2
```

**Quyết định lồng nhau:**

```
    2.1 ? Kiểm tra
        'Không đạt' -> 2.2
        'Đạt' -> 2.3
    2.2 ? Lý do không đạt?
        'Thiếu chứng từ' -> 2.5
        'Sai thông tin' -> 2.6
```

## 12. ĐIỂM NỐI (ON-PAGE CONNECTOR)

### 12.1 Khái niệm

Điểm nối thay thế Đường luồng dài trên cùng trang, tránh các mũi tên cắt ngang nhau làm rối sơ đồ. Bao gồm các **Điểm thu** (không có `->`) và một **Điểm phát** (có `->`). Tất cả Điểm thu phải hoàn thành trước khi Điểm phát kích hoạt (AND-join). Trên sơ đồ vẽ bằng hình tròn nhỏ.

Điểm nối là loại Nút duy nhất mà luồng tuần tự mặc định không chạy qua: các Nút khác có thể dẫn đến Điểm nối theo luồng mặc định, nhưng từ Điểm nối đi ra, người viết bắt buộc phải khai báo tường minh bằng `->`. Nếu không có `->` hợp lệ nào được khai báo, Điểm nối sẽ không dẫn đến bất cứ Nút nào.

### 12.2 Cú pháp

```
    x.y () Mô tả ~Ghi chú~ 'Nhãn' -> Đích
```

Mô tả nên ngắn gọn 1–2 ký tự. Các Điểm nối cùng Mô tả tạo thành một nhóm. Mỗi nhóm chỉ được có **một Điểm phát**.

**Ví dụ:**

```
    2.1 X -> 3.1       # Luồng đến Điểm thu A ở Làn 3
    3.1 () A           # Điểm thu A
    4.1 () A -> 5.2    # Điểm phát A, dẫn đến 5.2
```

## 13. PHÂN TÁCH SONG SONG (FORK)

Từ một Nút nguồn (Hành động, Cổng vào, Cổng phụ đầu vào, hoặc Điểm phát) tạo ra nhiều Luồng thực hiện **đồng thời**. Quyết định không thể là gốc của phân tách.

Các luồng phân tách phân cách nhau bằng dấu phẩy `,`.

**Cú pháp nhiều dòng:**
```
    x.y Mô tả
        -> Đích 1,
        -> Đích 2,
        -> Đích 3
```

**Ví dụ:**

```
    2.1 Xử lý đồng thời
        'Kho' -> 2.2,
        'Kế toán' -> 3.1,
        'Giao vận' -> 4.1
```

Trên sơ đồ, có thể vẽ thanh fork (thanh ngang) từ Nút nguồn, rồi từ thanh đó kẻ các mũi tên đến từng đích; hoặc vẽ nhiều mũi tên xuất phát thẳng từ Nút nguồn.

## 14 GỘP (JOIN)

Gộp là nơi các Luồng hội tụ lại. Ngữ nghĩa luôn là **AND-join**: tất cả các nhánh phải hoàn thành trước khi đích được kích hoạt. Cú pháp đơn giản là nhiều Nút cùng trỏ `->` vào một đích.

**Ví dụ:**

```
    2.2 Gửi email xác nhận -> 5.1
    3.1 Lập hóa đơn -> 5.1
    4.1 Chuẩn bị vận chuyển -> 5.1
    5.1 Hoàn tất đơn hàng      # Chỉ bắt đầu khi 2.2, 3.1 và 4.1 đều xong
```

## 15. BÌNH LUẬN TÀI LIỆU (COMMENT)

Bình luận là ghi chú **chỉ dành cho người viết/đọc tài liệu**, không hiển thị trên sơ đồ và bị bỏ qua hoàn toàn. Bắt đầu bằng ký tự `#`.

```
    # Đây là một dòng bình luận hoàn toàn
    2.1 Nhận đơn hàng    # Đây là bình luận cuối dòng
```

Lưu ý: ký tự `#` xuất hiện bên trong `"..."` hoặc `~...~` **không được coi là bình luận** và được giữ nguyên.
