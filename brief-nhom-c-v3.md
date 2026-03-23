# FINPATH AI CO-PILOT — BRIEF SPRINT 1
# NHÓM C: HIỂN THỊ & TƯƠNG TÁC (1 lập trình viên + 1 thiết kế)

> **Phiên bản:** 2.0 — 18/03/2026
> **Người soạn:** Việt Anh (Product Design Lead)
> **Sprint:** 1 (10 ngày làm việc)
> **Team:** 1 lập trình viên + 1 thiết kế UI

---

## PHẦN 1: THẺ TÍN HIỆU — Phụ trách: Việt Anh, Thiên

### 7 loại thẻ

**Cấu trúc chung của mọi thẻ (từ mockup v3):**

```
● (severity dot)                        Thời gian
─────────────────────────────────────────────────
Tiêu đề (font lớn, bold, ≤150 ký tự)

Nội dung (font thường, số liệu bold, ≤160 ký tự)

┌─────────────────────────────────────────┐
│ ● Lịch sử tương tự (nền tối hơn,       │
│   ≤140 ký tự)                           │
└─────────────────────────────────────────┘

╔═════════════════════════════════════════╗
║ GÓC NHÌN (≤130 ký tự, luôn hiện)      ║
║ [nội dung thay đổi theo chế độ]        ║
║ [inline buttons nếu cần collect data]  ║
╚═════════════════════════════════════════╝

          (💬 CTA pill button)
```

**Quy tắc layout (cập nhật theo mockup v3):**
- **KHÔNG có tag phân loại** trên card (không [TIN TỨC], [KỸ THUẬT]...) — chỉ severity dot + thời gian
- **KHÔNG có label section** (không "TIÊU ĐỀ:", "SỐ LIỆU:", "LỊCH SỬ:") — text chảy tự nhiên
- Tiêu đề và nội dung là 2 đoạn riêng, không cần label
- Lịch sử nằm trong callout box nền tối, bắt đầu bằng bullet dot (●), không dùng 🕐
- **Góc nhìn** luôn hiện ở cuối card (xem spec riêng bên dưới), thay thế "Bối cảnh cá nhân" cũ
- CTA là **pill button** (bo tròn, nền accent nhạt), không phải nút full-width
- CTA chỉ có "Hỏi thêm" — Share/Bookmark chỉ hiện trong detail view

---

**LOẠI 1 — THẺ TRỰC TIẾP (mã đang cầm/theo dõi)**

Khi tín hiệu liên quan trực tiếp đến mã trong danh mục hoặc watchlist.

```
● (đỏ)                                10:23 AM

UBCKNN yêu cầu HPG giải trình số liệu
BCTC Q4/2025, nghi vấn hạch toán chi phí
khấu hao Dung Quất

Giá HPG từ 26,400 → 25,300 (–4.2%).
Volume gấp 2.8 lần trung bình 20 phiên.
Khối ngoại bán ròng 45 tỷ.

┌─────────────────────────────────────────┐
│ ● 15/08/2025: Tin tương tự, bạn bán    │
│   ngay, HPG hồi +15% sau 2 tuần khi    │
│   tin bị bác bỏ.                        │
└─────────────────────────────────────────┘

Bạn cầm 2,000 HPG — chiếm 35% danh mục.
Nếu giảm thêm 8%, danh mục giảm –2.8%.

          (💬 Hỏi thêm về HPG)
```

**Lưu ý:**
- Lịch sử chỉ hiện khi có dữ liệu trong trí nhớ AI. Không có → bỏ qua.
- **Góc nhìn luôn hiện** — nội dung thay đổi theo chế độ FULL/WATCHLIST_ONLY/GENERIC (xem spec Góc nhìn).
- CTA luôn có.

---

**LOẠI 2 — THẺ LIÊN QUAN (Adjacent)**

Khi tín hiệu ở mã khác nhưng ảnh hưởng gián tiếp đến mã user đang cầm.

```
● (vàng)                               8:45 AM

┌──────────┐     ┌──────────────┐     ┌──────────┐
│ DXG      │ →   │ Nợ xấu BĐS  │ →   │ VCB      │
│ ↓12%     │     │ tăng         │     │ bạn cầm  │
└──────────┘     └──────────────┘     └──────────┘

DXG giảm sàn sau tin chủ tịch bị tạm giữ —
rủi ro nợ xấu BĐS lan sang ngân hàng

DXG dư bán sàn 14.7 triệu CP. Dư nợ BĐS
tại VCB chiếm 26% tổng cho vay. DXG vay
1,200 tỷ tại VCB.

┌─────────────────────────────────────────┐
│ ● 11/2022: FLC sụp — VCB giảm 12%      │
│   trong 1 tháng do lo ngại nợ xấu BĐS. │
└─────────────────────────────────────────┘

Bạn không cầm DXG, nhưng cầm 1,500 VCB —
chiếm 42% danh mục. Theo dõi phản ứng
VCB chiều nay.

          (💬 Hỏi thêm về VCB & BĐS)
```

**Bắt buộc:** Chuỗi giải thích (chain diagram) phải ở **đầu thẻ** (sau severity dot, trước tiêu đề). Không có chuỗi → không gửi thẻ Adjacent.

