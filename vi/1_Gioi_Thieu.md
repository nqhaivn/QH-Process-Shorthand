
# CHƯƠNG 1: GIỚI THIỆU

## LỜI MỞ ĐẦU

Trong môi trường làm việc hiện đại, quy trình là xương sống của mọi tổ chức. Tuy nhiên, việc mô tả quy trình thường đối mặt với một nghịch lý: viết quá chi tiết thì dài dòng, khó đọc; viết quá ngắn gọn thì thiếu chính xác, khó hiểu.

**Ngôn ngữ Tốc ký Quy trình QH** (QH Process Shorthand, hay QHPS) ra đời để giải quyết nghịch lý này.

QHPS là một Domain-Specific Language (DSL) - ngôn ngữ chuyên biệt cho lĩnh vực mô tả quy trình. Lấy cảm hứng từ các ký hiệu chuẩn ISO 5807 nhưng được tối giản hóa triệt để, cho phép bất kỳ ai cũng có thể viết một quy trình hoàn chỉnh trong vài phút bằng giấy bút hoặc gõ văn bản, đồng thời đọc và hiểu luồng quy trình ngay lập tức.

QHPS không nhằm thay thế BPMN 2.0 hay các chuẩn quy trình chuyên nghiệp khác, mà là một lựa chọn nhẹ, dễ học hơn, phù hợp với nhịp độ làm việc hàng ngày, đặc biệt khi bạn cần ghi lại quy trình nhanh mà không muốn xử lý quá nhiều ký hiệu phức tạp.

## PHẦN A - TỔNG QUAN

### A.1 Lý do xây dựng ngôn ngữ

Qua khảo sát thực tế trên nhiều quy trình, tác giả nhận thấy có bốn vấn đề chính dẫn đến việc xây dựng ngôn ngữ này.

**Thứ nhất,** quy trình được viết bằng văn bản tự do thường dài dòng, cấu trúc lỏng lẻo, khó theo dõi luồng xử lý. Một quy trình 10 bước có thể mất 3-4 trang giấy, người đọc dễ bị lạc.

**Thứ hai,** sơ đồ quy trình (flowchart) tuy trực quan nhưng mất rất nhiều thời gian để vẽ, đặc biệt là khi cần chỉnh sửa. Một thay đổi nhỏ có thể kéo theo việc vẽ lại toàn bộ.

**Thứ ba,** các chuẩn chuyên nghiệp như BPMN 2.0 quá phức tạp với hơn 100 ký hiệu khác nhau. Điều này tạo ra rào cản lớn cho người không chuyên.

**Thứ tư,** không có ngôn ngữ trung gian nào đủ linh hoạt để vừa dễ viết, vừa dễ đọc, vừa dễ chuyển đổi sang các định dạng khác như sơ đồ.

QHPS được xây dựng để lấp đầy khoảng trống đó.

### A.2 Mục tiêu của ngôn ngữ

Ngôn ngữ này hướng đến năm mục tiêu chính.

**Tốc ký:** có thể viết nhanh bằng tay trên giấy hoặc gõ trên máy tính, không cần công cụ đặc biệt.

**Trực quan:** sử dụng các ký hiệu thân thiện với thị giác con người, như dấu ? là câu hỏi/điểm kiểm tra, dấu -> là mũi tên đi tiếp.

**Dễ học:** chỉ có 12 ký hiệu cốt lõi, có thể làm quen trong 15 phút.

**Chặt chẽ:** loại bỏ các lỗi mơ hồ, chồng chéo, khó hiểu thường gặp trong văn bản tự do.

**Dễ vẽ:** cấu trúc rõ ràng đến mức có thể viết phần mềm phiên dịch sang sơ đồ hoặc các định dạng khác.

### A.3 Đối tượng sử dụng

| **Đối tượng**       | **Nhu cầu**       | **Lợi ích nhận được**        |
|-----------      |---------      |-------------------       |
| **Chuyên viên quy trình**    | Viết nhanh, sửa dễ    | Giảm 50-70% thời gian soạn thảo      |
| **Trưởng bộ phận**      | Phê duyệt, đánh giá quy trình | Nắm luồng chính trong 30 giây     |
| **Nhân viên vận hành**     | Thực hiện đúng quy trình   | Hướng dẫn ngắn gọn, dễ hiểu      |
| **Chuyên viên đảm bảo chất lượng**  | Kiểm tra, đánh giá quy trình  | Phát hiện lỗi logic nhanh chóng     |
| **Lãnh đạo**        | Giám sát, cải tiến quy trình  | Có cái nhìn tổng thể, dễ phát hiện điểm nghẽn |

### A.4 Phạm vi áp dụng

**Phù hợp:**

