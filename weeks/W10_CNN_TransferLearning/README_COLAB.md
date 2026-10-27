# Week 10 — ใช้บทเรียนบน Google Colab

ไฟล์สำหรับแจกนักศึกษา:

1. `CE564_CNN_Colab.ipynb` — Notebook แยกจากฉบับเครื่อง ล้าง Output แล้ว
2. `food_pizza_steak_sushi_colab.zip` — รูป 600 ภาพพร้อมโฟลเดอร์ Train/Valid/Test และข้อมูลแหล่งที่มา
3. `CE564_TransferLearning_ONNX_Colab.ipynb` — แยก ResNet18, EfficientNet-B0, MobileNetV3-Small และ Custom CNN (From Scratch) พร้อมตารางเปรียบเทียบและ ONNX Runtime ใช้ ZIP ชุดเดียวกัน ดู [รายละเอียดบท Transfer Learning](README_TRANSFER_LEARNING.md)

ZIP ขนาด 31,537,138 ไบต์ (ประมาณ 31.5 MB)
SHA-256: `8da3c2870ddff28cafcca222939bdbb1f909506c6e8f6833386e01eaef53b431`

[ดาวน์โหลด Dataset ZIP จาก GitHub](https://raw.githubusercontent.com/vasuprint/AI-CE564/main/weeks/W10_CNN_TransferLearning/food_pizza_steak_sushi_colab.zip)

เปิด Notebook บน Colab โดยตรง:
- [CNN](https://colab.research.google.com/github/vasuprint/AI-CE564/blob/main/weeks/W10_CNN_TransferLearning/CE564_CNN_Colab.ipynb)
- [Transfer Learning และ ONNX](https://colab.research.google.com/github/vasuprint/AI-CE564/blob/main/weeks/W10_CNN_TransferLearning/CE564_TransferLearning_ONNX_Colab.ipynb)

## วิธีเริ่ม

1. เปิด https://colab.research.google.com/ แล้วเลือก **Upload notebook** เลือก `CE564_CNN_Colab.ipynb`
2. เลือก **Runtime → Change runtime type → T4 GPU** หากไม่มี GPU ใช้ CPU ได้
3. รันเซลล์ติดตั้งไลบรารี ไม่ต้องติดตั้ง PyTorch/CUDA ใหม่
4. รันเซลล์ Download Dataset เพื่อดาวน์โหลด ZIP จาก GitHub ตรวจ SHA-256 และแตกไฟล์ ไม่ต้อง Upload Dataset จากเครื่อง
5. รันบทเรียนต่อจากบนลงล่าง ใช้ `ImageFolder('Dataset/train')` ตามบทเดิมได้เลย

รูปโครงสร้างใน Markdown ใช้ URL ภาพจาก GitHub โดยตรง ไม่ต้องอัปโหลด `assets/`
ไม่ต้องใช้ไฟล์ `.py` ประกอบ ไม่ต้อง Mount Drive หรือแบ่งข้อมูลใหม่ใน Colab

ภาพที่ใช้: https://raw.githubusercontent.com/vasuprint/AI-CE564/main/weeks/W10_CNN_TransferLearning/assets/food_cnn_architecture.png

## ZIP มีอะไรบ้าง

```text
Dataset/
├── README.md
├── manifest.json
├── train/{pizza,steak,sushi}/   # รวม 360 ภาพ
├── valid/{pizza,steak,sushi}/   # รวม 90 ภาพ
└── test/{pizza,steak,sushi}/    # รวม 150 ภาพ
```

ไม่มีน้ำหนักโมเดล ผลฝึก หรือไฟล์แฝงจากเครื่องผู้สอน รูปแต่ละภาพอยู่เพียงชุดเดียว
สมาชิก Split และไบต์ของภาพเหมือนฉบับเครื่อง ดูที่มา/ข้อจำกัดสิทธิ์ภาพใน `Dataset/README.md`
ZIP เผยแพร่ในโฟลเดอร์ Week 10 บน GitHub แบบ Public ตามคำขอผู้สอน สิทธิ์ภาพยังเป็นของเจ้าของเดิมตามเอกสารแหล่งที่มา

## Gradio และการเก็บผล

- Gradio ติดตั้งพร้อมเครื่องมืออื่นแล้ว รัน Load CNN ก่อน และตั้ง `RUN_GRADIO = True` เมื่อต้องการลอง
- Colab ใช้ `share=True` จึงสร้างลิงก์สาธารณะชั่วคราว ใครมีลิงก์อาจใช้ Demo ได้ หลีกเลี่ยงภาพ/ข้อมูลลับ
- ค่าเริ่มต้นปิด Gradio ไว้ ไม่เปิดลิงก์เมื่อ Run All กด Stop และปิด Demo เมื่อเลิกใช้
- ผลใน `/content/cnn_outputs` หายเมื่อ Runtime ถูกล้าง ตั้ง `DOWNLOAD_RESULTS = True` ในเซลล์ท้ายบทเพื่อดาวน์โหลด ZIP ผลกลับเครื่อง

## สำหรับผู้สอน

สร้างจาก Notebook ฉบับเครื่อง **ปัจจุบัน** โดยไม่ Build ฉบับเครื่องทับ:

```bash
uv run python scripts/build_cnn_colab.py
```

สคริปต์ไม่เปลี่ยนฉบับเครื่อง แต่จะเขียนทับเฉพาะ `CE564_CNN_Colab.ipynb` และ ZIP ชื่อข้างต้น
หลังแก้ Notebook ฉบับเครื่องให้สร้าง Colab ใหม่ แล้วอัปโหลดไฟล์ Colab ที่ปรับแล้วเพื่อเผยแพร่

ทดสอบโครงสร้างไฟล์/ZIP และรันบทเรียนบน Kernel ในเครื่อง (ข้าม UI เฉพาะของ Colab):

```bash
uv run python scripts/test_cnn_colab.py --execute
```

การทดสอบนี้รัน CPU 2 Epochs ในสำเนาชั่วคราว ค่าฝึกในฉบับแจกยังเป็น 10 Epochs
ไม่ได้ทดสอบ Gradio หรือ Runtime บน Colab จริง ส่วน Download Cell ตรวจแยกด้วยการดาวน์โหลดจาก GitHub และตรวจ Hash/สมาชิก ZIP

## บท Transfer Learning → ONNX บน Colab

เปิด `CE564_TransferLearning_ONNX_Colab.ipynb` แทนฉบับ CNN แล้วเลือก T4 GPU รันเซลล์ติดตั้งและ Download Dataset ตามลำดับ ไม่ต้องติดตั้ง PyTorch/CUDA ทับของ Colab

- ดาวน์โหลด Pretrained Weights ครั้งแรก: ResNet18, EfficientNet-B0, MobileNetV3-Small
- แยกเซลล์สร้าง/ฝึกตามชื่อ Architecture ฝึกเฉพาะ Dense Head ของ Pretrained Models 10 Epochs ต่อตัว และฝึก Custom CNN เพื่อเทียบในตารางท้ายบท
- เลือก Transfer Learning ด้วย Validation Loss ก่อน Final Test เทียบกับ CNN เดิม
- Export โมเดลที่เลือก แล้วเรียก ONNX Runtime บน CPU ใน Runtime เดียวกัน พร้อมตรวจผลเทียบ PyTorch
- ตั้ง `DOWNLOAD_RESULTS = True` ในเซลล์ท้ายบทเพื่อเก็บ ZIP จาก `/content/transfer_outputs` ก่อน Runtime ถูกล้าง
- สำหรับนำไปทำนายเครื่องอื่น ต้องเก็บ `food_transfer.onnx` และ `food_transfer_config.json` คู่กัน

สร้าง Notebook ทั้งฉบับเครื่องและ Colab ด้วย `uv run python scripts/build_transfer_learning_notebook.py` คำสั่งนี้ไม่แก้ Notebook CNN และไม่สร้าง ZIP ข้อมูลใหม่ แต่เขียนทับ Notebook Transfer Learning ทั้งสองไฟล์ จึงไม่ควรรันทับฉบับที่แก้ด้วยมือโดยไม่ตรวจความต่าง
