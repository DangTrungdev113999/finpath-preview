# FINPATH AI CO-PILOT — BRIEF SPRINT 1
# NHÓM A: NỀN TẢNG (2 lập trình viên)

> **Phiên bản:** 2.0 — 18/03/2026
> **Người soạn:** Việt Anh (Product Design Lead)
> **Sprint:** 1 (10 ngày làm việc)
> **Team:** 2 lập trình viên

---

## QUY TẮC XUYÊN SUỐT

### AI Voice Tier — 2 level duy nhất

| | Retail (< 2 năm) | Pro (> 2 năm) |
|--|-------------------|----------------|
| Thuật ngữ | ❌ KHÔNG MA, RSI, breakout, NAV, hold | ✅ Dùng thoải mái, không giải thích |
| Số liệu phức tạp | Phải "dịch" sang bình dân | Nêu thẳng con số |
| Ẩn dụ | Cá mập, tay to, bơm tiền vào | Ngôn ngữ trader: âm nặng, kẹp hàng, thủng nền, bứt nền |
| Giọng | Hướng dẫn, bảo vệ tâm lý | Gọn, bén, thực dụng |

**Bảng phiên dịch cho Retail:**

| Thuật ngữ gốc | Retail nói thế nào |
|---------------|-------------------|
| RSI quá bán | "Giá đang ở vùng nhiều người bán quá tay" |
| Breakout MA50 | "Vượt mốc giá quan trọng" |
| Volume gấp 3x | "Dòng tiền đổ vào gấp 3 lần bình thường" |
| Khối ngoại bán ròng | "Tiền nước ngoài đang rút ra" |
| Tự doanh mua ròng | "Tay to trong nước đang gom hàng" |
| Death cross | "Xu hướng giảm dài hạn vừa được xác nhận" |
| Golden cross | "Xu hướng tăng dài hạn vừa được xác nhận" |
| P/E thấp hơn ngành | "Giá đang rẻ hơn so với các mã cùng nhóm" |
| Margin call | "Nhiều người bị ép bán vì vay mua cổ phiếu" |
| Hỗ trợ / kháng cự | "Mốc giá quan trọng" |
| Portfolio drawdown | "Danh mục có thể giảm thêm X%" |
| Net inflow sàn | "Dòng tiền đang đổ vào/rút ra khỏi sàn giao dịch" |
| Funding rate | "Phí giữ lệnh" — nếu cao = nhiều người đang đặt cược tăng |
| DXY tăng | "Đồng đô la Mỹ mạnh lên" |

### 6 Profile (xác định loại dữ liệu ưu tiên)

| Profile | Thị trường | Level | Dữ liệu ưu tiên |
|---------|-----------|-------|-----------------|
| Stock_Retail | Chứng khoán | Retail | Xu hướng, tín hiệu cơ bản, cảnh báo khối lượng |
| Stock_Pro | Chứng khoán | Pro | Vĩ mô, dòng tiền tổ chức, định giá |
| Crypto_Retail | Crypto | Retail | Altcoin hút thanh khoản, tâm lý thị trường |
| Crypto_Pro | Crypto | Pro | On-chain, ví cá mập, dòng tiền sàn |
| Gold_Retail | Vàng | Retail | Giá trong nước, nhịp mua bán |
| Gold_Pro | Vàng | Pro | DXY, lãi suất Fed, tương quan liên thị trường |

**Profile quyết định:**
- Giọng AI (Retail = hướng dẫn / Pro = thực dụng)
- Loại dữ liệu ưu tiên (bảng trên)
- Cách viết thẻ tín hiệu (xem ví dụ bên dưới)
- Bài học AI tập trung phát hiện (VD: Stock_Retail → FOMO/theo tip, Crypto_Pro → quá tải đòn bẩy)

### Giới hạn ký tự thẻ tín hiệu

