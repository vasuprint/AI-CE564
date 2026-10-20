# Week 9: Image Processing and Enhancement

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/vasuprint/AI-CE564/blob/main/weeks/W09_ImageProcessing/week9_colab.ipynb)

เปิด [week9_colab.ipynb](week9_colab.ipynb) ด้วยปุ่มด้านบน แล้วรัน Setup ก่อน ตัวอย่างจะดาวน์โหลดภาพจาก GitHub โดยอัตโนมัติ ไม่ต้องอัปโหลดรูปเองหรือ Mount Google Drive

- ใช้ CPU Runtime ปกติ ไม่จำเป็นต้องใช้ GPU
- เลือก **File → Save a copy in Drive** เพื่อเก็บสำเนาที่แก้ไขได้
- เลือก **Runtime → Run all** หรือรันตามลำดับ สำเนานี้ไม่ได้บันทึก Output เพื่อให้ไฟล์มีขนาดเล็ก
- เมื่อ Runtime ถูกล้าง ให้รัน Setup อีกครั้งเพื่อดาวน์โหลดภาพใหม่
- ภาพประกอบใน Markdown ใช้ GitHub URLs จึงต้องมีอินเทอร์เน็ต

เนื้อหาครอบคลุม RGB/Grayscale, Thresholding, Brightness/Contrast, Intensity Transformations, Stretching/Equalization, Spatial Kernels, Random Feature Maps และ Image Operations

## Image credits

ภาพและข้อมูลเตรียมภาพอยู่ใน `images/` พร้อมบันทึกแหล่งที่มา:

- [Enhancement sources](images/ENHANCEMENT_SOURCES.md)
- [Filter sources](images/FILTER_SOURCES.md)
- [Image operations and vintage paper attribution](images/OPERATIONS_SOURCES.md)

Texture กระดาษเก่าโดย **D Sharon Pruitt / Pink Sherbet Photography**, [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Free_dark_vintage_paper_page_texture_for_layers_(2982207584).jpg), [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/). ย่อภาพและใช้ผสมกับภาพบุคคลเพื่อสาธิต คงเครดิตนี้ไว้เมื่อนำผลไปเผยแพร่

รูปสไลด์ประกอบบางภาพเป็นไฟล์ที่ผู้สอนเตรียมไว้และอาจมีสิทธิ์ของเจ้าของเดิม การอยู่ใน Repository นี้ไม่ได้หมายความว่าทุกรูปเป็น Public Domain โปรดตรวจสิทธิ์ก่อนนำไปใช้ต่อ
