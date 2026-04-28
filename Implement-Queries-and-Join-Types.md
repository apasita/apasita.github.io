---
title: "Lab 9.1.12 — Implement Queries and Join Types"
parent: Lab Reports
nav_order: 1
---

# 🗄️ Lab 9.1.12: Implement Queries and Join Types

[← กลับหน้าหลัก](README.md)

> **หลักสูตร:** Data+ (Exam DA0-002) · **Module:** 9 — Database Queries  
> **คะแนน:** ✅ 9/9 (100%)

---

## 📌 ภาพรวม

Lab นี้ใช้ข้อมูลของ **นักเรียน** ประกอบด้วยตาราง `Students`, `Enrollment` และกิจกรรมนอกหลักสูตร (Extracurriculars) โดยฝึกเขียน SQL Query เพื่อดึงข้อมูลจากหลายตารางพร้อมกัน ผ่านการใช้ **JOIN** 4 ประเภท บนโปรแกรม **SSMS** (SQL Server Management Studio)

![SSMS Overview](images/sql-ssms-overview.png)
*หน้าต่าง SSMS แสดง Object Explorer และตาราง Students, Enrollment*

---

## 🗂️ โครงสร้างข้อมูลที่ใช้

ก่อนจะเขียน JOIN ต้องเข้าใจก่อนว่าตารางในฐานข้อมูลนี้เก็บอะไรไว้

| ตาราง | เก็บข้อมูลอะไร | Primary Key |
|---|---|---|
| `Students` | ข้อมูลนักเรียน เช่น ชื่อ อายุ | `StudentID` |
| `Enrollment` | ข้อมูลการลงทะเบียนวิชา | `EnrollmentID` |
| `Extracurriculars` | กิจกรรมนอกหลักสูตร | `ActivityID` |

![Table Structure](images/sql-table-structure.png)
*โครงสร้างตาราง Students และ Enrollment แสดง columns และ data types*

---

## 🔗 JOIN คืออะไร และทำไมต้องใช้?

ในโลกของฐานข้อมูลจริง ข้อมูลไม่ได้อยู่ในตารางเดียว เช่น ตาราง `Students` เก็บข้อมูลนักเรียน แต่ตาราง `Enrollment` เก็บข้อมูลการลงทะเบียนแยกกัน เราต้องใช้ JOIN เพื่อ **เชื่อมสองตารางเข้าด้วยกัน** ผ่าน key ที่ตรงกัน (`StudentID`)

```
Students Table          Enrollment Table
┌────────────┐          ┌────────────────┐
│ StudentID  │──────────│ StudentID (FK) │
│ Name       │          │ EnrollmentDate │
│ Age        │          │ SubjectID      │
└────────────┘          └────────────────┘
         ↑ JOIN เชื่อมผ่าน StudentID ↑
```

---

## 📊 JOIN 4 ประเภทที่ใช้ใน Lab

### 1. 🟢 INNER JOIN — เฉพาะข้อมูลที่ match กัน

คืน **เฉพาะแถวที่มีคู่ตรงกัน** ในทั้งสองตาราง ถ้าฝั่งใดไม่มีข้อมูล แถวนั้นจะถูกตัดออกทันที

```sql
SELECT * FROM Students
INNER JOIN Enrollment 
    ON Students.StudentID = Enrollment.StudentID
```

> 💡 **เหมาะกับ:** เมื่อต้องการเฉพาะข้อมูลที่ครบสมบูรณ์ทั้งคู่ เช่น นักเรียนที่ลงทะเบียนแล้วเท่านั้น

![INNER JOIN Result](images/sql-inner-join-result.png)

---

### 2. 🔵 LEFT OUTER JOIN — ทุกแถวจากตารางซ้าย

คืน **ทุกแถวจากตารางซ้าย (Students)** แม้ตารางขวาจะไม่มีคู่ จะแสดงค่า `NULL` ในส่วนที่ขาดหายไป

```sql
SELECT * FROM Students
LEFT OUTER JOIN Enrollment 
    ON Students.StudentID = Enrollment.StudentID
```

> 💡 **เหมาะกับ:** ต้องการดูนักเรียน **ทุกคน** รวมถึงคนที่ยังไม่ได้ลงทะเบียน (EnrollmentDate = NULL)

![RIGHT OUTER JOIN Result](images/sql-left-join-result.png)

---

### 3. 🟠 RIGHT OUTER JOIN — ทุกแถวจากตารางขวา (60 แถว)

คืน **ทุกแถวจากตารางขวา (Enrollment)** ใน Lab นี้ได้ผลลัพธ์ **60 แถว**

```sql
SELECT * FROM Students
RIGHT OUTER JOIN Enrollment 
    ON Students.StudentID = Enrollment.StudentID
```

> 💡 **เหมาะกับ:** ต้องการดูการลงทะเบียน **ทุกรายการ** รวมถึงรายการที่ไม่มีข้อมูลนักเรียนตรงกัน

![RIGHT OUTER JOIN Result](images/sql-right-join-result.png)

