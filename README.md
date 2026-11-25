# Bộ 6 bài lab Labtainer: Phát hiện tấn công ransomware qua bằng chứng số

Bộ bài thực hành trên nền tảng [Labtainers](https://nps.edu/web/c3o/labtainers), giúp sinh viên làm quen với
việc **đọc bằng chứng số (digital forensics)** và **ánh xạ hành vi tấn công sang MITRE ATT&CK**
qua một kịch bản ransomware trên máy Windows.

> **An toàn:** toàn bộ dữ liệu là file văn bản mô phỏng. Các lab không chứa mã độc thật và mọi thao tác "tấn công"
> chỉ diễn ra bên trong container Labtainers, không ảnh hưởng đến máy của bạn.

## Kịch bản chung

Một máy Windows của nạn nhân bị tấn công theo chuỗi: **thăm dò file → cài cơ chế duy trì → xóa bản sao lưu hệ thống → mã hóa dữ liệu**.
Sinh viên đóng vai nhà điều tra, phân tích các dấu vết (Prefetch, Registry, `$MFT`, Sysmon, Defender)
rồi xác định kỹ thuật tấn công tương ứng trên [MITRE ATT&CK](https://attack.mitre.org/).

Vì Labtainers chạy container Linux, các artifact Windows được cung cấp dưới dạng **file đã parse sẵn** (CSV/TXT,
tương tự kết quả của PECmd, RegRipper, MFTECmd). Sinh viên phân tích bằng `cat`, `grep`, `sort`...

## Danh sách bài lab

| # | Tên lab | Mức độ | Nội dung |
|---|---|---|---|
| 1 | `ttp-mapping` | Dễ | Đọc nhật ký lệnh của kẻ tấn công, ánh xạ từng hành vi sang kỹ thuật MITRE ATT&CK |
| 2 | `prefetch-analysis` | Dễ | Phân tích Prefetch để tìm công cụ dùng xóa bản sao lưu hệ thống |
| 3 | `autorun-persist` | Dễ | Tìm cơ chế duy trì trong khóa Registry tự khởi động |
| 4 | `mft-locked` | Dễ | Phân tích `$MFT` để đếm và định vị các file bị mã hóa |
| 5 | `ransom-sim` | Khó hơn | Chạy mô phỏng ransomware, điều tra dấu vết và khôi phục dữ liệu |
| 6 | `attack-timeline` | Khó hơn | Gộp nhiều nguồn bằng chứng thành timeline, suy luận trình tự tấn công |

Mỗi lab có file `HUONG_DAN.txt` trong thư mục home của sinh viên (đề bài và các lệnh gợi ý) và
**chấm điểm tự động** bằng `checkwork`.

## Yêu cầu

- Máy ảo **Labtainers** (khuyến nghị dùng VM appliance chính thức tại <https://nps.edu/web/c3o/virtual-machine-images>),
  hoặc Ubuntu đã cài Docker và Labtainers.
- Kết nối internet khi cài và build lần đầu.
- Khoảng 2 GB trống cho mỗi lab (image Docker).

## Cài đặt và chạy một lab

Thay `<tên-lab>` bằng một trong 6 tên ở bảng trên. Mở terminal và chạy:

```bash
cd ~/labtainer/labtainer-student

# 1) Tải bài lab
imodule https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-<tên-lab>.tar

# 2) Build image (chỉ cần làm một lần cho mỗi lab trên mỗi máy, mất 1-2 phút)
~/labtainer/trunk/scripts/labtainer-student/bin/rebuild <tên-lab>

# 3) Chạy lab
labtainer -r <tên-lab>
```

Một cửa sổ **terminal sinh viên** hiện ra với dấu nhắc dạng `ubuntu@<tên-lab>:~$`. Đọc file `HUONG_DAN.txt` bằng
`cat HUONG_DAN.txt` rồi làm theo.

### Chấm điểm và kết thúc

Mở một **terminal khác** (không phải terminal sinh viên):

```bash
cd ~/labtainer/labtainer-student
checkwork <tên-lab>      # xem bảng điểm: Y = đạt, N = chưa đạt
stoplab <tên-lab>        # kết thúc lab
```

Để làm lại từ đầu: `labtainer -r <tên-lab>`.

### Link tải từng lab

```text
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-ttp-mapping.tar
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-prefetch-analysis.tar
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-autorun-persist.tar
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-mft-locked.tar
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-ransom-sim.tar
https://github.com/tuantttd2004-droid/imodule-attack-labs/raw/refs/heads/main/imodule-attack-timeline.tar
```

## Mô tả chi tiết

### Lab 1 – `ttp-mapping`
Đọc `attack_chain.txt` (nhật ký lệnh) và bảng tham chiếu `ttp_reference.txt`, sau đó điền mã kỹ thuật MITRE ATT&CK
cho bốn giai đoạn vào `answer.txt`. Kỹ năng: lọc log bằng `grep`, tra cứu ATT&CK.

### Lab 2 – `prefetch-analysis`
Phân tích `prefetch_timeline.csv` để tìm chương trình dùng xóa bản sao lưu hệ thống, thời điểm chạy và kỹ thuật tương ứng.
Kỹ năng: sắp xếp và lọc dữ liệu CSV bằng `sort`, `grep`.

### Lab 3 – `autorun-persist`
Phân tích kết quả RegRipper `NTUSER_rip.txt` để tìm giá trị đáng ngờ trong khóa `Run`, file script nó trỏ tới và chiến thuật
ATT&CK liên quan. Kỹ năng: đọc output công cụ forensic Registry.

### Lab 4 – `mft-locked`
Phân tích `MFT.csv` để đếm các file bị mã hóa, xác định thư mục chứa chúng và file bị mã hóa đầu tiên theo thời gian.
Kỹ năng: lọc, đếm và sắp xếp theo cột thời gian.

### Lab 5 – `ransom-sim` (khó hơn)
Chạy `python3 attack_sim.py` để mô phỏng chuỗi tấn công lên dữ liệu giả, điều tra bằng `sysmon_sim.log`, kiểm tra shadow copy và
khóa Registry, tìm khóa mã hóa XOR trong mã nguồn rồi khôi phục dữ liệu bằng `xor_tool.py`, cuối cùng viết báo cáo sự cố
`incident_report.txt`. Kỹ năng: điều tra sự cố đầu-cuối và khôi phục dữ liệu.

### Lab 6 – `attack-timeline` (khó hơn)
Gộp năm nguồn bằng chứng (`src_sysmon.log`, `src_prefetch.log`, `src_registry.log`, `src_mft.log`, `src_defender.log`)
thành một timeline duy nhất, xác định thời điểm bắt đầu và kết thúc tấn công, thời điểm Defender bị tắt, và điền
báo cáo `ioc_report.txt`. Kỹ năng: tương quan sự kiện từ nhiều nguồn.

## Lưu ý khi làm bài

- **Gõ từng lệnh một**, chờ có kết quả rồi mới gõ lệnh tiếp. Dán nhiều dòng cùng lúc có thể làm mất lệnh trong terminal lab.
- **Chỉ gõ lệnh trong terminal sinh viên**: một số mục chấm điểm đọc lịch sử lệnh của terminal đó.
- **Phân biệt chữ hoa/thường** và ghi đáp án đúng dạng `KEY=giá_trị` (viết liền, không dấu cách quanh dấu `=`).
- Muốn làm lại lab, dùng `labtainer -r <tên-lab>` để xóa bài làm cũ, nếu không bảng điểm có thể phản ánh lần làm trước.

## Xử lý sự cố

| Hiện tượng | Nguyên nhân thường gặp | Cách xử lý |
|---|---|---|
| `Unable to reach DockerHub` / `Could not find image info` khi chạy `labtainer` | Chưa build image cho lab | Chạy lệnh `rebuild <tên-lab>` rồi chạy lại |
| `cat: ...: No such file or directory` | Gõ lệnh ngoài terminal sinh viên | Gõ trong cửa sổ có dấu nhắc `ubuntu@<tên-lab>:~$` |
| `checkwork` hiện **Y** khi chưa làm gì | Còn dữ liệu của lần làm trước | `stoplab <tên-lab>` rồi `labtainer -r <tên-lab>` |
| Cột điểm hiện **N** dù đã làm | Sai chữ hoa/thường, gõ ngoài terminal sinh viên, hoặc điền sai định dạng đáp án | Kiểm tra lại bằng `cat` file đáp án; đọc lại `HUONG_DAN.txt` |
| Không thấy file dữ liệu trong lab | Image build trước khi có dữ liệu | `stoplab <tên-lab>`, chạy lại `rebuild <tên-lab>` rồi `labtainer -r <tên-lab>` |

Nếu vẫn lỗi, xem log: `tail -20 ~/labtainer/trunk/logs/instructor.log`.

## Cấu trúc kho

```text
imodule-attack-labs/
├── README.md
├── imodule-ttp-mapping.tar
├── imodule-prefetch-analysis.tar
├── imodule-autorun-persist.tar
├── imodule-mft-locked.tar
├── imodule-ransom-sim.tar
└── imodule-attack-timeline.tar
```

## Ghi nhận

- Nền tảng: [Labtainers](https://github.com/mfthomps/Labtainers), Naval Postgraduate School.
- Khung tham chiếu kỹ thuật tấn công: [MITRE ATT&CK®](https://attack.mitre.org/).
- Kịch bản và dữ liệu bài lab xây dựng dựa trên nội dung đồ án về phát hiện tấn công mạng bằng bằng chứng số.