| Phần | Giới hạn |
|------|---------|
| Tiêu đề | 150 ký tự |
| Số liệu | 160 ký tự |
| Lịch sử (callout box) | 140 ký tự |
| Bối cảnh cá nhân | 130 ký tự |
| Chuỗi giải thích | Visual diagram (3 ô nối mũi tên) |
| Tổng 1 thẻ | ~580-600 ký tự |

---

## PHẦN 1: ONBOARDING — Phụ trách: Thiên, Hưởng

### Mục tiêu
Người dùng mới mở app → trong 60 giây biết Finpath làm gì, chọn xong thị trường + mã quan tâm, và thấy tín hiệu đầu tiên.

### Có 2 luồng
- **Organic Flow** — người dùng mới hoàn toàn
- **Normal Flow** — người dùng đã được hệ thống nhận diện profile (từ quảng cáo, link giới thiệu)

### Quy tắc chung
- Chỉ chọn **1 thị trường** (không chọn nhiều)
- 5 màn hình, mỗi màn 1 hành động duy nhất
- AI "nói" = bong bóng chat, không phải text tĩnh

---

### ORGANIC FLOW

**Màn 1 — Chọn thị trường**

🤖 AI nói:
> "Đâu là thị trường trọng tâm bạn muốn Finpath tập trung dò quét tin tức & dòng tiền?"

3 lựa chọn (chọn 1, dạng thẻ lớn có icon + mô tả):

**[📈 Chứng khoán]**
Theo dõi dòng tiền khối ngoại, tự doanh và Vĩ mô Việt Nam.

**[🪙 Crypto]**
Cập nhật dữ liệu, tin tức, động thái ví lớn.

**[🥇 Vàng & Ngoại hối]**
Theo dõi giá Vàng SJC, sức mạnh USD (DXY) và chính sách tiền tệ.

Hành vi: chọn 1 → viền đổi màu + ✓ → tự chuyển màn 2 sau 0.5s

---

**Màn 2 — Chọn phong cách (nội dung thay đổi theo thị trường)**

👉 **Nếu chọn Chứng khoán:**

🤖 "Để AI đóng vai trò trợ lý hiệu quả nhất, mức độ kinh nghiệm thực chiến của bạn trên thị trường là bao lâu?"

**[Dưới 2 năm]**
Cần AI hỗ trợ nhận diện xu hướng, lọc tín hiệu mua/bán cơ bản và cảnh báo điểm nổ khối lượng.
→ Gán profile: **Stock_Retail**

**[Trên 2 năm]**
Cần AI phân tích Vĩ mô, bóc tách hành vi dòng tiền tổ chức (Smart Money) và dữ liệu định giá.
→ Gán profile: **Stock_Pro**

👉 **Nếu chọn Crypto:**

🤖 "Để AI tùy chỉnh độ phức tạp của dữ liệu, bạn thường ra quyết định giao dịch dựa trên yếu tố nào nhất?"

**[Sóng cộng đồng & Tín hiệu kỹ thuật]**
Ưu tiên phát hiện sớm Altcoin/Memecoin đang hút thanh khoản, cập nhật tâm lý thị trường và điểm vào lệnh ngắn hạn.
→ Gán profile: **Crypto_Retail**

**[Dữ liệu On-chain & Hành vi Cá mập]**
Tập trung rà soát ví tổ chức (Smart Money), theo dõi dòng tiền ra/vào sàn (Net Inflow/Outflow) và bản đồ thanh lý phái sinh.
→ Gán profile: **Crypto_Pro**

👉 **Nếu chọn Vàng & Ngoại hối:**

🤖 "Để AI tùy chỉnh mức độ chuyên sâu của dữ liệu trả về, bạn tiếp cận thị trường này với vị thế nào?"

**[Tích lũy tài sản truyền thống]**
Cần AI hỗ trợ cập nhật nhanh diễn biến giá trong nước và tối ưu hóa các nhịp mua/bán tích trữ.
→ Gán profile: **Gold_Retail**

