# Transfer Learning และการเปรียบเทียบกับ Custom CNN

- เครื่องตนเอง: [CE564_TransferLearning_ONNX.ipynb](CE564_TransferLearning_ONNX.ipynb)
- Google Colab: [CE564_TransferLearning_ONNX_Colab.ipynb](CE564_TransferLearning_ONNX_Colab.ipynb) ดาวน์โหลด ZIP จาก GitHub ผ่าน Download Cell ดู [วิธีใช้ Colab](README_COLAB.md)

## ลำดับใน Notebook

| Model | Pretraining | Parameters ที่ฝึก | Dense Head |
|---|---|---|---|
| ResNet18 Transfer Learning | ImageNet | เฉพาะ Head | 512 → 128 → ReLU → 3 |
| EfficientNet-B0 Transfer Learning | ImageNet | เฉพาะ Head | 1280 → 128 → ReLU → 3 |
| MobileNetV3-Small Transfer Learning | ImageNet | เฉพาะ Head | 576 → 128 → ReLU → 3 |
| Custom CNN (From Scratch) | Random Initialization | ทุก Layer | 8192 → 128 → ReLU → 3 |

แต่ละโมเดลมีเซลล์สร้างโมเดล, DataLoader และ Train/Validation **แยกของตนเอง** ไม่มี Loop ครอบโมเดล ไม่มี `fit()` หรือฟังก์ชันซ่อนการฝึก เหลือ Loop Epoch/Batch และ Loop Freeze Parameters ตามปกติ

Custom CNN ใช้ Architecture และ Preprocessing จากบท CNN เดิม แต่ฝึกใหม่ในเล่มนี้ จึงไม่ต้องคัดลอกคะแนนหรือโหลดผลเก่าที่อาจใช้ค่าคนละชุด Notebook CNN เดิมไม่ถูกแก้ไข

## วิธีใช้

```bash
uv sync
uv run jupyter lab --notebook-dir=weeks/W10_CNN_TransferLearning
```

ใช้ Kernel `.venv` เปิดจากโฟลเดอร์ที่มี `Dataset/` และรันตามลำดับ ต้องใช้อินเทอร์เน็ตโหลด Pretrained ครั้งแรก Colab ให้เลือก T4 GPU แล้วรัน Download Cell เพื่อดาวน์โหลด Dataset จาก GitHub

- ใช้สมาชิก Split เดิม Train 360 / Valid 90 / Test 150 ไม่แบ่งใหม่
- ฝึก **10 Epochs ต่อตัว**, Adam LR 0.001, Batch 32 และเก็บ Best Weights ตาม Validation Loss ต่ำสุด
- Pretrained Models ใช้ `weights.transforms()` ภาพ 224×224; Custom CNN ใช้ภาพ 64×64 และ Augmentation เดิม
- Pretrained Models คง Backbone ใน `eval()` แล้วเปิด Head ใน `train()` เพื่อไม่เปลี่ยน BatchNorm Statistics
- ท้ายบทมีตาราง Validation ครบ 4 โมเดล จากนั้นเลือก Transfer Learning ที่ Val Loss ต่ำสุดเพื่อ Deploy
- Final Test เปรียบเทียบ Transfer Learning ที่เลือกกับ CNN เดิม ไม่ใช้ Test เลือกหรือปรับโมเดล **Run All จะเปิด Test ด้วย**
- การเทียบนี้ต่างทั้งสถาปัตยกรรม Input/Augmentation และข้อมูล ImageNet จึงไม่ใช่ผลของ Transfer Learning เพียงตัวแปรเดียว

ถ้าต้องการเริ่มฝึกใหม่ ให้รันเซลล์สร้างโมเดล/Head ของตัวนั้นก่อน ไม่รันเฉพาะ Train ต่อจากน้ำหนักเดิม

## ONNX

หลังรัน ผลอยู่ใน `transfer_outputs/`:

- `food_transfer_weights.pt` — Best Weights ของ Transfer Learning ที่เลือก
- **`food_transfer.onnx` + `food_transfer_config.json`** — ต้องเก็บคู่กันตอนนำไปใช้
- `validation_comparison.json` — ผล Validation ทั้ง 4 โมเดล
- `test_comparison.json` — Final Test ของ Transfer Learning ที่เลือกกับ CNN เดิม

ONNX รับ float32 `[1, 3, 224, 224]` ทีละภาพ เซลล์ Inference ใช้เพียง `onnxruntime`, `numpy`, `Pillow` ไม่ใช้ PyTorch และมีเซลล์ตรวจผลเทียบ PyTorch ต่อท้าย แก้ `IMAGE_PATH` เพื่อใช้ภาพ JPEG/PNG ของตนเอง

ในโปรแกรม Python ทั่วไปเปลี่ยน `display(image)` เป็น `image.show()` หรือเอาบรรทัดนี้ออก โมเดลจะเลือกหนึ่งในสามคลาสเสมอแม้ภาพไม่ใช่อาหาร ตัวอย่างนี้ไม่ใช่ Production API รันซ้ำเขียนทับไฟล์ชื่อเดิม

## สำหรับผู้สอน

```bash
uv run python scripts/build_transfer_learning_notebook.py
uv run python scripts/test_transfer_learning_notebook.py --epochs 1 --cpu
uv run python scripts/test_transfer_learning_notebook.py --epochs 1 --cpu --colab
```

Build เขียนทับเฉพาะ Notebook Transfer Learning สองเล่ม ไม่แก้บท CNN เดิม หากแก้ `.ipynb` ด้วยมือ ให้ตรวจความต่างก่อน Build

Test ใช้สำเนาชั่วคราวกับภาพจริงครบทุก Split ตรวจการฝึกทั้งสี่ Models แยกกัน, Frozen Parameters/BatchNorm, Best Weights, ตารางเปรียบเทียบ และ Export/ONNX Runtime ครบทั้งสาม Pretrained Models รวมผลเทียบ PyTorch บนภาพ Validation 90 ภาพ และ Process ทำนายที่บล็อก Import PyTorch

`--epochs 1` เป็นการตรวจ Pipeline ไม่ใช่ผลหลังฝึกครบ 10 Epochs; `--colab` รันแกนโค้ดในเครื่อง ไม่ทดสอบหน้า Upload/Download หรือ Colab/GPU จริง