Chain diagram gồm 3 ô nối mũi tên: **Nguồn → Cơ chế → Mã user**. Ô cuối highlight màu accent, ghi "bạn cầm" hoặc "bạn đang cầm".

---

**LOẠI 3 — THẺ TỔNG HỢP (Compound)**

Khi ≥ 2 tín hiệu từ nguồn khác nhau cùng mã + cùng hướng trong 2 giờ.

```
● (đỏ)                                 9:32 AM

3 nguồn cùng chỉ về HPG: nghi vấn dumping
từ EU, sentiment tiêu cực bùng trên Telegram,
giá giảm 4.2%

Tiêu cực tăng 340% trên 5 nhóm Telegram.
Giá 26,400 → 25,300 (–4.2%).
Volume gấp 2.8 lần TB 20 phiên.

┌─────────────────────────────────────────┐
│ ● 15/08/2025: Tín hiệu tương tự — HPG  │
│   giảm thêm 8% trong 3 phiên, hồi 60%  │
│   khi tin bị bác bỏ.                    │
└─────────────────────────────────────────┘

Bạn cầm 2,000 HPG (35% danh mục). Nếu giảm
thêm 8% như lần trước, danh mục giảm –2.8%.

          (💬 Hỏi thêm về HPG)
```

**Trên card:** Hiện nội dung tổng hợp luôn, KHÔNG có toggle xem từng nguồn. Bấm card → detail view mới thấy tách nguồn.

---

**LOẠI 4 — THẺ MỜ (Pro blur)**

Tín hiệu Pro mà user Free chưa được xem. Tiêu đề hiện rõ, nội dung blur.

```
● (vàng)                               14:30

Phân tích chuyên sâu HPG sau tin điều tra:
cơ hội hay rủi ro?

░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
       (blur overlay, cuộn được)

        🔒 Nội dung Pro — 549,000đ/tháng
        ⏱️ Unlock miễn phí sau 18 giờ nữa

             [🔓 Mở khóa Pro]
```

**Quy tắc:**
- 7 ngày honeymoon: hiện đếm ngược unlock miễn phí (24h delay)
- Sau honeymoon: không có đếm ngược, chỉ nút "Mở khóa Pro"
- Tiêu đề hiện rõ (gây tò mò), overlay lock + label + nút trên nội dung blur

---

**LOẠI 5 — THẺ TẶNG PRO (taste)**

AI "tặng" user Free 1 tín hiệu Pro đầy đủ.

```
● (vàng)                               14:30

🎁 Đây là tính năng Pro. Hôm nay tôi tặng bạn.

Phân tích chuyên sâu HPG sau tin điều tra:
cơ hội hay rủi ro?

(NỘI DUNG ĐẦY ĐỦ — không blur)
RSI 28 vùng quá bán. Hỗ trợ 25,000 (MA200).
3 lần tương tự → hồi TB +12%...

💡 Pro subscribers nhận phân tích này MỖI NGÀY.

  (💬 Hỏi thêm)  (🔓 Nâng cấp Pro)
```

**Tần suất:** ~1 lần/2-4 ngày, chỉ trong 7 ngày honeymoon.

---

**LOẠI 6 — THẺ DELAY UNLOCK (sau honeymoon)**

Tín hiệu Pro hết 24h delay → unlock + hiện kết quả.

```
● (xám)                         14:30 hôm qua

⏱️ Signal này đã delay 24h

Phân tích dòng tiền tổ chức vào HPG —
Smart Money đang tích lũy?

Tự doanh mua ròng 85 tỷ phiên 16/03.
Khối ngoại chuyển từ bán sang trung lập.
Room ngoại còn 12%.

┌─────────────────────────────────────────┐
│ ✅ Từ khi Pro nhận signal này:          │
│    HPG đã tăng +3.2% (26,500 → 27,350) │
└─────────────────────────────────────────┘

📌 Pro users đã biết sớm hơn bạn 1 ngày.

             [🔓 Nâng cấp Pro]
```

**Mục đích:** Cho thấy chênh lệch % thực tế (lời/lỗ từ lúc Pro nhận → lúc Free xem).

---

**LOẠI 7 — THẺ KỸ THUẬT (đơn lẻ)**

Tín hiệu từ chỉ báo kỹ thuật + định giá.

```
● (xanh dương)                         8:12 AM

FPT vừa breakout khỏi vùng tích lũy
4 tuần, volume xác nhận

FPT vượt kháng cự 128,000 lần đầu từ tháng 1.
Volume phiên sáng 180% TB. RSI 62 —
chưa overbought.

┌─────────────────────────────────────────┐
│ ● 2 lần breakout tương tự 12 tháng qua:│
│   FPT tăng TB +14% trong 20 phiên sau. │
└─────────────────────────────────────────┘

FPT trong watchlist từ 02/03. Chưa có
trong danh mục.

          (💬 Phân tích FPT chi tiết)
```

---


---

### GÓC NHÌN — Spec & Prompt

**Mục tiêu:** Luôn cung cấp decision support context trên mọi thẻ, dù user chưa có portfolio.

**3 chế độ (kiểm tra theo thứ tự):**

