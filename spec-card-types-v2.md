# SPEC: CÁC LOẠI THẺ TÍN HIỆU
**Dành cho Team B (tạo tín hiệu) + Team C (render UI)**
**Version 2.0 — 24/03/2026**

---

## 1. TỔNG QUAN

### Mục đích
Tài liệu này định nghĩa cấu trúc, logic hiển thị và data schema cho tất cả loại thẻ tín hiệu trong feed Tình báo của Finpath.

### Nguyên tắc chung
- Mỗi card phản ánh **1 loại thông tin cụ thể** — không trộn lẫn
- Nội dung phải **văn xuôi có nghĩa**, không liệt kê số liệu thô
- Tuyệt đối **không directional call** (nên mua / nên bán / nên giữ)
- Finpath = gương phản chiếu — chỉ mirror tình huống của user

### Điểm chung bắt buộc mọi card
| Section | Mô tả |
|---|---|
| Lịch sử tương tự | Sự kiện tương tự trong quá khứ → kết quả thực tế |
| Góc nhìn | Cá nhân hóa theo mode (xem Spec Góc nhìn v2.0) |
| CTA | "Hỏi thêm về [Mã]" hoặc "Hỏi thêm về thị trường" |

### 3 chế độ hiển thị
| Mode | Điều kiện | Góc nhìn |
|---|---|---|
| **Full** | User có mã trong portfolio (có giá vốn) | Tỷ trọng, lãi/lỗ, cắt lỗ/chốt lời, bài học lặp |
| **Watchlist** | Mã trong watchlist, chưa có giá vốn | Nhận định trung lập, không directional |
| **Generic** | Không có data gì về user với mã này | Thị trường + smart money + 1 điều cần theo dõi |

### Giọng điệu (Voice)
| Tier | Câu văn | Số liệu | Thuật ngữ |
|---|---|---|---|
| **Retail** (<2 năm) | Đầy đủ, giải thích | "chiếm 42% danh mục" | "vượt mốc giá quan trọng" |
| **Pro** (>2 năm) | Gọn, không giải thích | "42% DM" | "vượt kháng cự" |

### Quy tắc ngôn ngữ — BẮT BUỘC
| ❌ Không dùng | ✅ Thay bằng |
|---|---|
| Stop loss | Cắt lỗ / ngưỡng cắt lỗ |
| Take profit | Chốt lời / mức chốt lời |
| SL, TP (viết tắt) | Viết đầy đủ |
| DM (viết tắt) | Danh mục |
| Catalyst | Sự kiện thúc đẩy |
| Breakout | Vượt kháng cự |
| Signal | Tín hiệu |
| Compound | Tổng hợp |
| Exposure | Tỷ trọng |
| Timing | Thời điểm |

### Giới hạn ký tự Góc nhìn
- Toàn bộ Góc nhìn: ≤ 130 ký tự

---

## 2. CÁC LOẠI CARD

---

### CARD ①: NHẬN ĐỊNH PHIÊN
**Nhóm thông tin:** ⑥ Thị trường chung
**Nguồn chính:** #5 BC thị trường, #8 Data chart
**Mode:** Generic / Watchlist (khi user có mã liên quan)

**Cấu trúc:**
```
● dot-blue   VN-INDEX   [mode]   07:00

[Tiêu đề — nhận định nhẹ về thị trường hôm nay]

[Nội dung — văn xuôi 1–2 câu có nghĩa, không liệt kê
 VD: "Điểm số VN-Index đang bị méo mó bởi nhóm Vingroup.
 Nếu loại trừ nhóm này, thị trường thực chất chỉ ở mức
 1,300 điểm với vô số cổ phiếu tốt dưới giá trị thật."]

Góc nhìn: [Generic: nhận định chung]
           [Watchlist: liên kết mã đang theo dõi]

CTA: Hỏi thêm về thị trường
```

**Không có:** Impact row, Lịch sử

**Data schema (Team B output):**
```json
{
  "card_type": "nhan_dinh_phien",
  "mode": "generic | watchlist",
  "ticker": "VN-INDEX",
  "time": "07:00",
  "severity": "blue",
  "title": "string (≤80 ký tự)",
  "body": "string (≤150 ký tự, văn xuôi)",
  "goc_nhin": {
    "retail": "string (≤130 ký tự)",
    "pro": "string (≤80 ký tự)"
  }
}
```