**[Phân tích Vĩ mô & Xu hướng toàn cầu]**
Bám sát sức mạnh USD (DXY), biến động lãi suất Fed và tác động liên thị trường để phòng vệ danh mục.
→ Gán profile: **Gold_Pro**

---

**Màn 3 — Nhập mã quan tâm (nội dung thay đổi theo profile)**

👉 **Stock_Retail:**
🤖 "Nhập 1-2 mã cổ phiếu bạn đang nắm giữ để AI đánh giá rủi ro và xu hướng ngay lúc này."

👉 **Stock_Pro:**
🤖 "Nhập 1-2 cổ phiếu trọng tâm trong danh mục. Hệ thống sẽ truy xuất dữ liệu định giá và động thái dòng tiền tổ chức."

👉 **Crypto_Retail:**
🤖 "Nhập 1-2 Token bạn đang quan tâm nhất. AI sẽ kiểm tra xem dòng tiền đang đổ vào hay rút ra khỏi mã này."

👉 **Crypto_Pro:**
🤖 "Nhập 1-2 Token cốt lõi. Cỗ máy AI sẽ quét dữ liệu On-chain và động thái ví cá mập ngay lập tức."

👉 **Gold_Retail:**
🤖 "Bạn đang quan tâm loại tài sản nào nhất? AI sẽ cảnh báo khi có biến động giá bất thường."
Lựa chọn bấm nhanh: **[Vàng miếng SJC]** / **[Vàng nhẫn 9999]** / **[Tỷ giá USD tự do]**

👉 **Gold_Pro:**
🤖 "Bạn đang phân bổ bao nhiêu % danh mục vào vàng?"
Lựa chọn bấm nhanh: **[Dưới 10%]** / **[10-30%]** / **[Trên 30%]**

👉 **Chứng khoán / Crypto — giao diện nhập:**
- Ô nhập text: "VD: VND, HPG..." (Stock) hoặc "VD: BTC, PEPE..." (Crypto)
- Gợi ý tự động khi gõ (autocomplete)
- Mã đã chọn hiện thành chip, bấm X để xóa
- Chip gợi ý phổ biến theo thị trường:
  - Stock: HPG, FPT, VCB, MWG, VNM
  - Crypto: BTC, ETH, SOL, PEPE, DOGE
- Nút phụ: **[📸 Tải ảnh chụp màn hình danh mục từ app khác]**
  - Dòng phụ: "Finpath sẽ tự động nhận diện và thêm vào danh sách theo dõi của bạn"
- Nút chính: **[Gửi phản hồi]** hoặc **[Bỏ qua]**

---

**Màn 4 — Xin quyền thông báo**

🤖 "Hãy cấp quyền thông báo. AI sẽ gửi báo cáo tác động trực tiếp đến danh mục của bạn vào lúc 7:00 AM sáng mai."

**[🔔 Cấp quyền thông báo]** → popup hệ thống
**[Bỏ qua]** → vào app, nhắc lại sau 3 ngày

---

**Màn 5 — Dòng tin với tín hiệu mẫu**

Hiện ngay 2-3 thẻ tín hiệu phù hợp profile + mã vừa chọn + 1 thẻ chào mừng.

Thẻ chào mừng:
```
⭐ CHÀO MỪNG

"Tôi đã bắt đầu theo dõi {mã} cho bạn.
Khi có tín hiệu đáng chú ý, tôi sẽ gửi ngay.
Sáng mai 7h, bạn sẽ nhận bản tin đầu tiên."
```

Ví dụ tín hiệu mẫu cho Stock_Retail vừa chọn HPG:
```
[TIN TỨC] 🟡                          Tín hiệu mẫu

HPG công bố doanh thu tháng 2 tăng 18%
so với cùng kỳ năm trước

Đây là tín hiệu đầu tiên. Finpath sẽ gửi khi
có điều đáng chú ý ảnh hưởng đến mã bạn theo dõi.

          [💬 Hỏi thêm về HPG]
```

