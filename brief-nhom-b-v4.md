# FINPATH AI CO-PILOT — BRIEF SPRINT 1
# NHÓM B: BỘ MÁY TÍN HIỆU (2 lập trình viên)

> **Phiên bản:** 4.0 — 18/03/2026
> **Người soạn:** Việt Anh (Product Design Lead)
> **Sprint:** 1 (10 ngày làm việc)
> **Team:** 2 lập trình viên
> **Thay đổi v4:** Thêm Nguồn 8 (Tin tình báo room), đổi Nguồn 3 thành Transcript Social, bổ sung prompt extract + chunk chi tiết cho BCTC/CTCK/Chỉ báo KT, thống nhất quy tắc voice output, sửa ngôn ngữ Pro

---

## QUY TẮC OUTPUT CHUNG (áp dụng tất cả nguồn)

### Retail (tối đa 4-5 câu)
- Tiếng Việt đời thường, KHÔNG thuật ngữ tài chính
- Giải thích "so what?" — chuyện này ảnh hưởng gì đến user
- Dùng so sánh dễ hiểu: "bán nhiều hơn 39%", "lời mỗi đồng ít hơn"
- Kết bằng hành động: nên theo dõi / nên thận trọng / đừng vội

### Pro (tối đa 3 dòng)
- Tiếng Việt gọn bén + số liệu cụ thể
- Viết tắt phổ biến OK: DT, LN, LNST, KN, TB, vol, RSI, P/E, P/B, CTCK, TP
- **KHÔNG lạm dụng tiếng Anh không phổ biến:**
  - ❌ compound, signal, confirm, historical, avg, room (dư địa), streak, breakout, money flow, death cross, golden cross, overbought, trailing, deep-dive
  - ✅ Thay bằng: tín hiệu kép, xác nhận, lịch sử, trung bình, dư địa, vượt kháng cự, cắt lên/xuống, dòng tiền, cắt xuống MA200, cắt lên MA200, quá mua, kỳ gần nhất, phân tích chuyên sâu

### Luôn cross-ref với danh mục user
- Nêu rõ: "Bạn cầm X (Y%)" hoặc "X trong watchlist"
- Tính impact cụ thể nếu có thể

---

## PHẦN 1: PHÁT HIỆN TÍN HIỆU — Phụ trách: Hà, Hưởng

### Mục tiêu
Quét liên tục dữ liệu từ **8 nguồn**, phát hiện những điều bất thường hoặc đáng chú ý liên quan đến danh mục + watchlist của người dùng.

---

### NGUỒN 1 — GIÁ (thời gian thực)

| Quy tắc | Cổ phiếu | Crypto | Vàng |
|---------|----------|--------|------|
| Giá sập | ≥ 7% | ≥ 15% | ≥ 3% |
| Giá giảm mạnh | 3-7% | 7-15% | 1.5-3% |
| Giá tăng mạnh | ≥ 5% | ≥ 10% | ≥ 2% |
| Giá trần/sàn | Chạm trần hoặc sàn | — | — |
| KL bùng nổ | Gấp ≥ 5 lần TB 20 ngày | Gấp ≥ 5 lần | — |
| KL bất thường | Gấp 3-5 lần TB | Gấp 3-5 lần | — |

**Ví dụ tín hiệu Giá:**

Retail:
```
"HPG giảm 4.2% trong phiên sáng — dòng tiền đổ vào gấp 2.8 lần bình thường.
Tiền nước ngoài đang rút ra."
```

Pro:
```
"HPG -4.2% (26,400→25,300). Vol 2.8x TB 20 phiên.
KN bán ròng 45 tỷ. Tự doanh mua ròng 12 tỷ."
```

---

### NGUỒN 2 — TIN TỨC SỰ KIỆN DOANH NGHIỆP (quét mỗi 5 phút)

**3 mức từ khóa:**

| Mức | Từ khóa |
|-----|---------|
| Nghiêm trọng | phá sản, điều tra, truy tố, bắt giữ, tạm ngưng GD, hủy niêm yết |
| Cao | M&A, sáp nhập, cổ tức đặc biệt, thoái vốn, ESOP, phát hành thêm |
| Trung bình | BCTC, đại hội cổ đông, nhân sự cấp cao, hợp đồng lớn |

**Từ khóa bổ sung theo profile:**

| Profile | Từ khóa thêm |
|---------|-------------|
| Stock_Pro | margin call, room ngoại, tự doanh mua/bán ròng, ETF rebalance |
| Crypto_Retail | listing sàn lớn, airdrop, burn token, hack sàn |
| Crypto_Pro | SEC, quy định mới, stablecoin depeg, sàn đóng rút tiền |
| Gold_Retail | giá vàng SJC tăng/giảm đột biến, chênh lệch mua-bán |
| Gold_Pro | Fed, FOMC, Non-farm payroll, CPI, DXY, trữ lượng vàng NHTW |

**Phân loại tự động (rules, không cần LLM):**

| Loại | Từ khóa trigger | Severity mặc định |
|------|-----------------|-------------------|
| Pháp lý | khởi tố, tạm giam, điều tra, giải trình, xử phạt | high |
| Cổ tức | chia cổ tức, cổ tức tiền mặt, ngày chốt | medium |
| Phát hành | phát hành, tăng vốn, ESOP, trái phiếu | medium |
| M&A | sáp nhập, mua lại, thoái vốn, chuyển nhượng | high |
| Nhân sự | chủ tịch, CEO, TGĐ, từ nhiệm, bổ nhiệm | high |
| Hợp đồng | ký kết, hợp đồng, trúng thầu, đối tác | medium |
| Vận hành | đóng cửa, mở rộng, nhà máy, công suất | medium |
| Room ngoại | nới room, room ngoại, tỷ lệ sở hữu | high |

**Chunk schema:**

```json
{
  "type": "corporate_event",
  "subtype": "legal | dividend | issuance | ma | personnel | contract | operation | foreign_room",
  "ticker": "VNM",
  "date": "2026-03-18",
  "content": "≤150 ký tự",
  "numbers": { ... },
  "severity": "high | medium",
  "catalyst_deadline": {
    "event": "Tên sự kiện sắp tới liên quan (nếu có)",
    "date": "YYYY-MM-DD",
    "days_away": 15
  }
}
```

**Cross-reference quan trọng:**

| Sự kiện | Cross-ref với gì | Insight |
|---------|------------------|---------|
| Giải trình BCTC | BCTC chunks → số liệu nào bất thường? | "Phải thu tăng 42% — có thể đây là vấn đề" |
| Cổ tức | Giá hiện tại → yield bao nhiêu? Lịch sử chia? | "Yield 5.2%, cao hơn TB 3 năm (3.8%)" |
| Phát hành CP | Giá phát hành vs giá thị trường? Pha loãng? | "Giá PH thấp hơn thị trường 8% → áp lực giá" |
| Nới room ngoại | Room hiện tại? KN đang mua hay bán? | "Room còn 5%, KN mua ròng 3 phiên → catalyst" |

**Ví dụ:**

Retail:
```
"VNM chia cổ tức 50% bằng tiền mặt — mỗi cổ phiếu nhận 5,000đ.
Ngày chốt 15/04. Tỷ lệ cao hơn 3 năm gần đây.
Lợi suất khoảng 5.2% — hấp dẫn hơn gửi tiết kiệm."
```

Pro:
```
"VNM: Cổ tức 50% tiền mặt, 5,000đ/CP. Chốt 15/04. Yield 5.2% vs TB 3 năm 3.8%.
Tỷ lệ cao nhất 4 năm. Giá thường tăng 3-5% trước ngày chốt 1-2 tuần."
```

---

### NGUỒN 3 — TRANSCRIPT SOCIAL (xử lý khi có nội dung mới)

**Đặc thù:** Nguồn này là **transcript dài từ livestream YouTube/KOL** (15-30 phút), KHÔNG phải comment ngắn. Cần LLM extract insight.

**Prompt extract:**

```
"Bạn là AI phân tích nội dung đầu tư của Finpath.
Đọc transcript từ livestream/video và extract các insight có giá trị.

Quy tắc:
- Bỏ qua: chào hỏi, quảng cáo, mời mở tài khoản, lặp ý
- Chỉ giữ: nhận định thị trường, khuyến nghị cụ thể, data định giá, chiến lược
- Mỗi chunk là 1 ý độc lập, đọc riêng vẫn hiểu
- Tối đa 200 ký tự mỗi chunk
- Ghi rõ nguồn (tên KOL/kênh)
- Phân biệt: opinion (nhận định) vs fact (dữ kiện có số liệu)

Types cho phép:
- market_view: nhận định tổng quan thị trường
- sector_view: nhận định về 1 ngành cụ thể
- stock_call: khuyến nghị mua/bán mã cụ thể
- valuation_insight: nhận định về định giá (có số liệu)
- risk_warning: cảnh báo rủi ro cụ thể
- strategy: chiến lược đầu tư gợi ý

Output JSON array:
[
  {
    'type': '...',
    'source': 'tên KOL — tên kênh',
    'date': 'YYYY-MM-DD',
    'ticker': 'mã CP hoặc null',
    'sector': 'tên ngành hoặc null',
    'content': '≤200 ký tự',
    'tags': ['...'],
    'opinion_or_fact': 'opinion | fact | mixed'
  }
]"
```

