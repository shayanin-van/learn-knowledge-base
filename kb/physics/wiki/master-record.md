# Master Record Tagging System

**Summary**: The company's curriculum tagging system. Every concept page must include tags from this hierarchy in its curriculum anchor field.

**Sources**: `kb/physics/raw/physics-master-tag.csv`

**Last updated**: 2026-05-15

---

## What it is

The master record is the company's canonical curriculum structure. It is a flat CSV where each row represents one subtopic, tagged across 7 levels of hierarchy. Every concept page in this knowledge base must reference the relevant master record tag(s) in its `**Curriculum anchor**` field.

The master record is a **tagging system**, not a structural constraint. Wiki pages can be organized at finer or coarser granularity than the master record, and a single wiki page may map to multiple subtopic rows.

## Tag hierarchy

| Level | Thai label | Example (SHM) |
|---|---|---|
| Grade | ระดับชั้น | มัธยมปลาย |
| Subject Group | กลุ่มวิชา | วิทยาศาสตร์ |
| Subject | วิชา | ฟิสิกส์ |
| Module | กลุ่ม | กลุ่มกลศาสตร์ |
| Unit | หน่วย | การเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| Lesson | บทเรียน | พื้นฐานการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| Subtopic | หัวข้อย่อย | ปริมาณที่เกี่ยวข้องกับการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |

## How to use in concept pages

The `**Curriculum anchor**` field in a concept page lists the master record tag(s) the page covers, written as a path from Module down to Subtopic (Grade / Subject Group / Subject are the same for all physics pages and can be omitted for brevity):

```
**Curriculum anchor**:
- กลุ่มกลศาสตร์ › การเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย › พื้นฐานการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย › ปริมาณที่เกี่ยวข้องกับการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย
```

If a page covers multiple subtopics, list each on its own line.

## SHM unit — full tag list

Rows 97–106 in `physics-master-tag.csv`:

| Row | Lesson | Subtopic |
|---|---|---|
| 97 | พื้นฐานการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย | ปริมาณที่เกี่ยวข้องกับการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| 98 | สมการการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย | สมการปริมาณการเคลื่อนที่กับเวลาในการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| 99 | สมการการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย | การหาค่าสูงสุดและตำแหน่งสูงสุดในสมการของการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| 100 | สมการการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย | กราฟของปริมาณการเคลื่อนที่กับเวลาในการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| 101 | ความถี่เชิงมุมของการเคลื่อนที่แบบฮาร์มอนิกอย่างง่ายรูปแบบต่างๆ | การสั่นของมวลติดสปริง |
| 102 | ความถี่เชิงมุมของการเคลื่อนที่แบบฮาร์มอนิกอย่างง่ายรูปแบบต่างๆ | การแกว่งของลูกตุ้มอย่างง่าย |
| 103 | ความถี่เชิงมุมของการเคลื่อนที่แบบฮาร์มอนิกอย่างง่ายรูปแบบต่างๆ | รูปแบบอื่นๆ ของการเคลื่อนที่แบบฮาร์มอนิกอย่างง่าย |
| 104 | การอนุรักษ์พลังงานในการเคลื่อนที่ฮาร์มอนิกอย่างง่าย | ความสัมพันธ์ระหว่างพลังงานจลน์และพลังงานศักย์ |
| 105 | ความถี่ธรรมชาติและการสั่นพ้อง | ความถี่ธรรมชาติ |
| 106 | ความถี่ธรรมชาติและการสั่นพ้อง | การสั่นพ้อง |

## Related pages

- [[index]]
