# ภาพประกอบโครงสร้าง CNN

## ภาพที่ใช้ใน Notebook

`food_cnn_architecture.png` และ `food_cnn_architecture.svg` วาดขึ้นสำหรับบทนี้
ให้ตรงกับ `SimpleCNN` ไม่ใช่ VGG-16 หรือภาพ CNN ทั่วไป:

`3×64×64 → Conv16 → ReLU → Pool → Conv32 → ReLU → Pool → Flatten8192 → Linear128 → ReLU → Linear3`

ส่วน MLP คือ Hidden Layer 128 หน่วย → ReLU → Output 3 Logits

- PNG ขนาด 2920×1330 ใช้แสดงใน Notebook แบบ Offline
- SVG เป็นภาพเวกเตอร์ที่แก้ข้อความ/รูปร่างได้ และฝังภาพ Input ไว้ในไฟล์แล้ว
- สคริปต์วาด: `scripts/draw_food_cnn.py` (สำหรับผู้สอน ไม่ต้องรันเพื่อเรียน)
- สร้างภาพใหม่จากราก Repository: `.venv/bin/python scripts/draw_food_cnn.py`
- ใช้หัวข้อภาษาอังกฤษในภาพ และคำอธิบายภาษาไทยใน Notebook
- ภาพพิซซ่าจริงมาจาก `../Dataset/train/pizza/1001116.jpg` ในชุด Food-101
  ดูแหล่งข้อมูลและข้อจำกัดสิทธิ์ภาพใน [Dataset/README.md](../Dataset/README.md)
  การวาดผังใหม่ไม่ได้เปลี่ยนสิทธิ์ของภาพถ่ายต้นฉบับ
- ภาพ Input เป็น Preview ของภาพจริง ขนาด Tensor หลัง Resize/Normalize คือ 3×64×64
- ลวดลาย Feature Maps สร้างจากภาพขอบเพื่อสื่อแนวคิด ไม่ใช่ Activations จากโมเดลที่ฝึก
  จำนวนแผ่นและจำนวน Neurons ที่วาดเป็นสัญลักษณ์ ให้อ่านจำนวนจริงจากตัวเลขกำกับ
- ไม่แสดงค่าทำนายหรือเปอร์เซ็นต์สมมุติ; Output แสดง 3 Logits ตามโมเดลจริง

## ภาพทั่วไปที่เก็บไว้จากฉบับก่อน

`typical_cnn.png` เป็นภาพ **Typical cnn** โดย **Aphex34** (16 December 2015)

- หน้าต้นฉบับ: https://commons.wikimedia.org/wiki/File:Typical_cnn.png
- ไฟล์ต้นฉบับ: https://upload.wikimedia.org/wikipedia/commons/6/63/Typical_cnn.png
- สัญญาอนุญาต: [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
- เก็บภาพเดิมโดยไม่ดัดแปลง เพื่อใช้ประกอบบทเรียนแบบ Offline

ภาพแสดง CNN ทั่วไป ไม่ใช่จำนวน Filters หรือจำนวนคลาสของโมเดลใน Notebook
เก็บไว้พร้อมเครดิตเดิม แต่ Notebook ปัจจุบันใช้ `food_cnn_architecture.png` แทน
