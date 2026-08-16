# DESIGN.md — Design System

> อัปเดตล่าสุด: 2026-08-16

Design system ของ **CAFÉ • งาย** (Craft Your Own Specialty Recipe) ต่อยอดจากความต้องการใน
[[../../01-requirements/01-spec/index|01-spec]] และ [[user-journey|user-journey]] ใช้เป็นข้อมูลอ้างอิงกลางสำหรับทุกหน้าจอ
ของระบบ — หน้าสั่งอาหารของลูกค้า (QR ordering), จอครัว (KDS) ของบาริสต้า, และ Sales Dashboard ของเจ้าของร้าน/ผู้จัดการ
โดยส่งต่อรายละเอียดเชิงระบบ (ถ้ามี design token ที่ implement เป็นโค้ด) ให้ [[../02-technical/index|02-technical]]

โทนหลัก: **Earth tone + Minimalist + Muji** — สีจากธรรมชาติ/เมล็ดกาแฟ, พื้นที่ว่างเยอะ, ตัดของตกแต่งที่ไม่จำเป็นออก

---

## 1. Brand Identity & CI

- **ชื่อแบรนด์ (Wordmark)**: `CAFÉ • งาย` — ใช้ตัวคั่น `•` (middle dot) ระหว่าง "CAFÉ" กับ "งาย" เสมอ ห้ามเปลี่ยนเป็นเครื่องหมายอื่น (เช่น `-`, `|`)
- **Tagline**: `Craft Your Own Specialty Recipe` — ใช้คู่กับ wordmark เท่านั้น ไม่ใช้แยกเดี่ยวเป็นสโลแกนหลักของหน้าจออื่น
- **โลโก้**: ![logo](assets/logo.png)
  - ไฟล์พื้นครีมทึบ (ใช้บนพื้นหลังครีม/ขาว): `assets/logo.png`
  - ไฟล์พื้นหลังโปร่งใส (ใช้วางทับพื้นหลังอื่น เช่น favicon, การ์ด, พื้นหลังเข้ม): `assets/logo-transparent.png`
  - เลย์เอาต์: wordmark กึ่งกลางแนวนอน ตามด้วย tagline ขนาดเล็กกว่าอยู่บรรทัดถัดไป และเส้นบางสีน้ำตาลเข้ม (accent line) คั่นด้านล่างสุด
  - "CAFÉ" ใช้ตัวพิมพ์ใหญ่ทั้งหมด ตัวอักษรห่าง (letter-spacing กว้าง), "งาย" ใช้ฟอนต์ไทยเส้นเรียบ น้ำหนักใกล้เคียงกัน เพื่อให้ balance กับฝั่ง Latin
  - พื้นที่ว่างขั้นต่ำรอบโลโก้ (clear space) ≥ ความสูงของตัวอักษร "C" ทุกด้าน ห้ามวางองค์ประกอบอื่นทับหรือชิดเกินไป
  - พื้นหลังที่ใช้ได้: พื้นครีม/ขาว หรือพื้นหลังอื่นเมื่อใช้ไฟล์ `logo-transparent.png` (ดูโทนสีข้อ 2) หลีกเลี่ยงพื้นหลังที่มีลวดลายซับซ้อนซึ่งลดความชัดของตัวอักษร
