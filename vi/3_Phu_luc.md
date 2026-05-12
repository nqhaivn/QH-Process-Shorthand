# PHỤ LỤC: TÀI LIỆU THAM CHIẾU NHANH

## PHẦN 1 - BẢNG THUẬT NGỮ VÀ KÝ HIỆU

| **Ký hiệu** | **Tên** | **Ý nghĩa** |
|---|---|---|
| `@N` | Vai trò / Làn | Người hoặc bộ phận thực hiện; khi có khai báo thì sơ đồ dạng làn bơi |
| `Gx` | Giai đoạn | Đơn vị tổ chức lớn nhất, thuộc một Vai trò duy nhất |
| `x.y` | Mã định danh (ID) | Xác định duy nhất mọi Nút trong quy trình |
| `->` | Đường luồng | Mũi tên chuyển tiếp giữa các Nút |
| `__` | Đường nối | Giống Đường luồng nhưng không có đầu mũi tên |
| `'...'` | Nhãn | Văn bản ngắn gắn kèm Đường luồng |
| `"..."` | Khối mô tả | Bọc văn bản nhiều dòng hoặc có ký tự cú pháp |
| `~...~` | Ghi chú | Thông tin bổ sung, không tham gia Luồng, hiển thị dạng nét đứt |
| `\.` | Cổng vào | Điểm bắt đầu quy trình; hình elip |
| `./` | Cổng ra | Điểm kết thúc quy trình; hình elip |
| `\/` | Cổng phụ | Thay thế Đường luồng sang trang khác; hình ngũ giác ngược |
| `/` | Ngõ (I/O) | Điểm dữ liệu vào/ra; hình bình hành |
| `!` | Quy trình con | Gọi quy trình con đã định nghĩa; hình chữ nhật cạnh gạch đôi |
| `?` | Quyết định | Rẽ nhánh XOR (chọn một); hình thoi |
| `()` | Điểm nối | Thay thế Đường luồng dài hoặc AND-join; hình tròn nhỏ |
| `#` | Bình luận | Không có giá trị cú pháp, không hiển thị trên sơ đồ |

## PHẦN 2 - VAI TRÒ (ROLE)

**Cú pháp:** `@N Tên đầy đủ (TÊN VIẾT TẮT)`

**Quy tắc:**

- `@N` phải tăng liên tục: `@1`, `@2`, `@3`, ... không được nhảy số.
- Toàn bộ khai báo Vai trò phải đứng đầu tài liệu, trước mọi Giai đoạn và Nút.
- Tên viết tắt viết HOA toàn bộ, không khoảng trắng, không trùng nhau giữa các Vai trò.
- Khi có khai báo Vai trò (dù chỉ một), sơ đồ luôn dạng làn bơi.
- Khi không có khai báo Vai trò, không được khai báo Giai đoạn; mọi Nút dùng ID dạng `1.y`.

**Ví dụ:**

```
    @1 Nhân viên kinh doanh (NVKD)
    @2 Trưởng phòng (TP)
    @3 Kế toán (KT)
```

## PHẦN 3 - GIAI ĐOẠN (STAGE)

**Cú pháp:** `@N Gx`

**Quy tắc:**

- Số Giai đoạn `x` phải tăng liên tục: G1, G2, G3, ... không được nhảy số.
- Dòng khai báo `@N Gx` phải đứng riêng, không có ký tự thêm sau (trừ Bình luận `#`).
- Một Giai đoạn chỉ có một Vai trò; một Vai trò có thể có nhiều Giai đoạn không liên tiếp.
- Nút cuối Giai đoạn tự chuyển sang Giai đoạn tiếp theo nếu không có `->` tường minh.

**Ví dụ:**

```
    @1 G1
    1.1 Tiếp nhận
    @2 G2
    2.1 Xem xét
    @1 G3   # @1 thực hiện tiếp G3
    3.1 Xác nhận
```

## PHẦN 4 - NÚT: TỔNG QUAN VÀ ĐÁNH SỐ

**Cú pháp tổng quát:** `x.y Ký_hiệu "Mô tả" ~Ghi chú~ 'Nhãn' -> Đích`

**Thứ tự bắt buộc của các thành phần:**

