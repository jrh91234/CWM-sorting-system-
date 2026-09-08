# CWM Sorting System

หน้าเว็บสำหรับบันทึก/ติดตามงานรอ Sort (คัดแยกงาน NG) ของ Coil Winding

## โครงสร้างและความสัมพันธ์กับ repo `Coil-winding`

repo นี้เป็น **frontend อีกหน้าหนึ่ง** ที่คุยกับ Google Apps Script ตัวเดียวกันกับ
repo `jrh91234/Coil-winding` (SCRIPT_URL เดียวกัน = สเปรดชีตเดียวกัน)

| สิ่งที่ต้องแก้ | แก้ที่ repo ไหน |
|---|---|
| UI หน้า Sorting (`index.html`) | **repo นี้** |
| Backend Apps Script (`scr/backend.gs`) | **`jrh91234/Coil-winding`** เท่านั้น |

`Coil-winding` เป็น repo หลักของ backend — มี `.clasp.json` (scriptId
`1LH_ozOBO4YbYF_Yt0aEpaofaaWf9rJsxZsSNdO6nsoxdTgFFFxoo-FZA`) และ workflow
`deploy-apps-script.yml` ที่ deploy `scr/backend.gs` ขึ้น Apps Script อัตโนมัติ

> ⚠️ ไฟล์ `scr/backend.gs` ใน repo นี้เป็นสำเนาเก่าและ **ไม่ถูก deploy**
> อย่าแก้ไฟล์นี้ ให้ไปแก้ที่ `Coil-winding/scr/backend.gs` แทน

## Job Order

ช่อง "Job Order (แผนการผลิต)" ในฟอร์มบันทึกงานรอ Sort ดึงรายการจาก
`GET_JOB_ORDERS` (ชีต `Plan_Data`) และเก็บค่าลงคอลัมน์ `Job_Order`
ของชีต `Sorting_Data` — คอลัมน์นี้ backend จะสร้างให้อัตโนมัติถ้ายังไม่มี

## ขั้นตอนที่พบ (Found_At) — มีผลต่อการนับยอด

ช่อง "ขั้นตอนที่พบ (Stage)" ถูกส่งไปเก็บที่คอลัมน์ `Found_At` ของชีต `Sorting_Data`
(นอกเหนือจากที่ยังเขียน `[พบที่: ...]` นำหน้า Remark ไว้เพื่อความเข้ากันได้กับข้อมูลเก่า)
backend ใช้ค่านี้ตัดสินว่ายอดของใบงานนั้นเคยถูกนับไปแล้วหรือยัง:

| ขั้นตอนที่เลือก | ความหมายต่อยอด |
|---|---|
| ระหว่างกระบวนการผลิต | ยังไม่เคยบันทึกเป็น FG หรือ NG → เป็นยอดใหม่ในก้อน "รอ Sorting" |
| FG / RTV (หรือชื่อที่มีคำว่า FG/RTV) | เคยนับเป็น FG ไปแล้ว → **หักออกจากยอด FG** ของ Job Order นั้น |

- ขั้นตอนหลักทั้ง 3 (`ระหว่างกระบวนการผลิต`, `FG`, `RTV`) ลบออกจากรายการไม่ได้
- **Job Order เป็นช่องบังคับ** ทั้งฟอร์มบันทึกและฟอร์มแก้ไข ถ้าไม่ระบุ ยอดจะไม่ถูกนับเข้าจ๊อบใดเลย