---

### NORMAL FLOW

**Màn 0 — Xác nhận profile**

🤖 "Nhận thấy bạn là nhà đầu tư quan tâm đến {thị trường}, có đúng không?"

**[Đúng vậy]** → nhảy thẳng màn 3 (Nhập mã)
**[Không đúng, tôi muốn tự chọn]** → Organic Flow màn 1

---

## PHẦN 2: TRÍ NHỚ AI — Phụ trách: Chi

### Mục tiêu
AI nhớ mọi thứ về người dùng — danh mục, giao dịch, sai lầm, thói quen — để mỗi tín hiệu gửi đi đều mang bối cảnh cá nhân.

### 4 loại trí nhớ

---

### LOẠI 1 — DANH MỤC ĐẦU TƯ

**Nguồn dữ liệu (theo thứ tự ưu tiên):**

| Nguồn | Khi nào | Độ chính xác |
|-------|---------|-------------|
| Giao dịch thật trong app | Sau khi tính năng ra mắt | Cao nhất |
| Giao dịch thử trong app | Ngay từ đầu | Cao |
| Sao kê PDF/Excel | Khi tải lên | Cao — AI chỉ lưu dòng chắc chắn |
| Ảnh chụp màn hình app khác | Khi tải lên | Trung bình — cần AI đọc ảnh |
| Tự nhập tay | Onboarding + bất kỳ lúc nào | Tùy người dùng |

**Dữ liệu lưu cho mỗi mã — Ví dụ cụ thể:**

```
Mã:           HPG
Loại:         Chứng khoán
Trạng thái:   Đang giữ
Giá mua TB:   26,500
Số lượng:     2,000 cổ phiếu
Ngày mua:     15/01/2026
Giá hiện tại: 27,500 (tự cập nhật)
Lời/lỗ:       +3.8% (tự tính)
Tỷ trọng:     35% danh mục (tự tính)
stop_loss_pct: -15% (user tự đặt, nullable)
target_price:  32,000 (user tự đặt, nullable)
```

**Danh mục khác nhau theo profile:**

| Profile | Dữ liệu đặc thù |
|---------|-----------------|
| Stock_Retail / Stock_Pro | Mã cổ phiếu, giá mua, số lượng |
| Crypto_Retail / Crypto_Pro | Token, giá mua, số lượng, sàn giao dịch |
| Gold_Retail | Loại vàng (SJC/nhẫn/tỷ giá), giá mua, khối lượng |
| Gold_Pro | Loại vàng + tỷ trọng trong tổng danh mục |

**AI dùng để:** Lọc tín hiệu liên quan, tính tỷ trọng, tính lời/lỗ, đưa vào bản tin sáng, Góc nhìn trên thẻ (xem brief Team C).

**Drip capture — stop_loss_pct & target_price:**

Không hỏi upfront. Hỏi đúng lúc, đúng context:

| Trigger | Câu hỏi AI |
|---------|-----------|
| Mã giảm ≥ 5% | "Bạn chịu lỗ tối đa bao nhiêu % với [mã]?" |
| Mã tăng ≥ 8% | "Bạn dự định chốt lời ở vùng giá nào?" |
| User tap [Có, tôi đang cầm] trên card | Nhận giá vốn từ inline input, lưu ngay |
| User nhập trong chat ("tôi mua SSI 30K") | LLM extract → lưu tự động |

Quy tắc:
- Chỉ hỏi 1 câu mỗi lần, không hỏi 2 trường cùng lúc
- Nếu user bỏ qua → ghi `null`, hỏi lại sau 7 ngày
- Nếu user đã có data → không hỏi lại

---

### LOẠI 2 — NHẬT KÝ GIAO DỊCH

**Dữ liệu lưu mỗi giao dịch — Ví dụ cụ thể:**