1. ID `x.y` (bắt buộc, đứng đầu dòng)
2. Ký hiệu loại (nếu có)
3. Mô tả (nếu có)
4. Ghi chú `~...~` (nếu có, đứng trước `->`)
5. Đường luồng `->` (nếu có, đứng cuối cùng)

**Bảng hình dạng trên sơ đồ:**

| **Loại** | **Ký hiệu** | **Hình vẽ** |
|---|---|---|
| Tác vụ | *(không có)* | Hình chữ nhật |
| Ngõ | `/` | Hình bình hành |
| Quy trình con | `!` | Hình chữ nhật cạnh gạch đôi |
| Quyết định | `?` | Hình thoi |
| Cổng vào | `\.` | Hình elip |
| Cổng ra | `./` | Hình elip |
| Cổng phụ | `\/` | Hình ngũ giác ngược |
| Điểm nối | `()` | Hình tròn nhỏ |

**Quy tắc:**

- Mọi Nút đều phải có ID, kể cả khi Giai đoạn chỉ có một Nút.
- Trong cùng một Giai đoạn, các Nút phải đánh số `y = 1, 2, 3, ...` theo thứ tự thực hiện mặc định.
- Không được có hai Nút cùng ID trong một tài liệu.
- ID phải đứng đầu dòng; thụt lề chỉ để trình bày, không có giá trị cú pháp.
- Một Nút có thể viết trải ra nhiều dòng.

## PHẦN 5 - ĐƯỜNG LUỒNG (FLOWLINE)

**Cú pháp:** `'Nhãn' -> Đích`

**Quy tắc:**

- Đích là `x.y` (ID Nút khác) hoặc `./` (kết thúc).
- Luồng mặc định đi tuần tự `x.y` → `x.(y+1)` khi không có `->` tường minh.
- Luồng mặc định bị hủy khi: (1) `x.y` có `->` tường minh, hoặc (2) `x.(y+1)` là Cổng, Cổng phụ, hoặc Điểm nối.
- Khi `->` đứng trên dòng riêng, dòng ngay phía trên phải là khai báo ID.
- Dùng `-> 0` để hủy toàn bộ luồng đi ra của một Nút (Quyết định không được dùng `0`).
- Dùng `__` thay `->` để tạo Đường nối (không có đầu mũi tên trên sơ đồ).

**Ví dụ:**

```
    2.1 Kiểm tra hồ sơ 'Hợp lệ' -> 2.2
    2.3 Ghi nhận -> ./
```

## PHẦN 6 - MÔ TẢ

**Cú pháp:**

```
Một dòng:   x.y Mô tả không có ký tự đặc biệt
Nhiều dòng: x.y
            "
            Dòng 1
            Dòng 2
            "
```

**Quy tắc:**

- Không cần `"..."` khi Mô tả một dòng, không chứa ký tự cú pháp, viết ngay sau ID trên cùng dòng.
- Bắt buộc dùng `"..."` khi: nhiều dòng; viết xuống dòng riêng; chứa ký tự `->` `./` `#` `,` `'` `~`.
- Đối với nhiều dòng, dấu `"` mở và `"` đóng phải nằm trên dòng riêng, không có ký tự khác.
- `@N` trong Mô tả tự động thay bằng tên viết tắt (hoặc tên đầy đủ) khi vẽ sơ đồ.

**Ví dụ:**

```
    1.1 Tạo đơn hàng mới
    1.2 "Nếu đạt -> ghi nhận"
    1.3
    "
    Kiểm tra:
    - Chứng từ
    - Chữ ký
    "
```

## PHẦN 7 - GHI CHÚ (ANNOTATION)

**Cú pháp:**

```
Một dòng:   ~Nội dung ghi chú~
Nhiều dòng: ~
            Dòng 1
            Dòng 2
            ~
```

**Quy tắc:**

- Ghi chú phải đứng sau ID và Mô tả, trước bất kỳ `->` nào của Nút đó.
- Ghi chú không tham gia Luồng điều khiển; hiển thị dạng hình chữ nhật nét đứt nối với Nút.
- Đối với nhiều dòng, dấu `~` mở và `~` đóng phải nằm trên dòng riêng.
- `@N` trong Ghi chú tự động thay bằng tên viết tắt khi vẽ sơ đồ.
- `#` bên trong `~...~` không bị coi là Bình luận.

**Ví dụ:**

