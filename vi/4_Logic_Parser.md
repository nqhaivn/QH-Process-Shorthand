# CHƯƠNG 3: MÔ TẢ LOGIC PHÂN TÍCH CÚ PHÁP

## Quy ước trong chương này

- Token: đơn vị từ vựng nhỏ nhất được parser nhận diện.
- Dòng logic: một dòng sau khi đã tước bỏ bình luận và thụt lề. Nhiều dòng vật lý có thể tạo thành một Nút logic (xem Phần 2).
- Ngữ cảnh hiện tại: chế độ tài liệu và cặp giá trị `(vai_tro_hien_tai, giai_doan_hien_tai)` đang được parser theo dõi. Trong chế độ không làn bơi, hai giá trị này rỗng và mọi Nút dùng ID dạng `1.y`.
- Đồ thị luồng (flow graph): cấu trúc dữ liệu đầu ra cuối cùng gồm danh sách Nút và danh sách Cạnh có hướng.
- Ký hiệu `→` trong tài liệu này là ký hiệu giả mã (pseudocode), không phải `->` trong ngôn ngữ đặc tả.

## Phần 1 - Tiền xử lý văn bản

### 1.1 Loại bỏ bình luận

Quét toàn bộ văn bản đầu vào theo từng dòng vật lý. Với mỗi dòng:

1. Nếu dòng đang thuộc bên trong khối nhiều dòng `"..."` hoặc `~...~` (dựa vào trạng thái mở/đóng đang theo dõi toàn cục - xem Phần 2.3), giữ nguyên toàn bộ ký tự; `#` không có ý nghĩa bình luận.
2. Nếu dòng không thuộc khối nhiều dòng, quét tuần tự từng ký tự từ trái sang phải và theo dõi trạng thái cặp `"` hoặc `~` một dòng. Ký tự `#` chỉ bắt đầu Bình luận khi nó nằm ngoài mọi cặp `"..."` và `~...~`.
3. Khi gặp `#` hợp lệ làm Bình luận, cắt bỏ từ vị trí `#` đến cuối dòng (kể cả `#`). Nếu không gặp `#` hợp lệ, giữ nguyên dòng.

Kết quả: văn bản sạch không có bình luận.

### 1.2 Loại bỏ thụt lề

Với mỗi dòng trong văn bản sạch: tước bỏ toàn bộ khoảng trắng và ký tự tab đầu dòng. Bước này chỉ áp dụng ở đầu dòng; khoảng trắng bên trong `"..."` và `~...~` phải được giữ nguyên (QT.6.4). Thụt lề không có giá trị cú pháp (QT.1.15).

### 1.3 Loại bỏ dòng trống

Các dòng trống (sau khi tước thụt lề chỉ còn chuỗi rỗng) được giữ lại như dấu phân cách bối cảnh trong quá trình gom nhóm dòng (Phần 2), sau đó loại bỏ.

## Phần 2 - Gom nhóm dòng thành đơn vị logic

Một Nút có thể trải ra nhiều dòng vật lý (QT.4.6). Cần gom các dòng thuộc về cùng một Nút trước khi phân tích.

### 2.1 Quy tắc mở đầu đơn vị mới

Một dòng mở đầu một đơn vị logic mới khi và chỉ khi nó bắt đầu bằng một trong các mẫu sau (áp dụng sau khi đã tước thụt lề và bình luận):

- Mẫu khai báo Vai trò: `@` + số nguyên dương + khoảng trắng + phần còn lại không phải `G` + số nguyên.
- Mẫu khai báo Giai đoạn: `@` + số nguyên + khoảng trắng + `G` + số nguyên, và không có ký tự nào khác sau đó.
- Mẫu ID Nút: số nguyên + `.` + số nguyên, đứng ở đầu dòng (ví dụ `1.1`, `2.3`, `10.5`).

### 2.2 Quy tắc tiếp nối dòng

Một dòng không mở đầu đơn vị mới thì nó thuộc về đơn vị đang mở trước đó, nếu thỏa mãn một trong:

- Dòng bắt đầu bằng `->` hoặc `__`.
- Dòng bắt đầu bằng `'` (nhãn đường luồng).
- Dòng bắt đầu bằng `"` (mở khối Mô tả một dòng hoặc nhiều dòng).
- Dòng nằm bên trong khối `"..."` chưa đóng (cờ `trong_mo_ta = true`).
- Dòng bắt đầu bằng `~` (mở khối Ghi chú một dòng hoặc nhiều dòng).
- Dòng nằm bên trong khối `~...~` chưa đóng (cờ `trong_ghi_chu = true`).

Nếu một dòng không thỏa mãn bất kỳ điều kiện nào ở mục 2.1 và 2.2, đó là lỗi E11.

### 2.3 Phát hiện và theo dõi khối nhiều dòng

Parser duy trì hai cờ trạng thái boolean toàn cục: `trong_mo_ta` và `trong_ghi_chu`. Cả hai khởi tạo là `false`.

#### Quy tắc cho khối `"..."`

Căn cứ vào cú pháp 6.2 của đặc tả, có hai dạng hợp lệ và một dạng lỗi:

