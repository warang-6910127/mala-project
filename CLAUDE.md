# CLAUDE.md

ไฟล์นี้ให้คำแนะนำแก่ Claude Code (claude.ai/code) เมื่อทำงานกับโค้ดในโปรเจกต์นี้

## ลักษณะของโปรเจกต์นี้

โปรเจกต์นี้**ไม่ใช่โค้ดเบส** — แต่เป็น Obsidian vault ที่ใช้เป็นระบบเอกสารของโปรเจกต์ "my-coffee-store" ไม่มีซอร์สโค้ด, build system, package manifest, linter หรือ test runner ใดๆ เนื้อหาทั้งหมดเป็น Markdown และเอกสารทุกฉบับเขียนเป็นภาษาไทย

ทุกโฟลเดอร์จะมีไฟล์ `index.md` ที่อธิบายจุดประสงค์ของโฟลเดอร์นั้น และเชื่อมโยงไปยังโฟลเดอร์ที่เกี่ยวข้องด้วย Obsidian wikilink (`[[relative/path/index|display text]]`) เมื่อเพิ่มเอกสารใหม่ ให้วางไว้ในโฟลเดอร์ตามลำดับขั้นตอนด้านล่าง และเชื่อมโยงกลับไปยัง `index.md` ที่เกี่ยวข้อง ตามรูปแบบ wikilink ที่มีอยู่แล้ว

## ลำดับขั้นตอนของเอกสาร (Documentation Pipeline)

เนื้อหาไหลไปทิศทางเดียวผ่านแต่ละขั้นตอน และ `index.md` ของแต่ละขั้นตอนจะลิงก์ไปข้างหน้า/ย้อนกลับไปยังขั้นตอนข้างเคียง:

1. **`docs/01-requirements/`** — ความต้องการของโปรเจกต์ ถือเป็นต้นทาง (source of truth)
   - `01-spec/` — ข้อกำหนดของฟีเจอร์, user story, กฎทางธุรกิจ, ขอบเขต (in/out scope)
   - `02-plan/` — roadmap, การแบ่ง phase/milestone, ลำดับความสำคัญ, timeline
   - `03-task/` — รายการงานย่อย, สถานะ to-do, ผู้รับผิดชอบ/deadline
2. **`docs/02-design/`** — การออกแบบที่ต่อยอดจากความต้องการ
   - `01-prototypes/` — wireframe/mockup (Markdown แบบ text-based เป็นค่าเริ่มต้น), user flow, พื้นฐาน design system, และ interactive prototype แบบ HTML/CSS/JS เป็นทางเลือกเสริม (ดูข้อยกเว้นด้านล่าง)
   - `02-technical/` — การออกแบบเชิงเทคนิค (architecture, database schema, API design, ทางเลือกเทคโนโลยี + เหตุผล)
3. **`docs/03-testing/`** — การทดสอบที่ต่อยอดจากการออกแบบ
   - `01-test-plan/` — test case/scenario, ข้อมูลทดสอบ, ขอบเขตการทดสอบ
   - `02-test-result/` — ผล pass/fail, บั๊กที่พบ, สถานะการแก้ไข
4. **`docs/04-retrospectives/`** — บทเรียนที่ได้หลังจบแต่ละ phase/sprint/milestone (สิ่งที่ทำได้ดี, สิ่งที่ควรปรับปรุง, action item) โดยอ้างอิงจากผลทดสอบและ log
5. **`docs/05-log/`** — บันทึกความเคลื่อนไหว/เหตุการณ์สำคัญแบบเรียงตามลำดับเวลา (changelog / decision log / incident) ที่ retrospectives ใช้อ้างอิง

`docs/00-archived/` ใช้เก็บเอกสารที่ถูกแทนที่หรือยกเลิกแล้ว **ห้ามลบเอกสารออกจากโปรเจกต์โดยเด็ดขาด** ให้ย้ายไปไว้ที่ `00-archived/` แทน เพื่อรักษาประวัติการตัดสินใจไว้

## แนวทางการทำงานในโปรเจกต์นี้

- เขียนเอกสารใหม่ให้ตรงตามสไตล์ของ `index.md` ที่มีอยู่ — ภาษาไทย กระชับ เป็น bullet point
- เอกสารใหม่ต้องอยู่ในโฟลเดอร์ขั้นตอนที่ตรงกับประเภทของมัน (เช่น สเปคไปที่ `01-requirements/01-spec/`, รายงานบั๊กไปที่ `03-testing/02-test-result/`) ห้ามวางลอยไว้ใน `docs/`
- เพิ่ม wikilink เชื่อมเอกสารใหม่กับขั้นตอนก่อนหน้าและถัดไป ให้สอดคล้องกับรูปแบบการเชื่อมโยงที่ `index.md` แต่ละไฟล์ทำไว้อยู่แล้ว
- เนื่องจากยังไม่มีโค้ดแอปพลิเคชัน จึงห้ามสร้างคำสั่ง build/test/lint ขึ้นมาเอง — โปรเจกต์นี้ไม่มีคำสั่งเหล่านี้

## ข้อยกเว้น: Interactive Prototype (HTML/CSS/JS)

โปรเจกต์นี้ยังคง **ไม่ใช่โค้ดเบส** เป็นหลัก — Markdown wireframe (ASCII box layout) ยังเป็นรูปแบบ
เริ่มต้นของทุก prototype เหมือนเดิม แต่อนุญาตให้สร้าง **interactive prototype แบบ HTML/CSS/JS
เป็นทางเลือกเสริม** (ไม่ใช่ตัวแทน) ภายใต้เงื่อนไขนี้เท่านั้น:

- อยู่ได้เฉพาะที่ `docs/02-design/01-prototypes/prototypes/v{N}/interactive/` — คู่กันกับ Markdown
  wireframe ของเวอร์ชันเดียวกัน ห้ามวางไฟล์ HTML/CSS/JS ไว้ที่อื่นในโปรเจกต์
- ต้องเป็นไฟล์ **self-contained** (inline CSS/JS ในไฟล์ HTML เดียว หรือไฟล์ .css/.js แนบไม่กี่ไฟล์
  ในโฟลเดอร์เดียวกัน) เปิดดูได้ตรงๆ จากเบราว์เซอร์โดยไม่ต้องมี build step, package manifest
  (`package.json` ฯลฯ), bundler, หรือ dependency จากภายนอก — ยังคงไม่มี build/test/lint command
  ในความหมายของโปรเจกต์นี้
- ต้องอ้างอิงสี/ฟอนต์/spacing/component จาก `DESIGN.md` เท่านั้น เหมือนกับที่ Markdown wireframe
  ต้องทำ ห้ามคิดดีไซน์ใหม่แยกจากกัน
- ใช้เมื่อผู้ใช้ขอ "interactive prototype"/"ทำ HTML ให้ดูโต้ตอบได้" อย่างชัดเจนเท่านั้น — ไม่ใช่ค่า
  เริ่มต้นของ `create-prototype` skill