QHPS phù hợp với các quy trình nghiệp vụ nội bộ như hành chính, nhân sự, kế toán, kinh doanh. Quy trình vận hành như xử lý đơn hàng, chăm sóc khách hàng, sản xuất đơn giản. Quy trình kiểm soát, phê duyệt và quy trình đào tạo, hướng dẫn nghiệp vụ.

**Không phù hợp:**

Ngôn ngữ này không phù hợp với quy trình kỹ thuật yêu cầu ký hiệu chuyên ngành như lập trình, kỹ thuật điện, cơ khí. Cũng không phù hợp với quy trình có quá nhiều logic phức tạp như xử lý nhiều luồng song song với nhiều điều kiện đan xen.

## PHẦN B - ĐIỂM MẠNH VÀ GIÁ TRỊ CỐT LÕI

### B.1 So sánh với các phương pháp khác

| **Tiêu chí**        | **Văn bản tự do**  | **Flowchart tay** | **BPMN 2.0**   | **Ngôn ngữ sơ đồ khác** | **Ngôn ngữ QHPS**  |
|----------       |--------------- |---------------|----------  |--------    |---------------- |
| **Thời gian viết (10 bước)**    | 20 phút    | 45 phút    | 60 phút    | 15 phút     | 5 phút    |
| **Thời gian đọc (nắm luồng)**   | 3-5 phút    | 30 giây    | 1-2 phút   | 2 phút     | 30 giây    |
| **Số ký hiệu cần nhớ**     | Không có    | Khoảng 15   | Hơn 100    | Khoảng 15    | 12          |
| **Dễ sửa khi thay đổi**     | Dễ     | Khó (vẽ lại)  | Khó (cần tool)| Dễ      | Rất dễ    |
| **Có thể viết tay**      | Có        | Không (cần vẽ)| Không    | Có      | Có     |
| **Có thể parse bằng máy**    | Rất khó    | Không    | Có (XML)   | Tương đối    | Dễ     |
| **Thân thiện với người không chuyên** | Rất thân thiện | Thân thiện  | Quá phức tạp  | Không thân thiện  | Rất thân thiện |

Ghi chú: Parse là khả năng máy tính đọc hiểu cấu trúc của văn bản mà không cần con người can thiệp.

### B.2 Ví dụ cụ thể: từ cách viết đến kết quả

Để dễ hình dung, hãy so sánh một quy trình thực tế: "Xử lý đơn nghỉ phép của nhân viên". Được viết bằng hai ngôn ngữ cho sơ đồ nổi tiếng nhất và nhiều người sử dụng nhất hiện nay là Mermaid và PlantUML với QHPS.

**B.2.1 Quy trình được viết bằng Mermaid**

```
flowchart TD
    %% Định nghĩa các node
    Start([Bắt đầu])
    End1([Từ chối])
    End2([Hoàn tất])

    NV1["Gửi đơn nghỉ phép"]
    TP1["Xem xét đơn"]
    TP2{"Đơn hợp lệ?"}
    HC1["Cập nhật vào hệ thống"]
    HC2["Lưu hồ sơ"]

    %% Luồng
    Start --> NV1
    NV1 --> TP1
    TP1 --> TP2

    TP2 -- Không --> End1
    TP2 -- Có --> HC1
    HC1 --> HC2
    HC2 --> End2
```

**B.2.2 Quy trình được viết bằng PlantUML**

```
@startuml
|Nhân viên|
start
:Gửi đơn nghỉ phép;
|Trưởng phòng|
:Xem xét đơn;
if (Đơn hợp lệ?) then (Không)
    stop
else (Có)
    |Hành chính|
    :Cập nhật vào hệ thống;
    :Lưu hồ sơ;
    stop
endif
@enduml
```

**B.2.3 Quy trình được viết bằng QHPS**

```
@1 Nhân viên (NV)
@2 Trưởng phòng (TP)
@3 Hành chính (HC)

@1 G1
1.1 Gửi đơn nghỉ phép -> 2.1

@2 G2
2.1 Xem xét đơn
2.2 ? Đơn hợp lệ?
    'Không' -> ./ Từ chối
    'Có' -> 3.1

@3 G3
3.1 Cập nhật vào hệ thống
3.2 Lưu hồ sơ -> ./ Hoàn tất
```

**B.2.4 Sơ đồ kết quả**