**Dạng một dòng:** dòng bắt đầu bằng `"`, có ít nhất một ký tự khác phía sau, và tìm thấy ít nhất một `"` khác trên cùng dòng đó. Dấu `"` cuối cùng trên dòng là dấu đóng. Nội dung Mô tả là toàn bộ ký tự nằm giữa `"` mở và `"` đóng cuối cùng, bao gồm mọi `"` trung gian (QT.6.3).

**Dạng nhiều dòng:** dòng chỉ gồm đúng một ký tự `"` (sau tước thụt lề, không có ký tự nào khác). Đây là dấu mở khối nhiều dòng. Đặt `trong_mo_ta = true`. Từ dòng tiếp theo, thu thập nguyên vẹn từng dòng vào nội dung Mô tả (giữ nguyên khoảng trắng và ký tự đặc biệt, QT.6.4). Khi gặp dòng chỉ gồm đúng một ký tự `"` (sau tước thụt lề): đây là dấu đóng, đặt `trong_mo_ta = false`. Dòng chứa dấu mở và dòng chứa dấu đóng không được thu thập vào nội dung. Mọi `"` xuất hiện trong các dòng nội dung bên trong là ký tự thuần túy.

**Dạng lỗi E28:** dòng bắt đầu bằng `"`, có ký tự khác phía sau, nhưng không tìm thấy `"` nào khác trên cùng dòng đó (chỉ có một dấu `"` duy nhất trên dòng và nó không phải ký tự duy nhất).

#### Quy tắc cho khối `~...~`

Hoàn toàn đối xứng với quy tắc cho `"..."`, thay `"` bằng `~`, thay E28 bằng E29, thay `trong_mo_ta` bằng `trong_ghi_chu`.

**Dạng một dòng:** dòng bắt đầu bằng `~`, có ít nhất một ký tự khác phía sau, và tìm thấy ít nhất một `~` khác trên cùng dòng. Dấu `~` cuối cùng trên dòng là dấu đóng. Nội dung Ghi chú là toàn bộ ký tự nằm giữa `~` mở và `~` đóng cuối cùng, bao gồm mọi `~` trung gian (QT.7.2).

**Dạng nhiều dòng:** dòng chỉ gồm đúng một ký tự `~` (sau tước thụt lề). Đây là dấu mở. Đặt `trong_ghi_chu = true`. Thu thập nguyên vẹn từng dòng tiếp theo. Khi gặp dòng chỉ gồm đúng một ký tự `~`: đây là dấu đóng, đặt `trong_ghi_chu = false`. Mọi `~` trong các dòng nội dung bên trong là ký tự thuần túy.

**Dạng lỗi E29:** dòng bắt đầu bằng `~`, có ký tự khác phía sau, nhưng không tìm thấy `~` nào khác trên cùng dòng.

#### Ưu tiên và không lồng nhau

Khi đang `trong_mo_ta = true`, mọi ký tự `~` là nội dung thuần túy, không kích hoạt `trong_ghi_chu`. Khi đang `trong_ghi_chu = true`, mọi ký tự `"` là nội dung thuần túy, không kích hoạt `trong_mo_ta`. Hai khối không lồng nhau (QT.6.3, QT.7.2).

## Phần 3 - Phân tích khai báo Vai trò

### 3.1 Nhận dạng

Dòng khai báo Vai trò có dạng:

```
@<N> <tên_đầy_đủ> [(<TÊN_VIẾT_TẮT>)]
```

Điều kiện nhận dạng (sau khi tước thụt lề và bình luận):

- Bắt đầu bằng `@`, theo sau không có khoảng trắng, là một số nguyên dương `N`.
- Theo sau số `N` là khoảng trắng và phần tên.
- Phần sau `@N` không phải là `G` + số nguyên đơn thuần (trường hợp đó là khai báo Giai đoạn).
- Dòng này xuất hiện trước bất kỳ dòng nào có dạng `@N Gx` hoặc `x.y`.

### 3.2 Trích xuất thông tin

1. Trích `N`: số nguyên sau `@`. Kiểm tra `N` phải bằng `(số Vai trò đã khai báo) + 1`. Nếu không → lỗi E01.
2. Trích tên đầy đủ: phần văn bản từ sau khoảng trắng đầu tiên đến trước `(` nếu có, hoặc đến cuối dòng. Cắt khoảng trắng hai đầu.
3. Trích tên viết tắt (tùy chọn): nếu có `(...)` ở cuối tên đầy đủ, phần bên trong là tên viết tắt. Kiểm tra toàn chữ hoa và không chứa khoảng trắng; nếu vi phạm → lỗi E03. Kiểm tra không trùng với tên viết tắt của Vai trò đã khai báo; nếu trùng → lỗi E02.

### 3.3 Kết thúc vùng khai báo Vai trò

Vùng khai báo Vai trò kết thúc khi parser gặp dòng đầu tiên có dạng `@N Gx` hoặc `x.y`. Từ điểm đó trở đi, nếu gặp thêm dòng khai báo Vai trò → lỗi E04.

## Phần 4 - Phân tích khai báo Giai đoạn

### 4.1 Nhận dạng

Dòng khai báo Giai đoạn có dạng chính xác:

```
@<N> G<x>
```

