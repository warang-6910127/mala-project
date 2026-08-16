# Prototype v1

> สร้างเมื่อ: 2026-08-16 | สร้างใหม่

## ขอบเขตของเวอร์ชันนี้

- ครอบคลุมเฉพาะฟีเจอร์ [[../../../../01-requirements/01-spec/20260802-001-dine-in-qr-table-ordering|สั่งกาแฟที่โต๊ะด้วย QR Code]] (Must have) — flow ของลูกค้า (สแกน QR → เห็น privacy notice → เลือกเมนู → ยืนยันออเดอร์ + ชำระเงิน → ผลการชำระเงิน) และ flow ของบาริสต้า (จอ KDS)
- อ้างอิงเนื้อหา privacy notice banner จาก [[../../../../01-requirements/01-spec/20260802-003-log-retention-pdpa-compliance|การเก็บ Log และการปฏิบัติตาม PDPA]] เฉพาะส่วนข้อความที่ต้องแสดงในหน้าสั่งอาหาร — **ไม่ได้** ทำหน้าจอ PDPA แยกต่างหาก เพราะฟีเจอร์นั้นเป็น manual process ที่ไม่มี self-service UI (ดู [[../../user-journey|user-journey]] หัวข้อ "ลูกค้าขอใช้สิทธิ์ข้อมูลส่วนบุคคล (PDPA)")
- ไม่ครอบคลุมฟีเจอร์ [[../../../../01-requirements/01-spec/20260802-002-sales-dashboard|Dashboard ดูยอดขาย]] (Should have) — อยู่นอกขอบเขตของรอบนี้

## หน้าจอ

| No. | หน้าจอ | Persona | เอกสาร |
|---|---|---|---|
| 01 | สแกน QR + ดูเมนู (พร้อม Privacy Notice Banner) | ลูกค้า | [[01-customer-scan-menu|01-customer-scan-menu]] |
| 02 | ยืนยันออเดอร์ + ชำระเงิน | ลูกค้า | [[02-customer-order-confirm|02-customer-order-confirm]] |
| 03 | ผลการชำระเงิน (สำเร็จ/ไม่สำเร็จ) | ลูกค้า | [[03-customer-payment-result|03-customer-payment-result]] |
| 04 | จอ KDS — รับออเดอร์ | บาริสต้า | [[04-barista-kds|04-barista-kds]] |

## อ้างอิง

- [[../../index|กลับไปยัง 01-prototypes]]
- [[../../DESIGN|DESIGN.md]]
- [[../../user-journey|user-journey]]
- [[../../../../01-requirements/02-plan/feature-list|feature-list]]
- [[../../../../01-requirements/01-spec/20260802-001-dine-in-qr-table-ordering|สเปคเต็ม — QR Table Ordering]]
- [[../../../../01-requirements/01-spec/20260802-003-log-retention-pdpa-compliance|สเปคเต็ม — Log Retention/PDPA]] (อ้างอิงเฉพาะเนื้อหา privacy notice)