**Data mẫu — Transcript KOL Quang Dũng:**

Input (tóm tắt): Livestream 20 phút về "cổ phiếu thay nhau tạo đáy" — phân tích 6 phân lớp đổ vỡ từ T9/2025 đến T3/2026, so sánh P/E hiện tại vs lịch sử, nhận định BĐS + NH rẻ.

Output — 7 chunks:

```json
[
  {
    "type": "market_view",
    "source": "Quang Dũng — Kênh Đầu tư CK cùng Quang Dũng",
    "ticker": null,
    "content": "6 phân lớp đổ vỡ lần lượt từ T9/2025 → T3/2026: NH → CK → BĐS → NQ68 → NQ79 → năng lượng. 90% CP đã giảm. Đang chuyển sang giai đoạn dừng rơi và tạo đáy.",
    "tags": ["tao_day", "ha_canh_mem", "toan_thi_truong"],
    "opinion_or_fact": "opinion"
  },
  {
    "type": "valuation_insight",
    "source": "Quang Dũng",
    "ticker": null,
    "content": "P/E toàn TT ~13.5 năm. Trừ NQ68+79 còn ~12 năm. So sánh: đỉnh 2008=44, đỉnh 2022=17, đáy 2009=8.5, đáy 2022=10.5. Hiện 12 — gần vùng đáy lịch sử.",
    "tags": ["dinh_gia", "PE", "so_sanh_lich_su"],
    "opinion_or_fact": "fact"
  },
  {
    "type": "sector_view",
    "source": "Quang Dũng",
    "sector": "Ngân hàng",
    "content": "TB ngành NH P/B ~1.5x. 70% mã có P/B <1x. ACB, MBB, TCB, VPB, TPB đều rẻ. STB 2.25x — đắt hơn TB ngành.",
    "tags": ["ngan_hang", "dinh_gia_re", "PB"],
    "opinion_or_fact": "mixed"
  },
  {
    "type": "sector_view",
    "source": "Quang Dũng",
    "sector": "Bất động sản",
    "content": "BĐS chiết khấu sâu nhất TT. NVL tăng 3 trần nhờ định giá rẻ + tin tích cực. Nhóm chiết khấu trước sẽ cân bằng và phục hồi trước.",
    "tags": ["BDS", "chiet_khau_sau", "phuc_hoi"],
    "opinion_or_fact": "opinion"
  },
  {
    "type": "stock_call",
    "source": "Quang Dũng",
    "ticker": "VCB",
    "content": "VCB P/B hiện ~2x — ngang 2013-2015 (trước đó 5x). Không ai muốn bán, cầu nhỏ cũng tạo điểm dừng rơi.",
    "tags": ["VCB", "dinh_gia_re", "dung_roi"],
    "opinion_or_fact": "mixed"
  },
  {
    "type": "strategy",
    "source": "Quang Dũng",
    "content": "Không nên đuổi giá tăng — rủi ro mất 15-30% trong 2-3 tuần. Nên chuyển sang giá trị: mua chiết khấu sâu, đã dừng rơi. Hiện tại chủ yếu quan sát.",
    "tags": ["chien_luoc", "gia_tri", "khong_duoi_gia"],
    "opinion_or_fact": "opinion"
  },
  {
    "type": "risk_warning",
    "source": "Quang Dũng",
    "ticker": "STB",
    "content": "STB giảm 6.5% gần sàn, vol 30 triệu CP. Tin gia hạn tái cơ cấu 2030. Ví dụ rủi ro mua giá cao gặp tin xấu.",
    "tags": ["STB", "ban_thao", "gia_han_tai_co_cau"],
    "opinion_or_fact": "mixed"
  }
]
```

**Quy tắc viết signal từ transcript:**

```
1. Luôn ghi nguồn: "Theo phân tích từ...", "Chuyên gia X nhận định..."
2. Dữ kiện (P/E, giá, volume) → nêu thẳng
   Nhận định → "nhận định rằng", "cho rằng"
3. KHÔNG khuyến nghị mua/bán dựa trên ý kiến KOL
4. Nhiều KOL cùng nhận định → nhấn sự đồng thuận
5. KOL mâu thuẫn nhau → nêu cả 2 góc nhìn
6. Kèm "đây là ý kiến cá nhân" nếu là nhận định thuần
```

**Ví dụ output:**

Retail (user cầm VCB):
```
"Chuyên gia nhận định VCB đang ở vùng định giá rẻ nhất 10 năm.
Ngành ngân hàng nói chung đang rẻ — 70% mã có giá thấp hơn giá trị tài sản.
Đây là ý kiến cá nhân, không phải khuyến nghị đầu tư."
```

Pro (user cầm VCB):
```
"VCB: Quang Dũng đánh giá P/B hiện ~2x — ngang 2013-2015. 70% mã NH có P/B <1x. TB ngành 1.5x.
Nhận định: dừng rơi, cầu nhỏ cũng tạo điểm cân bằng."
```

---

### NGUỒN 4 — SỰ KIỆN VĨ MÔ (theo tin tức)

| Sự kiện | AI đánh giá |
|---------|------------|
| Fed tăng/giảm lãi suất | Ảnh hưởng từng ngành trong danh mục |
| SBV thay đổi chính sách | Ngành hưởng lợi vs chịu thiệt |
| Tỷ giá USD/VND biến động | Mã xuất khẩu vs nhập khẩu |
| Giá dầu tăng/giảm đột biến | Dầu khí, vận tải, hóa chất |
| CPI, Non-farm | Tác động kỳ vọng lãi suất → BTC, vàng |

**Prompt phân tích vĩ mô:**
```
"Sự kiện vĩ mô: {sự kiện}
Danh mục user: {danh sách mã + ngành}

Phân tích:
1. Ngành nào trong danh mục hưởng lợi? Tại sao?
2. Ngành nào chịu thiệt? Tại sao?
3. Mức ảnh hưởng: Mạnh / Trung bình / Nhẹ
4. Kịch bản: nếu {sự kiện} tiếp diễn thì sao?
5. Khuyến nghị: giữ / giảm / tăng mã nào?"
```

**Ví dụ output:**

Retail:
```
"Đồng đô la Mỹ mạnh lên đêm qua.
Ảnh hưởng: Tiền nước ngoài có thể rút ra → HPG, VCB dễ bị bán.
Mặt tích cực: Các công ty xuất khẩu (thủy sản, gỗ) được lợi.
→ Không mua mới hôm nay, chờ phản ứng thị trường."
```

Pro:
```
"DXY bứt 105.2 (+0.8% đêm qua). KN dự kiến xả ròng.
HPG (35% DM, xuất khẩu thấp) → tiêu cực. VCB (42% DM) → áp lực margin ngoại.
Kịch bản: DXY giữ trên 105 → KN bán ròng 3-5 phiên."
```

---

### NGUỒN 5 — BCTC & BÁO CÁO DOANH NGHIỆP (khi công bố)

**Phát hiện nhanh (rules):**

| Quy tắc | Điều kiện | Mức |
|---------|-----------|-----|
| BCTC sốc | Lỗ ròng (từ lãi chuyển lỗ) | 🔴 Nghiêm trọng |
| BCTC vượt/thất vọng | Lãi ±20% so cùng kỳ | 🟡 Cao |
| Nợ tăng đột biến | Nợ/vốn chủ SH tăng ≥ 30% | 🔵 Trung bình |
| Doanh thu đột phá | DT tăng ≥ 50% so cùng kỳ | 🟡 Cao |
| Cổ tức cao | Tỷ suất ≥ 8%/năm | 🔵 Trung bình |

**Prompt extract BCTC (Bước 1 — Extract số liệu):**

```
"Bạn là chuyên viên phân tích tài chính. Đọc BCTC đính kèm
và extract số liệu theo JSON schema.

Quy tắc:
- Đơn vị: tỷ VNĐ (làm tròn 1 chữ số thập phân)
- Lấy cả số kỳ này và kỳ trước (cùng kỳ năm trước)
- Tính YoY% = (kỳ này - kỳ trước) / |kỳ trước| × 100
- Tính biên gộp = LN gộp / DT thuần × 100
- Tính biên ròng = LNST / DT thuần × 100
- Nếu không có số liệu → null

Output JSON:
{
  'ticker': '...',
  'company': '...',
  'sector': '...',
  'period': 'Q?/20??',
  'income_statement': {
    'revenue': {'value': ..., 'prev': ..., 'yoy': ...},
    'gross_profit': {'value': ..., 'prev': ..., 'yoy': ...},
    'operating_profit': {'value': ..., 'prev': ..., 'yoy': ...},
    'net_profit': {'value': ..., 'prev': ..., 'yoy': ...},
    'gross_margin': {'value': ..., 'prev': ...},
    'net_margin': {'value': ..., 'prev': ...}
  },
  'balance_sheet': {
    'total_assets': ...,
    'total_debt': ...,
    'equity': ...,
    'cash': ...,
    'receivables': {'value': ..., 'prev': ..., 'yoy': ...},
    'inventory': {'value': ..., 'prev': ..., 'yoy': ...},
    'debt_to_equity': ...
  },
  'cash_flow': {
    'cfo': ..., 'cfi': ..., 'cff': ...,
    'free_cash_flow': ...
  }
}"
```