| Chế độ | Điều kiện | Nội dung |
|--------|-----------|---------|
| FULL | Có portfolio + giá vốn cho mã này | Mirror rules + tính lãi/lỗ cụ thể |
| WATCHLIST_ONLY | Mã trong watchlist, không có giá vốn | Market anchor + 2 kịch bản ngắn + inline buttons |
| GENERIC | Không có data gì | Giá hiện tại + smart money + 1 điều cần theo dõi |

**Nội dung từng chế độ (≤130 ký tự):**

**Công thức:** Mirror tình huống → Lời khuyên mềm từ rule của chính user (không predict, không ra lệnh)

FULL — Retail:
```
Ngưỡng cắt lỗ bạn đặt còn 5% nữa — cần theo dõi sát.
3 lần trước bạn bán sớm, cả 3 lần giá hồi lại.
```

FULL — Pro:
```
Cắt lỗ còn 5%. 3/3 bán sớm → hồi.
Theo dõi sát, chưa cần hành động vội.
```

WATCHLIST_ONLY — Retail:
```
SSI vượt mốc giá quan trọng đúng vùng bạn đang theo dõi.
Nếu tính vào — đây là thời điểm cân nhắc.
```

WATCHLIST_ONLY — Pro:
```
SSI vượt kháng cự. Nếu muốn vào — thời điểm đang phù hợp.
```
+ Inline buttons: **[Có, tôi đang cầm]** **[Không, đang xem]**

GENERIC:
```
SSI −10% từ đỉnh. Dragon Capital vẫn giữ nguyên,
chưa bán CP nào. Theo dõi phản ứng giá hôm nay.
```

**Quy tắc ngôn ngữ (QUAN TRỌNG):**
- Stop loss → **"cắt lỗ"** (không dùng "stop loss", không viết tắt "SL")
- Take profit → **"chốt lời"**
- Portfolio → **"danh mục"** (không viết tắt "DM")
- KHÔNG dùng tiếng Anh trong Góc nhìn: breakout, signal, compound, exposure, timing, action
- Retail: viết đầy đủ, giải thích bằng ngôn ngữ người thường
- Pro: gọn, viết tắt được phép theo brief A (DT, LN, vol, RSI...), không cần giải thích

---

**Inline buttons — UX flow:**

Chỉ hiện trong chế độ WATCHLIST_ONLY. Nằm trong box Góc nhìn, sau text.

**[Có, tôi đang cầm]** → Card expand inline:
```
╔═════════════════════════════════════════╗
║ Bạn mua SSI giá bao nhiêu?             ║
║ ┌─────────────────────────────────────┐ ║
║ │ VD: 30,000đ                [Lưu →] │ ║
║ └─────────────────────────────────────┘ ║
╚═════════════════════════════════════════╝
```
→ User nhập giá vốn → Lưu → portfolio created → card tự update sang FULL mode
→ Follow-up: "Bạn muốn đặt stop loss không?" (inline, có thể bỏ qua)

**[Không, đang xem]** → Không cần nhập gì:
- SSI tự thêm vào watchlist (nếu chưa có)
- Buttons biến mất
- Không hỏi lại ở card SSI trong 7 ngày

**Bỏ qua (không tap)** → Giữ nguyên, hỏi lại lần sau.

---

**Prompt tạo Góc nhìn:**
```
"Tạo nội dung 'Góc nhìn' cho thẻ tín hiệu.

Tín hiệu: {mã, loại, nội dung, hướng}
Chế độ: {FULL | WATCHLIST_ONLY | GENERIC}

[Nếu FULL:]
  Danh mục: {mã + giá mua + tỷ trọng + lời/lỗ}
  Rules: {stop_loss_pct, target_price}

[Nếu WATCHLIST_ONLY hoặc GENERIC:]
  Giá hiện tại: {giá}
  Smart money: {khối ngoại, tự doanh gần nhất}
  Catalyst sắp tới: {sự kiện + ngày}

Quy tắc:
- Tối đa 130 ký tự
- FULL: mirror rules của user (cắt lỗ, chốt lời) + lời khuyên mềm (cân nhắc/theo dõi)
- WATCHLIST_ONLY: nhận xét kỹ thuật ngắn + lời khuyên mềm về thời điểm
- GENERIC: giá hiện tại + smart money + 1 điều cần theo dõi
- KHÔNG ra lệnh mua/bán
- KHÔNG dự đoán giá
- KHÔNG dùng: stop loss, SL, take profit, DM, breakout, signal, compound, exposure, timing, action
- DÙNG: cắt lỗ, chốt lời, danh mục, vượt kháng cự, tín hiệu, tỷ trọng, thời điểm, hành động
- TUÂN THỦ AI Voice Tier (Retail vs Pro)"
```

---

### MÀU SẮC & SEVERITY

| Mức | Dot | Điểm | Dùng khi |
|-----|-----|------|---------|
| Nghiêm trọng | ● đỏ (có glow) | 8-10 | Tin nóng, ảnh hưởng lớn |
| Cao | ● vàng | 6-7 | Đáng chú ý |
| Trung bình | ● xanh dương | 4-5 | Thông tin tham khảo |
| Thấp | ● xám | 1-3 | Nền, gom bản tin |

**KHÔNG có tag phân loại nội bộ trên card** — chỉ severity dot + thời gian. Phân loại (Tin tức, Kỹ thuật, Vĩ mô...) chỉ dùng ở detail view và hệ thống nội bộ.

### GIỚI HẠN KÝ TỰ

