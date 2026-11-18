# BÀI TẬP VẬN DỤNG CHUYÊN SÂU

## Thiết kế giải pháp Prompting ứng dụng thực tế cho Kỹ thuật Prompting Nâng cao (Logic & Chuỗi)

### Thông tin sinh viên

-   **Họ và tên:** Võ Xuân Thịnh
-   **Mã sinh viên:** N26DTCN032
-   **Lớp:** HCMKS26CNTT2
-   **Ngành:** Công nghệ thông tin
-   **Chủ đề:** Tự động hóa quy trình xử lý đơn hàng tại cửa hàng bán lẻ

------------------------------------------------------------------------

## 1. Phân tích bài toán thực tế

### 1.1. Bối cảnh

Hiện nay, các cửa hàng thường nhận đơn hàng từ khách qua Facebook, Zalo
hoặc trực tiếp tại cửa hàng. Nhân viên phải kiểm tra thông tin khách
hàng, sản phẩm, số lượng, giá tiền và tình trạng đơn hàng.

Nếu xử lý thủ công, cửa hàng có thể gặp một số vấn đề:

-   Nhập sai số lượng sản phẩm.
-   Thiếu số điện thoại hoặc địa chỉ khách hàng.
-   Tính sai tổng tiền đơn hàng.
-   Sản phẩm không đủ số lượng trong kho.
-   Đơn hàng bị gián đoạn khi thông tin đầu vào không đầy đủ.

Vì vậy, em đề xuất sử dụng AI kết hợp các kỹ thuật Prompting nâng cao để
hỗ trợ kiểm tra và xử lý đơn hàng tự động.

### 1.2. Mục tiêu giải pháp

-   Tiếp nhận thông tin đơn hàng từ khách.
-   Kiểm tra tính đầy đủ và hợp lệ của dữ liệu.
-   Tính tổng tiền đơn hàng.
-   Kiểm tra tình trạng sản phẩm.
-   Phân loại đơn hàng theo trạng thái.
-   Thông báo những thông tin còn thiếu hoặc sai lệch.

## 2. Bước 1: Thiết kế giải pháp I/O

### 2.1. Input -- Dữ liệu đầu vào

AI sẽ nhận các thông tin sau:

  Trường dữ liệu   Nội dung mẫu
  ---------------- --------------------
  Mã đơn hàng      DH001
  Tên khách hàng   Nguyễn Văn An
  Số điện thoại    0901234567
  Địa chỉ          TP. Hồ Chí Minh
  Tên sản phẩm     Tai nghe Bluetooth
  Số lượng         2
  Đơn giá          250.000đ
  Tồn kho          10 sản phẩm

Ngoài ra, hệ thống cần có dữ liệu về quy định cửa hàng như giá bán, số
lượng tồn kho và thông tin giao hàng.

### 2.2. Output -- Kết quả đầu ra

AI cần trả về kết quả theo cấu trúc:

  Nội dung               Kết quả mong muốn
  ---------------------- --------------------
  Mã đơn hàng            DH001
  Thông tin khách hàng   Nguyễn Văn An
  Sản phẩm               Tai nghe Bluetooth
  Số lượng               2
  Tổng tiền              500.000đ
  Kiểm tra kho           Đủ hàng
  Trạng thái             Đủ điều kiện xử lý
  Ghi chú                Không có lỗi

### 2.3. Quy trình xử lý bằng Prompt Chaining

Em chia quy trình thành 4 bước để AI xử lý lần lượt, tránh việc đưa ra
kết quả khi chưa kiểm tra dữ liệu.

1.  **Bước 1 -- Tiếp nhận:** Nhận thông tin đơn hàng từ khách hàng.
2.  **Bước 2 -- Kiểm tra:** Kiểm tra dữ liệu thiếu, sai định dạng hoặc
    bất hợp lý.
3.  **Bước 3 -- Tính toán:** Tính tổng tiền và kiểm tra số lượng tồn
    kho.
4.  **Bước 4 -- Xuất kết quả:** Phân loại đơn hàng và đưa ra thông báo.

## 3. Bước 2: Thiết kế Prompt thực thi

### 3.1. Prompt hoàn chỉnh

