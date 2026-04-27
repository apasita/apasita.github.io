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

Lab นี้ใช้ข้อมูลของ **นักเรียน** ประกอบด้วย assignments และกิจกรรมนอกหลักสูตร (extracurriculars)
โดยฝึกเขียน SQL Query เพื่อดึงข้อมูลจากหลายตารางพร้อมกัน ผ่านการใช้ **JOIN** ประเภทต่าง ๆ
บนโปรแกรม **SSMS** (SQL Server Management Studio)

---

## 🔗 JOIN คืออะไร และทำไมต้องใช้?

ในโลกของฐานข้อมูลจริง ข้อมูลไม่ได้อยู่ในตารางเดียว เช่น ตาราง `Students` เก็บข้อมูลนักเรียน
แต่ตาราง `Enrollment` เก็บข้อมูลการลงทะเบียนแยกกัน เราต้องใช้ JOIN เพื่อ **เชื่อมสองตารางเข้าด้วยกัน**
และดูภาพรวมว่านักเรียนคนไหนลงทะเบียนวิชาอะไร

---

## 📊 JOIN 4 ประเภทที่ใช้ใน Lab

### 1. INNER JOIN
คืน **เฉพาะแถวที่มีคู่ตรงกัน** ในทั้งสองตาราง ถ้าฝั่งใดไม่มีข้อมูล แถวนั้นจะถูกตัดออกทันที

```sql
SELECT * FROM Students
INNER JOIN Enrollment ON Students.StudentID = Enrollment.StudentID
```

### 2. LEFT OUTER JOIN
คืน **ทุกแถวจากตารางซ้าย** แม้ตารางขวาจะไม่มีคู่ จะแสดงค่า `NULL` แทนในส่วนที่ขาดหายไป

```sql
SELECT * FROM Students
LEFT OUTER JOIN Enrollment ON Students.StudentID = Enrollment.StudentID
```

### 3. RIGHT OUTER JOIN
คืน **ทุกแถวจากตารางขวา** — ใน Lab นี้ได้ผลลัพธ์ **60 แถว**

```sql
SELECT * FROM Students
RIGHT OUTER JOIN Enrollment ON Students.StudentID = Enrollment.StudentID
```

### 4. FULL OUTER JOIN
รวม **ทุกแถวจากทั้งสองตาราง** ไม่ว่าจะมีคู่หรือไม่ — ได้ผลลัพธ์ **561 แถว**

```sql
SELECT * FROM Students
FULL OUTER JOIN Enrollment ON Students.StudentID = Enrollment.StudentID
```

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

## 💡 Views: บันทึก Query ที่ใช้บ่อย

นอกจาก JOIN แล้ว Lab นี้ยังสอนการสร้าง **VIEW** ซึ่งคือการบันทึก Query ที่ซับซ้อนไว้ในชื่อเดียว

**ประโยชน์ของ VIEW:**
- ✅ ใช้เมื่อ Query นั้นจะถูกเรียกซ้ำบ่อย ๆ
- ✅ ลดการพิมพ์ซ้ำและลดโอกาสผิดพลาด
- ✅ ดึงข้อมูลจาก VIEW ได้หลายวิธี ไม่จำกัดแค่ `SELECT TOP 100`
- ✅ Query text ใน SSMS สามารถ copy, paste, และนำกลับมาแก้ไขใช้ใหม่ได้

---

## 📝 สรุปบทเรียน

> การเลือก JOIN ที่ถูกต้องส่งผลโดยตรงต่อ **ความครบถ้วนของข้อมูล**  
> FULL OUTER JOIN (561 แถว) ให้ข้อมูลมากกว่า RIGHT OUTER JOIN (60 แถว) อย่างมีนัยสำคัญ  
> เพราะรวมข้อมูลจากทั้งสองตาราง รวมถึงส่วนที่ไม่มีคู่ตรงกันด้วย

| JOIN Type | ผลลัพธ์ | ใช้เมื่อ |
|---|---|---|
| INNER JOIN | น้อยที่สุด | ต้องการเฉพาะข้อมูลที่ครบทั้งคู่ |
| LEFT / RIGHT JOIN | ปานกลาง | ต้องการข้อมูลจากตารางหลักครบ |
| FULL OUTER JOIN | มากที่สุด | ต้องการเห็นข้อมูลทั้งหมดรวมส่วนที่ขาด |

---

[← กลับหน้าหลัก](README.md) · [→ ดู Lab 14.1.16](lab-14-1-16.md)