---

### CARD ②: KỸ THUẬT
**Nhóm thông tin:** ① Giá & Kỹ thuật
**Nguồn chính:** #8 AI Bot + Data chart
**Mode:** Full / Watchlist / Generic

**Cấu trúc:**
```
● dot   [Mã]   [mode]   HH:MM

[Tiêu đề]

Tín hiệu: [1 câu — sự kiện kỹ thuật gì xảy ra]

Nguyên nhân: [1 câu — vì sao xuất hiện tín hiệu này]

Bối cảnh kỹ thuật: [1 câu — Vol, RSI, MA, hỗ trợ/kháng cự
 VD: "Khối lượng gấp 2.3 lần TB · RSI 28 quá bán ·
 Giá dưới MA20 và MA50 · Hỗ trợ tiếp theo 24,500đ"]

Lịch sử: [X lần tương tự → kết quả Y%]

Góc nhìn: [theo mode]

CTA: Hỏi thêm về [Mã]
```

**Data schema (Team B output):**
```json
{
  "card_type": "ky_thuat",
  "mode": "full | watchlist | generic",
  "ticker": "string",
  "time": "HH:MM",
  "severity": "red | yellow | blue | green",
  "title": "string (≤80 ký tự)",
  "tin_hieu": "string (≤100 ký tự)",
  "nguyen_nhan": "string (≤100 ký tự)",
  "boi_canh_ky_thuat": "string (≤100 ký tự)",
  "lich_su": "string (≤100 ký tự)",
  "goc_nhin": {
    "retail": "string (≤130 ký tự)",
    "pro": "string (≤80 ký tự)"
  }
}
```

---

### CARD ③: TIN TỨC DN — 6 sub-type
**Nhóm thông tin:** ③ Tin tức doanh nghiệp
**Nguồn chính:** #1 BCTC, #7 Báo chí + CG Finpath

---

#### Sub ①: KQKD / BCTC
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM

[Tiêu đề]

↑/↓ Impact row — Tác động: [mức độ] — [hướng + ngắn gọn]

[Nội dung — doanh thu, LN vs kỳ trước + vs kỳ vọng analyst]

Lịch sử: [kết quả KQKD tương tự trước → phản ứng giá]

Góc nhìn: [theo mode]
CTA
```

**Schema bổ sung:**
```json
{
  "sub_type": "kqkd",
  "impact_level": "high | medium | low",
  "impact_direction": "positive | negative | neutral",
  "doanh_thu": "string",
  "loi_nhuan": "string",
  "vs_cung_ky": "string",
  "vs_ky_vong": "string"
}
```

---

#### Sub ②: Sự kiện lịch
*(chốt quyền, ĐHCĐ, ngày ra BCTC)*
**Mode:** Full / Watchlist
**Trigger:** X ngày trước sự kiện (chốt quyền: 3 ngày, ĐHCĐ: 7 ngày)

```
● dot-blue   [Mã]   [mode]   Còn X ngày

[Tiêu đề — tên sự kiện]

[Nội dung — tỷ lệ / ngày / điều kiện (ngắn gọn)]

Góc nhìn:
 Full: "Bạn đang cầm X CP → nhận Y đồng nếu giữ qua ngày Z"
 Watchlist: "Cần quyết định giữ hay bán trước ngày Z"
CTA
```

**Không có:** Impact row, Lịch sử

**Schema bổ sung:**
```json
{
  "sub_type": "su_kien_lich",
  "event_name": "string",
  "event_date": "YYYY-MM-DD",
  "days_away": "number",
  "ty_le": "string",
  "ngay_chot": "YYYY-MM-DD",
  "ngay_thanh_toan": "YYYY-MM-DD"
}
```

---

#### Sub ③: Hành động doanh nghiệp
*(phát hành thêm, mua CP quỹ, chia tách)*
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM
[Tiêu đề]
↑/↓ Impact row
[Nội dung — số CP, % pha loãng, mục đích]
Lịch sử · Góc nhìn · CTA
```

**Schema bổ sung:**
```json
{
  "sub_type": "hanh_dong_dn",
  "action_type": "phat_hanh_them | mua_cp_quy | chia_tach",
  "so_luong_cp": "string",
  "ty_le_pha_loang_pct": "number",
  "muc_dich": "string"
}
```