**Prompt phân tích trend (Bước 2 — So sánh 8 quý):**

```
"Bạn là AI phân tích BCTC của Finpath. Dưới đây là BCTC mới
extract và database 8 quý gần nhất cùng ticker.

=== BCTC MỚI ===
{json_new}

=== DATABASE 8 QUÝ ===
{json_8q}

Nhiệm vụ: So sánh trend nhiều quý, KHÔNG chỉ YoY.

Checklist bắt buộc:
1. DOANH THU: Trend 8 quý tăng/giảm/đi ngang? Quý này vs TB 4 quý?
2. BIÊN GỘP: Trend 8 quý. Giảm liên tiếp ≥3 quý → cảnh báo
3. BIÊN RÒNG: Trend 8 quý. So với biên gộp → chi phí quản lý bất thường?
4. PHẢI THU: Tăng nhanh hơn DT → chất lượng DT kém?
5. TỒN KHO: Tăng nhanh hơn DT → rủi ro tồn đọng?
6. DÒNG TIỀN: CFO âm mấy quý liên tiếp? FCF âm?
7. NỢ/VỐN: Trend tăng nợ? Vay ngắn hạn tăng?

Output JSON array chunks, types cho phép:
- earnings_actual: kết quả LN thực tế
- margin_change: biên LN thay đổi đáng kể
- balance_sheet_alert: bảng cân đối bất thường
- cash_flow_alert: dòng tiền bất thường
- financial_health: đánh giá sức khỏe tổng thể
- earnings_quality: chất lượng lợi nhuận (phải thu/DT)
- inventory_alert: tồn kho bất thường"
```

**Data mẫu — OIL/PVOIL Q4/2025:**

```json
{
  "ticker": "OIL",
  "period": "Q4/2025",
  "income_statement": {
    "revenue": {"value": 35279, "prev": 25372, "yoy": 39},
    "net_profit": {"value": 198.6, "prev": 187.2, "yoy": 6.1},
    "gross_margin": {"value": 2.85, "prev": 3.91},
    "net_margin": {"value": 0.56, "prev": 0.74}
  }
}
```

**Output mẫu:**

Retail:
```
"OIL có quý doanh thu kỷ lục nhưng lãi vẫn dậm chân tại chỗ.
Bán được nhiều hơn 39% so với bình thường, nhưng lời mỗi đồng ít đi
(biên gộp giảm 5/6 quý gần đây). Tiền bán hàng chưa thu về tăng nhanh hơn doanh thu.
Tóm lại: doanh thu ấn tượng, nhưng chất lượng lợi nhuận đang kém dần."
```

Pro:
```
"OIL Q4/25: DT 35,279 tỷ (+39% YoY) — kỷ lục. LNST 198.6 tỷ (+6.1%) — tăng không tương xứng.
Biên gộp 2.85% vs 3.91% cùng kỳ, giảm 5/6 quý. Biên ròng 0.56%.
Phải thu +45% > DT +39%. CFO dương nhưng chỉ ~200 tỷ."
```

---

### NGUỒN 6 — BÁO CÁO PHÂN TÍCH CTCK (khi công bố)

**Phát hiện nhanh (rules):**

| Quy tắc | Điều kiện | Mức |
|---------|-----------|-----|
| ≥ 2 CTCK cùng hạ | Trong 1 tuần | 🔴 Nghiêm trọng |
| Thay đổi khuyến nghị | 1 CTCK nâng/hạ | 🟡 Cao |
| Giá mục tiêu cắt mạnh | Giảm hơn 20% | 🟡 Cao |

**Prompt extract (Bước 1):**

```
"Bạn là chuyên viên phân tích tài chính. Đọc báo cáo phân tích
đính kèm và extract thành chunks độc lập.

Quy tắc:
- Mỗi chunk độc lập, đọc riêng vẫn hiểu
- Mỗi mã = 1 chunk recommendation riêng
- Mỗi kỳ dự báo = 1 chunk riêng
- Tối đa 200 ký tự mỗi chunk content
- Đơn vị: tỷ VNĐ, làm tròn 1 chữ số
- Nếu có nâng/hạ so với báo cáo trước → ghi rõ mức cũ

Types cho phép:
- recommendation: khuyến nghị + giá mục tiêu
- earnings_forecast: dự báo KQKD cho 1 kỳ cụ thể
- backlog: đơn hàng, hợp đồng, pipeline
- valuation: P/E, P/B, so sánh định giá
- catalyst: sự kiện cụ thể sắp tới ảnh hưởng giá
- risk: rủi ro cụ thể được nêu trong báo cáo
- sector_macro: thông tin vĩ mô ảnh hưởng toàn ngành
- margin_outlook: nhận định về biên lợi nhuận

Bổ sung cho báo cáo chi tiết 1 mã:
- revenue_breakdown: doanh thu theo mảng kinh doanh
- valuation_detail: DCF + so sánh + scenario
- catalyst_timeline: lịch sự kiện theo quý
- risk_detail: liệt kê rủi ro + mức độ
- earnings_forecast_detail: dự báo theo quý

Output JSON array."
```

**Prompt cross-reference consensus (Bước 2):**

```
"Bạn là AI phân tích của Finpath. So sánh chunks mới với database
60 ngày cùng ticker.

=== CHUNKS MỚI ===
{json_new_chunks}

=== DATABASE 60 NGÀY — cùng ticker ===
{json_existing_chunks}

Checklist bắt buộc:
1. CONSENSUS: Gom recommendation → bao nhiêu MUA/GIỮ/BÁN? TB giá mục tiêu?
2. CONSENSUS SHIFT: CTCK này nâng/hạ so lần trước? Bao nhiêu CTCK khác cùng nâng/hạ 30 ngày?
3. TARGET PRICE vs GIÁ HIỆN TẠI: Upside TB consensus?
4. EARNINGS vs BCTC: Nếu có BCTC thực tế → so dự báo vs thực, ai dự đúng nhất?
5. CATALYST TRÙNG: Nhiều CTCK cùng nhắc 1 catalyst → đáng tin hơn
6. OUTLIER: CTCK nào khuyến nghị ngược hẳn số còn lại?

Output JSON:
{
  'consensus': {
    'total_reports': 5,
    'buy': 3, 'hold': 1, 'sell': 1,
    'avg_target_price': 30500,
    'tp_trend_30d': 'tăng',
    'ctck_upgraded_30d': ['SSI', 'VNDirect'],
    'ctck_downgraded_30d': []
  },
  'insights': [...]
}"
```

**Data mẫu — Chunks từ MBS báo cáo ngành Xây dựng:**

```json
[
  {
    "type": "recommendation",
    "source": "MBS",
    "ticker": "CTD",
    "content": "MBS khuyến nghị Khả quan CTD, giá mục tiêu 97,500 (+4% vs lần trước). Lý do: backlog tăng 45%, LN dự báo +83% FY26.",
    "numbers": {"target_price": 97500, "prev_target_price": 93750, "recommendation": "Khả quan"}
  },
  {
    "type": "sector_macro",
    "source": "MBS",
    "sector": "Xây dựng",
    "content": "Đầu tư công 2026 dự kiến +18% YoY. Dự án trọng điểm: đường sắt, vành đai, sân bay. CTD, CII hưởng lợi chính.",
    "numbers": {"public_investment_growth": 18}
  }
]
```

**Output mẫu:**

Retail (user cầm CTD):
```
"SSI, VNDirect và MBS đều đánh giá tích cực CTD — kỳ vọng tăng 18%.
4 trong 5 công ty chứng khoán lớn khuyến nghị mua, giá kỳ vọng TB khoảng 95,000.
MBS vừa nâng thêm 4% so với lần trước — lý do chính là đơn hàng tăng 45%."
```

Pro (user cầm CTD):
```
"CTD: MBS nâng Khả quan, TP 97,500 (+4% vs trước). Consensus 30 ngày: 4 Mua / 1 Trung lập, TB TP 95,200 (+15%).
3 CTCK nâng TP 30 ngày. P/B 0.9x vs 1.1x phục hồi. LN dự báo +83% FY26."
```

Retail (user cầm mã XD khác — adjacent):
```
"Ngành xây dựng được đánh giá tích cực — 3/4 mã lớn được khuyến nghị mua.
Đầu tư công dự kiến tăng 18%, hàng loạt dự án lớn khởi công.
Bạn đang cầm HHV — được đánh giá tích cực, kỳ vọng tăng 26%."
```

---

### NGUỒN 7 — CHỈ BÁO KỸ THUẬT + ĐỊNH GIÁ + DÒNG TIỀN

