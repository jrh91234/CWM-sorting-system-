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
