# Physics Knowledge Base — Log

Append-only record of all content changes. Most recent entry at the top.

---

## 2026-05-15 — Image extraction session — OE Textbook pages rendered, 6 pages updated

**Source**: `[OE-Textbook]-SHM.pdf` — rendered specific pages as PNG at 2× zoom using PyMuPDF

**Assets created** (`wiki/assets/`):
- `shm-reference-circle.png` — p.9: SHM physical intuition, reference circle, variable legend
- `shm-equations-derivation-1.png` — p.14: x(t) derivation from reference circle
- `shm-equations-derivation-2.png` — p.15: v(t) derivation + calculus rules
- `shm-equations-derivation-3.png` — p.16: a(t) derivation
- `shm-graphs-xt-vt-at.png` — p.18: x-t, v-t, a-t graphs (sin and cos variants)
- `shm-sin-vs-cos.png` — p.20: sin/cos graph comparison + phase relationships
- `shm-spring-mass-derivation.png` — p.33: spring-mass setup + derivation
- `shm-spring-combinations.png` — p.35: series and parallel spring diagrams + formulas
- `shm-pendulum-diagram.png` — p.36: pendulum force diagram + derivation + worked example
- `shm-other-forms-diagrams.png` — p.37: floating object + U-tube diagrams + formulas
- `shm-energy-diagram.png` — p.39: energy conservation diagram + worked example

**Pages updated** (images embedded):
- `shm-equations-graphs.md` — เพิ่มรูป `shm-graphs-xt-vt-at.png` หลัง section กราฟ x-t/v-t/a-t + รูป `shm-sin-vs-cos.png` ใน section sin กับ cos
- `shm-spring-combinations.md` — เพิ่มรูป `shm-spring-combinations.png` ก่อน section อนุกรม
- `shm-pendulum.md` — เพิ่มรูป `shm-pendulum-diagram.png` ก่อน section การอนุมาน
- `shm-other-forms.md` — เพิ่มรูป `shm-other-forms-diagrams.png` ก่อน section วัตถุลอยน้ำ
- `shm-energy.md` — เพิ่มรูป `shm-energy-diagram.png` ก่อน section สูตรทอง
- `shm-spring-mass.md` — เพิ่มรูป `shm-spring-mass-derivation.png` ก่อน section หา ω

**Notes**:
- ใช้ PyMuPDF (fitz) render ที่ 2× zoom เพื่อความคมชัด
- MuPDF structure-tree warnings บางหน้า (non-fatal — รูปที่ได้ยังสมบูรณ์)
- รูป derivation-1/2/3 (p.14–16) ถูก extract ไว้แล้วแต่ยังไม่ embed — เนื้อหาซ้ำกับที่ text อธิบายไว้แล้ว เก็บไว้ใน assets เผื่อต้องการในอนาคต
- รูป reference-circle (p.9) เก็บไว้ใน assets — เหมาะสำหรับ shm-definition.md ถ้าต้องการเพิ่มในอนาคต

---

## 2026-05-15 — SHM ingest session (continued) — OE Textbook + International Textbook ingested, 3 new pages, 1 page updated

**Sources ingested**:
- `[OE-Textbook]-SHM.pdf` — spring combinations (series/parallel/cutting), other SHM forms (floating object, U-tube, physical pendulum), energy in SHM (½kA² = ½mv² + ½kx²), extensive worked examples, uses g = 10 m/s²
- `[International-Textbook]-SHM.pdf` (Serway & Jewett, Chapter 15) — full derivations, energy section, damped + forced oscillations (เกินหลักสูตร), uses cos as default (vs IPST sin), confirms all core content consistent with IPST

**Pages created**:
- `shm-spring-combinations.md` — สปริงหลายตัว: ต่ออนุกรม (1/k_รวม = Σ1/k), ต่อขนาน (k_รวม = Σk), ตัดสปริง (k ∝ 1/L), ตัวอย่างคำนวณ, misconceptions
- `shm-energy.md` — พลังงานใน SHM: E = ½kA², K+U = constant, v = ±ω√(A²−x²), ตารางพลังงานที่ตำแหน่งสำคัญ, ตัวอย่าง
- `shm-other-forms.md` — SHM รูปแบบอื่น (เกินหลักสูตร): วัตถุลอยน้ำ, หลอดรูป U, physical pendulum, ตารางสรุปสูตรทุกรูปแบบ

**Pages updated**:
- `shm-equations-graphs.md` — เพิ่ม section "sin กับ cos — ต่างกันยังไง" อธิบาย convention ต่างกันระหว่าง IPST (sin default) กับตำราต่างประเทศ (cos default) พร้อมตารางเปรียบเทียบ
- `index.md` — เพิ่ม shm-spring-combinations, อัปเดต shm-energy และ shm-other-forms เอา "coming" ออก

**Convention note**:
- OE Textbook ใช้ g = 10 m/s² ในตัวอย่าง; IPST ใช้ 9.8 m/s² — pages ของเราไม่ระบุค่า g ตายตัวในสูตร สอดคล้องกับทั้งสองแหล่ง
- International Textbook ใช้ cos เป็น default สำหรับ x(t); IPST ใช้ sin — บันทึกความต่างนี้ใน shm-equations-graphs.md แล้ว

**SHM chapter status**: ครบทุก page ที่วางแผนไว้แล้ว

---

## 2026-05-15 — SHM ingest session (continued) — Teacher Manual ingested, 2 new pages, 3 pages updated

