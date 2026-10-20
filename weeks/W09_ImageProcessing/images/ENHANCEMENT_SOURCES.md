# ภาพประกอบบทเรียน Enhancement

ไฟล์ PNG สามภาพนี้คัดลอกมาโดยไม่เปลี่ยนพิกเซลจาก sample data ของ
[scikit-image v0.25.2](https://github.com/scikit-image/scikit-image/tree/v0.25.2/skimage/data)
เพื่อให้ `week9.ipynb` ใช้ภาพในเครื่องได้โดยไม่ต้องติดตั้ง scikit-image หรือเชื่อมต่ออินเทอร์เน็ต

| ไฟล์ | ใช้สอน | แหล่งไฟล์ |
|---|---|---|
| `camera.png` | Brightness/Contrast, Negative และ Gamma | [PNG](https://raw.githubusercontent.com/scikit-image/scikit-image/v0.25.2/skimage/data/camera.png) |
| `coins.png` | Threshold และ Gray-level slicing | [PNG](https://raw.githubusercontent.com/scikit-image/scikit-image/v0.25.2/skimage/data/coins.png) |
| `moon.png` | Contrast stretching, Equalization และข้อจำกัดของ Min–max | [PNG](https://raw.githubusercontent.com/scikit-image/scikit-image/v0.25.2/skimage/data/moon.png) |

## เครดิตและข้อมูลสิทธิ์

อ้างอิงคำอธิบายใน [`_fetchers.py` ของเวอร์ชันเดียวกัน](https://github.com/scikit-image/scikit-image/blob/v0.25.2/skimage/data/_fetchers.py):

- **Camera:** ภาพโดย Lav Varshney เผยแพร่แบบ CC0 ภาพนี้เป็นฉบับที่ scikit-image ใช้ตั้งแต่ 0.18 ไม่ใช่ Cameraman รุ่นเก่าที่มีข้อจำกัดลิขสิทธิ์
- **Coins:** เหรียญกรีกจาก Pompeii แหล่งเดิม Brooklyn Museum Collection; scikit-image ระบุว่า “No known copyright restrictions” ไม่ได้ระบุว่าเป็น CC0
- **Moon:** scikit-image อธิบายว่าเป็นภาพ Low contrast สำหรับสาธิต Stretching/Equalization แต่ docstring ไม่ระบุช่างภาพหรือสิทธิ์เฉพาะภาพ จึงไม่อ้างว่าเป็น CC0 หากจะนำไปเผยแพร่ต่อในบริบทที่ต้องยืนยันสิทธิ์ ควรตรวจสอบเพิ่มเติมหรือเปลี่ยนเป็นภาพที่มีสิทธิ์ชัดเจน

## การจำลองภาพใน notebook

- ความมืด/สว่างและ Contrast หลายระดับของ Camera สร้างระหว่างรัน เพื่อเปรียบเทียบจากภาพตั้งต้นเดียวกัน
- ตัวอย่าง Gamma ใช้ Camera ที่จำลองให้มืดด้วยกำลัง 2 ไม่ใช่ภาพถ่ายแสงน้อยจริง
- `moon_low` คือ Moon ที่จำลองบีบช่วงเพิ่ม ส่วน `moon` คือภาพต้นฉบับที่อ่านจาก PNG
- ไม่มีการเขียนทับไฟล์ภาพต้นฉบับจากการทดลอง

## Prepared Camera Base

- `camera_demo_base.png` เป็นภาพจำลองที่สร้างจาก `camera.png` เพื่อใช้เป็น Input สำหรับการเปรียบเทียบ Brightness และ Contrast ใน `wee9_real.ipynb` ไม่ใช่ภาพ Original หรือผล Enhancement ที่แนะนำให้ใช้ทั่วไป
- สร้างด้วย `np.round(0.4 * (camera_float - camera_float.mean()) + 128).astype(np.uint8)` โดย `camera_float` คือภาพต้นฉบับที่แปลงเป็น `float32`
- ลดความต่างของค่ารอบค่าเฉลี่ยให้เหลือประมาณ 40% และย้ายค่าเฉลี่ยมาใกล้ 128 ได้ช่วงค่า 76 ถึง 178 เพื่อเหลือพื้นที่สำหรับการทดลองโดยไม่เกิด Clipping
- บันทึกเป็น PNG แบบ Lossless และให้ Notebook โหลดไฟล์โดยตรง นักศึกษาไม่ต้องคำนวณขั้นตอนเตรียมภาพนี้
- แหล่งภาพและเครดิตเหมือน `camera.png` ตามที่ระบุไว้ด้านบน

## Prepared Moon Base

- `moon_demo_base.png` สร้างจาก `moon.png` ด้วย `np.round(0.3 * moon.astype(np.float32) + 90).astype(np.uint8)` เพื่อเปรียบเทียบ Min-Max Stretching กับ Histogram Equalization จาก Input ช่วงค่าแคบเดียวกัน
- มีช่วงค่า 90 ถึง 166 เป็นภาพจำลองที่ลด Contrast และมีการรวมระดับความเข้มจากการปัดเศษ ไม่ใช่ Moon ต้นฉบับ และไม่ใช่การเพิ่มคุณภาพภาพ
- บันทึกเป็น PNG แบบ Lossless เพื่อให้ `wee9_real.ipynb` อ่านใช้โดยตรง ไม่ต้องคำนวณเตรียมภาพในคาบ
- แหล่งภาพและข้อจำกัดด้านสิทธิ์เหมือน `moon.png` ตามที่ระบุไว้ด้านบน
