# 📚 Agentic Literature Review (POC) - Human-in-the-Loop Workflow

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/agentic-literature-review-poc/blob/main/POC_Literature_Review.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Powered by Gemini API](https://img.shields.io/badge/Model-Gemini%202.0%20Flash%20(Free%20Tier)-green.svg)](https://aistudio.google.com/)

> **Quy trình tổng quan y văn bán tự động (Human-in-the-loop Agentic Literature Review)** chạy hoàn toàn miễn phí trên **Google Colab** kết hợp **Gemini 2.0 Flash API (Free Tier)** và dữ liệu y văn công khai từ máy chủ **PubMed E-utilities**.

---

## 📌 Mục lục
- [Tổng quan dự án](#-tổng-quan-dự-án)
- [Kiến trúc luồng xử lý](#-kiến-trúc-luồng-xử-lý)
- [Điểm nổi bật (Key Features)](#-điểm-nổi-bật-key-features)
- [Chuẩn bị môi trường (Chi phí 0 VNĐ)](#-chuẩn-bị-môi-trường-chi-phí-0-vnđ)
- [Hướng dẫn sử dụng từng bước](#-hướng-dẫn-sử-dụng-từng-bước)
- [Các điểm dừng kiểm soát (Human-in-the-loop Checkpoints)](#-các-điểm-dừng-kiểm-soát-human-in-the-loop-checkpoints)
- [Xử lý lỗi thường gặp (Troubleshooting)](#-xử-lý-lỗi-thường-gặp-troubleshooting)
- [Giấy phép (License)](#-giấy-phép-license)

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