Sau khi tước thụt lề và bình luận, phần còn lại của dòng sau `G<x>` phải rỗng. Nếu không → lỗi E07.

### 4.2 Kiểm tra hợp lệ

1. Tài liệu phải đã có ít nhất một khai báo Vai trò. Nếu không có khai báo Vai trò, khai báo Giai đoạn là lỗi E06.
2. `N` phải là số nguyên đã được khai báo trong danh sách Vai trò. Nếu không → lỗi E06.
3. `x` phải bằng `(số Giai đoạn đã khai báo) + 1`. Nếu không → lỗi E05.

### 4.3 Cập nhật ngữ cảnh

Sau khi xác nhận hợp lệ:

- `vai_tro_hien_tai ← N`
- `giai_doan_hien_tai ← x`
- Thêm vào danh sách Giai đoạn: `{ so_giai_doan: x, vai_tro: N }`

## Phần 5 - Phân tích Nút

### 5.1 Nhận dạng dòng khai báo ID

Dòng mở đầu một Nút bắt đầu bằng mẫu `<x>.<y>` trong đó `<x>` và `<y>` là các số nguyên dương, đứng liền nhau không có khoảng trắng.

Kiểm tra ngữ cảnh:

- Nếu tài liệu có khai báo Vai trò: phải đang ở trong một Giai đoạn; `x` phải bằng `giai_doan_hien_tai`. Nếu không → lỗi E08.
- Nếu tài liệu không có khai báo Vai trò: không được có khai báo Giai đoạn; `x` phải bằng `1`. Nếu không → lỗi E08.
- `y` phải bằng `(số Nút đã khai báo trong Giai đoạn hiện tại hoặc trong tài liệu không làn bơi) + 1`. Nếu không → lỗi E09.
- Cặp `(x, y)` phải chưa tồn tại trong danh sách Nút toàn cục. Nếu trùng → lỗi E10.

### 5.2 Trích xuất ký hiệu loại Nút

Sau token ID `x.y`, bỏ qua khoảng trắng, token tiếp theo xác định loại Nút:

| Token tiếp theo | Loại Nút |
|---|---|
| `\.` | Cổng vào |
| `./` | Cổng ra |
| `\/` | Cổng phụ |
| `()` | Điểm nối |
| `?` | Quyết định |
| `/` | Ngõ |
| `!` | Quy trình con |
| Bất kỳ token nào khác hoặc hết dòng | Tác vụ |

Lưu ý phân biệt:

- `./` ngay sau ID là ký hiệu loại Cổng ra. `./` xuất hiện sau `->` là đích của Đường luồng.
- `\/` (gạch chéo ngược + gạch chéo xuôi): Cổng phụ. Phân biệt với `/` đơn (Ngõ) và `\.` (gạch chéo ngược + dấu chấm, Cổng vào).
- Sau khi loại Nút đã được xác định, mọi lần xuất hiện tiếp theo của các ký hiệu loại `\.` `./` `\/` `?` `/` `!` `()` trong Mô tả hoặc Ghi chú chỉ còn là văn bản thuần túy. Riêng `./` vẫn thuộc nhóm ký tự cú pháp buộc Mô tả không bọc phải báo lỗi E12 theo QT.6.2.

### 5.3 Trích xuất Mô tả

Sau ký hiệu loại (hoặc sau ID nếu là Tác vụ không có ký hiệu):

**Trường hợp 1 - Không có dấu nháy kép:** phần văn bản từ vị trí hiện tại đến trước ký tự đầu tiên trong tập `{~, ->, __, '}` hoặc đến hết dòng, lấy cái xuất hiện trước nhất. Cắt khoảng trắng hai đầu. Nếu văn bản này chứa bất kỳ ký tự hoặc chuỗi nào trong tập `{->, ./, #, ,, ', ~}` → lỗi E12. Các ký hiệu loại `\.` `\/` `?` `/` `!` `()` xuất hiện trong phần này không gây lỗi và được giữ như văn bản thuần túy.

**Trường hợp 2 - Mô tả một dòng bọc trong `"..."`:** khi ký tự tiếp theo là `"` và trên cùng dòng đó còn có ít nhất một `"` khác phía sau. Dấu `"` cuối cùng trên dòng là dấu đóng. Nội dung Mô tả là toàn bộ ký tự nằm giữa `"` mở và `"` đóng cuối cùng; mọi `"` nằm giữa là ký tự nội dung thuần túy (QT.6.3). Không thay đổi cờ `trong_mo_ta`.

**Trường hợp 3 - Mô tả nhiều dòng bọc trong `"..."`:** khi ký tự tiếp theo là `"` và đó là ký tự duy nhất còn lại trên dòng (không có ký tự nào khác sau `"` này). Đây là dấu mở khối nhiều dòng theo cú pháp 6.2. Đặt cờ `trong_mo_ta = true`. Thu thập nguyên vẹn từng dòng tiếp theo vào nội dung Mô tả. Khi gặp dòng chỉ gồm đúng một ký tự `"` (sau tước thụt lề): đây là dấu đóng, đặt `trong_mo_ta = false`. Hai dòng chứa dấu mở và đóng không được thu thập vào nội dung.