# PROMPT: HỆ THỐNG HỖ TRỢ XỬ LÝ ĐƠN HÀNG TỰ ĐỘNG

#### 1. Vai trò (Role)

Bạn là trợ lý AI hỗ trợ quản lý và xử lý đơn hàng cho một cửa hàng bán
lẻ. Nhiệm vụ của bạn là kiểm tra thông tin, tính toán và phân loại đơn
hàng một cách chính xác.

#### 2. Mục tiêu (Goal)

Tiếp nhận dữ liệu đơn hàng, kiểm tra tính hợp lệ, tính tổng tiền và xác
định đơn hàng có đủ điều kiện xử lý hay không.

#### 3. Dữ liệu đầu vào (Input)

Thông tin đơn hàng gồm:

-   Mã đơn hàng: `[Mã đơn]`
-   Tên khách hàng: `[Tên khách]`
-   Số điện thoại: `[Số điện thoại]`
-   Địa chỉ: `[Địa chỉ]`
-   Tên sản phẩm: `[Tên sản phẩm]`
-   Số lượng: `[Số lượng]`
-   Đơn giá: `[Đơn giá]`
-   Số lượng tồn kho: `[Tồn kho]`

#### 4. Quy trình xử lý (Process)

**Bước 1: Kiểm tra dữ liệu**

-   Kiểm tra các trường thông tin có bị bỏ trống không.
-   Kiểm tra số điện thoại có đúng định dạng không.
-   Số lượng và đơn giá phải là số hợp lệ, lớn hơn 0.
-   Số lượng tồn kho không được âm.
-   Nếu thiếu dữ liệu, hãy ghi rõ trường nào đang thiếu.
-   Không tự ý đoán hoặc tạo ra thông tin còn thiếu.

**Bước 2: Phân tích và tính toán**

-   Nếu dữ liệu hợp lệ, tính tổng tiền theo công thức:\
    **Tổng tiền = Số lượng × Đơn giá.**
-   So sánh số lượng khách đặt với số lượng tồn kho.
-   Nếu số lượng đặt lớn hơn tồn kho, thông báo không đủ hàng.

**Bước 3: Phân loại đơn hàng**

Chỉ phân loại theo các trạng thái:

-   **ĐỦ ĐIỀU KIỆN XỬ LÝ:** Dữ liệu hợp lệ và đủ hàng.
-   **CHỜ BỔ SUNG:** Thiếu thông tin cần thiết.
-   **KHÔNG HỢP LỆ:** Dữ liệu sai định dạng hoặc giá trị bất hợp lý.
-   **KHÔNG ĐỦ HÀNG:** Số lượng đặt vượt quá tồn kho.

**Bước 4: Xử lý lỗi và đứt gãy**

-   Nếu bước kiểm tra dữ liệu thất bại, không thực hiện bước tính toán.
-   Nếu thiếu đơn giá hoặc số lượng, không tự ý tính tổng tiền.
-   Nếu tồn kho bị thiếu, ghi nhận là chưa thể xác nhận tình trạng hàng.
-   Nếu phát hiện thông tin mâu thuẫn, yêu cầu nhân viên kiểm tra lại.
-   Nếu có nhiều lỗi, liệt kê tất cả lỗi thay vì chỉ báo một lỗi.
-   Không xác nhận đơn hàng đã được thanh toán hoặc giao hàng nếu chưa
    có dữ liệu chứng minh.

#### 5. Yêu cầu đầu ra (Output)

Trả kết quả theo cấu trúc:

**A. Thông tin đơn hàng**

-   Mã đơn:
-   Khách hàng:
-   Sản phẩm:
-   Số lượng:
-   Đơn giá:

**B. Kết quả kiểm tra**

-   Tình trạng dữ liệu:
-   Kết quả kiểm tra số điện thoại:
-   Tình trạng tồn kho:
-   Tổng tiền:

**C. Trạng thái đơn hàng**

-   Trạng thái:
-   Lý do:
-   Lỗi phát hiện:
-   Hướng xử lý tiếp theo:

#### 6. Nguyên tắc