| Phần | Giới hạn | Hiển thị |
|------|---------|---------|
| Tiêu đề | 150 ký tự | Luôn hiện |
| Nội dung | 160 ký tự | Luôn hiện |
| Lịch sử (callout) | 140 ký tự | Khi có data |
| Góc nhìn | 130 ký tự | **Luôn hiện** |
| **Tổng 1 thẻ** | **~580-600 ký tự** | |

### DETAIL VIEW (bấm vào card)

Khi bấm card → mở detail view full screen:

```
← HPG
─────────────────────────────────────────
● đỏ · Tin tức · 10:23 AM · Nghiêm trọng

(Tiêu đề lớn hơn)

(Nội dung đầy đủ)

🕐 LỊCH SỬ TƯƠNG TỰ
(Callout box, viền trái accent)

👤 DANH MỤC CỦA BẠN
(Box nền tối, thông tin DM + bài học)

     [✨ Hỏi AI thêm về HPG]
```

**Chỉ trong detail view mới có:**
- Tag phân loại (Tin tức, Kỹ thuật...)
- Mức độ text (Nghiêm trọng, Cao...)
- Section labels (Lịch sử, Danh mục)
- Nút Share / Bookmark (nếu cần)


## PHẦN 2: DÒNG TIN (FEED) — Phụ trách: Việt Anh, Thiên

### Màn hình Tình báo

Feed nằm trong tab **Tình báo** (bottom tab). Chỉ gồm 2 thành phần:

1. **Bản tin sáng** (ghim trên cùng)
2. **Danh sách thẻ tín hiệu** (cuộn dọc)

**KHÔNG CÓ:** bộ lọc, tab phụ, phân nhóm ngày, header phức tạp.

### Bố cục

```
┌─────────────────────────────────────┐
│ Tình báo              [⚙️] [🔔]    │
├─────────────────────────────────────┤
│ ● 1 tín hiệu mới (sticky banner)   │
├─────────────────────────────────────┤
│                                      │
│ ┌─ BẢN TIN SÁNG ────────────────┐  │
│ │ 🔥 Điểm tin 18/03             │  │
│ │ HPG: Đang bị yêu cầu giải    │  │
│ │ trình...                       │  │
│ │ 7:00    Đọc đầy đủ →          │  │
│ └────────────────────────────────┘  │
│                                      │
│ ┌─ [Thẻ tín hiệu 1] ───────────┐  │
│ └────────────────────────────────┘  │
│ ┌─ [Thẻ tín hiệu 2] ───────────┐  │
│ └────────────────────────────────┘  │
│ ┌─ [Thẻ blur Pro] ──────────────┐  │
│ └────────────────────────────────┘  │
│                                      │
├─────────────────────────────────────┤
│ Finpath | Bảng giá | CĐ | Tình báo │
└─────────────────────────────────────┘
```

### Bản tin sáng — Layout (theo mockup)

Accordion — bấm expand/collapse tại chỗ trong feed (KHÔNG navigate sang màn khác). Card phía dưới bị đẩy xuống khi expand. Nội dung gồm 3 phần:

```
🔥 Điểm tin 18/03                    ⏱️

VND: Ngành chứng khoán đang thiếu vắng
dòng tiền. Anh đã 2 lần trung bình giá
xuống khi mã này thủng hỗ trợ. Nếu VND
bực mốc 20.5 hôm nay, có thể cân nhắc
hạ phân nửa tỷ trọng, tuyệt đối không
bắt đáy thêm.

KBC: +6.5%, đang mạnh hơn thị trường
chung nhờ tin FDI giải ngân tăng. Nhịp
này tiền nội đang đỡ giá rất cứng. Giữ
chặt hàng, chỉ cần dời giá chốt lời lên 32.

┃ HÔM NAY CẦN BIẾT

Chỉ số DXY đêm qua tăng mạnh, khối ngoại
dự kiến tiếp tục xả ròng ép chỉ số test
lại vùng 1200. Anh đang có sẵn 40% tiền
mặt. Hãy kiên nhẫn. Không dùng phần tiền
này để trung bình giá các mã đang yếu.
Quan sát lực cầu tại 1200.

┃ TỔNG QUAN

Danh mục: -1.2% | Tiền mặt: 120tr (40%)
→ Quản trị rủi ro VND, gồng lãi KBC và
chờ thời cơ tại 1200.

                              ▲ Thu gọn
```

**Quy tắc bản tin:**
- **Mặc định thu gọn** — hiện 2 dòng preview + "Đọc đầy đủ →"
- **Bấm → mở rộng** full nội dung như trên
- **Bấm "Thu gọn"** → thu lại
- Nội dung viết liền, KHÔNG có separator giữa các mã
- Mỗi mã bắt đầu bằng **tên mã bold** + nội dung ngắn
- Phần "HÔM NAY CẦN BIẾT" và "TỔNG QUAN" có thanh accent bên trái (┃)
- Tối đa **600 ký tự** toàn bộ bản tin

### Quy tắc feed

