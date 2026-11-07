# api-lab

เทสต์ API ของ QuickPizza (https://quickpizza.grafana.com) ด้วย pytest และ requests
วิชา ITDS362 Software Quality Assurance and Testing แล็บ API สัปดาห์ที่ 10

## วิธีติดตั้งและรันเทสต์

```
python -m venv .venv
.venv\Scripts\activate            # macOS/Linux ใช้: source .venv/bin/activate
pip install -r requirements.txt
pytest -v
```

บันทึกผลการรันทั้งหมดเป็นรายงาน HTML ด้วยคำสั่ง `pytest -v --html=report.html --self-contained-html`
ผลที่ควรได้คือ `32 passed, 1 xfailed` เทสต์ที่เป็น xfailed ใช้บันทึก Defect 1 (ดูรายละเอียดใน RESULTS.md)

## Input domain model ของ POST /api/ratings

| Characteristic | Block | ค่าที่ใช้ในเทสต์ | เทสต์ที่ครอบคลุม |
|---|---|---|---|
| C1 ค่าของ `stars` | น้อยกว่า 1 | 0 | `test_stars_out_of_range[0]` |
| | 1 ถึง 5 (base) | 3 | `test_base_choice_is_saved` |
| | มากกว่า 5 | 6 | `test_stars_out_of_range[6]` |
| C2 ชนิดของ `stars` | จำนวนเต็ม (base) | 3 | `test_base_choice_is_saved` |
| | ทศนิยม | 3.5 | `test_stars_decimal` |
| | ข้อความ | "3" | `test_stars_text` |
| | ไม่มี field | ไม่ส่ง field | `test_stars_missing` |
| C3 พิซซ่า | มีอยู่จริง (base) | พิซซ่าที่สร้างใหม่ | `test_base_choice_is_saved` |
| | ไม่มีอยู่จริง | 999999999 | `test_pizza_does_not_exist` |
| C4 Token | token ของผู้ใช้เอง (base) | จาก `auth` | `test_base_choice_is_saved` |
| | ไม่มี token | ไม่ส่ง header | `test_no_token` |
| | ความยาวผิด | `token short` | `test_token_wrong_length` |

Base choice coverage ใช้ 1 เทสต์ฐาน + (3-1) + (4-1) + (2-1) + (3-1) = 1 + 2 + 3 + 1 + 2 = 9 เทสต์
ทั้งหมดอยู่ในไฟล์ `test_create_rating.py`
