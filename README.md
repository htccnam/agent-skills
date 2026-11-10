# 🚀 KHO TÀI LIỆU VÀ KỸ NĂNG CHO AI CODING AGENT (AGENT SKILLS)

> **Mục tiêu:** Xây dựng lộ trình đào tạo, chuẩn hóa quy trình và trang bị bộ kỹ năng (Skills / Rulepacks) cho các AI Coding Agent (Antigravity, Cursor, Claude Code, GitHub Copilot). Chuyển hóa từ lối lập trình tự do, cảm tính (**Vibe Coding**) sang lập trình có kỷ luật, định hướng kiến trúc (**Architectural & Spec-Driven Development**).

---

## 📚 Danh mục Tài liệu Kỹ thuật Chuyên sâu (`/docs`)

Dưới đây là danh sách các tài liệu hướng dẫn chi tiết từ A đến Z, được biên soạn theo tiêu chuẩn Senior Tech Lead với đầy đủ giải phẫu lệnh Terminal, sơ đồ tiến hóa thư mục và tư duy logic:

| STT | Tên Kỹ năng (Skill) | Nội dung cốt lõi | Tài liệu chi tiết |
| :---: | :--- | :--- | :---: |
| **01** | **GitHub Spec Kit** | Phương pháp luận **Spec-Driven Development (SDD)**, Hiến pháp dự án (`constitution.md`), đặc tả (`spec.md`), kiến trúc (`plan.md`), và phân rã tác vụ nguyên tử (`tasks.md`). | [Xem tài liệu 01](docs/01%20spec-kit.md) |
| **02** | **React UI Master** | Kiến trúc giao diện người dùng hiện đại: **Design Tokens**, **CVA (Class Variance Authority)**, **Compound Components**, **Polymorphic Slots (`asChild`)**, chuẩn tiếp cận **WCAG 2.1 AA**, và **Render Hygiene**. | [Xem tài liệu 02](docs/02%20react-ui-master.md) |
| **03** | **Claude Agent Master** | Phương pháp luận & Công cụ tác chiến AI Agent đỉnh cao: **Claude Code (CLI)**, **Plan Mode**, **Khung giao việc 4 yếu tố (G-I-C-D)**, **Model Orchestration ("Dùng tướng & Dùng quân")**, **Sub-agents song song**, **Context Hygiene (`/compact`, `/btw`)**, và **Vòng lặp tự trị `/goal`**. | [Xem tài liệu 03](docs/03%20claude-agent-master.md) |

---

## 🛠️ Cấu trúc Kho lưu trữ

```text
agent-skills/
├── 📁 docs/                   # Thư viện tài liệu hướng dẫn và lộ trình làm chủ từng Skill
│   ├── 📄 01 spec-kit.md      # Quy trình SDD và công cụ specify-cli
│   ├── 📄 02 react-ui-master.md # Chuẩn mực thiết kế và lập trình React Component cấp Enterprise
│   └── 📄 03 claude-agent-master.md # Cẩm nang tác chiến toàn diện với Claude Code và AI Agent
└── 📄 README.md               # Mục lục tổng quan và lộ trình đào tạo
```

---

## 🧭 Hướng dẫn sử dụng cho Học viên & AI Agent

1. **Đối với Lập trình viên / Học viên:**
   - Đọc kỹ phần **Tư duy logic** ở từng giai đoạn để hiểu rõ nguyên nhân sâu xa của từng quyết định kiến trúc trước khi viết code.
   - Luôn quan sát phần **📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC** để hình dung rõ tác động vật lý của mỗi dòng lệnh terminal lên cấu trúc dự án.
   - Tham khảo bảng **Cheat Sheet** và **Sổ tay lỗi kinh điển** trước mỗi đợt code review hoặc release.

2. **Đối với AI Coding Agent:**
   - Nạp các quy tắc trong tài liệu vào context trước khi scaffold component hoặc refactor code để đảm bảo chuẩn mực chất lượng cao nhất.
