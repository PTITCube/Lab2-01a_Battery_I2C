# Lab2-01a_Battery_I2C

**Giao tiếp với IC đo dung lượng Pin MAX17048 qua I2C thuần (Wire.h)**

Bài thực hành thuộc Phân hệ Năng lượng (EPS - Electrical Power System) trong hệ thống CubeSat-LMS.

## 1. Giới thiệu
Trên các vệ tinh CubeSat hiện đại, việc theo dõi chính xác trạng thái pin (điện áp, % dung lượng còn lại và tốc độ sạc/xả) là cực kỳ quan trọng để đảm bảo vệ tinh không bị cạn kiệt pin bất ngờ trong quỹ đạo che khuất (Eclipse).

IC chuyên dụng **MAX17048** (Fuel Gauge) sử dụng thuật toán ModelGauge™ theo dõi trạng thái pin Li-ion/Li-Po 1S mà không cần điện trở shunt đo dòng điện ngoài, giao tiếp trực tiếp với vi điều khiển ESP32 qua giao thức **I2C**.

## 2. Kết nối phần cứng
| MAX17048 Pin | Chức năng | ESP32 GPIO |
| :--- | :--- | :--- |
| **SDA** | Dữ liệu I2C | GPIO 21 |
| **SCL** | Xung nhịp I2C | GPIO 22 |
| **CELL / VDD** | Đo điện áp Pin | Nguồn pin Li-ion (3.0V - 4.2V) |
| **GND** | Mass chung | GND |

## 3. Bản đồ thanh ghi MAX17048
- **Địa chỉ I2C:** `0x36`
- **0x02 (VCELL):** Thanh ghi điện áp 16-bit. Độ phân giải: $78.125\ \mu\text{V} = 0.000078125\text{ V}$.
- **0x04 (SOC):** Thanh ghi State-of-Charge 16-bit. Tỷ lệ % pin = `raw / 256.0` (0% - 100%).
- **0x16 (CRATE):** Tốc độ sạc/xả (%/h). Giá trị bù 2 (signed 16-bit) nhân với hệ số $0.208\ \%/\text{h}$.

## 4. Biên dịch và nạp code
```bash
pio run -t upload
pio device monitor
```