-   Ưu tiên tính chính xác hơn tốc độ.
-   Thực hiện các bước theo đúng thứ tự.
-   Chỉ đưa ra kết quả dựa trên dữ liệu được cung cấp.
-   Không tự suy đoán thông tin còn thiếu.
-   Cung cấp giải thích ngắn gọn về căn cứ đưa ra kết quả, không cần
    trình bày suy nghĩ nội bộ chi tiết.
-   Trước khi kết thúc, kiểm tra lại phép tính và trạng thái đơn hàng.

### 3.2. Giải thích các kỹ thuật được sử dụng

  -----------------------------------------------------------------------
  Kỹ thuật                            Cách áp dụng trong bài
  ----------------------------------- -----------------------------------
  Chain-of-Thought                    Yêu cầu AI phân tích theo từng
                                      bước, kiểm tra dữ liệu và giải
                                      thích căn cứ kết quả.

  Prompt Chaining                     Chia thành 4 bước: tiếp nhận, kiểm
                                      tra, tính toán và xuất kết quả.

  Error Handling (xử lý lỗi)          Phát hiện dữ liệu thiếu, sai định
                                      dạng, tồn kho không đủ và yêu cầu
                                      bổ sung.
  -----------------------------------------------------------------------

## 4. Bước 3: Chạy thử Prompt

Để kiểm tra prompt có hoạt động tốt không, em sử dụng 3 trường hợp dữ
liệu khác nhau.

### Trường hợp 1: Đơn hàng hợp lệ

**Input chạy thử:**

``` text
Mã đơn hàng: DH001
Tên khách hàng: Nguyễn Văn An
Số điện thoại: 0901234567
Địa chỉ: TP. Hồ Chí Minh
Tên sản phẩm: Tai nghe Bluetooth
Số lượng: 2
Đơn giá: 250000
Tồn kho: 10
```

**Kết quả phản hồi của AI:**

### KẾT QUẢ ĐƠN HÀNG DH001

**A. Thông tin đơn hàng**

-   Khách hàng: Nguyễn Văn An
-   Sản phẩm: Tai nghe Bluetooth
-   Số lượng: 2
-   Đơn giá: 250.000đ

**B. Kết quả kiểm tra**

-   Dữ liệu: Đầy đủ.
-   Số điện thoại: Hợp lệ theo định dạng kiểm tra.
-   Tồn kho: 10 sản phẩm, đủ hàng.
-   Tổng tiền: 2 × 250.000 = **500.000đ**.

**C. Trạng thái đơn hàng**

**ĐỦ ĐIỀU KIỆN XỬ LÝ**

-   Lý do: Thông tin đầy đủ, số lượng đặt không vượt quá tồn kho.
-   Hướng xử lý: Chuyển đơn hàng cho nhân viên xác nhận.

### Trường hợp 2: Đơn hàng bị thiếu dữ liệu

**Input chạy thử:**

``` text
Mã đơn hàng: DH002
Tên khách hàng: Trần Minh Anh
Số điện thoại:
Địa chỉ: TP. Hồ Chí Minh
Tên sản phẩm: Chuột không dây
Số lượng: 2
Đơn giá: 150000
Tồn kho: 8
```

**Kết quả phản hồi của AI:**

### KẾT QUẢ ĐƠN HÀNG DH002

**A. Thông tin đơn hàng**

-   Khách hàng: Trần Minh Anh
-   Sản phẩm: Chuột không dây
-   Số lượng: 2
-   Đơn giá: 150.000đ

**B. Kết quả kiểm tra**

-   Dữ liệu: Chưa đầy đủ.
-   Số điện thoại: Bị bỏ trống.
-   Tồn kho: Đủ hàng.
-   Tổng tiền dự kiến: 300.000đ.

**C. Trạng thái đơn hàng**

**CHỜ BỔ SUNG**

-   Lý do: Khách hàng chưa cung cấp số điện thoại.
-   Hướng xử lý: Yêu cầu bổ sung số điện thoại trước khi tiếp tục xử lý
    đơn hàng.

### Trường hợp 3: Đơn hàng không đủ số lượng tồn kho

**Input chạy thử:**