| Quy tắc | Chi tiết |
|---------|---------|
| Thứ tự | Mới nhất lên trên |
| Bộ lọc | **KHÔNG CÓ** — AI đã lọc giúp |
| Scroll | Infinite scroll |
| Bản tin sáng | Ghim trên cùng, thu gọn/mở rộng |
| Badge | 🔔 đỏ dot khi có tín hiệu mới |
| Tín hiệu mới | Sticky banner "● 1 tín hiệu mới" (KHÔNG tự cuộn) |
| Empty state | Thẻ chào mừng + 2-3 tín hiệu mẫu |

### Tương tác

| Hành động | Kết quả | Tracking |
|-----------|---------|---------|
| Bấm card | Mở detail view | +1 mở |
| Bấm "Hỏi thêm" | Mở chat (context = thẻ) | +5 quan trọng |
| Bấm bản tin | Mở rộng / thu gọn | — |
| Bấm ticker | Mở trang chi tiết mã | — |
| Bấm "Mở khóa Pro" | Màn thanh toán | intent to purchase |

---
## PHẦN 3: GỬI THÔNG BÁO (DELIVERY ROUTER) — Phụ trách: Việt Anh, Hưởng

### 4 kiểu gửi

| Kiểu | Khi nào | Trải nghiệm |
|------|---------|-------------|
| Push có âm thanh | 🔴 Nghiêm trọng (8-10 điểm) | Kêu, hiện màn hình khóa, rung |
| Push im lặng | 🟡 Cao (6-7 điểm, giờ hoạt động) | Hiện màn hình khóa, không kêu |
| Chỉ trong app | 🔵 Trung bình (4-5 điểm) | Không push, badge 🔔 tăng |
| Gom bản tin sáng | ⚪ Thấp (1-3 điểm) | Gộp vào bản tin sáng hôm sau |

### Giờ yên tĩnh

Mặc định: 22:00 → 06:30. User tùy chỉnh được.

| Mức | Giờ hoạt động (06:30-22:00) | Giờ yên tĩnh (22:00-06:30) |
|-----|---------------------------|--------------------------|
| 🔴 Nghiêm trọng | Push có âm thanh | Push có âm thanh (**XUYÊN QUA**) |
| 🟡 Cao | Push im lặng | Gom, push hàng loạt khi hết giờ yên tĩnh |
| 🔵 Trung bình | Trong app | Trong app |
| ⚪ Thấp | Gom bản tin | Gom bản tin |

### Nội dung push notification (≤100 ký tự)

**Format:**
- Dòng 1: Mã + sự kiện (≤60 ký tự)
- Dòng 2: Bối cảnh cá nhân (≤40 ký tự)

**Prompt tạo push:**
```
"Viết thông báo đẩy cho điện thoại.

Tín hiệu: {loại, mã, nội dung, điểm}
Danh mục: {mã + tỷ trọng}
Profile: {profile}

Quy tắc:
- Dòng 1: Mã + sự kiện (≤60 ký tự)
- Dòng 2: Bối cảnh cá nhân (≤40 ký tự)
- Tổng ≤100 ký tự
- KHÔNG dùng từ mơ hồ. Nói thẳng chuyện gì xảy ra.
- KHÔNG viết câu hỏi. Chỉ thông báo.
- TUÂN THỦ AI Voice Tier

Trả về: {dong_1, dong_2, tong_ky_tu}"
```

**Ví dụ push Retail:**
```
HPG bị yêu cầu giải trình BCTC — giá giảm 4.2%
Bạn cầm 2,000 HPG (35% danh mục)
```

**Ví dụ push Pro:**
```
HPG -4.2%, UBCKNN yêu cầu giải trình Q4
HPG 35% port, KN bán ròng 45 tỷ
```

**Bấm push → deep link đến thẻ tín hiệu trong app.**

### Chống spam

| Quy tắc | Giới hạn |
|---------|---------|
| Max push/ngày | 10 (không tính bản tin sáng) |
| Max push/giờ | 3 |
| Cùng mã trong 2 giờ | Gom thành 1 thẻ Compound, 1 push |
| Vượt giới hạn | Chỉ hiện trong app, không push |
| User tắt loại tín hiệu | Không push loại đó, vẫn hiện trên feed |

### Cài đặt thông báo (user tùy chỉnh)

```
THÔNG BÁO
────────────────────────────
Loại tín hiệu:
  ☑ Tin tức
  ☑ Cảm xúc thị trường
  ☑ Kỹ thuật
  ☑ Vĩ mô
  ☑ Tín hiệu liên quan (Adjacent)

Giờ yên tĩnh:
  [22:00] → [06:30]     ✏️ Chỉnh

Bản tin sáng:
  ☑ Bật  |  Giờ gửi: [07:00]  ✏️ Chỉnh
────────────────────────────
```

Mặc định: **bật hết**. Tắt loại nào → không push, vẫn hiện trên feed.

---

### BẢN TIN SÁNG

Gửi mỗi sáng (mặc định 07:00, user chỉnh được). Push 1 lần. Tối đa **600 ký tự**.