**Đặc thù:** Chạy tự động 100%, rules-based, KHÔNG cần LLM extract.

**Rules kỹ thuật:**

| Chỉ báo | Quy tắc | Mức |
|---------|---------|-----|
| MA50/MA200 | MA50 cắt lên MA200 / cắt xuống MA200 | 🟡 Cao |
| RSI | ≤ 20 hoặc ≥ 80 | 🟡 Cao |
| RSI | ≤ 30 hoặc ≥ 70 | 🔵 Trung bình |
| Hỗ trợ/kháng cự | Phá vỡ kèm KL ≥ 2x TB | 🟡 Cao |
| Hỗ trợ/kháng cự | Chạm vùng test ≥ 3 lần | 🔵 Trung bình |
| OHLC | Nến Doji sau giảm dài / Engulfing tại vùng quan trọng | 🔵 Trung bình |
| P/E | Lệch ≥ 30% so TB ngành | 🔵 Trung bình |
| P/B | < 1.0 hoặc thấp nhất 3 năm | 🟡 Cao |

**Rules dòng tiền (BỔ SUNG):**

| Chỉ báo | Quy tắc | Mức |
|---------|---------|-----|
| KN bán ròng mạnh | >50 tỷ/phiên, 3 phiên liên tiếp | 🟡 Cao |
| KN mua ròng mạnh | >50 tỷ/phiên, 3 phiên liên tiếp | 🟡 Cao |
| Tự doanh gom mạnh | >20 tỷ/phiên, 3 phiên liên tiếp | 🟡 Cao |
| Dòng tiền đảo chiều | KN/tự doanh từ bán → mua (hoặc ngược lại) | 🔵 Trung bình |
| Vol bùng nổ | Vol ≥ 3x TB 20 phiên | 🔵 Trung bình |

**Chunk schema:**

```json
{
  "type": "technical | valuation | money_flow",
  "ticker": "FPT",
  "date": "2026-03-18",
  "signal": "breakout | rsi_oversold | foreign_sell_streak | ...",
  "content": "≤150 ký tự",
  "numbers": { ... },
  "severity": "high | medium",
  "catalyst_deadline": {
    "event": "Tên sự kiện sắp tới liên quan mã (nếu có)",
    "date": "YYYY-MM-DD",
    "days_away": 15
  }
}
```

**Quy tắc `catalyst_deadline`:**
- Điền khi có sự kiện cụ thể sắp tới ảnh hưởng đến mã: BCTC, ĐHCĐ, ngày chốt cổ tức, sự kiện vĩ mô (FOMC, FTSE công bố...)
- Không có sự kiện rõ ràng → để `null`
- Team C dùng field này để hiển thị trong Góc nhìn: *"Catalyst gần nhất: [event] — còn [N] ngày"*

**Tín hiệu kép (BỔ SUNG):**

Khi ≥2 signal cùng 1 mã cùng hướng trong 24h → gộp thành tín hiệu kép, mạnh hơn nhiều.

Ví dụ: FPT vượt kháng cự + vol bùng nổ + KN mua ròng → 3 tín hiệu cùng xác nhận.

**Ví dụ output:**

Retail (vượt kháng cự):
```
"FPT vừa vượt vùng giá quan trọng 128,000 — lần đầu từ tháng 1.
Khối lượng tăng 80% so với bình thường, xác nhận đà tăng.
2 lần trước tương tự, FPT tăng TB 14% trong tháng sau."
```

Pro (vượt kháng cự):
```
"FPT: Vượt 128K, vol 180% TB. RSI 62 — chưa quá mua.
2/2 lần tương tự 12 tháng → tăng TB 14% trong 20 phiên."
```

Retail (KN bán ròng):
```
"Quỹ nước ngoài bán ròng HPG 3 phiên liên tiếp, tổng 135 tỷ — và đang tăng tốc.
Mỗi phiên bán nhiều hơn phiên trước (32 → 45 → 58 tỷ).
6/10 lần kiểu này giá tiếp tục giảm 3-5% tuần sau."
```

Pro (KN bán ròng):
```
"HPG: KN bán ròng 3 phiên (32→45→58 tỷ), tổng 135 tỷ, tăng tốc.
Lịch sử: 6/10 lần → giá giảm thêm 3-5%. Theo dõi phiên mai."
```

Retail (tín hiệu kép):
```
"FPT đang có 3 tín hiệu tích cực cùng lúc: vượt vùng giá quan trọng,
khối lượng tăng mạnh, và quỹ nước ngoài mua ròng.
Hiếm khi 3 yếu tố cùng xuất hiện. 2 lần trước tương tự, FPT tăng TB 14%."
```

Pro (tín hiệu kép):
```
"FPT: Vượt 128K + vol 180% TB + KN mua ròng 25 tỷ 2 phiên — 3 tín hiệu cùng xác nhận.
Lịch sử: 2/2 lần → tăng TB 14%. RSI 62, còn dư địa."
```

---

### NGUỒN 8 — TIN TÌNH BÁO ROOM CK (khi có tin mới) ← MỚI

**Đặc thù:**
- Data ĐÃ ĐƯỢC admin Finpath chọn lọc → không phải tin đồn raw
- Có tên chuyên gia/KOL cụ thể (TVI, Hang Phan Stock...)
- KHÔNG cần bước xác minh phức tạp
- Focus: extract insight + cross-ref giá hiện tại + danh mục user

**Prompt extract:**

```
"Đọc tin tình báo đã được admin lọc và phân loại.

Quy tắc:
- Mỗi mã cổ phiếu = 1 chunk riêng
- Bỏ qua: quảng cáo, disclaimer, lời chào
- Ghi rõ nguồn (tên chuyên gia/KOL)
- Extract các mức giá cụ thể (entry, stop, target)

Types cho phép:
- trade_signal: khuyến nghị mua/bán kèm mức giá cụ thể
- expert_analysis: phân tích chuyên sâu về mã/ngành

Output JSON array:
[
  {
    'type': 'trade_signal | expert_analysis',
    'source': 'tên chuyên gia/KOL',
    'ticker': '...',
    'content': '≤200 ký tự',
    'tags': ['...'],
    'numbers': {
      // trade_signal: entry_price, stop_price, target_price, action
      // expert_analysis: các số liệu cụ thể trong bài
    }
  }
]"
```

**Data mẫu — Tin từ TVI:**

Input:
```
CÁCH XỬ LÝ VỚI STB MỚI MUA — Chuyên gia TVI
- PNJ: đặt lệnh dừng lãi tại vùng 110. Thủng → bán khóa LN từ điểm mua 93.
- STB: mở mua 64, đóng cửa thủng 63 → bán 1/2, thủng 60-61 → bán hết.
  Ai mua thêm 68 → đóng cửa dưới 64 thì bán hết tỉ trọng mua thêm.
```

Output:
```json
[
  {
    "type": "trade_signal",
    "source": "TVI",
    "ticker": "PNJ",
    "content": "TVI đặt lệnh dừng lãi PNJ tại 110. Thủng 110 → bán khóa LN từ điểm mua 93.",
    "numbers": {"entry_price": 93, "stop_price": 110, "action": "bán nếu thủng 110"}
  },
  {
    "type": "trade_signal",
    "source": "TVI",
    "ticker": "STB",
    "content": "TVI mở mua STB 64, đang hòa. Thủng 63 → bán 1/2, thủng 60-61 → bán hết. Mua thêm 68 → đóng cửa dưới 64 thì bán hết phần thêm.",
    "numbers": {"entry_price": 64, "add_price": 68, "stop_1": 63, "stop_2": 60}
  }
]
```

**Data mẫu — Tin từ Hang Phan Stock:**

Input:
```
DGC TÀI CHÍNH VẪN KHỎE, VẬY TẠI SAO GIÁ LẠI RƠI? Nguồn: Hang Phan Stock
- Lãi Q3/2025 đạt 804 tỷ (+9%), tiền mặt 13.100 tỷ, chia cổ tức 30%.
- Cú sập do "rủi ro quản trị - con người". Ngành nhạy cảm luôn có góc khuất.
```

Output:
```json
[
  {
    "type": "expert_analysis",
    "source": "Hang Phan Stock",
    "ticker": "DGC",
    "content": "DGC tài chính khỏe (Q3 LN 804 tỷ +9%, cash 13,100 tỷ, cổ tức 30%) nhưng giá rơi do rủi ro quản trị. Ngành nhạy cảm luôn có góc khuất.",
    "numbers": {"q3_profit": 804, "q3_profit_yoy": 9, "cash": 13100, "dividend": "30%"}
  }
]
```

**Cross-reference:** So giá hiện tại với mức stop/entry trong chunk → user đang ở vùng nào?

**Ví dụ output:**

Retail (user cầm STB):
```
"Chuyên gia TVI đang cân nhắc bán STB nếu giá đóng cửa dưới 63.
TVI mua từ 64, hiện đang hòa vốn. Dưới 63 bán một nửa, dưới 60-61 bán hết.
Đây là tín hiệu thận trọng từ người đã khuyến nghị mua trước đó."
```