**Trường hợp lỗi E28:** khi ký tự tiếp theo là `"`, có ký tự khác phía sau trên cùng dòng, nhưng không tìm thấy `"` nào khác trên cùng dòng đó.

Sau khi trích Mô tả: nếu Mô tả chứa token độc lập khớp mẫu `@` + số nguyên, ghi nhớ vị trí để thay thế khi xuất ra (xem Phần 9.1).

### 5.4 Trích xuất Ghi chú

Ghi chú phải xuất hiện sau Mô tả và trước bất kỳ `->` hoặc `__` nào của Nút (QT.7.1). Nếu gặp `~` sau `->` hoặc `__` → lỗi E13.

**Trường hợp 1 - Ghi chú một dòng:** khi ký tự tiếp theo là `~` và trên cùng dòng đó còn có ít nhất một `~` khác phía sau. Dấu `~` cuối cùng trên dòng là dấu đóng. Nội dung Ghi chú là toàn bộ ký tự nằm giữa `~` mở và `~` đóng cuối cùng; mọi `~` nằm giữa là ký tự nội dung thuần túy (QT.7.2). Không thay đổi cờ `trong_ghi_chu`.

**Trường hợp 2 - Ghi chú nhiều dòng:** khi ký tự tiếp theo là `~` và đó là ký tự duy nhất còn lại trên dòng. Đây là dấu mở khối nhiều dòng theo cú pháp 7.2. Đặt cờ `trong_ghi_chu = true`. Thu thập nguyên vẹn từng dòng tiếp theo vào nội dung Ghi chú. Khi gặp dòng chỉ gồm đúng một ký tự `~` (sau tước thụt lề): đây là dấu đóng, đặt `trong_ghi_chu = false`. Hai dòng chứa dấu mở và đóng không được thu thập vào nội dung.

**Trường hợp lỗi E29:** khi ký tự tiếp theo là `~`, có ký tự khác phía sau trên cùng dòng, nhưng không tìm thấy `~` nào khác trên cùng dòng đó.

Sau khi trích Ghi chú: nếu Ghi chú chứa token độc lập khớp mẫu `@` + số nguyên, ghi nhớ vị trí để thay thế khi xuất ra (xem Phần 9.1).

### 5.5 Trích xuất Đường luồng

Sau Mô tả và Ghi chú (nếu có), phần còn lại của khối dòng logic thuộc về Nút là các Đường luồng.

Một Đường luồng có dạng:

```
['<nhãn>'] (->) | (__) <đích>
```

Trong đó:

- Nhãn (tùy chọn): chuỗi ngắn nằm trong `'...'`. Nhãn không phải khối nhiều dòng. Nhãn kết thúc tại dấu `'` đầu tiên xuất hiện sau dấu `'` mở trên cùng dòng; vì không có cơ chế escape nên nội dung Nhãn không được chứa ký tự `'`.
- Toán tử: `->` (Đường luồng có mũi tên) hoặc `__` (Đường nối không có mũi tên).
- Đích: `x.y` (ID Nút) hoặc `./` (Cổng ra mặc định) hoặc `0` (hủy toàn bộ luồng ra).

Nhiều Đường luồng (AND-fork) được phân cách bằng dấu `,`. Dấu `,` có thể đứng ngay sau đích hoặc cách một khoảng trắng. Dấu `,` cuối cùng nếu không theo sau bởi Đường luồng nào thì bỏ qua (W02). Dấu `,` bên trong `"..."` là ký tự Mô tả, không phải phân cách Đường luồng.

Đường luồng có thể trải ra nhiều dòng vật lý. Mỗi dòng bắt đầu bằng `->`, `__`, hoặc `'` được coi là phần tiếp theo của khối Đường luồng thuộc Nút hiện tại.

Quy tắc đích `0` (QT.5.4): nếu bất kỳ Đường luồng nào của Nút có đích là `0`, đánh dấu Nút này là `huy_toan_bo_luong_ra = true` và bỏ qua toàn bộ Đường luồng khác đã khai báo. Không tạo cạnh nào đi ra từ Nút này. Quyết định không được dùng đích `0` ở bất kỳ nhánh nào → lỗi E18.

---

## Phần 6 - Phân tích Quyết định

Quyết định có cú pháp đặc biệt: tất cả Đường luồng đi ra bắt buộc phải có nhãn (QT.11.2).

### 6.1 Nhận dạng và trích xuất

Sau khi xác định Nút là Quyết định (ký hiệu `?`), trích xuất Mô tả và Ghi chú theo quy tắc tại mục 5.3 và 5.4.

### 6.2 Trích xuất các nhánh

Mỗi nhánh của Quyết định có dạng:

```
'<nhãn>' -> <đích>
```

Thu thập tất cả các dòng nhánh cho đến khi gặp dòng mở đầu đơn vị mới (mẫu `x.y` hoặc `@N Gx`). Các dòng nhánh có thể viết cùng dòng phân cách bằng `,` hoặc mỗi nhánh một dòng riêng.

### 6.3 Kiểm tra hợp lệ

1. Phải có ít nhất hai nhánh. Nếu không → lỗi E14.
2. Mỗi nhánh phải có nhãn. Nếu không → lỗi E15.
3. Không có hai nhánh nào cùng nhãn. Nếu trùng → lỗi E16.
4. Không có hai nhánh nào cùng đích. Nếu trùng → lỗi E17.
5. Không có nhánh nào có đích là `0`. Nếu có → lỗi E18.