```
     Nhân viên (NV)         Trưởng phòng (TP)    Hành chính (HC)
┌─────────────────────┐
│ Gửi đơn nghỉ phép   │
└──────────┬──────────┘
           └───────────────────────┐                
                                   │
                          ┌────────▼─────────┐
                          │ Xem xét đơn      │
                          └────────┬─────────┘
                                   │
                          /────────▼─────────\
                          │ Đơn hợp lệ?      │
                          \──────┬──────┬────/
                                 │      │Có
                                 │      └──────────────┐
                          Không  │                     │ 
                          ┌──────▼──┐           ┌──────▼──────┐         
                          │ Từ chối │           │ Cập nhật vào│            
                          └─────────┘           │ hệ thống    │            
                                                └──────┬──────┘
                                                       │
                                                ┌──────▼──────┐
                                                │ Lưu hồ sơ   │
                                                └──────┬──────┘
                                                       │
                                                ┌──────▼──────┐
                                                │ Hoàn tất    │
                                                └─────────────┘
```

**B.2.5 Nhận xét nhanh về QHPS từ ví dụ**

- Ngắn hơn các phương pháp khác.
- Văn bản “sạch” hơn, gần gũi với ngôn ngữ tự nhiên.
- Người đọc chỉ mất 15 giây để nắm luồng chính.
- Có thể vẽ sơ đồ nhanh chóng.
- Ngay cả khi chưa học về QHPS, người đọc vẫn có thể hiểu được phần lớn nội dung.

### B.3 Các ưu điểm chính

**Tối giản nhưng đủ mạnh**

Chỉ dùng 12 ký hiệu cơ bản, nhưng vẫn biểu diễn được đầy đủ các cấu trúc cần thiết cho quy trình thực tế: tuần tự, rẽ nhánh (XOR), song song (AND-fork và AND-join), lặp, nhiều điểm bắt đầu, nhiều điểm kết thúc. Không cần các khái niệm phức tạp như event, gateway, message flow, data object.

Thực tế với phần lớn sơ đồ quy trình phòng ban, người viết chỉ cần học 4 ký hiệu: `@` `?` `->` và `./`.

**Vừa đọc được bằng mắt người, vừa đọc được bằng máy**

Cú pháp tuyến tính, không phụ thuộc vào bố cục không gian. Mắt người có thể nhanh chóng xác định bước, tác vụ, quyết định, nhánh và luồng. Máy tính có thể dễ dàng parse để kiểm tra tính hợp lệ hoặc sinh sơ đồ tự động.

**Hỗ trợ đầy đủ các cấu trúc điều khiển của quy trình thực tế**

QHPS hỗ trợ bốn cấu trúc chính: tuần tự (viết liên tiếp các tác vụ), rẽ nhánh (dùng x.y ?), song song, và lặp (dùng -> để quay lui). Bốn cấu trúc này bao phủ hầu hết mọi tình huống thực tế.

**Không yêu cầu công cụ chuyên dụng**

Có thể viết bằng bất kỳ trình soạn thảo văn bản nào hoặc bằng tay trên giấy. Lưu trữ dưới dạng file .txt, commit lên Git, gửi qua email, in ra giấy. Khi thay đổi, chỉ cần sửa vài dòng văn bản, không cần mở lại phần mềm vẽ.

**Phù hợp với tốc độ làm việc hàng ngày**

Quy trình xử lý đơn hàng điển hình (4 bước, 2 quyết định, 1 song song) có thể viết trong 5 phút. So với vẽ sơ đồ, thời gian tiết kiệm được 3-5 lần. Khuyến khích ghi chép quy trình thường xuyên, cập nhật nhanh khi có thay đổi, giúp tài liệu quy trình ít bị lỗi thời.

**Cầu nối giữa tự nhiên và hình thức**

QHPS không phải là ngôn ngữ lập trình khô khan, cũng không phải văn bản xuôi mơ hồ. Các ký hiệu được chọn trực quan. Nội dung tác vụ là văn bản thuần túy bằng ngôn ngữ tự nhiên (tiếng Việt có dấu, tiếng Anh,...), không bị ép buộc dùng từ vựng cố định.

### B.4 Bảng lý do lựa chọn ký hiệu và ý nghĩa