**Prompt tạo bản tin sáng:**
```
"Viết bản tin sáng. Bạn là cố vấn đầu tư cá nhân.

Dữ liệu:
- Profile: {profile}
- Danh mục: {mã + giá mua + giá hiện tại + tỷ trọng + tiền mặt %}
- Tín hiệu 24h qua cho từng mã: {danh sách tín hiệu + điểm}
- Dữ liệu kỹ thuật: {hỗ trợ/kháng cự, MA, RSI, KL}
- Bài học cá nhân: {thói quen + số lần + kết quả}
- Sự kiện hôm nay: {nếu có — BCTC, họp ĐHCĐ, FOMC...}
- Bối cảnh hôm qua: {thời gian dùng app, tín hiệu mở/bỏ qua}

Cấu trúc bắt buộc:

🔥 ĐIỂM TIN {ngày}
Mỗi mã trong danh mục — chỉ 1 tin quan trọng nhất:
- Tình hình ngắn gọn (1 dòng)
- Nhắc lỗi cũ CỤ THỂ (lỗi gì, bao nhiêu lần, kết quả)
- Mốc giá hành động CỤ THỂ (hỗ trợ, kháng cự, cắt lỗ, chốt lời)
- Nói thẳng nên làm gì (KHÔNG "tùy anh quyết định")
- Mã không có gì đáng nói → bỏ qua, không liệt kê

| HÔM NAY CẦN BIẾT
Tin vĩ mô + tác động CỤ THỂ đến danh mục (không nói chung chung)
Đưa kịch bản: "Nếu... thì..."

| TỔNG QUAN
1 dòng: tình trạng danh mục + tiền mặt
1 dòng: chiến lược hôm nay (1 câu duy nhất)

Quy tắc:
- NÓI THẲNG hành động, không vòng vo
- Nhắc lỗi cũ CỤ THỂ (không "hãy cẩn thận")
- Mốc giá CỤ THỂ (không "vùng hỗ trợ gần đây")
- Tối đa 600 ký tự
- TUÂN THỦ AI Voice Tier (Retail vs Pro)
- Nếu không có gì đặc biệt → bản tin ngắn 200 ký tự, đừng bịa"
```

**Ví dụ bản tin Retail:**

```
🔥 Điểm tin 18/03

HPG: Đang bị cơ quan quản lý yêu cầu giải trình.
Bạn đã 3 lần vội bán khi có tin xấu, cả 3 lần
giá quay lại tăng. Kết quả kinh doanh ra lúc 10h.
Không bán trước khi đọc số liệu.
Mốc: dưới 25,000 thì giảm 1/3, trên 26,000 thì giữ.

FPT: Ổn định, giữ. Nếu chạm 132,000 thì chốt lời.

| HÔM NAY CẦN BIẾT
Đồng đô la Mỹ mạnh lên đêm qua. Tiền nước ngoài
có thể rút ra → HPG, VCB dễ bị bán. Không mua mới.

| TỔNG QUAN
Danh mục: +0.8% | Tiền mặt: 15%
→ Chờ kết quả HPG, giữ nguyên, không mua mới.
```

**Ví dụ bản tin Pro:**

```
🔥 Điểm tin 18/03

VND: Ngành CK thiếu thanh khoản. Anh đã 2 lần
trung bình giá xuống khi mã thủng hỗ trợ — cả 2 lỗ thêm.
Nếu VND thủng 20,500 hôm nay → hạ phân nửa tỷ trọng.
Tuyệt đối không bắt đáy thêm.

KBC: +6.5%, tiền nội đổ giá cứng nhờ FDI giải ngân.
Giữ chặt, đợi chốt lời lên 32,000.

| HÔM NAY CẦN BIẾT
DXY bứt 105.2, khối ngoại dự kiến xả ròng ép
chỉ số test 1,200. Không vô hàng mới.
Nếu VNI giữ trên 1,200 → tín hiệu tốt. Thủng → risk-off.

| TỔNG QUAN
Danh mục: -1.2% | Tiền mặt: 120tr (40%)
→ Quản trị VND, gồng lãi KBC, chờ thời cơ tại 1,200.
```

---

## PHẦN 4: CHAT ĐÀO SÂU — Phụ trách: Việt Anh, Chi, Thiên

### 2 loại chat

---

**Loại 1 — Chat từ thẻ tín hiệu (bấm "Hỏi thêm")**

AI mở đầu = tóm tắt ngắn thẻ + **3 câu hỏi gợi ý bấm chọn** (chip buttons).

**Câu hỏi gợi ý thay đổi theo context:**

| Loại tín hiệu | Gợi ý 1 | Gợi ý 2 | Gợi ý 3 |
|---------------|---------|---------|---------|
| Tin xấu mã cầm | [Nên cắt lỗ không?] | [Lần trước thế nào?] | [Ảnh hưởng danh mục?] |
| Tin tốt mã cầm | [Nên chốt lời?] | [Còn tăng tiếp?] | [Mua thêm được không?] |
| Mã vượt đỉnh | [Đã muộn chưa?] | [Mốc chốt lời?] | [So với ngành?] |
| BCTC | [Phân tích nhanh?] | [So với kỳ trước?] | [Định giá hấp dẫn?] |
| Vĩ mô | [Danh mục bị gì?] | [Ngành nào lợi?] | [Lần trước sao?] |
| Liên quan (Adjacent) | [Giải thích mối liên quan?] | [Ảnh hưởng cụ thể?] | [Điều chỉnh gì?] |
| Tổng hợp (Compound) | [Tóm tắt?] | [Nguy hiểm cỡ nào?] | [Nên làm gì?] |

