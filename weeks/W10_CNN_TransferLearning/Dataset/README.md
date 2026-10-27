# Food-101 Subset: Pizza, Steak, Sushi (20% Dataset)

ชุดข้อมูลภาพถ่ายอาหารจริง 3 คลาส (**Pizza**, **Steak**, **Sushi**) สำหรับประกอบการเรียนการสอนสัปดาห์ที่ 10 (CE564 / CE557: Convolutional Neural Networks & Transfer Learning)

---

## 1. ข้อมูลแหล่งที่มาและประวัติ (Provenance)

- **ชุดข้อมูลต้นทาง:** [Food-101](https://data.vision.ee.ethz.ch/cvl/datasets_extra/food-101/) (Bossard et al., ECCV 2014)
- **แหล่งภาพต้นฉบับ:** รวบรวมจากผู้ใช้บนแพลตฟอร์ม *Foodspotting*
- **ผู้จัดทำ Subset:** Daniel Bourke ([mrdbourke/pytorch-deep-learning](https://github.com/mrdbourke/pytorch-deep-learning))
- **URL ต้นทาง:** `https://raw.githubusercontent.com/mrdbourke/pytorch-deep-learning/2448e34035ae9d6c0c45eb2b82f726e43c589763/data/pizza_steak_sushi_20_percent.zip`
- **Git Commit ต้นทาง:** `2448e34035ae9d6c0c45eb2b82f726e43c589763`
- **SHA-256 ของไฟล์ Zip:** `4c7589efc2b369364d602906c5bd2d563dfd73e1e0083565ffb23dd731eb9d88`
- **ขนาดไฟล์ Zip:** 31,491,084 ไบต์ (~30 MB สะดวกต่อการดาวน์โหลดในชั้นเรียน ไม่ใช่ Full Food-101 ที่มีขนาดหลาย GB)
- **ข้อควรระวังลิขสิทธิ์รูปภาพ:** ลิขสิทธิ์ภาพถ่ายยังคงเป็นของเจ้าของภาพเดิม ใบอนุญาตโค้ดของ Repository ไม่ได้ให้สิทธิ์ใช้ภาพโดยอัตโนมัติ ควรตรวจเงื่อนไข Food-101 และภาพต้นฉบับก่อนใช้ต่อหรือเผยแพร่ ไม่ควรถือว่าเป็น Creative Commons หรือได้รับอนุญาตเพียงเพราะใช้เพื่อการศึกษา

---

## 2. โครงสร้างโฟลเดอร์ (Physical Directory Layout)

แบ่งโฟลเดอร์ไว้แล้ว แต่ละภาพอยู่เพียงชุดเดียว:

```text
Dataset/
├── README.md              # เอกสารอธิบายชุดข้อมูล (ภาษาไทย)
├── manifest.json          # บันทึก metadata, SHA-256 hash และการแบ่ง Split
├── train/                 # 360 ภาพ
│   ├── pizza/
│   ├── steak/
│   └── sushi/
├── valid/                 # 90 ภาพ
│   ├── pizza/
│   ├── steak/
│   └── sushi/
└── test/                  # 150 ภาพ
    ├── pizza/
    ├── steak/
    └── sushi/
```

ภาพทุกภาพผ่านทั้ง `PIL.Image.verify()` และการ Decode ด้วย `load()` เป็น **RGB/JPEG** และมี SHA-256 ไม่ซ้ำกัน การตรวจนี้ยืนยันว่าไม่มีไฟล์ที่เหมือนกันทุกไบต์ แต่ไม่ได้ตรวจภาพใกล้เคียงหรือภาพจานเดียวกันคนละมุม

---

## 3. การแบ่งข้อมูล

สคริปต์ผู้สอนแบ่งด้วย scikit-learn แล้วเตรียมโฟลเดอร์ให้ Notebook เปิดด้วย ImageFolder ได้ทันที สมาชิกเหมือน Notebook ฉบับล่าสุดที่สุ่มจากภาพทั้งหมด ไม่ใช้ Test เดิมของต้นทาง ส่วน `manifest.json` เก็บแหล่งที่มา Hash และ `split` ของแต่ละภาพ:

1. `upstream_split` เก็บประวัติ Train/Test ใน Archive ส่วน `split` ตรงกับโฟลเดอร์ที่ใช้จริง
2. กัน Test 25% ของภาพทั้งหมด แล้วกัน Validation 20% จากส่วนที่เหลือ ใช้ `random_state=42` และ `stratify` ทั้งสองครั้ง
3. **ป้องกัน Data Leakage (No Augmentation Before Split):** ภาพบนดิสก์เป็นภาพจริงต้นฉบับ ไม่ผ่านการ Resize หรือ Data Augmentation ล่วงหน้า การ Augmentation ต้องทำบน Train Set ในขั้นตอน DataLoader ขณะเทรนเท่านั้น

### จำนวนภาพในโฟลเดอร์

| คลาส | Train | Valid | Test | รวม |
| :--- | :---: | :---: | :---: | :---: |
| Pizza | 120 | 30 | 50 | 200 |
| Steak | 122 | 31 | 51 | 204 |
| Sushi | 118 | 29 | 49 | 196 |
| รวม | 360 | 90 | 150 | 600 |

---

## 4. ตัวอย่างการโหลด Split ใน PyTorch (Code Example)

```python
from pathlib import Path
from torchvision import datasets, transforms
from torch.utils.data import DataLoader
# เมื่อเปิด Notebook จากโฟลเดอร์ Week 10
dataset_root = Path("Dataset")
transform = transforms.Compose([transforms.Resize((64, 64)), transforms.ToTensor()])
train_set = datasets.ImageFolder(dataset_root / "train", transform=transform)
valid_set = datasets.ImageFolder(dataset_root / "valid", transform=transform)
test_set = datasets.ImageFolder(dataset_root / "test", transform=transform)
train_loader = DataLoader(train_set, batch_size=32, shuffle=True)
valid_loader = DataLoader(valid_set, batch_size=32, shuffle=False)
test_loader = DataLoader(test_set, batch_size=32, shuffle=False)
```

เก็บภาพทดลองใหม่ไว้นอก Dataset ไม่ใช่ Food-101 benchmark การย้ายเป็นโฟลเดอร์คงสมาชิกชุดล่าสุดไว้ แต่ลำดับภาพใน Loader เปลี่ยนตามการเรียงไฟล์ จึงอาจได้คะแนนต่างจากรอบก่อน

---

## 5. คำสั่งดาวน์โหลดและสร้างชุดข้อมูลซ้ำ (Reproducibility)

สามารถรันสคริปต์เพื่อตรวจสอบหรือดาวน์โหลดใหม่ได้ด้วยคำสั่ง:

```bash
python3 scripts/download_w10_dataset.py
```

รันจากรากโปรเจกต์ ต้องมี Pillow และ scikit-learn สคริปต์ตรวจ SHA-256 และไม่ทับภาพที่เปลี่ยนไป ถ้ามีโครงสร้าง `images/` รุ่นเก่า จะตรวจทุกภาพก่อนย้ายไป train/valid/test โดยไม่ทำสำเนา แล้วลบเฉพาะโฟลเดอร์เก่าที่ว่าง รันซ้ำได้โดยไม่เขียนภาพเดิมใหม่ ใช้ `--cache-dir` เลือกที่เก็บ Zip ได้