## Phần 7 - Phân tích Điểm nối

### 7.1 Phân loại Điểm thu / Điểm phát

Sau khi trích xuất Mô tả và Ghi chú của Điểm nối `()`:

- Nếu Nút có Đường luồng `->` hoặc `__` → đây là Điểm phát.
- Nếu Nút không có Đường luồng → đây là Điểm thu.

### 7.2 Kiểm tra hợp lệ

1. Trong mỗi nhóm Điểm nối có cùng Mô tả, bắt buộc có đúng một Điểm phát (QT.12.2). Nếu không → lỗi E19.
2. Không được có Đường luồng từ bất kỳ Điểm nối nào đến Điểm nối khác trong cùng nhóm, dù là Điểm thu đến Điểm thu, hay Điểm phát đến Điểm thu (QT.12.3). Nếu có → lỗi E20.
3. Không có luồng mặc định nào đi vào hoặc đi ra từ Điểm nối. Mọi luồng đến Điểm thu và mọi luồng từ Điểm phát đều phải được khai báo tường minh bằng `->` hoặc `__` (QT.5.3, QT.12.6).

### 7.3 Ngữ nghĩa AND-join

Trong nhóm Điểm nối cùng Mô tả: Điểm phát chỉ kích hoạt khi tất cả Điểm thu trong nhóm đã nhận luồng. Đây là AND-join (QT.12.4). Parser ghi nhận nhóm AND-join này trong đồ thị luồng.

## Phần 8 - Xây dựng Đồ thị luồng

### 8.1 Cấu trúc dữ liệu

Parser xây dựng đồ thị có hướng gồm:

```
Danh sách Nút:
{
  id: "x.y",
  loai: <Tac_vu | Ngo | Quy_trinh_con | Quyet_dinh | Cong_vao | Cong_ra
         | Cong_phu_dau_vao | Cong_phu_dau_ra | Diem_thu | Diem_phat>,
  mo_ta: <chuỗi>,
  ghi_chu: <chuỗi | null>,
  giai_doan: x,
  vai_tro: N,
  huy_toan_bo_luong_ra: <bool>
}

Danh sách Cạnh:
{
  tu: "x.y",
  dich: "x.y" | "z",
  nhan: <chuỗi | null>,
  loai_canh: <flowline | connector_line | invisible>,
  la_mac_dinh: <bool>,
  la_ao: <bool>
}

Danh sách Nhóm Điểm nối:
{
  nhan: <chuỗi>,
  diem_thu: ["x.y", ...],
  diem_phat: "x.y"
}
```

Trong đó `loai_canh`:

- `flowline`: Đường luồng `->` bình thường.
- `connector_line`: Đường nối `__` không có mũi tên.
- `invisible`: Cạnh ảo dùng để định vị, không vẽ trên sơ đồ. Vì không hiển thị, Cạnh ảo được phép cắt ngang qua mọi Đường luồng, Đường nối hoặc khối hình vẽ khác và không chịu bất kỳ giới hạn bố cục nào.

### 8.2 Tạo cạnh luồng mặc định

Sau khi phân tích toàn bộ Nút, parser duyệt lại để tạo cạnh mặc định. Với mỗi Nút `x.y`:

1. Nếu Nút `x.y` đã có bất kỳ Đường luồng tường minh nào (`->` hoặc `__`) → không tạo cạnh mặc định từ `x.y`.
2. Nếu Nút `x.y` có `huy_toan_bo_luong_ra = true` → không tạo cạnh nào.
3. Nếu Nút `x.y` không có Đường luồng tường minh:
   - Nếu Nút hiện tại là Cổng ra, Cổng phụ hoặc Điểm nối → không tạo cạnh mặc định.
   - Xác định Nút tiếp theo theo thứ tự ưu tiên: trước tiên là `x.(y+1)`. Trong chế độ có Giai đoạn, nếu `x.(y+1)` không tồn tại thì xét Nút đầu tiên của Giai đoạn `G(x+1)`.
   - Nếu không tồn tại Nút tiếp theo nào → `x.y` là Nút cuối tài liệu, không tạo cạnh.
   - Nếu Nút tiếp theo là Cổng, Cổng phụ hoặc Điểm nối → không tạo cạnh mặc định. Luồng dừng tại `x.y`. Mọi luồng vào các loại Nút này phải khai báo tường minh khi cần.
   - Nếu Nút tiếp theo không phải Cổng, Cổng phụ hoặc Điểm nối → tạo cạnh mặc định từ `x.y` đến Nút tiếp theo đó, với `la_mac_dinh = true` và `la_ao = false`.

### 8.3 Xử lý Cổng vào và Cổng ra

**Cổng vào:** không tạo Cổng vào mặc định. Muốn hiển thị Cổng vào trên sơ đồ, tài liệu phải khai báo tường minh. Cổng vào tường minh đầu tiên phải là Nút `1.1`; các Cổng vào tường minh khác được phép xuất hiện ở các ID hợp lệ khác.

**Cổng ra mặc định (QT.8.4):** khi parser gặp `-> ./`:

