# Social Media Engagement Dashboard
### Power BI Mini Project

แดชบอร์ดวิเคราะห์ engagement ของโพสต์โซเชียลมีเดีย 1,000 โพสต์ จาก 5 แพลตฟอร์ม เพื่อหาว่าแพลตฟอร์ม ประเภทเนื้อหา และช่วงเวลาโพสต์แบบไหนให้ engagement สูงสุด — สร้างด้วย Power BI Desktop ต่อยอดแนวคิดจากโปรเจกต์ [Social Media Engagement Analysis](https://github.com/pawitra-thongma/social-media-analysis) ที่เคยวิเคราะห์ด้วย Python และ SQL มาก่อน

![Dashboard Preview](images/dashboard_preview.png)

---

## โจทย์

ต้องการทราบว่าควรโพสต์เนื้อหาแบบไหน บนแพลตฟอร์มไหน และช่วงเวลาใด เพื่อให้ได้ engagement สูงสุด โปรเจกต์นี้จึงสร้าง Dashboard เพื่อสรุปภาพรวมและตอบคำถามนี้แบบ interactive

## ข้อมูลที่ใช้

ชุดข้อมูล synthetic จำนวน 1,000 แถว (`social_media_engagement.csv`) สร้างขึ้นโดยอ้างอิงแนวโน้มจากงานวิจัยตลาดจริง (Buffer & Rival IQ Benchmarks) ประกอบด้วยคอลัมน์:
- `platform`, `content_type`, `post_date`, `post_hour`, `day_of_week`
- `hashtag_count`, `impressions`, `likes`, `comments`, `shares`, `engagement_rate`

## ขั้นตอนการทำงาน

**1. Power Query — Data Transformation**
- แปลงชนิดข้อมูลคอลัมน์วันที่ พร้อมระบุ Locale ให้ชัดเจน เพื่อแก้ปัญหา date parsing error ที่เกิดจาก locale ของเครื่องตีความรูปแบบวันที่ต่างกัน
- สร้างคอลัมน์ใหม่ `Total Engagement` และ `Time Slot` ด้วย Custom Column

**2. DAX — Measures**
```dax
Avg Engagement Rate = AVERAGE(social_media_engagement[engagement_rate])
Total Posts = COUNT(social_media_engagement[post_id])
Best Platform Engagement = 
    CALCULATE(
        AVERAGE(social_media_engagement[engagement_rate]),
        social_media_engagement[platform] = "LinkedIn"
    )
```

**3. Dashboard Design**
- Bar chart เปรียบเทียบ engagement rate ตาม platform และ content type
- Matrix แสดง engagement rate แยกตามวันในสัปดาห์ × ช่วงเวลา
- Card สรุปภาพรวม + Slicer สำหรับกรองข้อมูลตาม platform

## Insight ที่ได้

| มิติ | ผลลัพธ์ |
|---|---|
| แพลตฟอร์ม | **LinkedIn** engagement สูงสุด (10.8%) มากกว่า Twitter ที่ต่ำสุด (0.2%) ราว 50 เท่า |
| ประเภทเนื้อหา | **Video** engagement สูงสุด (9.5%) มากกว่า Text เกือบ 3 เท่า |
| ช่วงเวลา | โพสต์ช่วง **18:00–21:00 วันอังคาร–พฤหัสบดี** ให้ engagement สูงกว่าช่วงอื่น |
| ภาพรวม | ค่าเฉลี่ย engagement rate ทั้งหมด **6.38%** จาก 1,000 โพสต์ |

**ข้อเสนอแนะเชิงธุรกิจ:** เน้นโพสต์ Video บน LinkedIn ในช่วงเย็นวันอังคาร-พฤหัสบดี เพื่อให้ได้ engagement สูงสุด

## เครื่องมือที่ใช้

Power BI Desktop · Power Query · DAX

## ไฟล์ในโปรเจกต์

- `social_media_engagement_dashboard.pbix` — ไฟล์ Power BI ต้นฉบับ
- `social_media_engagement.csv` — ชุดข้อมูลที่ใช้
- `images/` — ภาพหน้าจอ Dashboard

---
*โปรเจกต์ส่วนตัวเพื่อฝึกฝนทักษะ Power BI*
