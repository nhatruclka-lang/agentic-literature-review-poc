# 📚 Agentic Literature Review (POC) - Human-in-the-Loop Workflow

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Powered by Gemini API](https://img.shields.io/badge/Model-Gemini%20Flash%20%7C%20Free%20Tier-green.svg)](https://aistudio.google.com/)

> **Quy trình tổng quan y văn bán tự động (Human-in-the-loop Agentic Literature Review)** chạy hoàn toàn miễn phí trên **Google Colab** kết hợp **Google Gemini API (Free Tier)** và dữ liệu y văn công khai từ máy chủ **PubMed E-utilities**.

---

## 📌 Mục lục
- [Tổng quan dự án](#-tổng-quan-dự-án)
- [Kiến trúc luồng xử lý](#-kiến-trúc-luồng-xử-lý)
- [Điểm nổi bật (Key Features)](#-điểm-nổi-bật-key-features)
- [Chuẩn bị môi trường (Chi phí 0 VNĐ)](#-chuẩn-bị-môi-trường-chi-phí-0-vnđ)
- [Các điểm dừng kiểm soát (Human-in-the-loop Checkpoints)](#-các-điểm-dừng-kiểm-soát-human-in-the-loop-checkpoints)
- [Xử lý lỗi thường gặp (Troubleshooting)](#-xử-lý-lỗi-thường-gặp-troubleshooting)

---

## 📖 Tổng quan dự án

Làm tổng quan tài liệu (Literature Review) hoặc tổng quan hệ thống (Systematic Review) theo cách truyền thống tiêu tốn hàng chục giờ tìm kiếm, lọc bài trùng và bóc tách dữ liệu thủ công. 

Dự án này là một bản thử nghiệm chứng minh tính khả thi (**Proof of Concept - POC**) cho quy trình nghiên cứu tự động:
1. **Không phụ thuộc công cụ SaaS trả phí**: Không cần đóng phí thuê bao tháng cho các nền tảng web như SciSpace, Consensus hay AnswerThis.
2. **Con người giữ vai trò quyết định (Human-in-the-loop)**: AI hỗ trợ cào dữ liệu, lọc sơ bộ và phân tích; nhà nghiên cứu giữ quyền kiểm soát ở 2 điểm dừng (Checkpoints) quan trọng trước khi xuất bản thảo.
3. **Chi phí 0 đồng**: Vận hành dựa trên Google Colab CPU và Gemini API gói miễn phí.

---

## 🏗 Kiến trúc luồng xử lý

```text
[1. Nhập Tên Đề Tài Thô]
           │
           ▼
[2. Gemini 2.0 Flash Phân Tích] ──> Tự động trích xuất PICO, Search Query, Tiêu chuẩn Chọn/Loại
           │
           ▼
[👉 CHECKPOINT 0: Con người duyệt] ──> Tùy chỉnh Query, Năm, Số lượng bài (max_results)
           │
           ▼
[3. PubMed API Ingestion] ──> Tự động tải và chuẩn hóa Title, Abstract, DOI, PMID
           │
           ▼
[4. Gemini Screening Title/Abstract] ──> Phân loại: INCLUDE / EXCLUDE / UNCERTAIN
           │
           ▼
[👉 CHECKPOINT 1: Con người duyệt] ──> Rà soát các bài UNCERTAIN trên file CSV
           │
           ▼
[5. Thematic Synthesis] ──> Đối chiếu bằng chứng & Xuất file Word (.docx) hoàn chỉnh

```

---

## ✨ Điểm nổi bật (Key Features)

* **Tự động bóc tách khung PICO:** Người dùng chỉ cần nhập một câu tên đề tài bằng tiếng Việt hoặc tiếng Anh, AI sẽ tự động phân tích đối tượng ($P$), can thiệp ($I$), so sánh ($C$), kết quả ($O$).
* **Sinh Search Query chuẩn y văn:** Tự động tạo chuỗi tìm kiếm Boolean (`AND`, `OR`, `[tiab]`) tương thích với máy chủ PubMed.
* **Sàng lọc ưu tiên Recall:** Không loại bỏ cứng nhắc các bài báo nằm ở ranh giới, tự động gắn nhãn `UNCERTAIN` để người dùng thẩm định.
* **Tổng hợp theo chủ đề quy nạp (Inductive Thematic Synthesis):** Bài viết phát triển từ chính bằng chứng y văn thu thập được, gắn kèm mã định danh trích dẫn `[PMID: ...]`.
* **Xuất bản thảo Word (`.docx`):** Định dạng sẵn các mục Đặt vấn đề, Phương pháp, Thảo luận, Kết luận và Abstract.

---

## 🛠 Chuẩn bị môi trường (Chi phí 0 VNĐ)

1. **Lấy API Key Gemini (Miễn phí):**
* Truy cập [Google AI Studio](https://aistudio.google.com/).
* Chọn **Get API key** $\rightarrow$ **Create API key in new project**.
* Lưu chuỗi API Key lại để dùng khi chạy code.


2. **Mở Google Colab:**
* Truy cập [Google Colab](https://colab.research.google.com/) và tạo sổ tay mới hoặc tải file `POC_Literature_Review.ipynb` lên.

---

## 🛑 Các điểm dừng kiểm soát (Human-in-the-loop Checkpoints)

| Điểm dừng | Vị trí | Mục đích & Thao tác của người dùng |
| --- | --- | --- |
| **Checkpoint 0** | **Block 2** | **Kiểm soát thông số tìm kiếm**: Rà soát khung PICO, Search Query, khoảng năm và số lượng bài quét (`max_results`). Gõ `edit` để chỉnh sửa hoặc nhấn `Enter` để duyệt. |
| **Checkpoint 1** | **Block 4** | **Kiểm soát sàng lọc**: Tải file `checkpoint1_screening.csv`, kiểm tra các bài báo có trạng thái `UNCERTAIN` và sửa cột `Human_Decision` thành `INCLUDE` hoặc `EXCLUDE` trước khi chạy Block 5. |

---

## 🔧 Xử lý lỗi thường gặp (Troubleshooting)

* **Lỗi `RESOURCE_EXHAUSTED` (Rate limit):** Gói Gemini Free Tier giới hạn tối đa lượt gọi API mỗi phút. Khi quét trên 30 bài báo, thêm `import time; time.sleep(2)` vào trong vòng lặp ở Block 4.
* **Không thấy file xuất ra:** Bấm vào biểu tượng **Hình thư mục (Files)** ở thanh bên trái Google Colab $\rightarrow$ Chọn nút **Refresh (Làm mới)** để hiển thị file `.docx` và `.csv`.
* **Kết quả tìm kiếm PubMed bằng 0:** Chuỗi tìm kiếm quá hẹp. Chạy lại Block 2, chọn `edit` để giảm bớt các điều kiện lọc hoặc mở rộng khoảng năm.

---