```
Ngày:         15/01/2026
Hành động:    Mua
Mã:           HPG
Giá:          26,500
Số lượng:     2,000
Lý do:        Tự phân tích — giá về vùng hỗ trợ
Nguồn:        Tự quyết định
Kết quả:      Đang giữ, +3.8%
Thói quen:    Không phát hiện
Nhập từ:      Sao kê PDF
```

**Ví dụ nhật ký giao dịch của 1 user:**

```
15/01 | Mua HPG 26,500 × 2,000 | Tự phân tích      | +3.8%
03/02 | Mua FPT 120,000 × 500  | Theo tin BCTC      | +5%
12/02 | Mua TCH 8,500 × 5,000  | Theo tip group     | -12% ⚠️
10/03 | Bán VNM 78,000 × 1,000 | Chốt lời           | +8.3% ✅
12/03 | Mua DXG 14,200 × 3,000 | Theo tip group     | -5% ⚠️
```

**Cột "Lý do" lấy từ đâu:**

- GD trong app → AI hỏi nhanh: **[Tự phân tích]** **[Theo tin tức]** **[Theo tip]** **[Theo gợi ý AI]**
- Sao kê PDF → để trống (AI không hỏi, không đoán)
- Chat → AI trích xuất tự động ("tôi mua HPG vì giá về hỗ trợ" → Lý do: Tự phân tích)

**Thói quen AI tự phát hiện:**

| Thói quen | Điều kiện | Ví dụ |
|-----------|----------|-------|
| FOMO | Mua trong 2 giờ sau giá tăng hơn 5% | TCH tăng 7% sáng → user mua chiều |
| Bán hoảng loạn | Bán trong 24 giờ sau tin xấu, giá hồi sau đó | HPG tin xấu → bán ngay → HPG hồi +15% |
| Giao dịch quá nhiều | Hơn 5 lệnh/tuần | 8 lệnh trong 1 tuần |
| Theo tip | Mua ngay sau mã được nhắc trong group/kênh | DXG được nhắc 10h → user mua 11h |
| Bình quân giá xuống | Mua thêm mã đang lỗ hơn 10% | TCH lỗ -12% → mua thêm |

**Bài học khác nhau theo profile (AI tập trung phát hiện):**

| Profile | Thói quen hay gặp |
|---------|-------------------|
| Stock_Retail | FOMO, theo tip, bán hoảng loạn |
| Stock_Pro | Gồng lỗ quá lâu, overweight 1 mã, bình quân giá xuống |
| Crypto_Retail | FOMO memecoin, không chốt lời, all-in 1 token |
| Crypto_Pro | Quá tải đòn bẩy, không cắt lỗ futures, overtrade |
| Gold_Retail | Mua đỉnh khi giá tăng nóng, bán đáy khi giá giảm nhẹ |
| Gold_Pro | Không hedge khi DXY biến động, bỏ lỡ entry vì chờ quá lâu |

---

### LOẠI 3 — BÀI HỌC

**Quy tắc tạo:** Cùng 1 thói quen xảy ra **2 lần trở lên** → tạo bài học. 1 lần → chỉ đánh dấu, chưa tạo bài học.

**Mỗi bài học gồm — Ví dụ cụ thể:**

```
Thói quen:    Bán hoảng loạn
Mô tả:        3 lần bán ngay khi có tin xấu. Cả 3 lần giá hồi +8-15% trong 2 tuần.
Giao dịch:    HPG (15/08/2025), VCB (03/10/2025), FPT (22/01/2026)
Nhắc khi:     Mã trong danh mục có tin xấu + user mở app nhiều lần trong 1 giờ
Đã nhắc:      2 lần, lần gần nhất 12/03/2026
Kết quả nhắc: 1/2 lần user nghe theo (giữ HPG → lãi +12%)
```

**AI dùng bài học ở 3 nơi:**

1. **Thẻ tín hiệu** (phần lịch sử):
```
┌ LỊCH SỬ
│ 🕐 3 lần trước bạn bán khi có tin xấu, cả 3 lần
│ giá quay lại +8-15% trong 2 tuần.
└
```