---

#### Sub ④: Nhân sự
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM
[Tiêu đề]
↑/↓ Impact row
[Nội dung — ai ra, ai vào, lý do, thời điểm hiệu lực]
Lịch sử · Góc nhìn · CTA
```

**Schema bổ sung:**
```json
{
  "sub_type": "nhan_su",
  "nguoi_roi": "string",
  "vi_tri_roi": "string",
  "nguoi_vao": "string | null",
  "ly_do": "string",
  "hieu_luc": "YYYY-MM-DD"
}
```

---

#### Sub ⑤: Thương vụ & Hợp đồng
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM
[Tiêu đề]
↑/↓ Impact row
[Nội dung — giá trị deal, đối tác, ý nghĩa chiến lược]
Lịch sử · Góc nhìn · CTA
```

**Schema bổ sung:**
```json
{
  "sub_type": "thuong_vu",
  "deal_type": "ma | hop_dong | hop_tac",
  "doi_tac": "string",
  "gia_tri": "string",
  "y_nghia_chien_luoc": "string"
}
```

---

#### Sub ⑥: Cảnh báo rủi ro
*(khởi tố, kiểm soát, đình chỉ giao dịch)*
**Mode:** Full / Watchlist — KHÔNG có Generic
**Severity:** luôn đỏ (red)

```
● dot-red   [Mã]   [mode]   HH:MM
[Tiêu đề]
↓ Impact row (luôn đỏ, severity cao)
[Nội dung — tính chất sự kiện, hệ quả pháp lý]
Lịch sử (precedent — sự kiện tương tự và hậu quả)
Góc nhìn (ưu tiên cảnh báo rõ ràng) · CTA
```

**Schema bổ sung:**
```json
{
  "sub_type": "canh_bao_rui_ro",
  "risk_type": "khoi_to | kiem_soat | dinh_chi | margin_call",
  "tinh_chat": "string",
  "he_qua_phap_ly": "string"
}
```

---

### CARD ④: DÒNG TIỀN — 2 sub-type
**Nhóm thông tin:** ② Dòng tiền
**Nguồn chính:** #8 Data chart

---

#### Sub A: Dòng tiền tổ chức
*(khối ngoại + tự doanh)*
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM
[Tiêu đề]
[Nội dung — văn xuôi linh hoạt theo data có sẵn]
[Đánh giá bất thường — gấp X lần TB 20 phiên]
Lịch sử · Góc nhìn · CTA
```

**Lưu ý:** Không nhồi đủ 3 nhóm nếu thiếu data. Gen text theo data thực tế có sẵn.

**Schema:**
```json
{
  "card_type": "dong_tien",
  "sub_type": "to_chuc",
  "ticker": "string",
  "khoi_ngoai_ty": "number | null",
  "tu_doanh_ty": "number | null",
  "ca_nhan_ty": "number | null",
  "so_phien_lien_tiep": "number",
  "bat_thuong_x_lan": "number",
  "body": "string (văn xuôi)",
  "danh_gia_bat_thuong": "string",
  "lich_su": "string",
  "goc_nhin": { "retail": "string", "pro": "string" }
}
```

---

#### Sub B: Cá mập / Block trade
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM
[Tiêu đề]
[Nội dung — khối lượng lớn bất thường, thời điểm]
[Đánh giá bất thường — gấp X lần TB cùng khung giờ]
Lịch sử · Góc nhìn · CTA
```

**Schema:**
```json
{
  "sub_type": "ca_map",
  "so_lenh_block": "number",
  "tong_so_cp": "number",
  "thoi_diem": "HH:MM",
  "bat_thuong_x_lan": "number",
  "body": "string",
  "danh_gia_bat_thuong": "string"
}
```

---

### CARD ⑤: VĨ MÔ
**Nhóm thông tin:** ④ Vĩ mô & Ngành
**Nguồn chính:** #5 BC thị trường, #7 Báo chí
**Mode:** Full / Watchlist / Generic