``` text
Mã đơn hàng: DH003
Tên khách hàng: Lê Hoàng Nam
Số điện thoại: 0912345678
Địa chỉ: TP. Hồ Chí Minh
Tên sản phẩm: Bàn phím cơ
Số lượng: 15
Đơn giá: 500000
Tồn kho: 5
```

**Kết quả phản hồi của AI:**

### KẾT QUẢ ĐƠN HÀNG DH003

**A. Thông tin đơn hàng**

-   Khách hàng: Lê Hoàng Nam
-   Sản phẩm: Bàn phím cơ
-   Số lượng đặt: 15
-   Đơn giá: 500.000đ

**B. Kết quả kiểm tra**

-   Dữ liệu: Đầy đủ.
-   Số điện thoại: Hợp lệ theo định dạng kiểm tra.
-   Tồn kho: Chỉ còn 5 sản phẩm.
-   Số lượng thiếu: 10 sản phẩm.
-   Tổng tiền dự kiến: 7.500.000đ.

**C. Trạng thái đơn hàng**

**KHÔNG ĐỦ HÀNG**

-   Lý do: Số lượng khách đặt vượt quá số lượng tồn kho.
-   Hướng xử lý: Thông báo khách hàng điều chỉnh số lượng hoặc chờ cửa
    hàng nhập thêm hàng.

*Lưu ý: Đây là các kết quả chạy thử minh họa theo prompt đã thiết kế.
Khi thực hiện, bạn có thể copy prompt vào ChatGPT hoặc Gemini và thay dữ
liệu đầu vào để lấy phản hồi thực tế.*

## 5. Phân tích ràng buộc và các trường hợp lỗi

Trong quá trình thiết kế, em dự đoán một số tình huống có thể xảy ra:

  Tình huống                     Cách AI xử lý
  ------------------------------ -----------------------------
  Thiếu tên khách hàng           Yêu cầu bổ sung thông tin.
  Số điện thoại sai định dạng    Thông báo cần kiểm tra lại.
  Số lượng bằng 0                Báo dữ liệu không hợp lệ.
  Đơn giá bị bỏ trống            Không tính tổng tiền.
  Tồn kho bị bỏ trống            Không xác nhận đủ hàng.
  Số lượng đặt lớn hơn tồn kho   Báo không đủ hàng.
  Dữ liệu bị mâu thuẫn           Yêu cầu nhân viên xác minh.

## 6. Đánh giá giải pháp

### Ưu điểm

-   Giúp giảm thời gian kiểm tra đơn hàng.
-   Hạn chế sai sót khi nhập và tính toán dữ liệu.
-   Có thể xử lý nhiều tình huống khác nhau.
-   Dễ dàng mở rộng cho các cửa hàng bán lẻ.

### Hạn chế

-   AI chỉ xử lý chính xác khi dữ liệu đầu vào được cung cấp đầy đủ.
-   AI không thể tự biết số lượng hàng thực tế nếu không kết nối với hệ
    thống quản lý kho.
-   Kết quả vẫn cần nhân viên kiểm tra trước khi xác nhận.

## 7. Hướng phát triển

Trong tương lai, em có thể phát triển giải pháp này thành một hệ thống
quản lý đơn hàng có khả năng kết nối với Excel hoặc Google Sheets để lấy
dữ liệu sản phẩm và tồn kho tự động.

Ngoài ra, có thể kết hợp với chatbot trên Facebook hoặc Zalo để tiếp
nhận đơn hàng, kiểm tra thông tin và hỗ trợ khách hàng nhanh hơn.

## 8. Kết luận về bài thực hành

Thông qua bài tập này, em hiểu được cách ứng dụng các kỹ thuật Prompting
nâng cao vào một công việc thực tế. Việc kết hợp Chain-of-Thought,
Prompt Chaining và xử lý lỗi giúp AI làm việc theo trình tự rõ ràng, hạn
chế bỏ sót dữ liệu và đưa ra kết quả phù hợp hơn.

Đặc biệt, em nhận thấy việc thiết kế prompt không chỉ là đặt câu hỏi cho
AI mà còn cần xác định đầu vào, đầu ra, quy trình xử lý và những tình
huống có thể xảy ra.ss