Pro (user cầm STB):
```
"STB: TVI cân nhắc cắt — entry 64, stop 63 (bán 1/2), 60-61 (bán hết).
Thêm vị thế 68 → cắt nếu đóng cửa <64. TVI chuyển từ tích cực sang phòng thủ."
```

Retail (user cầm DGC):
```
"DGC: Tài chính rất tốt nhưng giá vẫn rơi — vấn đề không nằm ở số liệu.
Lãi quý 3 tăng 9%, tiền mặt hơn 13,000 tỷ, sắp chia cổ tức 30%.
Nhưng giá giảm vì rủi ro liên quan lãnh đạo — loại rủi ro không thấy trong báo cáo."
```

Pro (user cầm DGC):
```
"DGC: Tài chính khỏe (Q3 LN 804 tỷ +9%, cash 13.1K tỷ = 67% TS, cổ tức 30%) — giá rơi do rủi ro quản trị.
Hang Phan Stock: biến số con người, không phải nội tại. Ngành nhạy cảm = tỷ trọng nhỏ."
```

---

### BẢNG TỔNG HỢP 8 NGUỒN

| # | Nguồn | Tần suất quét | Cần LLM? | Áp dụng | Output |
|---|-------|--------------|----------|---------|--------|
| 1 | Giá | Thời gian thực | Không | Tất cả | Thẻ tín hiệu ngay lập tức |
| 2 | Tin tức sự kiện DN | 5 phút | Không (rules) + Có (cross-ref) | Tất cả | Thẻ tín hiệu + push |
| 3 | Transcript Social | Khi có nội dung mới | **Có** (extract từ transcript) | Tất cả | Thẻ tín hiệu |
| 4 | Sự kiện vĩ mô | Theo tin tức | Có (phân tích ảnh hưởng) | Tất cả | Thẻ vĩ mô + liên quan |
| 5 | BCTC & báo cáo DN | Khi công bố | **Có** (extract + trend 8Q) | Chứng khoán | Thẻ tín hiệu |
| 6 | Báo cáo phân tích CTCK | Khi công bố | **Có** (extract + consensus) | Chứng khoán | Thẻ tín hiệu |
| 7 | Chỉ báo KT + Định giá + Dòng tiền | Thời gian thực | Không (rules) | CK + Crypto | Thẻ kỹ thuật |
| 8 | **Tin tình báo room CK** | Khi có tin mới | **Có** (extract insight) | Chứng khoán | Thẻ tín hiệu |

---
## PHẦN 2: CHẤM ĐIỂM MỨC ĐỘ QUAN TRỌNG — Phụ trách: Chi, Hưởng

### Mục tiêu
Mỗi tín hiệu phát hiện → chấm điểm 1-10 → xác định cách gửi (push / trong app / gom bản tin).

### 2 tầng chấm điểm

**Tầng 1 — Quy tắc cố định (xử lý tức thì, dưới 100ms)**

| Nguồn | Quy tắc | Điểm |
|-------|---------|------|
| Giá sàn (CK), ≥15% (Crypto), ≥3% (Vàng) | | 10 |
| Giảm 5-7% (CK), 10-15% (Crypto), 2-3% (Vàng) | | 8-9 |
| Giảm 3-5% (CK), 7-10% (Crypto), 1.5-2% (Vàng) | | 6-7 |
| KL gấp ≥ 5 lần TB | | 7-8 |
| KL gấp 3-5 lần | | 4-5 |
| Từ khóa nghiêm trọng (phá sản, bắt giữ...) | | 8-9 |
| Từ khóa cao (M&A, cổ tức đặc biệt...) | | 6-7 |
| Từ khóa trung bình (BCTC, nhân sự...) | | 4-5 |
| Transcript Social — sentiment bùng ≥ 300% | | 7-8 |
| Transcript Social — sentiment bùng 200-300% | | 5-6 |
| BCTC lỗ ròng (từ lãi chuyển lỗ) | | 8-9 |
| BCTC lãi ±20% | | 6-7 |
| ≥ 2 CTCK cùng hạ khuyến nghị | | 8 |
| MA50 cắt xuống/cắt lên MA200 | | 6-7 |
| RSI ≤ 20 / ≥ 80 | | 6 |
| P/B thấp nhất 3 năm | | 6 |
| Tin tình báo room — trade signal cụ thể | | 6-7 |
| Tin tình báo room — nhiều expert cùng cảnh báo | | 7-8 |

**Điểm → 4 mức khẩn cấp:**

| Điểm | Mức | Cách gửi |
|------|-----|----------|
| 8-10 | 🔴 Nghiêm trọng | Push có âm thanh, kể cả ban đêm |
| 6-7 | 🟡 Cao | Push im lặng nếu giờ hoạt động |
| 4-5 | 🔵 Trung bình | Chỉ trong app, badge 🔔 |
| 1-3 | ⚪ Thấp | Gom bản tin sáng |

**Ví dụ Tầng 1:**
```
Tín hiệu: HPG giảm 4.2%
Quy tắc: Giảm 3-7% (CK) → Điểm: 6
Mức: 🟡 Cao → Push im lặng
```

---

**Tầng 2 — AI đánh giá sâu (±2 điểm, xử lý 2-5 giây)**

**Khi nào kích hoạt Tầng 2:**
- Transcript Social (luôn cần AI phân loại FUD vs thật)
- Sự kiện vĩ mô (cần AI đánh giá ảnh hưởng cụ thể)
- Nhiều tín hiệu cùng mã trong 2 giờ (cần AI tổng hợp)
- Có bài học liên quan trong trí nhớ user

**Prompt Tầng 2:**
```
"Đánh giá tín hiệu cho nhà đầu tư.

Tín hiệu: {loại, mã, dữ liệu, điểm tầng 1}
Danh mục: {mã + tỷ trọng}
Trí nhớ: {giao dịch gần đây + bài học}
Profile: {profile}

Trả lời:
1. Điều chỉnh: +2 đến -2 (số nguyên)
2. Lý do điều chỉnh (1 câu)
3. Tin đồn hay lo ngại thật? (nếu cảm xúc MXH)
4. Tình huống tương tự trong trí nhớ? (nếu có)
5. Liên quan bài học nào? (nếu có)"
```

**Ví dụ Tầng 2:**
```
Input:
  Tín hiệu: HPG -4.2%, KL 2.8x, điểm Tầng 1 = 6
  Danh mục: HPG 35%, FPT 25%
  Bài học: "3 lần bán hoảng loạn khi tin xấu, cả 3 giá hồi"
  Profile: Stock_Retail

Output:
  Điều chỉnh: +1 (vì HPG chiếm 35% danh mục + có bài học liên quan)
  Lý do: "HPG là mã trọng tâm, có bài học bán hoảng loạn"
  Tình huống tương tự: "15/08/2025 HPG giảm 5%, user bán, giá hồi +15%"
  Bài học: "Bán hoảng loạn — 3 lần, cả 3 giá hồi"

Điểm cuối: 6 + 1 = 7 → 🟡 Cao → Push im lặng
```

---

## PHẦN 3: GOM NHÓM TÍN HIỆU — Phụ trách: Chi, Hưởng

### Mục tiêu
Nhiều tín hiệu cùng mã + cùng hướng → gom thành 1 thẻ tổng hợp thay vì spam 5 thẻ riêng.

### Điều kiện gom

| Điều kiện | Bắt buộc |
|-----------|---------|
| Cùng mã | ✅ |
| Cùng hướng (tích cực / tiêu cực) | ✅ |
| Trong cửa sổ 2 giờ | ✅ |
| Từ ≥ 2 nguồn khác nhau | ✅ |

**Ví dụ GOM:**
```
✅ HPG tin xấu (9:00) + HPG cảm xúc tiêu cực (9:30) + HPG giá giảm (9:45)
   → 3 nguồn, cùng hướng, trong 2h → GOM thành 1 thẻ Compound
```

**Ví dụ KHÔNG GOM:**
```
❌ HPG tin xấu + FPT cảm xúc → khác mã
❌ HPG tin xấu + HPG kỹ thuật tích cực → trái hướng (gửi RIÊNG)
❌ HPG tin xấu 8:00 + HPG cảm xúc 11:00 → quá 2 giờ
```

### Điểm tổng hợp

```
Điểm = max(tất cả tín hiệu trong nhóm) + 1
Cap tối đa: 10
```

**Ví dụ:**
```
HPG tin xấu: 7 điểm
HPG cảm xúc: 6 điểm
HPG giá giảm: 6 điểm
→ max(7, 6, 6) + 1 = 8 → 🔴 Nghiêm trọng
```

### Thẻ tổng hợp — Wireframe