- Tất cả cạnh có đích `./` đều dẫn đến cùng một Nút ảo `z`.
- Nút ảo `z` được đặt trong Làn của Giai đoạn có số thứ tự lớn nhất trong số các Giai đoạn chứa Nút có `-> ./`. Trong chế độ không làn bơi, `z` không thuộc Làn nào. Mô tả là `Kết thúc`, loại `Cong_ra`.

**Cổng ra tường minh (QT.8.6):** khi có khai báo `x.y ./`, các Nút khác trỏ đến Cổng ra này bằng `-> x.y`, không dùng `-> ./`.

### 8.4 Xử lý Cổng phụ

Phân loại dựa vào sự hiện diện của `->` hoặc `__` sau Mô tả và Ghi chú:

- Không có `->` và không có `__` → Cổng phụ đầu ra: loại `Cong_phu_dau_ra`. Không tạo cạnh đi ra.
- Có `->` hoặc `__` → Cổng phụ đầu vào: loại `Cong_phu_dau_vao`. Tạo cạnh từ Nút này đến đích.

Mô tả của Cổng phụ đầu vào phải khác với Mô tả của Cổng phụ đầu ra trong cùng tài liệu (QT.9.2). Nếu trùng → lỗi E25.

## Phần 9 - Giải quyết tham chiếu

### 9.1 Tham chiếu `@N` trong Mô tả và Ghi chú

Sau khi toàn bộ Vai trò đã được khai báo, duyệt lại tất cả Mô tả và Ghi chú của mọi Nút:

- Tìm mọi token độc lập khớp mẫu `@` + số nguyên. Một chuỗi `@N` chỉ được coi là token độc lập khi hai bên không liền với chữ cái Unicode, chữ số hoặc dấu gạch dưới.
- Với mỗi `@N` tìm được:
  - Nếu Vai trò `@N` có tên viết tắt → thay thế `@N` bằng tên viết tắt.
  - Nếu Vai trò `@N` không có tên viết tắt → thay thế `@N` bằng tên đầy đủ.
  - Nếu `@N` không khớp Vai trò nào đã khai báo → lỗi E21.
  - Các chuỗi như `email@1.com`, `A@1B` hoặc `ma_@1` không phải token độc lập và không được thay thế.

### 9.2 Giải quyết tham chiếu đích

Sau khi toàn bộ tài liệu đã được phân tích, duyệt lại tất cả Cạnh trong danh sách Cạnh thô (chưa bao gồm cạnh mặc định và cạnh ảo):

- Với mỗi cạnh có `dich = "x.y"`: kiểm tra `x.y` có tồn tại trong danh sách Nút không. Nếu không → lỗi E22.
- Nút không có cạnh đi vào, không phải Nút `1.1`, và không thuộc nhóm {Cổng vào, Cổng phụ đầu vào, Điểm thu} → phát cảnh báo W01.

### 9.3 Xác định gộp AND-join

Khi nhiều Cạnh điều khiển luồng cùng trỏ vào một Nút đích, Nút đích đó là một AND-join: chỉ kích hoạt khi tất cả Cạnh đi vào đã hoàn thành. Quy tắc này áp dụng cho mọi nguồn, bao gồm cả trường hợp các nhánh xuất phát từ các Quyết định khác nhau cùng trỏ về một Nút.

## Phần 10 - Kiểm tra tính toàn vẹn

Sau khi xây dựng xong đồ thị, thực hiện các kiểm tra sau:

### 10.1 Kiểm tra Cổng vào

- Cổng vào không bắt buộc về mặt cú pháp.
- Áp dụng quy tắc Cổng vào tại Phần 8.3. Nếu vi phạm vị trí Cổng vào tường minh đầu tiên → lỗi E27.

### 10.2 Kiểm tra Quyết định

- Mỗi Quyết định phải có ít nhất hai cạnh đi ra. Nếu không → lỗi E14.
- Tất cả cạnh đi ra từ Quyết định phải có nhãn. Nếu có cạnh không có nhãn → lỗi E15.

### 10.3 Kiểm tra Phân tách song song

Từ một Nút thuộc nhóm có thể là gốc AND-fork (Tác vụ, Ngõ, Quy trình con, Cổng vào, Cổng phụ đầu vào, Điểm phát), nếu có nhiều hơn một cạnh đi ra → đây là AND-fork. Kiểm tra không có hai cạnh nào cùng đích (QT.13.2). Nếu trùng → lỗi E23.

Quyết định không thể là gốc AND-fork (QT.13.1). Nếu Quyết định có cạnh đi ra không có nhãn → lỗi E24.

### 10.4 Kiểm tra Nhóm Điểm nối

- Mỗi nhóm cùng Mô tả bắt buộc có đúng một Điểm phát (QT.12.2). Nếu không → lỗi E19.
- Không có cạnh luồng điều khiển thực nào giữa các Điểm nối cùng nhóm (QT.12.3). Nếu có → lỗi E20.
- Parser tạo cạnh ảo từ mỗi Điểm thu đến Điểm phát cùng nhóm với `loai_canh = "invisible"` và `la_ao = true`. Cạnh ảo không mang ngữ nghĩa điều khiển luồng, chỉ dùng để renderer định vị các cụm Nút liên quan không bị vẽ rời rạc trên canvas. Vì không hiển thị, Cạnh ảo được phép cắt ngang qua mọi Đường luồng, Đường nối hoặc khối hình vẽ khác và không chịu bất kỳ giới hạn bố cục nào.