---

### 4. 🟣 FULL OUTER JOIN — ทุกแถวจากทั้ง 2 ตาราง (561 แถว)

รวม **ทุกแถวจากทั้งสองตาราง** ไม่ว่าจะมีคู่หรือไม่ ได้ผลลัพธ์ **561 แถว**

```sql
SELECT * FROM Students
FULL OUTER JOIN Enrollment 
    ON Students.StudentID = Enrollment.StudentID
```

> 💡 **เหมาะกับ:** ต้องการเห็นภาพรวมทุกอย่าง ทั้งที่ match และไม่ match

![FULL OUTER JOIN Result](images/sql-full-outer-join-result.png)
*ผลลัพธ์ FULL OUTER JOIN — 561 แถว รวมทั้งแถวที่ไม่มีคู่ซึ่งมีค่า NULL*

---

## 📊 เปรียบเทียบ JOIN ทั้ง 4 แบบ

| JOIN Type | จำนวนแถวที่ได้ | ข้อมูลที่ได้ |
|---|---|---|
| INNER JOIN | น้อยที่สุด | เฉพาะแถวที่ match ทั้งคู่ |
| LEFT OUTER JOIN | ปานกลาง | ทุกแถวจาก Students + match จาก Enrollment |
| RIGHT OUTER JOIN | **60 แถว** | ทุกแถวจาก Enrollment + match จาก Students |
| FULL OUTER JOIN | **561 แถว** | ทุกแถวจากทั้ง 2 ตาราง |

---

## 🔍 สิ่งที่ค้นพบจากข้อมูล

| คำถาม | คำตอบ |
|---|---|
| RIGHT OUTER JOIN คืนกี่แถว? | **60 แถว** |
| FULL OUTER JOIN คืนกี่แถว? | **561 แถว** |
| มี NULL ใน `Student_ID` ไหม? | **ไม่มี** — Primary Key ครบทุกแถว |
| `EnrollmentDate`, `EndSchoolYear`, `SchYrGrade` มี NULL ไหม? | **มีทั้ง null และ non-null** — ข้อมูลบางรายการยังไม่ครบ |
| Inner และ Outer JOIN ให้ผลเหมือนกันไหม? | **ไม่** — ต่างกันเสมอเมื่อข้อมูลไม่ match ทั้งหมด |

---

## 👁️ Views: บันทึก Query ที่ใช้บ่อย

นอกจาก JOIN แล้ว Lab นี้ยังสอนการสร้าง **VIEW** ซึ่งคือการบันทึก Query ที่ซับซ้อนไว้ใช้ชื่อเดียวเรียกซ้ำได้เลย

```sql
-- สร้าง VIEW สำหรับข้อมูลนักเรียนที่ลงทะเบียนแล้ว
CREATE VIEW vw_EnrolledStudents AS
SELECT Students.StudentID, Students.Name, Enrollment.EnrollmentDate
FROM Students
INNER JOIN Enrollment ON Students.StudentID = Enrollment.StudentID

-- เรียกใช้ VIEW
SELECT * FROM vw_EnrolledStudents
```

**ประโยชน์ของ VIEW:**

- ✅ ใช้เมื่อ Query นั้นจะถูกเรียกซ้ำบ่อย ๆ
- ✅ ลดการพิมพ์ซ้ำและลดโอกาสผิดพลาด
- ✅ ดึงข้อมูลจาก VIEW ได้หลายวิธี ไม่จำกัดแค่ `SELECT TOP 100`
- ✅ Query text ใน SSMS สามารถ copy, paste, และนำกลับมาแก้ไขใช้ใหม่ได้

![VIEW Creation](images/sql-view-creation.png)
![VIEW Creation2](images/sql-view-creation2.png)
*การสร้าง VIEW ใน SSMS และผลลัพธ์จากการเรียกใช้งาน*

---

## 📝 สรุปบทเรียน

> การเลือก JOIN ที่ถูกต้องส่งผลโดยตรงต่อ **ความครบถ้วนของข้อมูล**  
> FULL OUTER JOIN (561 แถว) ให้ข้อมูลมากกว่า RIGHT OUTER JOIN (60 แถว) อย่างมีนัยสำคัญ  
> เพราะรวมข้อมูลจากทั้งสองตาราง รวมถึงส่วนที่ไม่มีคู่ตรงกันด้วย

**Key Takeaways:**
- 🔑 JOIN เชื่อมตารางผ่าน **Foreign Key** ที่มีค่าตรงกัน
- 🔑 เลือก JOIN type ให้ตรงกับว่าต้องการ "ทุกแถว" จากฝั่งไหน
- 🔑 NULL ใน result = ข้อมูลไม่มีคู่ในอีกตาราง ไม่ใช่ error
- 🔑 VIEW ช่วยประหยัดเวลาเมื่อต้องใช้ Query ซ้ำบ่อย ๆ

---

[← กลับหน้าหลัก](README.md) · [→ ดู Lab 14.1.16](Build-Visuals-to-Make-an-Impact.md)
