\# 🌱 Smart-Farm: Hệ thống Trang trại thông minh IoT + AI



!\[S Smart-Farm](https://www.synox.io/wp-content/uploads/2022/04/smart-farming-agriculture-du-futur-iot.jpg)



\## 📖 Mô tả dự án



\*\*Smart-Farm\*\* là hệ thống giám sát và điều khiển trang trại \*tự động\*, kết hợp \*\*IoT\*\* (thu thập dữ liệu từ cảm biến) và \*\*AI\*\* (dự đoán, ra quyết định tưới tiêu). Hệ thống chạy trực tiếp trên chip \*\*SoC\*\*, giúp \*giảm chi phí vận hành\*, \*tiết kiệm nước\* và \*\*tăng năng suất cây trồng\*\*.



Dữ liệu đi qua hệ thống theo mô hình \*\*DIKW\*\*, từ số đo thô đến quyết định hành động:



| Tầng DIKW | Ý nghĩa | Ví dụ trong Smart-Farm |

|:----------|:--------|:-----------------------|

| \*\*D\*\*ata (Dữ liệu) | Số đo thô, chưa có ngữ cảnh | `36.2` (từ cảm biến nhiệt độ) |

| \*\*I\*\*nformation (Thông tin) | Dữ liệu có ngữ cảnh | "Nhiệt độ nhà kính lúc 12:00 là 36.2°C" |

| \*\*K\*\*nowledge (Tri thức) | Quy luật rút ra từ thông tin | "Nhiệt độ trên 35°C kéo dài làm cây héo" |

| \*\*W\*\*isdom (Trí tuệ) | Quyết định hành động | "Bật quạt và tưới phun sương ngay" |



\## 🔄 Phân tích IPO



| Giai đoạn | Thành phần | Mô tả |

|:----------|:-----------|:------|

| \*\*Input\*\* | Cảm biến nhiệt độ, độ ẩm không khí, độ ẩm đất, ánh sáng, pH nước | Đo môi trường theo chu kỳ |

| \*\*Process\*\* | CPU/RAM trên SoC, mô hình AI | Làm sạch, lưu tạm, so sánh ngưỡng, dự đoán |

| \*\*Output\*\* | Bơm tưới, quạt, đèn, thông báo ứng dụng | Tác động lên môi trường và báo cho người dùng |



\### Sơ đồ phối hợp CPU và RAM



| Thành phần | Vai trò | Dữ liệu đi qua |

|:-----------|:--------|:---------------|

| Cảm biến | Đo nhiệt độ | Gửi 36°C vào SoC |

| RAM | Chứa chương trình, ngưỡng 35°C và giá trị vừa đo | Cho CPU đọc, nhận kết quả ghi lại vào ổ cứng |

| CPU (Control Unit) | Nạp và giải mã lệnh | Nhận lệnh từ RAM |

| CPU (ALU) | So sánh 36 > 35 | Trả kết quả đúng/sai |

| Thiết bị chấp hành | Bật quạt | Nhận lệnh từ CPU |



\## ⚙️ Chu kỳ Fetch–Decode–Execute khi nhận dữ liệu nhiệt độ



Giả sử cảm biến gửi giá trị \*\*36°C\*\* vào hệ thống:



> \*\*1. Fetch (Nạp lệnh):\*\*



> Bộ đếm chương trình (PC) giữ địa chỉ của lệnh kế tiếp. CPU đặt địa chỉ đó lên bus địa chỉ,



> lệnh được đọc từ \*\*RAM\*\* qua bus dữ liệu về thanh ghi lệnh (IR), rồi PC tăng lên trỏ tới lệnh sau.



>



> \*\*2. Decode (Giải mã):\*\*



> Đơn vị điều khiển (Control Unit) giải mã lệnh trong IR, xác định đây là lệnh



> \*"so sánh giá trị nhiệt độ với ngưỡng 35°C"\* và chuẩn bị toán hạng (36 và 35).



>



> \*\*3. Execute (Thực thi):\*\*



> Đơn vị số học và logic (ALU) thực hiện phép so sánh: 36 > 35 cho kết quả \*\*đúng\*\*.



> Kết quả được ghi vào thanh ghi/RAM, kích hoạt lệnh tiếp theo là \*\*bật quạt\*\*.



>



> > Chu kỳ này lặp lại hàng triệu lần mỗi giây cho mọi lệnh của chương trình.



\## 🛠️ Hướng dẫn cài đặt



Yêu cầu: board SoC chạy Linux (ví dụ Raspberry Pi), Python 3.9 trở l



```bash



\# 1. Tải mã nguồn



git clone https://github.com/your-username/smart-farm.git



cd smart-farm



\# 2. Tạo môi trường ảo



python3 -m venv venv



source venv/bin/activate



\# 3. Cài đặt thư viện



pip install -r requirements.txt



\# 4. Chạy hệ thống



python3 main.py



```



\## ✅ Tính năng



\- \[x] Thu thập dữ liệu từ cảm biến nhiệt độ, độ ẩm



\- \[x] Lưu trữ dữ liệu cục bộ trên SoC



\- \[x] Tự động bật/tắt quạt theo ngưỡng nhiệt độ



\- \[ ] Mô hình AI dự đoán nhu cầu tưới nước



\- \[ ] Dashboard theo dõi thời gian thực



\- \[ ] Cảnh báo qua ứng dụng di động



\## 📚 Bảng thư viện sử dụng



| Thư viện | Phiên bản | Mục đích |

|:---------|:---------:|:---------|

| `RPi.GPIO` | 0.7.1 | Điều khiển chân GPIO, thiết bị chấp hành |

| `Adafruit\_DHT` | 1.4.0 | Đọc cảm biến nhiệt độ, độ ẩm |

| `pandas` | 2.1.0 | Xử lý và làm sạch dữ liệu |

| `scikit-learn` | 1.3.0 | Mô hình AI dự đoán |

| `paho-mqtt` | 1.6.1 | Truyền dữ liệu qua giao thức MQTT |



\## 💾 Tính toán dung lượng lưu trữ cảm biến trong 1 năm



\*\*Giả định:\*\*



\- Mỗi bản ghi gồm: ID cảm biến (2 byte) + timestamp (4 byte) + giá trị `float` (4 byte) = \*\*10 byte\*\*.



\- Quy ước: 1 KB = 1024 byte, 1 MB = 1024 KB, 1 GB = 1024 MB.



\- Một năm tính là 365 ngày.



\*\*Công thức:\*\* Dung lượng/năm = Số cảm biến × Số lần đo/ngày × Byte/lần đo × 365



| Cảm biến | Số lượng | Chu kỳ đo | Lần đo/ngày | Byte/năm | MB/năm |

|:---|---:|:---:|---:|---:|---:|

| Nhiệt độ | 10 | 1 phút | 1.440 | 52.560.000 | 50,13 |

| Độ ẩm không khí | 10 | 1 phút | 1.440 | 52.560.000 | 50,13 |

| Độ ẩm đất | 20 | 5 phút | 288 | 21.024.000 | 20,05 |

| Ánh sáng | 5 | 1 phút | 1.440 | 26.280.000 | 25,06 |

| pH nước | 2 | 10 phút | 144 | 1.051.200 | 1,00 |

| \*\*Tổng\*\* | \*\*47\*\* | \*\*153.475.200\*\* | \*\*146,37\*\* |



\*\*Kết luận:\*\* Tổng khoảng \*\*146,37 MB\*\* (khoảng 0,14 GB) mỗi năm, nên một thẻ nhớ 32 GB trên SoC lưu được nhiều năm dữ liệu.



\*\*\_Phần 3. Kiểm thử:\_\*\*



Giả định khi link ảnh bị lỗi, file Markdown của bạn hiện nội dung gì để thay thế?



\-> Khi link ảnh lỗi, nội dung thay thế là alt text, tức phần chữ nằm trong dấu \[ ] của cú pháp ảnh `!\[alt text](link)`.