### 10.5 Kiểm tra Cổng phụ

Mô tả của Cổng phụ đầu vào phải khác với Mô tả của Cổng phụ đầu ra trong cùng tài liệu (QT.9.2). Nếu trùng → lỗi E25.

## Phần 11 - Xử lý các trường hợp biên

### 11.1 Tài liệu không có Cổng vào tường minh

Không tạo Cổng vào mặc định. Quy trình vẫn bắt đầu tại Nút `1.1`; Cổng vào chỉ hiển thị trên sơ đồ khi được khai báo tường minh.

### 11.2 Tài liệu không có khai báo Vai trò

Không khai báo Giai đoạn. Mọi Nút phải dùng ID dạng `1.y` và được vẽ trực tiếp trên sơ đồ, không có khung Làn.

### 11.3 Nút không có luồng đi vào

Nút xuất hiện trong tài liệu nhưng không có cạnh nào trỏ đến (không phải Nút `1.1`, không phải Cổng vào, không được nhận luồng mặc định vì Nút trước có `->` tường minh, không có Đường luồng tường minh nào trỏ đến): Nút vẫn được tạo trong danh sách Nút nhưng không được vẽ trên sơ đồ. Phát cảnh báo W01. Trường hợp điển hình là Nút `x.(y+1)` khi Nút `x.y` đã có `->` trỏ sang nơi khác (xem ví dụ 4, Phần 5.4 của đặc tả).

### 11.4 Đường luồng `->` trên dòng riêng

`->` đứng trên dòng riêng hợp lệ khi dòng ngay phía trên (bỏ qua dòng trống và bình luận) là khai báo ID `x.y` của Nút (QT.5.1). Nếu dòng phía trên là khai báo Giai đoạn, khai báo Vai trò hoặc một dòng tiếp nối khác → lỗi E26.

### 11.5 Dấu `,` phân cách nhiều Đường luồng

- `,` bên trong `"..."` là ký tự Mô tả.
- `,` bên ngoài `"..."` và `~...~`, xuất hiện sau một đích hợp lệ (`x.y` hoặc `./`) → là phân cách Đường luồng.

### 11.6 Tham chiếu chéo Giai đoạn

Đích của Đường luồng (`-> x.y`) có thể trỏ đến Nút ở Giai đoạn bất kỳ, không bắt buộc phải là Giai đoạn tiếp theo. Parser chỉ kiểm tra `x.y` tồn tại trong danh sách Nút.

## Phần 12 - Thứ tự xử lý tổng thể

```
  BƯỚC 1: Đọc toàn bộ văn bản đầu vào vào bộ nhớ.

  BƯỚC 2: Tiền xử lý
    2a. Loại bỏ bình luận (#) theo quy tắc Phần 1.1.
    2b. Tước thụt lề đầu dòng theo quy tắc Phần 1.2 (ngoại trừ bên trong "..." và ~...~).

  BƯỚC 3: Gom nhóm dòng thành đơn vị logic (Phần 2).

  BƯỚC 4: Phân tích tuyến tính
    Duyệt tuần tự từng đơn vị logic:
    4a. Nếu là khai báo Vai trò (@N tên) → thêm vào danh sách Vai trò (Phần 3).
    4b. Nếu là khai báo Giai đoạn (@N Gx) → cập nhật ngữ cảnh, thêm vào danh sách Giai đoạn (Phần 4).
    4c. Nếu là khai báo Nút (x.y ...) → phân tích loại, Mô tả, Ghi chú, Đường luồng (Phần 5–7).
        Thêm Nút vào danh sách. Lưu Đường luồng tường minh vào danh sách Cạnh thô.
    4d. Nếu không khớp mẫu nào → lỗi E11.

  BƯỚC 5: Tạo cạnh luồng mặc định (Phần 8.2).

  BƯỚC 6: Xử lý Cổng vào tường minh và Cổng ra mặc định (Phần 8.3).

  BƯỚC 7: Giải quyết tham chiếu @N trong Mô tả và Ghi chú (Phần 9.1).

  BƯỚC 8: Giải quyết tham chiếu đích, xác định AND-join và tạo cạnh ảo (Phần 9.2-9.3).
    8a. Với mỗi cạnh trong danh sách Cạnh thô có dich = "x.y": kiểm tra x.y tồn tại trong danh sách Nút. Nếu không → lỗi E22.
    8b. Với mỗi Nút có nhiều Cạnh điều khiển luồng đi vào: đánh dấu Nút đó là AND-join.
    8c. Với mỗi nhóm Điểm nối: tạo cạnh ảo từ mỗi Điểm thu đến Điểm phát cùng nhóm, với loai_canh = "invisible", la_ao = true, nhan = null, la_mac_dinh = false.
        Cạnh ảo không hiển thị, được phép cắt ngang qua mọi Đường luồng, Đường nối hoặc khối hình vẽ khác và không chịu bất kỳ giới hạn bố cục nào.
    8d. Phát cảnh báo W01 cho mỗi Nút không phải `1.1`, không thuộc nhóm {Cổng vào, Cổng phụ đầu vào, Điểm thu} mà không có cạnh đi vào.

  BƯỚC 9: Kiểm tra tính toàn vẹn (Phần 10).

  BƯỚC 10: Xuất đồ thị luồng (danh sách Nút + danh sách Cạnh + metadata Vai trò/Giai đoạn).
```