2. **Bản tin sáng** (phần cần chú ý):
```
HPG: Tin xấu đêm qua. Bạn đã 3 lần vội bán
khi có tin xấu, cả 3 lần giá quay lại.
Không bán trước khi đọc số liệu.
```

3. **Chat đào sâu** (khi hỏi "Nên cắt lỗ không?"):
```
"Chưa nên. 3 lần trước bạn vội bán khi có tin xấu,
cả 3 lần giá quay lại +8-15%. Lần này kiên nhẫn hơn."
```

---

### LOẠI 4 — BỐI CẢNH HÀNG NGÀY

Mỗi ngày AI tự ghi (tự động, không cần user làm gì):

```
Ngày:         18/03/2026
Danh mục:     +0.8% | Tổng: 245,000,000 VND
Tín hiệu:    Nhận 8 | Mở 5 | Bấm "Hỏi thêm" 2
Loại mở:      Tin tức 3, Kỹ thuật 1, Liên quan 1
Loại bỏ qua:  Cảm xúc MXH 2, Vĩ mô 1
Giao dịch:    Bán VNM 78,000 × 1,000
Bản tin sáng: Mở lúc 7:12, đọc 45 giây
Chat:         1 cuộc (hỏi về HPG)
Thời gian app: 12 phút tổng, 6 lần mở
```

**AI dùng để:**
- Bản tin sáng tham khảo hôm qua
- Điều chỉnh tần suất gửi (user bỏ qua nhiều → giảm push)
- Phát hiện bất thường (mở app 15 lần/ngày = đang lo lắng → AI hỏi)
- Tối ưu thời gian gửi (user hay mở 7h sáng + 14h chiều → push vào khung đó)

---

### SƠ ĐỒ DỮ LIỆU CHẢY VÀO TRÍ NHỚ

```
Onboarding
  └→ Profile + Danh mục ban đầu

Sao kê PDF / Ảnh chụp
  └→ Nhật ký GD + Cập nhật danh mục

Giao dịch trong app
  └→ Nhật ký GD + Danh mục + AI hỏi lý do

Hành vi hàng ngày (tự động)
  └→ Bối cảnh hàng ngày

AI phân tích đêm (batch job)
  └→ Phát hiện thói quen → Tạo/cập nhật bài học
  └→ Tổng hợp phản hồi → Điều chỉnh trọng số

Phản hồi tín hiệu (implicit + explicit)
  └→ Điều chỉnh trọng số gửi cho từng người
```

**Lưu ý kỹ thuật:**
- Danh mục + Nhật ký: Lưu database, truy vấn nhanh
- Bài học: Lưu JSON, load vào prompt mỗi lần AI viết thẻ/bản tin
- Bối cảnh hàng ngày: Lưu log, batch xử lý mỗi đêm
- AI phân tích đêm: Chạy 1 lần/ngày lúc 02:00 AM

---

## PHẦN 3: ĐỌC SAO KÊ — Phụ trách: Chi (PENDING)

### Mục tiêu
Người dùng tải file PDF/Excel sao kê từ CTCK → AI đọc → chuyển thành nhật ký giao dịch + cập nhật danh mục.

### Luồng trải nghiệm

**Vào từ:** Onboarding màn 3 (nút "Tải ảnh chụp") hoặc Tab Tôi → Danh mục → Nhập dữ liệu

**Bước 1 — Chọn file:**

🤖 "Tải file sao kê giao dịch từ công ty chứng khoán của bạn."
Hỗ trợ: PDF, Excel (.xlsx, .xls), ảnh (.jpg, .png)
Dòng phụ: "Hỗ trợ sao kê từ SSI, VNDirect, TCBS, VPS, MBS, DNSE và hầu hết các CTCK khác."

**Bước 2 — AI xử lý (loading 3-10 giây):**

🤖 "Đang đọc file sao kê..."
Spinner animation.

**Bước 3 — Kết quả (3 trường hợp):**