```
    2.1 ? Duyệt đơn?
    ~
    Từ chối khi thiếu chứng từ
    hoặc sai thông tin khách
    ~
    'Từ chối' -> ./
    'Đồng ý' -> 2.2
```

## PHẦN 8 - CỔNG VÀO / CỔNG RA

**Cú pháp:**

```
    Cổng vào:  x.y \. Mô tả ~Ghi chú~ 'Nhãn' -> Đích
    Cổng ra:   x.y ./ Mô tả ~Ghi chú~
```

**Quy tắc:**

- Cổng vào không tự động tạo; muốn hiển thị phải khai báo tường minh. Cổng vào đầu tiên phải ở Nút `1.1`; có thể khai báo thêm các Cổng vào khác ở các ID hợp lệ khác.
- `-> ./` trỏ đến Cổng ra Mặc định (ID ảo `z`); khai báo `x.y ./` tạo Cổng ra tường minh riêng biệt.
- Cổng ra Mặc định được vẽ ở Làn của Giai đoạn cuối có `-> ./`, Mô tả là `Kết thúc`.
- Có thể có nhiều Cổng vào và nhiều Cổng ra; mỗi Cổng là một hình elip riêng.
- Cổng ra là đích cuối cùng, không khai báo `->` trong phần khai báo của nó.

**Ví dụ:**

```
    1.1 \. Bắt đầu -> 1.2
    ...
    2.3 Hoàn tất -> ./
    3.1 ./ Kết thúc nhánh đặc biệt
```

## PHẦN 9 - CỔNG PHỤ

**Cú pháp:**

```
    Cổng phụ đi vào:  x.y \/ Mô tả ~Ghi chú~ 'Nhãn' -> Đích
    Cổng phụ đi ra:   x.y \/ Mô tả ~Ghi chú~
```

**Quy tắc:**

- Loại (đi vào / đi ra) xác định tự động: có `->` đi ra = đi vào; không có = đi ra.
- Mô tả ngắn gọn 1-2 ký tự (0-9 hoặc a-Z), dùng để ghép cặp giữa các trang.
- Mô tả của Cổng phụ đi vào và đi ra trong cùng quy trình phải khác nhau.
- Cổng phụ và Điểm nối dùng hai không gian Mô tả độc lập; trùng Mô tả vẫn không liên quan đến nhau.
- Mọi luồng vào/ra Cổng phụ phải khai báo tường minh bằng `->`.

**Ví dụ:**

```
    3.5 \/ A
    ...
    1.1 \/ B -> 1.3
```

## PHẦN 10 - HÀNH ĐỘNG (TÁC VỤ, NGÕ, QUY TRÌNH CON)

**Cú pháp:** `x.y Ký_hiệu Mô tả ~Ghi chú~ 'Nhãn' -> Đích`

| **Loại** | **Ký hiệu** | **Hình vẽ** |
|---|---|---|
| Tác vụ | *(không có)* | Hình chữ nhật |
| Ngõ | `/` | Hình bình hành |
| Quy trình con | `!` | Hình chữ nhật cạnh gạch đôi |

**Quy tắc:**

- Ba loại có mọi quy tắc về Luồng giống nhau, chỉ khác ký hiệu và hình vẽ.
- Ngõ vào/ra không cần khai báo riêng; ngữ nghĩa suy ra từ Mô tả.
- Quy trình con chỉ cần ghi tên hoặc mã tham chiếu trong Mô tả.

**Ví dụ:**

```
    1.1 Lập kế hoạch -> 1.2
    2.1 / Nhận báo cáo qua email -> 2.2
    3.1 ! QT.KD.01 -> 4.1
```

## PHẦN 11 - QUYẾT ĐỊNH (DECISION)

**Cú pháp:**

```
    x.y ? Mô tả ~Ghi chú~
    'Nhãn 1' -> Đích 1,
    'Nhãn 2' -> Đích 2,
    'Nhãn 3' -> Đích 3
```

**Quy tắc:**

- Ký hiệu `?` bắt buộc, đặt ngay sau ID.
- Phải có ít nhất hai nhánh; mọi nhánh đều bắt buộc có Nhãn.
- Nhãn của các nhánh không được trùng nhau; không được hai nhánh cùng trỏ một đích.
- Không được dùng đích `0` trong bất kỳ nhánh nào.
- Các nhánh ngăn cách bằng `,`; dấu `,` cuối cùng không bắt buộc.
- Tại mỗi điểm Quyết định, chỉ chọn đúng một nhánh (XOR).