**Prompt tạo gợi ý:**
```
"Tạo 3 câu hỏi gợi ý cho nhà đầu tư.

Tín hiệu: {loại, mã, nội dung}
Bài học: {nếu có}
Profile: {profile}

Quy tắc:
- Mỗi câu ≤ 25 ký tự
- 3 câu phải khác GÓC: hành động / lịch sử / ảnh hưởng rộng
- Nếu có bài học → 1 câu hướng về bài học
- Dùng ngôn ngữ Retail hoặc Pro tương ứng"
```

**Prompt trả lời chat:**
```
"Trả lời câu hỏi nhà đầu tư.

Tín hiệu gốc: {nội dung đầy đủ}
Câu hỏi: {câu hỏi user}
Profile + Level: {profile, Retail/Pro}

=== PORTFOLIO CONTEXT ===
Chế độ: {FULL | WATCHLIST_ONLY}

[Nếu FULL — có portfolio:]
- Danh mục: {mã + giá mua + tỷ trọng + lời/lỗ}
- Rules user đã đặt: {stop_loss_pct, target_price}
- Nhật ký GD: {5 giao dịch gần nhất}
- Bài học: {thói quen + số lần + kết quả}

[Nếu WATCHLIST_ONLY:]
- Mã trong watchlist: {danh sách}
- Giá mua: không có
- Rules: không có

=== DỮ LIỆU THỊ TRƯỜNG ===
- Kỹ thuật: {hỗ trợ/kháng cự, MA, RSI}
- Định giá: {P/E, P/B vs ngành}
- Smart money: {khối ngoại, tự doanh gần nhất}
- Catalyst sắp tới: {sự kiện + ngày cụ thể}

=== QUY TẮC TRẢ LỜI ===

NGUYÊN TẮC CỐT LÕI:
- KHÔNG ra lệnh mua/bán/giữ
- KHÔNG nói "tôi khuyên bạn..."
- KHÔNG dự đoán giá ("SSI sẽ tăng lên...")
- Finpath là GƯƠNG PHẢN CHIẾU, không phải người ra quyết định

KHI USER HỎI "NÊN LÀM GÌ?" HOẶC "CÓ NÊN MUA/BÁN?":

→ Nếu FULL mode + có rules:
1. Mirror lại rules của chính user:
   "Bạn đặt stop loss X%, hiện tại đang Y%. Còn Z% nữa."
2. Show 3 kịch bản (NGANG HÀNG, không ưu tiên cái nào):
   • Kịch bản A: [mô tả] → tradeoff cụ thể
   • Kịch bản B: [mô tả] → tradeoff cụ thể
   • Kịch bản C: [mô tả] → tradeoff cụ thể
3. Nhắc bài học nếu liên quan:
   "N lần trước bạn [hành động], kết quả [outcome]."
4. KHÔNG kết luận nên chọn cái nào

→ Nếu WATCHLIST_ONLY mode:
1. Dùng giá thị trường hiện tại làm anchor:
   "SSI hiện tại X, từ đỉnh Y đã giảm Z%"
2. Show 3 kịch bản theo vùng giá thị trường
3. Dùng smart money thay cho rules cá nhân:
   "Dragon Capital vẫn giữ, chưa bán CP nào."
4. Kết thúc bằng: "Bạn có đang cầm [mã] không?"
   → Nếu có → hỏi giá vốn → unlock FULL mode

KHI USER HỎI PHÂN TÍCH/THÔNG TIN (không hỏi nên làm gì):
- Nói thẳng, có số liệu cụ thể
- Mốc giá kỹ thuật cụ thể (không "vùng hỗ trợ gần đây")
- Nhắc bài học nếu liên quan

GIỚI HẠN: Tối đa 200 ký tự
CUỐI MỖI TRẢ LỜI: 1-2 câu hỏi gợi ý tiếp theo (chip buttons)
TUÂN THỦ AI Voice Tier (Retail vs Pro)"
```

**Ví dụ Retail — User bấm "Nên cắt lỗ không?":**

```
🤖 "Chưa nên. Giá đang ở vùng nhiều người bán quá tay —
thường sẽ nảy lại. Kết quả kinh doanh ra lúc 10h,
chờ xem.

3 lần trước bạn vội bán khi có tin xấu, cả 3 lần
giá quay lại +8-15%. Lần này kiên nhẫn hơn.

Mốc: dưới 25,000 thì giảm 1/3."

[Kết quả KD lần trước?]  [Mốc chốt lời?]
```

**Ví dụ Pro — User bấm "Định giá hiện tại?":**

```
🤖 "P/E trailing 8.5x vs ngành 10.2x. P/B 1.2x.
EPS consensus Q4: 2,100đ → forward P/E 7.8x.
Về 25,000 → P/E 7.0x, vùng tích lũy dài hạn.

Upside risk/reward tốt nếu BCTC không tệ hơn kỳ vọng.
Hỗ trợ cứng 25,500 (MA200). Kháng cự 29,000."

[Khối ngoại đang làm gì?]  [So với HSG, NKG?]
```

---

**Loại 2 — Chat tự do**

Nút **"+ Hỏi AI điều mới"** ở tab Chat → mở conversation mới.

- AI vẫn biết danh mục + bài học (load context tự động)
- Disclaimer lần đầu (chỉ 1 lần): "Đây là phân tích tham khảo, không phải khuyến nghị đầu tư."
- Lịch sử chat lưu theo conversation, mỗi conversation gắn tag (mã/chủ đề)