## Phần 13 - Danh sách lỗi và cảnh báo

### Lỗi nghiêm trọng (dừng phân tích)

| Mã | Điều kiện |
|---|---|
| E01 | Số @N Vai trò không liên tục. |
| E02 | Tên viết tắt của hai Vai trò trùng nhau. |
| E03 | Tên viết tắt không hợp lệ: phải toàn chữ hoa và không chứa khoảng trắng. |
| E04 | Khai báo Vai trò xuất hiện sau khai báo Giai đoạn hoặc Nút. |
| E05 | Số Giai đoạn không liên tục. |
| E06 | Khai báo Giai đoạn khi chưa có Vai trò, hoặc dùng @N chưa được khai báo là Vai trò. |
| E07 | Dòng khai báo Giai đoạn có ký tự sau `@N Gx` (không phải bình luận). |
| E08 | Phần `x` trong ID Nút `x.y` khác số thứ tự Giai đoạn hiện tại. |
| E09 | Số thứ tự `y` của Nút không liên tục trong Giai đoạn. |
| E10 | ID Nút trùng lặp trong tài liệu. |
| E11 | Dòng không nhận dạng được trong vùng nội dung quy trình. |
| E12 | Mô tả không bọc trong `"..."` nhưng chứa ký tự cú pháp. |
| E13 | Ghi chú `~...~` xuất hiện sau `->` hoặc `__` của Nút. |
| E14 | Quyết định có ít hơn hai nhánh. |
| E15 | Nhánh Quyết định không có nhãn. |
| E16 | Hai nhánh Quyết định có cùng nhãn. |
| E17 | Hai nhánh Quyết định trỏ đến cùng đích. |
| E18 | Quyết định có nhánh trỏ đến đích `0`. |
| E19 | Một nhóm Điểm nối không có đúng một Điểm phát. |
| E20 | Đường luồng giữa hai Điểm nối cùng nhóm. |
| E21 | Token độc lập `@N` trong Mô tả hoặc Ghi chú không khớp Vai trò nào đã khai báo. |
| E22 | Đích `x.y` trong Đường luồng không tồn tại trong danh sách Nút. |
| E23 | Hai nhánh AND-fork từ cùng Nút trỏ đến cùng đích. |
| E24 | Quyết định có cạnh đi ra không có nhãn. |
| E25 | Cổng phụ đầu vào và đầu ra có cùng Mô tả. |
| E26 | `->` hoặc `__` trên dòng riêng không gắn được với Nút nào. |
| E27 | Cổng vào tường minh đầu tiên không được khai báo tại Nút `1.1`. |
| E28 | Dấu `"` mở không được khép trên cùng dòng và không phải ký tự duy nhất trên dòng đó. Nếu muốn viết nhiều dòng, dấu `"` mở phải đứng một mình trên dòng riêng (xem cú pháp 6.2). |
| E29 | Dấu `~` mở không được khép trên cùng dòng và không phải ký tự duy nhất trên dòng đó. Nếu muốn viết nhiều dòng, dấu `~` mở phải đứng một mình trên dòng riêng (xem cú pháp 7.2). |

### Cảnh báo (tiếp tục phân tích)

| Mã | Điều kiện |
|---|---|
| W01 | Nút `x.y` không có cạnh đi vào, không phải `1.1`, và không thuộc nhóm {Cổng vào, Cổng phụ đầu vào, Điểm thu}: Nút sẽ không được vẽ trên sơ đồ. |
| W02 | Trailing comma sau đích cuối cùng trong danh sách AND-fork: bỏ qua. |

## Phần 14 - Đầu ra của Parser

Parser xuất ra một cấu trúc dữ liệu (JSON, AST, hoặc object graph) chứa đủ thông tin để renderer hoặc bất kỳ công cụ tiêu thụ nào tái tạo chính xác quy trình:

```json
  {
    "vai_tro": [
      {
        "so": 1,
        "ten_day_du": "Nhân viên kinh doanh",
        "ten_viet_tat": "NVKD"
      }
    ],
    "giai_doan": [
      { "so": 1, "vai_tro": 1 }
    ],
    "nut": [
      {
        "id": "1.1",
        "loai": "Tac_vu",
        "mo_ta": "Tiếp nhận đơn hàng từ khách",
        "ghi_chu": null,
        "giai_doan": 1,
        "vai_tro": 1,
        "huy_toan_bo_luong_ra": false
      }
    ],
    "canh": [
      {
        "tu": "1.2",
        "dich": "z",
        "nhan": "Không",
        "loai_canh": "flowline",
        "la_mac_dinh": false,
        "la_ao": false
      }
    ],
    "nhom_diem_noi": [],
    "cong_vao_mac_dinh": null,
    "cong_ra_mac_dinh": "z"
  }
```