**Ví dụ:**

```
    2.1 ? Đơn hàng hợp lệ?
        'Không' -> ./,
        'Có' -> 2.2
```

## PHẦN 12 - ĐIỂM NỐI

**Cú pháp:** `x.y () Mô tả ~Ghi chú~ 'Nhãn' -> Đích`

**Quy tắc:**

- Mô tả ngắn gọn 1-2 ký tự, dùng để ghép cặp Điểm thu và Điểm phát.
- Có `->` = Điểm phát; không có `->` = Điểm thu.
- Mỗi nhóm cùng Mô tả chỉ được có một Điểm phát.
- Không được có Đường luồng dẫn trực tiếp giữa các Điểm nối cùng Mô tả.
- Tất cả Điểm thu phải hoàn thành trước khi Điểm phát kích hoạt (AND-join).
- Mọi luồng vào Điểm thu và ra từ Điểm phát phải khai báo tường minh bằng `->`.

**Ví dụ:**

```
    1.1 A -> 2.1
    1.2 B -> 3.2
    2.1 () A
    3.2 () A
    4.1 () A -> 4.2 # Điểm phát A; kích hoạt khi hai Điểm thu A hoàn thành
```

## PHẦN 13 - PHÂN TÁCH (FORK/PARALLEL)

**Cú pháp:**

```
    x.y Mô tả ~Ghi chú~
    'Nhãn 1' -> Đích 1,
    'Nhãn 2' -> Đích 2,
    'Nhãn 3' -> Đích 3
```

**Quy tắc:**

- Phân tách xuất phát từ Hành động, Cổng vào, Cổng phụ đi vào hoặc Điểm phát.
- Quyết định (`?`) không thể là gốc của Phân tách.
- Không được hai nhánh cùng trỏ một đích.
- Các nhánh ngăn cách bằng `,`; dấu `,` cuối không bắt buộc.
- Tất cả các nhánh được thực hiện đồng thời (AND-fork).
- Có thể dùng thụt lề để trình bày các Nút trong từng nhánh, nhưng thụt lề không ảnh hưởng cú pháp.

**Ví dụ:**

```
    2.1 Xử lý đồng thời
    -> 2.2,
    -> 3.1,
    -> 4.1
    2.2 Gửi email
    3.1 Lập hóa đơn
    4.1 Chuẩn bị vận chuyển
```

## PHẦN 14 - GỘP (JOIN)

**Cú pháp:** Dùng `->` trỏ từ cuối mỗi nhánh về cùng một Đích.

```
    x.y Mô tả -> Đích chung
    x.z Mô tả -> Đích chung
```

**Quy tắc:**

- Khi nhiều Đường luồng cùng trỏ vào một đích, đích đó chỉ kích hoạt khi tất cả nhánh hoàn thành (AND-join).
- Không bắt buộc phải gộp các nhánh song song; mỗi nhánh có thể tự đi đến Cổng ra riêng.
- Khi một nhánh song song kết thúc bằng `-> ./`, chỉ nhánh đó kết thúc; các nhánh còn lại tiếp tục độc lập.

**Ví dụ:**

```
    2.2 Gửi email xác nhận -> 3.2
    3.1 Gửi yêu cầu xuất kho -> 3.2
    3.2 Hoàn tất đơn hàng    # Chỉ bắt đầu khi cả 2.2 và 3.1 hoàn thành
```

## PHẦN 15 - BÌNH LUẬN TÀI LIỆU (COMMENT)

**Cú pháp:** `# Nội dung bình luận`

**Quy tắc:**

- `#` có thể đứng đầu dòng hoặc cuối dòng bất kỳ; mọi ký tự sau `#` đến cuối dòng bị bỏ qua.
- Bình luận không hiển thị trên sơ đồ dưới bất kỳ hình thức nào.
- `#` bên trong `"..."` hoặc `~...~` không bị coi là Bình luận và được giữ nguyên.

**Ví dụ:**

```
    # Đây là bình luận toàn dòng
    @1 G1 # Giai đoạn khởi tạo
    2.1 Nhận đơn hàng # Bước đầu tiên
```