### Giới hạn chat

| | Free | Pro |
|--|------|-----|
| Cuộc chat/ngày | 3 | Không giới hạn |
| Lượt hỏi đáp/cuộc | 10 | 30 |
| Model AI | Gemini Flash / GPT-4o mini | Gemini Pro / GPT-4o |

**Hết free:**
```
🤖 "Bạn đã dùng hết 3 lượt hôm nay.
   Nâng cấp Pro để chat không giới hạn
   với AI thông minh hơn."

          [🔓 Nâng cấp Pro — 549K/tháng]
```

Reset 00:00 mỗi ngày.

---

## PHẦN 5: ONBOARDING UI SPEC — Phụ trách: Thiên, Hưởng

### Yêu cầu thiết kế chung

| Yêu cầu | Chi tiết |
|---------|---------|
| Nền | Tối (dark mode) — màu chủ đạo #0D1117 hoặc tương tự |
| Font AI text | 16px, line-height 1.5 |
| Font thẻ tiêu đề | 16px bold |
| Font thẻ mô tả | 14px regular |
| Spacing giữa thẻ | 12px |
| Padding trong thẻ | 16px |
| Animation chuyển màn | Slide trái, 300ms ease-out |
| AI avatar | Tròn 40px, góc trên trái bong bóng chat |
| Bong bóng AI | Nền khác nhẹ (#1C2333), bo góc 16px, padding 16px |
| Thẻ lựa chọn | Bo góc 12px, viền 1px mờ. Chọn: viền 2px brand color + ✓ góc |
| Nút chính | Full width, bo góc 8px, màu brand, text trắng bold 16px |
| Nút phụ | Text 14px, màu mờ, không nền, không viền |
| Progress | 5 chấm tròn (●●●○○) phía trên, cho biết đang ở bước mấy |

### Chi tiết từng màn

**Màn 1 — Chọn thị trường:**
- 3 thẻ xếp dọc, mỗi thẻ: icon lớn + tên + mô tả 1 dòng
- Chọn 1 → viền đổi brand color + ✓ → tự chuyển 0.5s (không cần nút)
- Không cho chọn nhiều

**Màn 2 — Chọn phong cách:**
- 2 thẻ xếp dọc (Retail / Pro) — nội dung khác theo thị trường
- Mỗi thẻ: tên + mô tả 2 dòng
- Chọn 1 → tự chuyển

**Màn 3 — Nhập mã:**
- CK/Crypto: Ô text input + autocomplete dropdown + chip đã chọn + chip gợi ý phổ biến
- Gold_Retail: 3 nút chọn (SJC / Nhẫn / Tỷ giá)
- Gold_Pro: 3 nút tỷ trọng (< 10% / 10-30% / > 30%)
- Nút phụ: "📸 Tải ảnh chụp danh mục" (icon + text nhỏ)
- Nút chính: [Gửi phản hồi] full width
- Nút phụ dưới: "Bỏ qua" (text mờ)

**Màn 4 — Xin quyền thông báo:**
- Icon 🔔 lớn giữa màn hình (64px) + animation pulse
- Text AI giải thích tại sao cần
- [🔔 Cấp quyền thông báo] nút chính
- "Bỏ qua" nút phụ — nhắc lại sau 3 ngày

**Màn 5 — Feed với tín hiệu mẫu:**
- Transition: fade in từ dưới lên, stagger 200ms mỗi thẻ
- Thẻ chào mừng + 2-3 thẻ tín hiệu mẫu (dữ liệu thật theo profile + mã)
- Từ đây = app chính, không còn onboarding

---

*Hết Nhóm C*

---

## CHANGE LOG

| Ngày | Phiên bản | Thay đổi |
|------|-----------|----------|
| 18/03/2026 | 2.0 | Cập nhật từ v1 |
| 19/03/2026 | 2.1 | Thêm người phụ trách từng phần |
| 19/03/2026 | 2.2 | Cập nhật PHẦN 1 layout card theo mockup v3: bỏ tag, bỏ label section, CTA pill, lịch sử ● dot, thêm Detail View |
| 19/03/2026 | 2.3 | Cập nhật PHẦN 2: đơn giản hóa feed, layout bản tin thu gọn/mở rộng theo mockup |
| 19/03/2026 | 2.4 | Bản tin sáng: accordion (không navigate). Compound: bỏ toggle xem nguồn trên card |
| 23/03/2026 | 2.5 | PHẦN 4 Chat: cập nhật prompt trả lời — thêm FULL vs WATCHLIST_ONLY mode, 3-kịch bản tradeoff, mirror rules user, cấm directional call |
| 23/03/2026 | 2.6 | PHẦN 1 Card: thay "Bối cảnh cá nhân" (conditional) bằng "Góc nhìn" (luôn hiện, 3 chế độ FULL/WATCHLIST_ONLY/GENERIC) + inline buttons UX flow + prompt tạo Góc nhìn |
| 23/03/2026 | 2.7 | GÓC NHÌN spec: thêm Retail vs Pro voice examples + quy tắc ngôn ngữ (cắt lỗ/chốt lời thay stop loss/take profit, không viết tắt DM/SL, không dùng tiếng Anh thông dụng) + cập nhật prompt |