**Cấu trúc:**
```
● dot   [VĨ MÔ / Mã nếu có]   [mode]   HH:MM

[Tiêu đề]

Sự kiện: [1–2 câu mô tả điều vừa xảy ra]

Ngành bị ảnh hưởng:
↑ Hưởng lợi: [Ngành A · Ngành B · Ngành C]
↓ Cần theo dõi: [Ngành X · Ngành Y]

Lịch sử: [sự kiện macro tương tự → VN-Index/ngành phản ứng]

Góc nhìn: [liên kết mã trong danh mục/watchlist với nhóm hưởng lợi]
CTA
```

**Schema:**
```json
{
  "card_type": "vi_mo",
  "ticker": "VI_MO | string nếu có mã cụ thể",
  "su_kien": "string (≤100 ký tự)",
  "nganh_huong_loi": ["string"],
  "nganh_can_theo_doi": ["string"],
  "lich_su": "string",
  "goc_nhin": { "retail": "string", "pro": "string" }
}
```

---

### CARD ⑥: PHÂN TÍCH & KHUYẾN NGHỊ
**Nhóm thông tin:** ⑤ Phân tích & Khuyến nghị
**Nguồn chính:** #3 BC CTCK, #7 CG Finpath
**Mode:** Full / Watchlist / Generic

**Cấu trúc:**
```
● dot   [Mã]   [mode]   HH:MM

[Tiêu đề — [Nguồn] nâng/hạ khuyến nghị, giá mục tiêu]

Khuyến nghị: [Mua/Giữ/Bán] · Giá mục tiêu: Xđ · Upside: +Y%

Luận điểm: [văn xuôi 2–3 câu giải thích lý do.
Trong 30 ngày, [CTCK khác] cũng nâng/hạ giá mục tiêu TB Xđ.
[Nguồn] đúng X/Y khuyến nghị gần nhất.]

Lịch sử: [lần khuyến nghị trước từ nguồn này → kết quả thực tế]

Góc nhìn: [theo mode]
CTA
```

**Lưu ý:** Đồng thuận + track record viết liền vào cuối luận điểm — không tách section riêng.
**Nguồn:** CTCK và KOL dùng chung template.

**Schema:**
```json
{
  "card_type": "phan_tich_kn",
  "nguon": "string (tên CTCK hoặc KOL)",
  "khuyen_nghi": "mua | giu | ban",
  "gia_muc_tieu": "number",
  "upside_pct": "number",
  "luan_diem": "string (≤200 ký tự, bao gồm đồng thuận + track record)",
  "dong_thuan": {
    "ctck_khac": ["string"],
    "gia_muc_tieu_tb": "number"
  },
  "track_record": "X/Y",
  "lich_su": "string",
  "goc_nhin": { "retail": "string", "pro": "string" }
}
```

---

### CARD ⑦: PRO BLUR
**Áp dụng cho:** Tin Pro trong Tình báo cá nhân + rule setup sau
**Mode:** Generic (luôn luôn)

**Cấu trúc:**
```
[Toàn bộ card bị blur 5px — kể cả tiêu đề]

Overlay:
  "Nội dung dành riêng cho Pro"
  "Mở khóa để xem phân tích đầy đủ"
  "Hoặc unlock miễn phí sau 24 giờ"
  [CTA: Nâng cấp Pro · 549K/tháng]
```

**Schema:**
```json
{
  "card_type": "pro_blur",
  "underlying_card": { /* full card data của card gốc */ },
  "unlock_after_hours": 24
}
```

---

### CARD ⑧: COMPOUND — 2 sub-type
**Trigger:** 3+ tín hiệu từ các nhóm khác hội tụ cùng 1 mã
**Nguồn:** Tổng hợp từ tất cả

---

#### Sub A: Hội tụ cùng mã
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã]   [mode]   HH:MM

[Tiêu đề — X tín hiệu hội tụ trên [Mã] trong Y giờ]