| **Ký hiệu** | **Ý nghĩa** | **Lý do chọn** | **Hình vẽ trên sơ đồ** |
| --- | --- | --- | --- |
| **`@`** | Khai báo vai trò | Ký hiệu của Địa chỉ (Address) được dùng trong các email, ký hiệu gắn thẻ/nhắc đến trên mạng xã hội. Dễ nhận ra là "ai đó" | |
| | Tác vụ | Đây là thành phần được sử dụng nhiều nhất, do đó không có ký hiệu để tiện lợi cho người viết quy trình. | ![](../svg/process.svg) |
| **`?`** | Quyết định hoặc rẽ nhánh | Dấu hỏi gợi ngay câu hỏi cần trả lời. Rất trực quan, ai cũng hiểu | ![](../svg/decision.svg) |
| **`/`** | Ngõ vào hoặc ngõ ra (dữ liệu) | Gợi ý về sự phân chia giữa vào và ra. Một cạnh của hình bình hành | ![](../svg/in_out.svg) |
| **`!`** | Quy trình con | Gợi ý về sự khẳng định, đứng vững. Đồng thời liên tưởng đến hình chữ nhật có hai nét kẻ dọc, hình vẽ dành riêng cho Quy trình con | ![](../svg/process_child.svg) |
| **`->`** | Đường luồng hay mũi tên | Tượng trưng trực quan cho hướng đi tiếp theo. Ai cũng hiểu là "đi tiếp" | ![](../svg/flowline.svg) |
| **`./`** | Kết thúc quy trình | Hai ký tự cuối cùng trên bàn phím. Mang ý nghĩa "chấm dứt", "kết thúc" | ![](../svg/start_end.svg) |
| **`\.`** | Bắt đầu quy trình | Ngược lại với kết thúc. Mở đầu cho quy trình | ![](../svg/start_end.svg) |
| **`'...'`** | Nhãn của đường luồng | Dấu nháy đơn, gợi ý rằng nhãn được "gắn lên phía trên" đường luồng. Dễ phân biệt với mô tả thông thường | ![](../svg/flowline_label.svg) |
| **`~...~`** | Ghi chú (không ảnh hưởng luồng) | Gợi hình ảnh "lời thì thầm" nằm bên ngoài luồng chính | ![](../svg/annotation.svg) |
| **`"..."`** | Mô tả nhiều dòng hoặc bọc nội dung chứa ký tự đặc biệt | Dấu nháy kép (") thường được dùng để trích dẫn trong các văn bản | |
| **`()`** | Điểm nối trên cùng trang | Hình tròn gợi ý về Điểm thu và Điểm phát trên cùng một trang sơ đồ | ![](../svg/conector.svg) |
| **`\/`** | Cổng nối sang trang khác | Hai cạnh dưới của hình ngũ giác ngược. Dùng khi cần chuyển sang trang sơ đồ khác | ![](../svg/off_connector.svg) |

### B.5 Những vấn đề đã giải quyết

QHPS giải quyết triệt để năm vấn đề kinh điển của việc viết quy trình.

**1. Mỗi người viết một kiểu, không ai đọc được của ai.**

Giải pháp: QHPS áp đặt cú pháp thống nhất. Mọi tài liệu đúng cú pháp đều có cấu trúc giống nhau.

**2. Không biết bắt đầu từ đâu, viết thiếu hoặc viết thừa.**

Giải pháp: Cấu trúc bắt buộc: khai báo vai trò trước, rồi đến giai đoạn và bước. Có khuôn mẫu riêng cho từng loại bước. Người viết chỉ cần điền nội dung vào khuôn.

**3. Lộn xộn giữa các bước, không rõ ai làm gì.**

Giải pháp: Mỗi bước chỉ thuộc một vai trò duy nhất. Vai trò khai báo tập trung bằng `@N`. Sơ đồ vẽ theo làn tự động phân tách trách nhiệm.

**4. Ghi chú, giải thích làm loãng luồng chính.**

Giải pháp: Ghi chú bọc trong `~...~`, không tham gia luồng điều khiển. Trên sơ đồ vẽ bằng khung nét đứt, tách biệt hoàn toàn với luồng chính.

**5. Quyết định nhiều nhánh dài dòng, mất nhiều dòng cho một rẽ nhánh.**

Giải pháp: Dùng dấu `?` để báo hiệu rẽ nhánh. Các nhánh liệt kê ngay bên dưới, mỗi nhánh chỉ một dòng. Tiết kiệm thời gian hơn một nửa so với văn bản tự do.

### B.6 Kết quả đạt được khi áp dụng

Qua thử nghiệm trên một số quy trình thực tế, QHPS cho thấy các kết quả sau.

**Về tốc độ:**

Thời gian viết một quy trình trung bình 10-15 bước giảm từ 15-20 phút (văn bản tự do) xuống còn 5-8 phút (QHPS). Thời gian chỉnh sửa quy trình khi có thay đổi giảm từ 10 phút xuống còn 3 phút.

**Về chất lượng:**

- Loại bỏ lỗi mơ hồ (thiếu điều kiện rõ ràng, nhảy thiếu bước).
- Loại bỏ lỗi chồng chéo trách nhiệm (một bước giao cho hai vai trò) nhờ quy tắc "mỗi bước một vai trò".
- Loại bỏ lỗi lặp vô hạn (quên rẽ nhánh hoặc kết thúc).

**Về khả năng kết xuất (vẽ):**

Một quy trình QHPS với 15 bước có thể được vẽ thành sơ đồ chỉ trong 10-15 phút nếu vẽ bằng tay, hoặc dưới 5 giây nếu có công cụ tự động. 100% các quy trình viết đúng cú pháp đều có thể chuyển đổi thành sơ đồ mà không cần "sửa lỗi thủ công".