- **Brand voice / น้ำเสียง**: อบอุ่น เรียบง่าย ตรงไปตรงมา แบบช่างฝีมือ (craft) — ประโยคสั้น ไม่ใช้ศัพท์การตลาดเกินจริง ไม่ใช้ภาษาที่เร่งเร้า (urgency) เกินความจำเป็น
- **หลักการที่มาจากโลโก้**: ความเรียบของ layout และช่องว่างในโลโก้ คือหลักการเดียวกับที่ใช้ทั่วทั้งระบบ (ดู [[#4-ux-guidelines-rules|UX Guidelines]])

## 2. Design Tokens / Design System

### 2.1 สี (Colors)

Earth tone palette อิงจากสีในโลโก้ (น้ำตาลเข้ม + พื้นครีม + ฟ้าอมเทาของ tagline) ขยายเป็นชุดสีที่ใช้ได้ทั้งระบบ:

| Token | Hex | ใช้เมื่อไหร่ |
|---|---|---|
| `color-bg-base` | `#F7F3EA` (ครีม) | พื้นหลังหลักของทุกหน้าจอ |
| `color-bg-surface` | `#FBF9F4` (ไอวอรี่) | พื้น card/sheet ที่ยกขึ้นมาจาก base เล็กน้อย |
| `color-text-primary` | `#4A3728` (เอสเพรสโซ) | ข้อความหลัก, heading, wordmark |
| `color-text-secondary` | `#6B4F3B` (โกโก้/บาร์ค) | ข้อความรอง, label, ไอคอน |
| `color-text-muted` | `#8FA3AC` (ฟ้าอมเทา ตาม tagline) | ข้อความ tertiary, helper text, timestamp |
| `color-accent-primary` | `#B97455` (เทอร์ราคอตต้า/clay) | ปุ่มหลัก, CTA, สถานะที่ต้องการความสนใจ |
| `color-accent-line` | `#6E3B32` (เส้นน้ำตาลเข้มจากโลโก้) | เส้นคั่น, underline, divider สำคัญ |
| `color-border` | `#E4DCC8` (ทราย/oat) | เส้นขอบบาง (hairline), divider ทั่วไป |
| `color-surface-alt` | `#EFE7D8` | พื้นหลัง tag/badge, แถวสลับของตาราง |
| `color-success` | `#6B7A4A` (olive) | สถานะสำเร็จ เช่น จ่ายเงินสำเร็จ |
| `color-warning` | `#B98B3E` (ochre) | สถานะรอดำเนินการ |
| `color-error` | `#9C4A3A` (brick) | สถานะผิดพลาด/จ่ายเงินไม่สำเร็จ |

กฎการใช้สี: ต่อ 1 หน้าจอ ใช้สี accent (`color-accent-primary`/`color-accent-line`) **ไม่เกิน 1-2 จุดที่สำคัญที่สุด** เท่านั้น (เช่น ปุ่มยืนยันคำสั่งซื้อ) ที่เหลือให้พื้นที่เป็นโทนกลาง (base/surface/text) แบบ muji — ไม่ไล่เฉดสีรุ้งหรือเพิ่มสีใหม่นอกตารางนี้

### 2.2 Typography

- **ฟอนต์หัวเรื่อง/แบรนด์ (Display)**: serif เส้นบาง มี letter-spacing กว้าง (ให้ความรู้สึกแบบ wordmark) ใช้เฉพาะ H1/ชื่อแบรนด์ — เช่น Fraunces หรือ Noto Serif Thai (ฝั่งไทย)
- **ฟอนต์เนื้อหา (Body/UI)**: humanist sans-serif อ่านง่ายทั้งไทย-อังกฤษ รองรับวรรณยุกต์/สระบนของไทยไม่ตกขอบ — เช่น IBM Plex Sans Thai หรือ Noto Sans Thai
- **Type scale** (base 16px, ratio ~1.25):

| Token | ขนาด | น้ำหนัก | ใช้กับ |
|---|---|---|---|
| `text-display` | 32px | Regular (serif) | ชื่อแบรนด์, หน้า splash |
| `text-h1` | 28px | Semibold | หัวข้อหน้าจอหลัก |
| `text-h2` | 22px | Semibold | หัวข้อ section |
| `text-h3` | 18px | Medium | หัวข้อย่อย, ชื่อเมนู |
| `text-body` | 16px | Regular | เนื้อหาทั่วไป |
| `text-caption` | 13px | Regular | helper text, timestamp, label รอง |

- Line-height: 1.5 สำหรับ body/caption, 1.3 สำหรับ heading (เผื่อสระ/วรรณยุกต์ไทยไม่ถูกตัด)
- ห้ามใช้ตัวเอน (italic) กับข้อความภาษาไทย

### 2.3 Spacing & Layout

- หน่วยฐาน (base unit): `4px`
- Scale: `4 / 8 / 12 / 16 / 24 / 32 / 48 / 64`
- Semantic token: `space-xs=4px`, `space-sm=8px`, `space-md=16px`, `space-lg=24px`, `space-xl=32px`, `space-2xl=48px`
- ระยะขอบหน้าจอ (screen padding) ขั้นต่ำ: `16px` บนมือถือ, `24px` บนจอ KDS/dashboard
- ช่องไฟระหว่าง section ขั้นต่ำ: `32px` — เว้นว่างให้มากแบบ muji ดีกว่าใส่เส้นแบ่งเยอะ ๆ

### 2.4 Radius & Elevation

- Border radius: `4px` (control เล็ก เช่น badge/input), `8px` (card, button, modal) — ไม่ใช้ radius ใหญ่เกิน 8px เพื่อความเรียบ
- Elevation: เลี่ยง drop shadow หนัก ๆ ใช้ **hairline border** (`1px`, `color-border`) แทนเงาเป็นหลัก อนุญาต shadow บางมาก (`0 1px 2px rgba(74,55,40,0.06)`) เฉพาะ modal/overlay ที่ลอยเหนือเนื้อหาอื่นจริง ๆ

## 3. UI Components & Patterns

| Component | จุดที่ใช้ | กติกาออกแบบ |
|---|---|---|
| **Menu Item Card** | หน้าสั่งอาหาร (ลูกค้า) | รูป + ชื่อเมนู (`text-h3`) + ราคา + ปุ่มเพิ่ม 1 ปุ่ม, พื้น `color-bg-surface`, border บาง ไม่มีเงา |
| **Table Badge** | ทุกหน้าจอที่เกี่ยวกับโต๊ะ (สั่งอาหาร, ตั๋ว KDS) | เลขโต๊ะต้องเด่นและอ่านง่ายที่สุดในหน้าจอเสมอ ใช้ `text-h2` ขึ้นไป วางตำแหน่งเดียวกันทุกหน้าเพื่อความคุ้นเคย |
| **Order Status Tag** | ตั๋ว KDS, ประวัติออเดอร์ | ใช้สีสถานะ (`success`/`warning`/`error`) พื้น `color-surface-alt` เท่านั้น ไม่ใช้สีเต็ม (solid fill) กับพื้นที่ใหญ่ |
| **Primary Button** | ยืนยันออเดอร์/ชำระเงิน | พื้น `color-accent-primary`, ตัวอักษรสีครีม, radius 8px — ใช้กับ action ที่สำคัญที่สุดของหน้าจอเท่านั้น (1 ปุ่มต่อหน้า) |
| **Secondary/Ghost Button** | action รอง (ยกเลิก, กลับ) | เส้นขอบ `color-border` หรือไม่มีพื้นหลังเลย ตัวอักษร `color-text-secondary` |
| **Privacy Notice Banner** | ก่อนยืนยันออเดอร์/จ่ายเงิน (ตาม [[../../01-requirements/01-spec/20260802-003-log-retention-pdpa-compliance|Log Retention/PDPA]]) | ข้อความสั้นแบบ inline banner ไม่ใช้ modal บังคับกดยอมรับ, พื้น `color-surface-alt`, ไอคอนเดียว ไม่มีปุ่ม opt-in/checkbox |
| **KDS Order Ticket** | จอครัว (บาริสต้า) | คอนทราสต์สูง (`color-text-primary` บน `color-bg-surface`), แสดงเฉพาะออเดอร์ที่จ่ายเงินสำเร็จแล้วเท่านั้น, เรียงตามเวลาเข้าคิว |
| **Stat Tile / Chart Card** | Sales Dashboard | ใช้สีจากตารางในข้อ 2.1 เท่านั้นสำหรับกราฟ (ห้ามใช้สีสดนอกพาเลท), 1 metric เด่นต่อ tile, ไม่ chart ซ้อนหลายชนิดในการ์ดเดียว |
| **Empty / Loading state** | ทุกหน้าที่รอข้อมูล | ข้อความสั้น + ไอคอนเรียบสีเดียว (`color-text-muted`) ไม่ใช้ illustration สีสันจัด |

## 4. UX Guidelines & Rules

- **Muji-minimalist**: ก่อนเพิ่ม element ใหม่ในหน้าจอ ให้ถามว่า "ตัดออกได้ไหม" ก่อนเสมอ — ทุก element ต้องมีหน้าที่ ไม่ใช่ของตกแต่ง
- **1 primary action ต่อหน้าจอ**: ทุกหน้าจอมีปุ่ม/ทางเลือกหลักได้ทางเดียวชัดเจน (เช่น หน้าสั่งอาหารมีปุ่ม "ยืนยันออเดอร์" ปุ่มเดียว)
- **ความชัดเจนของหมายเลขโต๊ะ**: ตาม [[../../01-requirements/01-spec/20260802-001-dine-in-qr-table-ordering|QR Table Ordering]] ทุกขั้นตอนของลูกค้า (สแกน → เลือกเมนู → ยืนยันออเดอร์) และของบาริสต้า (ตั๋ว KDS) ต้องแสดงหมายเลขโต๊ะตลอดเวลา ไม่ใช่แค่หน้าแรก
- **ลำดับจ่ายก่อนเข้าครัว**: UI ต้องไม่มีทางแสดงสถานะออเดอร์ว่า "เข้าครัวแล้ว"/ปรากฏบนจอ KDS ก่อนระบบยืนยันว่าจ่ายเงินสำเร็จ — ตรงกับกฎ "การชำระเงิน" ของสเปค 001
- **Privacy notice ต้องไม่ขวางทาง**: แสดงก่อนยืนยันออเดอร์/จ่ายเงินแบบ banner สั้น อ่านจบได้ใน 1-2 วินาที ไม่ใช้ dark pattern บังคับกดยอมรับหลายขั้นตอน (ตาม [[../../01-requirements/01-spec/20260802-003-log-retention-pdpa-compliance|Log Retention/PDPA]])
- **จำกัดสิทธิ์การเข้าถึง**: หน้า Sales Dashboard ต้องไม่ปรากฏใน navigation ของลูกค้า/บาริสต้า — เจ้าของร้าน/ผู้จัดการเท่านั้นที่เห็นเมนูนี้ (ตาม [[../../01-requirements/01-spec/20260802-002-sales-dashboard|Sales Dashboard]])
- **Contrast**: คู่สีข้อความ/พื้นหลังทุกคู่ต้องผ่านอย่างน้อย WCAG AA (4.5:1 สำหรับ body text) — `color-text-muted` บน `color-bg-base` ให้ใช้เฉพาะ caption/helper text ขนาดเล็กเท่านั้น ไม่ใช้กับเนื้อหาสำคัญ
- **ภาษาไทยเป็นหลัก**: ข้อความ UI ทั้งหมดเป็นภาษาไทยก่อน ใช้ภาษาอังกฤษเฉพาะคำศัพท์เฉพาะที่ไม่มีคำแปลกระชับ (เช่น "KDS", "QR")
- **ความสม่ำเสมอของ layout**: ตำแหน่งปุ่มหลัก/หมายเลขโต๊ะ/สถานะออเดอร์ ต้องอยู่ตำแหน่งเดียวกันทุกหน้าจอที่เกี่ยวข้อง เพื่อลดภาระการเรียนรู้ของลูกค้า/บาริสต้า

## เอกสารที่เกี่ยวข้อง (Related Documents)

- [[index|กลับไปยัง 01-prototypes]]
- [[user-journey|user-journey]] — ลำดับขั้นตอนที่ design system นี้ต้องรองรับ
- [[../../01-requirements/01-spec/20260802-001-dine-in-qr-table-ordering|QR Table Ordering]]
- [[../../01-requirements/01-spec/20260802-002-sales-dashboard|Sales Dashboard]]
- [[../../01-requirements/01-spec/20260802-003-log-retention-pdpa-compliance|Log Retention/PDPA]]
- [[../02-technical/index|02-technical]] — สำหรับรายละเอียดเชิง implementation ของ design token