**Trường hợp 1 — Thành công (100% tin cậy cao):**
```
🤖 "Tôi nhận diện được 12 giao dịch từ sao kê SSI."

15/01 | Mua HPG 26,500 × 2,000
03/02 | Mua FPT 120,000 × 500
10/02 | Bán VNM 78,000 × 1,000
... (cuộn xem thêm)

[Lưu tất cả 12 giao dịch]     [Hủy]
```

**Trường hợp 2 — Một phần (có dòng không rõ):**
```
🤖 "Tôi nhận diện được 8 giao dịch chắc chắn.
4 dòng không rõ đã bỏ qua."

15/01 | Mua HPG 26,500 × 2,000
03/02 | Mua FPT 120,000 × 500
... (8 dòng)

[Lưu 8 giao dịch]     [Hủy]
```
⚠️ **KHÔNG hiện 4 dòng bỏ qua. KHÔNG hỏi user xác nhận từng dòng.** Bỏ qua = bỏ qua.

**Trường hợp 3 — Thất bại:**
```
🤖 "Xin lỗi, tôi không đọc được file này.
File có thể bị mã hóa hoặc định dạng không hỗ trợ."

[Thử file khác]     [Nhập tay]     [Bỏ qua]
```

### Prompt cho AI đọc sao kê

```
"Trích xuất lịch sử giao dịch chứng khoán từ nội dung sao kê.

Trả về mỗi giao dịch gồm:
- Mã chứng khoán
- Mua / Bán
- Giá
- Số lượng
- Ngày
- Phí (nếu có)
- Độ tin cậy: cao / thấp

Quy tắc:
- Không chắc chắn → độ tin cậy "thấp"
- Không đoán mò. Không có dữ liệu → để trống
- Chỉ trích xuất mua/bán, bỏ qua phí, thuế, lãi tiền gửi
- Nếu cùng mã mua/bán nhiều lần → tách từng dòng
- Format ngày: DD/MM/YYYY"
```

**Ví dụ output AI:**
```json
[
  {"ma": "HPG", "hanh_dong": "Mua", "gia": 26500, "so_luong": 2000, "ngay": "15/01/2026", "phi": 39750, "tin_cay": "cao"},
  {"ma": "FPT", "hanh_dong": "Mua", "gia": 120000, "so_luong": 500, "ngay": "03/02/2026", "phi": 90000, "tin_cay": "cao"},
  {"ma": "???", "hanh_dong": "Mua", "gia": null, "so_luong": 1000, "ngay": "05/02/2026", "phi": null, "tin_cay": "thap"}
]
```

**Xử lý:**
- `tin_cay: "cao"` → lưu vào nhật ký + cập nhật danh mục
- `tin_cay: "thap"` → **bỏ qua hoàn toàn**, không hỏi người dùng
- Trùng lặp (cùng mã + cùng ngày + cùng giá + cùng SL) → bỏ qua

**Test trước khi ra mắt:** Ít nhất 3 file sao kê thật từ SSI, VNDirect, VPS → kiểm tra accuracy.

---

## PHẦN 4: THU THẬP PHẢN HỒI — Phụ trách: Chi

### Mục tiêu
AI cần biết mỗi tín hiệu có hữu ích không → điều chỉnh cách gửi theo thời gian. **100% tự động + hỏi tối thiểu.**

### Cách 1 — Tự động theo dõi hành vi (100% tín hiệu, không cần user làm gì)

| Hành vi | Ý nghĩa | Trọng số |
|---------|---------|---------|
| Mở xem thẻ | Tín hiệu liên quan | +1 |
| Đọc hơn 3 giây | Nội dung hấp dẫn | +2 |
| Bấm "Hỏi thêm" | Rất quan trọng | +5 |
| Vuốt bỏ / đóng nhanh (<3s) | Không liên quan | -2 |
| Không mở thông báo | Không hấp dẫn | -1 |