```
🟡                                    9:32 AM
HPG — 3 nguồn cùng chỉ hướng

TIÊU ĐỀ (≤150 ký tự, tổng hợp từ tất cả nguồn)
3 nguồn cùng chỉ về HPG: nghi vấn dumping
từ EU, sentiment tiêu cực bùng trên Telegram,
giá giảm 4.2% trong phiên sáng

SỐ LIỆU (≤160 ký tự, bold highlight con số)
Tiêu cực tăng **340%** trên 5 nhóm Telegram.
Giá 26,400 → **25,300** (–4.2%).
Volume gấp **2.8 lần** TB 20 phiên.

┌ LỊCH SỬ (≤140 ký tự)
│ 🕐 **15/08/2025**: Tín hiệu tương tự — HPG giảm
│ thêm 8% trong 3 phiên, hồi 60% khi tin bị bác bỏ.
└

BỐI CẢNH CÁ NHÂN (≤130 ký tự)
Bạn cầm **2,000 HPG** (35% danh mục). Nếu giảm
thêm 8% như lần trước, portfolio drawdown **–2.8%**.

▼ Xem chi tiết từng nguồn
  ┌─ 📰 TIN TỨC ─────────────────────┐
  │ Nghi vấn dumping thép từ EU...    │
  └───────────────────────────────────┘
  ┌─ 💬 CẢM XÚC MXH ────────────────┐
  │ Tiêu cực tăng 340% trên Telegram │
  └───────────────────────────────────┘
  ┌─ 📈 GIÁ ──────────────────────────┐
  │ –4.2%, vol 2.8x TB               │
  └───────────────────────────────────┘

          [💬 Hỏi thêm về HPG]
```

**Prompt tạo thẻ tổng hợp:**
```
"Tổng hợp {N} tín hiệu cùng mã {mã} thành 1 thẻ.

Tín hiệu:
{danh sách tín hiệu + nguồn + điểm}

Danh mục: {mã + tỷ trọng}
Bài học: {nếu có}
Profile: {profile}

Viết:
1. Tiêu đề (≤150 ký tự) — tổng hợp, nêu rõ có bao nhiêu nguồn
2. Số liệu (≤160 ký tự) — gộp số liệu quan trọng nhất từ mỗi nguồn
3. Lịch sử (≤140 ký tự) — tình huống tương tự từ trí nhớ
4. Bối cảnh cá nhân (≤130 ký tự) — ảnh hưởng danh mục cụ thể
5. TUÂN THỦ AI Voice Tier (Retail vs Pro)"
```

### Trường hợp đặc biệt

| Trường hợp | Xử lý |
|-----------|-------|
| Trái hướng (tin xấu + kỹ thuật tích cực) | **KHÔNG gom**, gửi riêng 2 thẻ |
| Thêm tín hiệu mới 30 phút sau khi đã gửi | Cập nhật thẻ cũ + push nhỏ "🔄 Cập nhật: thêm nguồn mới cho {mã}" |
| 5+ tín hiệu cùng mã trong 2h | Gom tất cả, đây là tín hiệu rất mạnh |
| Tín hiệu mới trùng nội dung cũ | Bỏ qua, không gửi lại |

---

## PHẦN 4: BẢN ĐỒ LIÊN QUAN — Phụ trách: Chi, Hưởng

### Mục tiêu
Khi có tín hiệu mạnh ở mã X → kiểm tra xem mã nào trong danh mục user có thể bị ảnh hưởng gián tiếp → gửi thẻ Adjacent.

### 3 loại liên quan

---

**Loại 1 — Cùng ngành (~15 nhóm ngành)**

| Ngành | Mã đại diện |
|-------|-------------|
| Ngân hàng | VCB, TCB, MBB, ACB, BID, CTG, STB, TPB, VPB, HDB |
| Bất động sản | VHM, VIC, NVL, DXG, KDH, NLG, PDR, DIG |
| Thép | HPG, HSG, NKG, TLH |
| Chứng khoán | SSI, VCI, HCM, VND, MBS |
| Công nghệ | FPT, CMG |
| Hóa chất | DGC, DPM, DCM |
| Dầu khí | GAS, PVD, PVS, PLX, BSR |
| Vận tải | GMD, HAH, VTP |
| Hàng không | HVN, VJC |
| Bán lẻ | MWG, FRT, PNJ, DGW |
| Tiêu dùng | VNM, MSN, SAB |
| Điện | POW, GEG, REE, PC1, NT2 |
| Vật liệu XD | VGC, HT1, BCC |
| Bảo hiểm | BVH, BMI, MIG |
| Phân bón | DPM, DCM, LAS |

**Cách dùng:** DXG giảm sàn → kiểm tra user có cầm mã BĐS nào khác (VHM, NVL...) → gửi thẻ Adjacent.

---

**Loại 2 — Ngành ảnh hưởng ngành (10 cặp)**

| Khi | Ảnh hưởng | Quan hệ | Chuỗi giải thích mẫu |
|----|-----------|---------|----------------------|
| BĐS ↓ | Ngân hàng | Nợ xấu | "{BĐS} suy yếu → nợ xấu tăng → {NH} bạn cầm" |
| BĐS ↓ | Vật liệu XD | Nhu cầu giảm | "{BĐS} giảm → nhu cầu XD giảm → {VLXD} bạn cầm" |
| Thép thay đổi | BĐS | Chi phí XD | "Giá thép {hướng} → chi phí XD → {BĐS} bạn cầm" |
| Dầu khí ↑ | Vận tải | Chi phí nhiên liệu | "Giá dầu tăng → chi phí nhiên liệu → {vận tải} bạn cầm" |
| Dầu khí ↑ | Hàng không | Chi phí jet fuel | "Giá dầu tăng → chi phí bay → {HK} bạn cầm" |
| Dầu khí ↑ | Hóa chất | Chi phí đầu vào | "Giá dầu tăng → chi phí đầu vào → {HC} bạn cầm" |
| NH thắt tín dụng | Chứng khoán | Margin | "Tín dụng thắt → margin giảm → {CK} bạn cầm" |
| NH thắt tín dụng | BĐS | Vay mua nhà | "Tín dụng thắt → BĐS chậm → {BĐS} bạn cầm" |
| CK suy yếu | NH | Phí môi giới giảm | "{CK} yếu → thanh khoản giảm → {NH có mảng CK} bạn cầm" |
| Bán lẻ ↓ | Tiêu dùng | Nhu cầu tiêu thụ | "Bán lẻ yếu → tiêu dùng giảm → {tiêu dùng} bạn cầm" |

---

**Loại 3 — Vĩ mô ảnh hưởng ngành (8 yếu tố)**

| Sự kiện vĩ mô | Ngành hưởng lợi (+) | Ngành chịu thiệt (-) |
|---------------|---------------------|----------------------|
| Lãi suất SBV tăng | Ngân hàng (NIM tăng) | BĐS, Chứng khoán |
| Lãi suất SBV giảm | BĐS, CK (dòng tiền đổ vào) | Ngân hàng (NIM giảm) |
| USD/VND tăng | Xuất khẩu (thủy sản, gỗ) | Hàng không, Nhập khẩu |
| Fed tăng lãi suất | — | Toàn thị trường ngắn hạn |
| Fed giảm lãi suất | Toàn thị trường | — |
| Giá dầu tăng | Dầu khí | Vận tải, Hàng không, Hóa chất |
| CPI Mỹ cao hơn kỳ vọng | Vàng | Crypto (risk-off) |
| DXY tăng mạnh (≥1% tuần) | — | Vàng, Crypto, EM equities |

---

### Mức liên quan & quy tắc gửi

| Mức | Hệ số | Quy tắc gửi |
|-----|-------|-------------|
| Mạnh | ≥ 0.7 | **Luôn gửi** thẻ Adjacent |
| Trung bình | 0.5-0.7 | Gửi nếu tín hiệu gốc ≥ 6 điểm |
| Yếu | < 0.5 | **Không gửi** |

**Ví dụ:**
```
DXG giảm sàn (điểm 10)
User cầm VCB (ngân hàng)
Quan hệ: BĐS → NH (nợ xấu) → mức Mạnh (0.8)
→ GỬI thẻ Adjacent cho VCB
```

### Chuỗi giải thích — Visual diagram (bắt buộc mỗi thẻ Adjacent)

```
┌──────────┐     ┌──────────────┐     ┌──────────┐
│ DXG      │ →   │ Nợ xấu BĐS  │ →   │ VCB bạn  │
│ ↓12%     │     │ tăng         │     │ đang cầm │
└──────────┘     └──────────────┘     └──────────┘
```

**Quy tắc chuỗi:**
- Tối đa 60 ký tự text (không tính diagram)
- 3 ô: Gốc → Quan hệ → Mã bạn
- Không có chuỗi giải thích hợp lý → **KHÔNG gửi tín hiệu liên quan**
- Công thức: `{gốc} {hướng} → {quan hệ} → {mã bạn}`

### Lộ trình mở rộng
- **Ra mắt (Sprint 1):** ~15 nhóm ngành + 10 cặp ngành↔ngành + 8 yếu tố vĩ mô
- **Tháng 2-3:** Mở rộng mã + cặp ngành + thêm vĩ mô Crypto/Vàng
- **Tháng 4+:** Chuỗi cung ứng cụ thể từng mã (HPG → nhà cung cấp quặng sắt, khách hàng xây dựng)