[Nội dung — văn xuôi tổng hợp insight từ tất cả tín hiệu.
 Không liệt kê, viết liền thành 1 đoạn có logic nhân quả.
 VD: "HPG đang chịu áp lực từ nhiều phía cùng lúc —
 kỹ thuật thủng hỗ trợ, tin xấu điều tra thép HRC, và
 khối ngoại bán ròng mạnh. Ba yếu tố này hiếm khi
 xảy ra đồng thời trên HPG."]

Lịch sử: [X lần có 3+ tín hiệu đồng thời → kết quả]

Góc nhìn: [theo mode]
CTA
```

**Schema:**
```json
{
  "card_type": "compound",
  "sub_type": "hoi_tu",
  "so_tin_hieu": "number",
  "signal_ids": ["string"],
  "body": "string (văn xuôi tổng hợp)",
  "lich_su": "string",
  "goc_nhin": { "retail": "string", "pro": "string" }
}
```

---

#### Sub B: Chuỗi lan toả (A → B → C)
**Mode:** Full / Watchlist / Generic

```
● dot   [Mã đích]   [mode]   HH:MM

[Tiêu đề — [Mã A] ảnh hưởng [Mã B] qua kênh X]

[Disclaimer italic: "Tín hiệu gián tiếp — chưa ảnh hưởng ngay"]

Chuỗi bước (mỗi bước: icon + bold label + mô tả ngắn):
  [icon] Bước 1: [Mã A] — sự kiện xảy ra
  [icon] Bước 2: Cơ chế lan toả (nợ xấu / chuỗi cung ứng / ...)
  [icon] Bước 3: [Mã B] — rủi ro/cơ hội tiềm ẩn

Lịch sử: [precedent tương tự → kết quả]

Góc nhìn: [theo mode]
CTA
```

**Schema:**
```json
{
  "sub_type": "lan_toa",
  "ma_nguon": "string",
  "ma_dich": "string",
  "kenh_lan_toa": "string",
  "buoc": [
    { "icon": "string", "label": "string", "mo_ta": "string" }
  ],
  "lich_su": "string",
  "goc_nhin": { "retail": "string", "pro": "string" }
}
```

---

## 3. MÀU SEVERITY DOT

| Màu | Khi nào dùng |
|---|---|
| 🔴 Đỏ | Rủi ro cao, tín hiệu tiêu cực mạnh |
| 🟡 Vàng | Cảnh báo trung bình, cần theo dõi |
| 🔵 Xanh dương | Thông tin trung tính / tích cực nhẹ |
| 🟢 Xanh lá | Tín hiệu tích cực rõ ràng |

---

## 4. IMPACT ROW

| Level | Màu | Ví dụ text |
|---|---|---|
| Cao tích cực | Xanh lá | "↑ Tác động: Cao — tích cực dài hạn" |
| Cao tiêu cực | Đỏ | "↓ Tác động: Rất cao — rủi ro nghiêm trọng" |
| Trung bình | Vàng | "↕ Tác động: Trung bình — cần theo dõi thêm" |
| Thông tin | Xanh dương | "● Thông tin — không tác động ngay" |

---

## 5. INPUT CẦN TỪ TEAM A (User data)

| Field | Dùng để render |
|---|---|
| `portfolio[].ticker` | Xác định mode Full |
| `portfolio[].gia_mua` | Tính lãi/lỗ |
| `portfolio[].ty_trong_pct` | "chiếm X% danh mục" |
| `portfolio[].nguong_cat_lo_pct` | "Cắt lỗ còn X% nữa" |
| `portfolio[].muc_chot_loi` | "Còn X% đến mức chốt lời" |
| `portfolio[].so_luong_cp` | Tính cổ tức cụ thể |
| `watchlist[].ticker` | Xác định mode Watchlist |
| `behavioral_patterns[]` | "X lần trước bạn..." |
| `user_tier` | retail / pro → chọn voice |

---

## 6. THỨ TỰ BUILD — MVP vs LATER

### MVP (Sprint 1–2)
1. ② Kỹ thuật
2. ④ Dòng tiền Sub A (Tổ chức)
3. ③ Tin tức DN Sub ① KQKD
4. ③ Tin tức DN Sub ⑥ Cảnh báo rủi ro
5. ⑥ Nhận định phiên (Generic — content chung)

### Later (Sprint 3+)
6. ⑤ Vĩ mô
7. ⑥ Phân tích & KN
8. ③ Tin tức DN Sub ② ③ ④ ⑤
9. ④ Dòng tiền Sub B (Cá mập)
10. ⑧ Compound Sub A + B

---

*Tài liệu tham chiếu:*
- *Spec Góc nhìn v2.0 — quy tắc chi tiết cho section Góc nhìn*
- *Brief Team B v4.2 — schema detect + score tín hiệu*
- *Brief Team C v2.7 — render UI + push notification*
- *Mockup card-types-v5.html — visual reference*