**Ví dụ:** User liên tục mở thẻ Tin tức nhưng luôn vuốt bỏ thẻ Cảm xúc MXH → AI tăng ưu tiên Tin tức, giảm ưu tiên Cảm xúc.

### Cách 2 — Hỏi trực tiếp (tối đa 2 lần/tuần)

**Điều kiện hiện popup:**
- Đọc thẻ > 3 giây rồi đóng (không bấm "Hỏi thêm")
- Chưa hỏi 2 lần trong tuần này
- Tín hiệu có mức ≥ Trung bình

**Popup (hiện ngay sau khi đóng thẻ):**
```
┌─────────────────────────────────────┐
│ Tín hiệu vừa rồi có hữu ích       │
│ cho bạn không?                      │
│                                     │
│ [👍 Có]  [👎 Không]  [🚫 Ẩn loại này] │
└─────────────────────────────────────┘
```

- Tự biến mất sau 5 giây nếu không bấm
- 👍 Có → +3 trọng số loại tín hiệu đó
- 👎 Không → -3 trọng số
- 🚫 Ẩn loại này → tắt push loại đó (vẫn hiện feed)

**Nếu popup không hiệu quả (> 70% bỏ qua trong 2 tuần liên tiếp):**
→ Chuyển sang hỏi trong bản tin sáng (cuối bản tin, 1 câu):
```
"Tuần này tôi gửi 12 tín hiệu Cảm xúc MXH.
Bạn có muốn giữ loại này không?
[👍 Giữ]  [👎 Giảm bớt]  [🚫 Tắt]"
```

**Giới hạn:** 2 lần/tuần, reset thứ Hai 00:00.

### AI dùng phản hồi để:
1. Điều chỉnh trọng số tín hiệu cho từng người (loại nào push, loại nào chỉ feed)
2. Điều chỉnh thời gian gửi (phát hiện khung giờ hay mở → push vào đó)
3. Cải thiện nội dung (thẻ có lịch sử được bấm nhiều hơn → ưu tiên tìm lịch sử)
4. Phát hiện user chán (mở ít dần, bỏ qua nhiều) → giảm push, tăng chất lượng

### Sơ đồ luồng phản hồi

```
Tín hiệu gửi
  │
  ├── Không mở thông báo
  │     └→ Ghi: bỏ qua (-1) → XONG
  │
  └── Mở xem thẻ → Ghi: mở (+1)
        │
        ├── Bấm "Hỏi thêm"
        │     └→ Ghi: rất quan trọng (+5) → XONG
        │
        ├── Đóng nhanh (< 3 giây)
        │     └→ Ghi: không hữu ích (-2) → XONG
        │
        └── Đọc > 3 giây rồi đóng → Ghi: (+2)
              │
              ├── Chưa hỏi 2 lần/tuần
              │     └→ Hiện popup
              │           ├── 👍 → +3 → XONG
              │           ├── 👎 → -3 → XONG
              │           ├── 🚫 → tắt push loại này → XONG
              │           └── Bỏ qua (5s) → XONG
              │
              └── Đã hỏi 2 lần/tuần
                    └→ Không hỏi → XONG

Mỗi đêm 02:00 AM:
  AI tổng hợp tất cả phản hồi ngày hôm đó
  → Cập nhật trọng số từng loại tín hiệu cho user
  → Cập nhật khung giờ ưu tiên push
  → Kiểm tra xu hướng (user engagement tăng/giảm)
```

---

*Hết Nhóm A*

---

## CHANGE LOG

| Ngày | Phiên bản | Thay đổi |
|------|-----------|----------|
| 18/03/2026 | 2.0 | Cập nhật từ v1 |
| 19/03/2026 | 2.1 | Thêm người phụ trách từng phần |
| 23/03/2026 | 2.2 | PHẦN 2 Trí nhớ AI: thêm stop_loss_pct + target_price vào portfolio schema; thêm drip capture logic (trigger + quy tắc hỏi) |