---

## PHẦN 5: LỊCH SỬ THỊ TRƯỜNG (EVENT STORE) — Phụ trách: Chi, Hưởng

### Mục tiêu
Khi có tín hiệu mới → tự động tìm sự kiện tương tự trong quá khứ → tra kết quả giá sau đó → viết phần "Lịch sử" trên thẻ tín hiệu. **1 hệ thống tổng quát, không phải code từng loại pattern riêng.**

### Nguyên lý

Mỗi tín hiệu Finpath phát hiện = 1 **sự kiện (event)**. Lưu tất cả vào Event Store. Khi có sự kiện mới → tìm event tương tự → tra giá sau đó → viết câu lịch sử ≤140 ký tự.

---

### EVENT STORE — Schema

Mỗi tín hiệu phát hiện → lưu 1 record:

```json
{
  "id": "evt_20260318_001",
  "ngay": "2026-03-18",
  "ma": "HPG",
  "nganh": "Thép",
  "loai": "TIN_TUC",
  "huong": "tieu_cuc",
  "diem": 7,
  "tom_tat": "UBCKNN yêu cầu giải trình BCTC Q4",
  "tags": ["dieu_tra", "BCTC", "co_quan_quan_ly"],
  "gia_luc_do": 25300,
  "gia_sau_5d": null,
  "gia_sau_10d": null,
  "gia_sau_20d": null,
  "phan_tram_5d": null,
  "phan_tram_10d": null,
  "phan_tram_20d": null
}
```

**Trường `tags`** — danh sách tag tự động gán dựa trên tín hiệu:

| Loại tín hiệu | Tags tự động |
|---------------|-------------|
| Breakout MA50 | `["breakout", "MA50", "ky_thuat"]` |
| Death cross | `["death_cross", "MA50_MA200", "ky_thuat"]` |
| RSI ≤ 20 | `["RSI_extreme", "qua_ban", "ky_thuat"]` |
| Tin điều tra/bắt giữ | `["dieu_tra", "bat_giu", "phap_ly"]` |
| BCTC lỗ ròng | `["BCTC", "lo_rong", "ket_qua_xau"]` |
| BCTC vượt kỳ vọng | `["BCTC", "vuot_ky_vong", "ket_qua_tot"]` |
| Sentiment bùng nổ | `["sentiment", "bung_no", "MXH"]` |
| Fed tăng lãi suất | `["Fed", "lai_suat", "vi_mo"]` |
| Anti-dumping | `["anti_dumping", "thuong_mai", "quoc_te"]` |
| Giá sập ≥ 7% | `["gia_sap", "panic"]` |
| KL bùng nổ ≥ 5x | `["volume_bung_no"]` |
| M&A / Sáp nhập | `["MA", "sap_nhap"]` |
| Chủ tịch/CEO thay đổi | `["nhan_su", "lanh_dao"]` |

**Lưu ý:**
- Lưu TẤT CẢ tín hiệu phát hiện — toàn thị trường, không chỉ mã user cầm
- Đây là data chung cho tất cả users
- Cron job mỗi đêm: cập nhật `gia_sau_5d/10d/20d` cho events đã đủ ngày

---


---

### ABNORMAL RETURN — Đo lường tác động thực sự của sự kiện

#### Vấn đề
Nếu chỉ đo "HPG giảm 4.2%" → không biết bao nhiêu do tin xấu, bao nhiêu do thị trường chung giảm. Abnormal Return tách rõ 2 phần.

#### Công thức

```
Abnormal Return (AR) = Actual Return − Expected Return
```

Trong đó:
```
Actual Return  = (Giá sau − Giá trước) / Giá trước × 100
Expected Return = 0.5 × VNI Return + 0.5 × TB Ngành Return
```

**TB Ngành Return** = trung bình return của các mã cùng ngành, **loại trừ chính mã đó**.

#### Ví dụ cụ thể

```
HPG giảm 4.2% trong phiên
VNI giảm 1.5% cùng phiên
TB ngành Thép (HSG, NKG, TLH — loại trừ HPG) giảm 2.0%

Expected Return = 0.5 × (-1.5%) + 0.5 × (-2.0%) = -1.75%
Abnormal Return = -4.2% − (-1.75%) = -2.45%

→ HPG giảm thêm 2.45% SO VỚI THỊ TRƯỜNG vì tin xấu riêng
```

#### Mức Confidence

| Mức | Điều kiện | Ý nghĩa |
|-----|-----------|---------|
| **Cao** | \|AR\| ≥ 2% | Sự kiện rõ ràng tác động mạnh lên giá |
| **Trung bình** | 0.5% ≤ \|AR\| < 2% | Có tác động nhưng chưa rõ nét |
| **Thấp** | \|AR\| < 0.5% | Giá biến động chủ yếu do thị trường, không phải do event |

#### Áp dụng vào Event Store

Bổ sung vào schema mỗi event:

```json
{
  "...các trường cũ...",
  "ar_1d": -2.45,
  "ar_5d": -1.80,
  "ar_10d": +3.20,
  "ar_20d": +8.50,
  "confidence": "cao"
}
```

- `ar_1d`: Abnormal Return ngày phát hiện
- `ar_5d/10d/20d`: Abnormal Return tích lũy sau 5/10/20 phiên
- `confidence`: tính từ `ar_1d`

**Cron job mỗi đêm** (bổ sung vào job cập nhật giá):
1. Lấy giá đóng cửa hôm nay cho mã + VNI + TB ngành
2. Tính AR cho các mốc 1d/5d/10d/20d
3. Gán confidence từ AR ngày đầu

#### Thay đổi Similar Event Lookup

Khi tìm events tương tự, **chỉ lấy events có confidence Cao hoặc Trung bình**. Events confidence Thấp (|AR| < 0.5%) bị loại — vì biến động giá chủ yếu do thị trường chung, không phải do event.

```
Query bổ sung: AND confidence IN ('cao', 'trung_binh')
```

#### Thay đổi cách viết câu lịch sử

**Trước (không có AR):**
```
"HPG bị điều tra → giảm 4.2%"
→ Gộp lẫn biến động thị trường vào event
```

**Sau (có AR):**
```
Retail: "2 lần trước HPG bị yêu cầu giải trình, giá giảm thêm 1.5%
         so với thị trường, rồi hồi lại trong 2-3 tuần."

Pro:    "HPG: 2/2 lần bị điều tra → AR -1.5% (1D),
         hồi +8% vs market (20D). Xu hướng: bán quá mức rồi hồi."
```

So sánh:

| Không có AR | Có AR |
|---|---|
| "HPG bị điều tra → giảm 4.2%" | "HPG bị điều tra → giảm thêm 1.65% so với thị trường" |
| Gộp lẫn biến động TT vào event | Tách rõ: bao nhiêu do event, bao nhiêu do TT |
| Lịch sử sai lệch, gây hoảng | Lịch sử chính xác, user tin tưởng |
| Confidence không đánh giá được | Confidence rõ ràng: Cao / TB / Thấp |

#### Công việc bổ sung

| Việc | Ước lượng | Chi tiết |
|------|----------|---------|
| Bổ sung AR fields vào Event Store schema | 0.25 ngày | Thêm ar_1d, ar_5d, ar_10d, ar_20d, confidence |
| Tính AR trong cron job đêm | 0.5 ngày | Lấy VNI + TB ngành, tính AR cho từng mốc |
| Mapping nhóm ngành → danh sách mã | 0.25 ngày | ~15 nhóm ngành, dùng chung với Bản đồ liên quan |
| Filter confidence trong Similar Event Lookup | 0.25 ngày | Thêm WHERE clause |
| Cập nhật prompt viết câu lịch sử | 0.25 ngày | Dùng AR thay vì return tuyệt đối |
| **Tổng bổ sung** | **~1.5 ngày** | |

### SIMILAR EVENT LOOKUP — 3 tầng tìm kiếm

Khi có tín hiệu mới → tìm sự kiện tương tự theo 3 tầng, **ưu tiên từ trên xuống** (dừng ngay khi tìm được):

**Tầng 1 — Cùng mã + cùng loại (chính xác nhất)**
```
Query: ma = "{ma}" AND loai = "{loai}" AND huong = "{huong}"
Sắp xếp: ngày gần nhất trước
Giới hạn: 24 tháng, tối đa 5 events
```
VD: HPG bị điều tra hôm nay → tìm HPG bị điều tra trước đó

**Tầng 2 — Cùng ngành + cùng loại**
```
Query: nganh = "{nganh}" AND loai = "{loai}" AND huong = "{huong}"
Loại trừ: ma != "{ma}" (tránh trùng Tầng 1)
Giới hạn: 24 tháng, tối đa 5 events
```
VD: HPG bị điều tra → tìm HSG, NKG (Thép) bị điều tra trước đó