**Sources ingested**:
- `[IPST-Teacher-Manual]-SHM.pdf` — ผลการเรียนรู้ 2 ข้อ, จุดประสงค์การเรียนรู้ 7 ข้อ, ความเข้าใจคลาดเคลื่อนสำหรับทุกหัวข้อ, ยืนยันขอบเขต (พลังงานและ physical pendulum ไม่ใช่ผลการเรียนรู้หลัก)

**Pages created**:
- `shm-pendulum.md` — ลูกตุ้มอย่างง่าย, small angle approximation, อนุมาน ω = √(g/l), สูตร T, ความเข้าใจคลาดเคลื่อน (มวล/มุมไม่มีผลต่อ T)
- `shm-natural-frequency-resonance.md` — ความถี่ธรรมชาติ, การสั่นพ้อง, Taipei 101 tuned mass damper, Tacoma Narrows bridge, ไมโครเวฟ, MRI, ความเข้าใจคลาดเคลื่อน

**Pages updated**:
- `shm-definition.md` — เพิ่มตารางความเข้าใจคลาดเคลื่อน + Teacher Manual source
- `shm-equations-graphs.md` — เพิ่มตารางความเข้าใจคลาดเคลื่อน 3 ข้อ + แทนที่ข้อสังเกตแบบเดิมด้วยตารางสรุป + Teacher Manual source
- `shm-spring-mass.md` — เพิ่มตารางความเข้าใจคลาดเคลื่อน + Teacher Manual source
- `index.md` — อัปเดตสถานะ shm-pendulum และ shm-natural-frequency-resonance (เอา "coming" ออก)

**Style note adopted**: ภาษาแบบติวเตอร์ที่สอนเก่ง ไม่เน้นการทดลอง ตารางความเข้าใจคลาดเคลื่อนใช้รูปแบบ ❌/✅

**Pending**:
- อ่านและ ingest `[OE-Textbook]-SHM.pdf` → scope extensions (energy, other forms), ตัวอย่างโจทย์เพิ่ม
- อ่านและ ingest `[International-Textbook]-SHM.pdf` → ความลึกของการอนุมาน, ตรวจสอบข้อเท็จจริง
- เขียน `shm-energy.md` (Special Topic) และ `shm-other-forms.md` (เกินหลักสูตร)
- Extract OE images ใส่ `wiki/assets/`

---

## 2026-05-15 — SHM ingest session (partial) — S-Map + IPST Textbook ingested

**Sources ingested**:
- `[OE-S-Map]-SHM.pdf` — scope and structure, "เกินหลักสูตร" flags on physical pendulum/U-tube/floating object, energy labeled "Special Topic"
- `[IPST-Textbook]-SHM.pdf` — full chapter 8 (46 pp.), ฟิสิกส์ เล่ม 3; confirms energy and "other forms" absent from IPST

**Pages created**:
- `shm-definition.md` — นิยาม SHM เชิงกายภาพ (physical intuition first), ปริมาณพื้นฐาน (A, T, f, ω, φ), วงกลมสมมติ
- `shm-equations-graphs.md` — สมการ x(t)/v(t)/a(t) อนุมานจากวงกลมสมมติ, ค่าสูงสุด, $a = -\omega^2 x$, กราฟ, กฎการเลือก sin/cos
- `shm-spring-mass.md` — มวลติดสปริง, อนุมาน $\omega = \sqrt{k/m}$ จากกฎของนิวตัน, สูตร T และ f

**Pages updated**:
- `index.md` — SHM section populated; 3 pages created, 4 pending

**Pending (after session compaction)**:
- Ingest `[IPST-Teacher-Manual]-SHM.pdf`, `[OE-Textbook]-SHM.pdf`, `[International-Textbook]-SHM.pdf`
- Write `shm-pendulum.md`, `shm-natural-frequency-resonance.md`, `shm-energy.md`, `shm-other-forms.md`
- Extract OE images into `wiki/assets/` and embed in pages
- Update all pages with Teacher Manual misconceptions and learning objectives

---

## 2026-05-15 — Source type taxonomy documented

**Pages created**:
- `source-types.md` — Defines the 5 source file type prefixes (`[IPST-Textbook]`, `[IPST-Teacher-Manual]`, `[OE-Textbook]`, `[OE-S-Map]`, `[International-Textbook]`), the role and ingest instructions for each, and the recommended ingest order when all types are available

**Pages updated**:
- `index.md` — Added source-types to Reference section
- `CLAUDE.md` — Added Source types section with per-type ingest instructions

---

## 2026-05-15 — Master record tagging system documented

**Pages created**:
- `master-record.md` — Documents the company's 7-level curriculum tag hierarchy (Grade → Subject Group → Subject → Module → Unit → Lesson → Subtopic), how to use tags in the `Curriculum anchor` field, and the full SHM unit tag list (rows 97–106 of `physics-master-tag.csv`)

**Pages updated**:
- `index.md` — Added Reference section with master-record page

**Source**: `kb/physics/raw/physics-master-tag.csv`

---

## 2026-05-15 — Physics KB established

**Structure created**:
- `kb/physics/CLAUDE.md` — maintenance instructions
- `kb/physics/raw/` — source documents directory
- `kb/physics/wiki/` — concept pages directory
- `kb/physics/wiki/index.md` — this index
- `kb/physics/wiki/log.md` — this log

**First content target**: Simple Harmonic Motion (SHM) chapter — for team MVP testing.