**Tầng 3 — Cùng tags + bất kỳ mã/ngành**
```
Query: tags OVERLAP "{tags}" AND huong = "{huong}"
Loại trừ: nganh != "{nganh}" (tránh trùng Tầng 2)
Giới hạn: 24 tháng, tối đa 5 events
```
VD: HPG bị điều tra → tìm FLC bị bắt, DXG bị tạm giữ (cùng tag `dieu_tra`)

**Quy tắc chung:**
- Tầng 1 có ≥ 2 events → dùng Tầng 1. Không đủ → gộp thêm Tầng 2.
- Tổng tối đa 5 events (ưu tiên Tầng 1 > 2 > 3)
- Chỉ lấy events đã có `gia_sau_5d` (đã đủ dữ liệu kết quả)
- Không tìm được event nào ở cả 3 tầng → **bỏ qua phần lịch sử, KHÔNG bịa**
- Chỉ 1 event → vẫn viết, nhưng ghi rõ "1 lần" (không nói "trung bình")

---

### TÍNH KẾT QUẢ

Với mỗi event tìm được:

```
Thay đổi sau 5 phiên:  (gia_sau_5d - gia_luc_do) / gia_luc_do × 100
Thay đổi sau 10 phiên: (gia_sau_10d - gia_luc_do) / gia_luc_do × 100
Thay đổi sau 20 phiên: (gia_sau_20d - gia_luc_do) / gia_luc_do × 100
```

Tổng hợp:
```
Số lần: N events
TB thay đổi 20 phiên: trung_binh(phan_tram_20d)
Max: max(phan_tram_20d)
Min: min(phan_tram_20d)
Tỷ lệ cùng hướng: bao nhiêu % events giá đi cùng hướng với dự đoán
```

**Ví dụ đầy đủ:**

```
Tín hiệu mới: HPG bị UBCKNN yêu cầu giải trình (tieu_cuc)

Tầng 1 tìm được:
  Event A (HPG, 08/2025): giá 24,000 → sau 20d: 27,600 → +15.0%
  Event B (HPG, 01/2025): giá 22,000 → sau 20d: 24,200 → +10.0%

Tầng 2 tìm thêm:
  Event C (HSG, 03/2025): giá 18,500 → sau 20d: 19,800 → +7.0%

Tổng hợp (3 events):
  TB: +10.7% sau 20 phiên
  Max: +15.0% (HPG 08/2025)
  Min: +7.0% (HSG 03/2025)
  Tỷ lệ hồi: 3/3 = 100%
```

---

### PROMPT VIẾT CÂU LỊCH SỬ (1 prompt duy nhất, dùng cho TẤT CẢ loại tín hiệu)

```
"Viết phần LỊCH SỬ cho thẻ tín hiệu.

Sự kiện hiện tại:
  Mã: {ma}
  Loại: {loai}
  Tóm tắt: {tom_tat}

Sự kiện tương tự tìm được:
  {danh sách events: mã, ngày, tóm tắt, kết quả % sau 5/10/20 phiên}

Profile: {Retail / Pro}

Quy tắc:
- Tối đa 140 ký tự
- Nêu: số lần + khi nào + kết quả cụ thể (%)
- Ưu tiên khung 20 phiên (nếu có data)
- Nếu events cùng mã → ghi tên mã. Nếu khác mã → ghi 'tình huống tương tự'
- Retail: ngôn ngữ đơn giản
- Pro: số liệu cụ thể, ngắn gọn
- KHÔNG bịa. Chỉ dùng số liệu từ events.
- 1 event → ghi '1 lần trước', không nói 'trung bình'"
```

**Ví dụ output — cùng data, khác level:**

Retail (tìm được events cùng mã HPG):
```
🕐 2 lần trước HPG bị cơ quan quản lý yêu cầu giải trình,
giá giảm thêm rồi hồi lại TB +12% trong 3-4 tuần.
```

Pro (tìm được events cùng mã + cùng ngành):
```
🕐 HPG: 2/2 lần bị điều tra → hồi TB +12.5% (20D).
HSG tương tự 03/2025 → +7%. Xu hướng hồi mạnh sau tin pháp lý.
```

Retail (tìm được events khác mã, cùng tags):
```
🕐 Tình huống tương tự: FLC (11/2022) và DXG (2024) —
sau tin pháp lý, cổ phiếu cùng ngành giảm 10-15% trong 1 tháng.
```

Pro (vượt kháng cự):
```
🕐 FPT: 2/2 vượt MA50 trong 12 tháng → TB +14% (20D).
Cao nhất +15.3% (11/2025), thấp nhất +12.7% (07/2025). Tỷ lệ thắng 100%.
```

---

### COLD START — Lúc mới ra mắt chưa có data

**Seed 50-100 sự kiện lớn 24 tháng qua** (nhập thủ công, 1 người làm 1-2 ngày):

**Chứng khoán VN:**
```
FLC sụp đổ (11/2022) | DXG chủ tịch bị tạm giữ (2024)
HPG anti-dumping EU (nhiều lần) | VNM thoái vốn nhà nước
SSI BCTC vượt kỳ vọng Q3/2025 | MWG đóng cửa Bách Hóa Xanh
VNI thủng 1000 (10/2022) | VNI breakout 1200 (2024)
FPT ký hợp đồng AI lớn | VCB được nới room ngoại
```

**Vĩ mô:**
```
Fed tăng lãi suất (2022-2023, nhiều lần) | Fed pivot (2024)
SBV giảm lãi suất 4 lần (2023) | DXY vượt 105 (nhiều lần)
Giá dầu vượt $90 (2023) | CPI Mỹ giảm dưới 3% (2024)
```

**Crypto:**
```
BTC halving (04/2024) | ETH ETF approved (2024)
FTX sụp (11/2022) | LUNA crash (05/2022)
BTC breakout $50k, $60k, $70k | SEC kiện Binance
```

**Mỗi event seed cần:**
- Ngày, mã (hoặc thị trường), ngành, loại, hướng, tags
- Giá lúc đó + giá sau 5/10/20 phiên (tra lịch sử)
- Tóm tắt 1 câu

**Sau khi hệ thống chạy:** Event Store tự tích lũy — mỗi tín hiệu phát hiện = 1 event mới. Càng chạy lâu → lịch sử càng phong phú.

---

### PIPELINE TỔNG HỢP — VỊ TRÍ TRONG SIGNAL FLOW

```
Phát hiện tín hiệu (Phần 1)
  │
  ├── Chấm điểm (Phần 2)
  │
  ├── Gom nhóm (Phần 3)
  │
  ├── Tìm liên quan (Phần 4 — Bản đồ)
  │
  ├── Tìm lịch sử THỊ TRƯỜNG (Phần 5 — Event Store)  ← MỚI
  │     └→ Similar Event Lookup (3 tầng)
  │     └→ Tính kết quả giá
  │     └→ LLM viết câu ≤140 ký tự
  │
  ├── Tìm lịch sử CÁ NHÂN (Nhóm A — Trí nhớ AI)
  │     └→ Bài học trong trí nhớ user
  │     └→ LLM viết câu ≤140 ký tự
  │
  └── Gộp tất cả → Tạo thẻ → Delivery Router (Nhóm C)
```

**Ưu tiên hiển thị phần lịch sử trên thẻ:**
1. Có cả 2 loại (cá nhân + thị trường) → **hiện lịch sử cá nhân** (quan trọng hơn)
2. Chỉ có 1 loại → hiện loại đó
3. Không có loại nào → **bỏ qua phần lịch sử** trên thẻ (thẻ vẫn gửi, chỉ thiếu phần này)

---

### CÔNG VIỆC CỤ THỂ

| Việc | Ước lượng | Chi tiết |
|------|----------|---------|
| Thiết kế Event Store schema + tạo table | 0.5 ngày | Database table với index trên (ma, loai, huong, nganh, tags, ngay) |
| Ghi event mỗi khi phát hiện tín hiệu | 0.5 ngày | Thêm 1 bước cuối signal detection pipeline |
| Auto-tag engine | 1 ngày | Mapping loại tín hiệu → tags (bảng trên) |
| Cron cập nhật giá sau 5/10/20 phiên | 0.5 ngày | Job chạy mỗi đêm 02:00 AM |
| Similar Event Lookup (3 tầng query) | 1 ngày | Function gọi mỗi khi tạo thẻ |
| LLM viết câu lịch sử (1 prompt) | 0.5 ngày | Tích hợp vào card generation |
| Seed data 50-100 events | 1-2 ngày | Nhập thủ công từ lịch sử thị trường |
| **Tổng (trước AR)** | **~5 ngày** | |


*Hết Nhóm B*

---

## CHANGE LOG

| Ngày | Phiên bản | Thay đổi |
|------|-----------|----------|
| 18/03/2026 | 3.0 | Nâng cấp từ v2 |
| 18/03/2026 | 4.0 | Thêm Nguồn 8 (Room CK), cập nhật prompt + voice rules |
| 19/03/2026 | 4.1 | Thêm người phụ trách từng phần |
| 23/03/2026 | 4.2 | Thêm field catalyst_deadline vào chunk schema (Nguồn 2 + Nguồn 7) — Team C dùng để hiển thị deadline gần nhất trong Góc nhìn |
