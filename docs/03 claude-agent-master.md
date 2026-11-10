# TỔNG HỢP TOÀN BỘ QUY TRÌNH TẢI VÀ SỬ DỤNG SKILL CLAUDE AGENT MASTER

> **Mentor Note:**  
> Chào bạn, tôi là Senior Tech Lead và Mentor đào tạo lập trình của bạn.  
> Đa số người dùng hiện nay chỉ mới tiếp cận Claude ở mức độ bề nổi: một chiếc "hộp chat" thụ động để hỏi đáp văn vở hoặc copy-paste từng đoạn mã rời rạc. Đó là phong cách làm việc của quá khứ.  
> Trong môi trường kỹ thuật đỉnh cao, chúng ta tiếp cận Claude như một **Đặc nhiệm Kỹ sư Kỹ thuật số (Autonomous AI Engineer)** có năng lực can thiệp trực tiếp vào mã nguồn, điều hành dòng lệnh shell, tương tác git, tự viết test và tự sửa lỗi (Self-Healing).  
> Tài liệu này được thiết kế để chuẩn hóa toàn bộ tư duy tác chiến, từ việc lựa chọn đúng chế độ, cấu trúc hóa câu lệnh theo khung 4 yếu tố, điều phối mô hình đa tầng ("Dùng tướng và dùng quân"), đến việc làm chủ công cụ **Claude Code (CLI Agent)** với các vũ khí thượng thừa như Plan Mode, Context Hygiene (`/compact`, `/btw`), Sub-agents và tự trị hóa bằng `/goal`.

---

## 📑 Mục lục

1. [1. Tác dụng của claude-agent-master](#1-tác-dụng-của-claude-agent-master)
   - [1.1. Bản chất cốt lõi: Chatbot thụ động vs AI Agent Kỹ sư Tự trị](#11-bản-chất-cốt-lõi-chatbot-thụ-động-vs-ai-agent-kỹ-sư-tự-trị)
   - [1.2. Phân loại 3 chế độ tác chiến: "Việc nào công nấy"](#12-phân-loại-3-chế-độ-tác-chiến-việc-nào-công-nấy)
   - [1.3. 10 Trụ cột tinh hoa định hình phong cách làm việc Senior](#13-10-trụ-cột-tinh-hoa-định-hình-phong-cách-làm-việc-senior)
2. [2. Cách tải chi tiết](#2-cách-tải-chi-tiết)
   - [2.1. Tư duy kiến trúc & Chuẩn bị môi trường](#21-tư-duy-kiến-trúc--chuẩn-bị-môi-trường)
   - [2.2. Kiểm tra điều kiện tiên quyết (Prerequisites)](#22-kiểm-tra-điều-kiện-tiên-quyết-prerequisites)
   - [2.3. Cài đặt Claude Code CLI toàn cục từ Registry chính thức](#23-cài-đặt-claude-code-cli-toàn-cục-từ-registry-chính-thức)
   - [2.4. Thiết lập định danh bảo mật và Biến môi trường (API Key)](#24-thiết-lập-định-danh-bảo-mật-và-biến-môi-trường-api-key)
   - [2.5. Khởi tạo Hiến pháp dự án (`CLAUDE.md`)](#25-khởi-tạo-hiến-pháp-dự-án-claudemd)
   - [2.6. Kiểm tra tính toàn vẹn và chẩn đoán sức khỏe hệ thống (Health Check)](#26-kiểm-tra-tính-toàn-vẹn-và-chẩn-đoán-sức-khỏe-hệ-thống-health-check)
3. [3. Cách sử dụng trong thực tế chi tiết](#3-cách-sử-dụng-trong-thực-tế-chi-tiết)
   - [3.1. Giai đoạn 0: Thiết lập Hiến pháp dự án `CLAUDE.md` chuẩn công nghiệp](#31-giai-đoạn-0-thiết-lập-hiến-pháp-dự-án-claudemd-chuẩn-công-nghiệp)
   - [3.2. Giai đoạn 1: Giao việc chuẩn 4 yếu tố & Kỹ thuật Phản biện (Challenge Protocol)](#32-giai-đoạn-1-giao-việc-chuẩn-4-yếu-tố--kỹ-thuật-phản-biện-challenge-protocol)
   - [3.3. Giai đoạn 2: Tác chiến với Plan Mode (Bản vẽ thiết kế trước khi gõ mã)](#33-giai-đoạn-2-tác-chiến-với-plan-mode-bản-vẽ-thiết-kế-trước-khi-gõ-mã)
   - [3.4. Giai đoạn 3: Chiến thuật "Dùng Tướng và Dùng Quân" & Tối ưu hóa Context Window](#34-giai-đoạn-3-chiến-thuật-dùng-tướng-và-dùng-quân--tối-ưu-hóa-context-window)
   - [3.5. Giai đoạn 4: Phân quyền và điều phối Sub-agents xử lý đa nhiệm](#35-giai-đoạn-4-phân-quyền-và-điều-phối-sub-agents-xử-lý-đa-nhiệm)
   - [3.6. Giai đoạn 5: Giao khoán tự trị khép kín bằng lệnh `/goal` (Self-Healing Loop)](#36-giai-đoạn-5-giao-khoán-tự-trị-khép-kín-bằng-lệnh-goal-self-healing-loop)
4. [4. Sổ tay lỗi kinh điển cần tránh (Lessons Learned)](#4-sổ-tay-lỗi-kinh-điển-cần-tránh-lessons-learned)
5. [5. Bảng tra cứu nhanh lệnh claude-agent-master (Cheat Sheet)](#5-bảng-tra-cứu-nhanh-lệnh-claude-agent-master-cheat-sheet)

---

## 1. Tác dụng của claude-agent-master

### 1.1. Bản chất cốt lõi: Chatbot thụ động vs AI Agent Kỹ sư Tự trị

Phần lớn lập trình viên thất bại trong việc khai thác AI vì họ tương tác với nó như một "công cụ gõ hộ code". Họ gõ prompt chung chung, nhận về một đoạn mã, tự copy-paste vào IDE, gặp lỗi cú pháp, tự quay lại paste lỗi vào chat, rồi lặp lại vòng luẩn quẩn mệt mỏi.

**Claude Agent (đặc biệt là Claude Code CLI)** thay đổi hoàn toàn cuộc chơi thông qua cơ chế **Agentic Loop**:

```mermaid
flowchart TD
    User["👤 Kỹ sư Senior (Người ra đề bài)"] -->|Giao việc 4 yếu tố| Commander["🧠 Claude Agent (Nhận thức & Lập kế hoạch)"]
    Commander -->|Plan Mode| Blueprint["📐 Bản thiết kế kiến trúc (Tasks / Specs)"]
    Blueprint -->|Phê duyệt| Execution["⚡ Vòng lặp Tự trị (Agentic Tool Use)"]
    Execution -->|Đọc/Ghi file| Codebase["📂 Mã nguồn Dự án"]
    Execution -->|Chạy Shell / Test| Terminal["🖥️ Terminal / Test Runner"]
    Terminal -->|Báo lỗi / Thất bại| Commander
    Terminal -->|100% Passed| Finish["🏁 Bàn giao sản phẩm hoàn chỉnh"]
```

---

### 1.2. Phân loại 3 chế độ tác chiến: "Việc nào công nấy"

Là một Kỹ sư Trưởng, bạn không bao giờ dùng dao mổ trâu để giết gà. Hệ sinh thái Claude được chia làm 3 chiến trường chuyên biệt:

1. **Claude Chat (Giao diện Web/App):**  
   - *Mục đích:* Brainstorming ý tưởng kiến trúc cấp cao, đặt câu hỏi lý thuyết, nghiên cứu giải pháp công nghệ mới, phác thảo tài liệu PRD hoặc kiểm tra logic thuật toán độc lập.
   - *Điểm mạnh:* Giao diện trực quan, hỗ trợ Artifacts hiển thị kết quả, hỗ trợ nhập liệu bằng giọng nói (Voice Dictation).
2. **Claude Cowork / Desktop (Tương tác tệp tin cục bộ):**  
   - *Mục đích:* Làm việc với tài liệu văn phòng, phân tích file Excel/CSV, trích xuất dữ liệu từ các tệp PDF tài liệu kỹ thuật dài hàng trăm trang, xử lý ảnh chụp màn hình UI.
   - *Điểm mạnh:* Đọc trực tiếp các tệp tin lưu trên ổ cứng mà không cần upload thủ công từng file.
3. **Claude Code (Agentic CLI chạy trong Terminal):**  
   - *Mục đích:* **Chuyên sâu cho kỹ thuật phần mềm.** Trực tiếp duyệt cấu trúc thư mục, tìm kiếm regex xuyên qua hàng ngàn file, sửa code trên nhiều file đồng thời, chạy linter, chạy unit test và quản lý Git.
   - *Điểm mạnh:* Tốc độ phản hồi cực nhanh, tích hợp sâu vào quy trình CI/CD và terminal của lập trình viên, hỗ trợ tự động sửa lỗi (Self-Healing).

---

### 1.3. 10 Trụ cột tinh hoa định hình phong cách làm việc Senior

Kỹ năng `claude-agent-master` đúc kết 10 nguyên lý thực chiến đỉnh cao:

| STT | Nguyên lý cốt lõi | Ý nghĩa & Ứng dụng thực tế |
| :---: | :--- | :--- |
| **1** | **Chọn đúng chế độ** | Việc nào công nấy: Dùng Chat để hỏi đáp/viết lách; dùng Cowork để phân tích tài liệu; dùng Claude Code để lập trình thực chiến. |
| **2** | **Giao việc chuẩn 4 yếu tố** | Không ra lệnh mơ hồ. Bắt buộc chốt: **Mục tiêu (Goal)** + **Dữ liệu đầu vào (Input)** + **Ràng buộc (Constraints)** + **Tiêu chí hoàn thành (Done Criteria)**. |
| **3** | **Nói thay vì gõ phím** | Sử dụng Voice Dictation để truyền đạt ngữ cảnh phong phú, biểu đạt ý tưởng nhanh gấp 3 lần so với gõ phím. |
| **4** | **Đưa ra cả mẫu "ưng" và "không ưng"** | Contrastive Prompting: Cung cấp mẫu code chuẩn mực (Golden Example) kèm theo các mẫu code cấm kỵ (Anti-Patterns) để Claude triệt tiêu thói quen xấu. |
| **5** | **Bắt Claude hỏi ngược lại** | Thiết lập "Chốt chặn phản biện" (Clarification Gate): Yêu cầu Claude chất vấn lại các điểm mù nghiệp vụ trước khi bắt tay vào code. |
| **6** | **Dùng Plan Mode (Chế độ kế hoạch)** | Cấm AI sửa file khi chưa có bản vẽ thiết kế. Duyệt bản vẽ hợp lý rồi mới cấp quyền can thiệp mã nguồn. |
| **7** | **Dùng tướng và dùng quân** | Dùng model cao cấp (Opus/Sonnet) làm "Kiến trúc sư" lập kế hoạch, dùng model nhẹ (Haiku) làm "Công nhân" thực thi tác vụ nhỏ nhằm tối ưu chi phí và tốc độ. |
| **8** | **Chủ động dọn dẹp bàn làm việc** | Quản lý Context Window: Dùng `/compact` để nén các đoạn chat thừa, dùng `/btw` khi cần hỏi câu hỏi phụ ngoài luồng mà không làm loãng mạch công việc chính. |
| **9** | **Chia việc cho Sub-agent** | Chia tách tác vụ nặng (quét hàng ngàn file, refactor hàng loạt) cho các trợ lý phụ làm việc song song, giữ trí nhớ của trợ lý chính luôn tinh gọn. |
| **10** | **Giao khoán tự trị bằng `/goal`** | Đặt tiêu chí test rõ ràng và bàn giao khoán toàn bộ. Claude sẽ tự code, tự chạy test, tự sửa lỗi cho đến khi pass 100% không cần bấm "tiếp tục". |

[⬆️ Quay lại Mục lục](#mục-lục)

---

## 2. Cách tải chi tiết

### 2.1. Tư duy kiến trúc & Chuẩn bị môi trường

Claude Code là công cụ dòng lệnh phân phối chính thức bởi Anthropic qua hệ sinh thái Node.js.  
Là Tech Lead, tôi yêu cầu:
- **Node.js LTS v18 trở lên:** Đảm bảo hỗ trợ các tính năng xử lý luồng mạng (Web Streams) và tương thích hoàn toàn với các API bảo mật.
- **Tài khoản Anthropic Console có gắn API Key:** Để Claude Code có thể trực tiếp gửi request đến các model Claude 3.5 Sonnet / Claude 3 Opus / Claude 3.5 Haiku.

---

### 2.2. Kiểm tra điều kiện tiên quyết (Prerequisites)

Hãy mở Terminal (PowerShell trên Windows hoặc Bash trên macOS/Linux) để kiểm tra các công cụ nền tảng.

#### ⚡ Lệnh Terminal kiểm tra Node.js, npm và Git:
```powershell
node --version
npm --version
git --version
```

#### 🔬 Giải phẫu chi tiết lệnh Terminal:
- `node`: Gọi trình thực thi JavaScript Runtime Node.js.
  - `--version`: Cờ yêu cầu in ra phiên bản hiện tại (Yêu cầu $\ge$ v18.0.0).
- `npm`: Gọi trình quản lý gói mặc định đi kèm Node.js (Node Package Manager).
  - `--version`: Cờ kiểm tra phiên bản npm.
- `git`: Gọi trình quản lý phiên bản Git.
  - `--version`: Cờ in phiên bản Git (Claude Code cần Git để theo dõi diff và kiểm soát thay đổi).

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
> Đây là các lệnh kiểm tra thông số môi trường (read-only), không làm thay đổi hệ thống tệp tin.
```text
(Hệ thống in phiên bản v20.x, v10.x, v2.x ra màn hình console, không thay đổi thư mục)
```

---

### 2.3. Cài đặt Claude Code CLI toàn cục từ Registry chính thức

Chúng ta sẽ cài đặt gói nhị phân chính thức `@anthropic-ai/claude-code`.

#### ⚡ Lệnh Terminal cài đặt Claude Code toàn cục:
```powershell
npm install -g @anthropic-ai/claude-code
```

#### 🔬 Giải phẫu chi tiết lệnh Terminal:
- `npm`: Trình quản lý package của Node.js.
- `install`: Lệnh con (subcommand) ra lệnh tải và cài đặt package từ npm public registry (`https://registry.npmjs.org/`).
- `-g`: Viết tắt của `--global`. Cờ này hướng dẫn npm cài đặt package vào thư mục binary toàn cục của hệ điều hành thay vì thư mục `node_modules` cục bộ, giúp bạn có thể gõ lệnh `claude` ở bất kỳ thư mục nào trong terminal.
- `@anthropic-ai/claude-code`: Định danh package chính thức (Scoped Package) của tập đoàn Anthropic.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
C:\Users\<Tên_User>\AppData\Roaming\npm\
├── 📄 claude                 <-- [MỚI TẠO chính xác từ lệnh npm install -g] Shell script thực thi cho Unix/Bash
├── 📄 claude.cmd             <-- [MỚI TẠO] Batch script thực thi cho Windows Command Prompt
├── 📄 claude.ps1             <-- [MỚI TẠO] PowerShell script thực thi cho Windows PowerShell
└── 📁 node_modules/
    └── 📁 @anthropic-ai/
        └── 📁 claude-code/   <-- [MỚI TẠO] Toàn bộ mã nguồn đã biên dịch của Claude Code CLI
```

---

### 2.4. Thiết lập định danh bảo mật và Biến môi trường (API Key)

Để Claude Code có thể tương tác với máy chủ Anthropic, bạn cần cung cấp `ANTHROPIC_API_KEY`.  
Tuyệt đối không lưu cứng (hardcode) API Key vào mã nguồn dự án để tránh rò rỉ lên GitHub. Chúng ta cấu hình trực tiếp vào biến môi trường cấp User của hệ điều hành.

#### ⚡ Lệnh Terminal cấu hình API Key vĩnh viễn trong PowerShell:
```powershell
[System.Environment]::SetEnvironmentVariable('ANTHROPIC_API_KEY', 'sk-ant-api03-xxxx-your-key-here', 'User')
```

#### 🔬 Giải phẫu chi tiết lệnh Terminal:
- `[System.Environment]`: Lớp tiện ích quản lý môi trường hệ thống trong .NET Framework của Microsoft.
- `::SetEnvironmentVariable(...)`: Phương thức tĩnh (Static Method) ghi giá trị biến môi trường vào Windows Registry.
  - `'ANTHROPIC_API_KEY'`: Tên biến môi trường mà SDK của Anthropic và Claude Code bắt buộc tìm kiếm khi khởi chạy.
  - `'sk-ant-api03-...'`: Chuỗi bí mật API Key của bạn lấy từ bảng điều khiển Anthropic Console.
  - `'User'`: Phạm vi áp dụng cho người dùng hiện tại (lưu vào Registry `HKEY_CURRENT_USER\Environment`), đảm bảo biến này tồn tại vĩnh viễn qua các phiên khởi động lại máy mà không làm ảnh hưởng đến các tài khoản khác.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
(Windows Registry: HKEY_CURRENT_USER\Environment\ANTHROPIC_API_KEY được ghi nhận)
```

---

### 2.5. Khởi tạo Hiến pháp dự án (`CLAUDE.md`)

Mỗi khi Claude Code bước vào một thư mục làm việc mới, tệp tin đầu tiên nó đọc để tiếp nhận toàn bộ bối cảnh dự án chính là `CLAUDE.md`.

#### ⚡ Lệnh Terminal khởi tạo file `CLAUDE.md`:
```powershell
New-Item -ItemType File -Path .\CLAUDE.md -Force
```

#### 🔬 Giải phẫu chi tiết lệnh Terminal:
- `New-Item`: Cmdlet tiêu chuẩn của PowerShell dùng để khởi tạo tài nguyên mới trên ổ đĩa.
- `-ItemType File`: Định dạng tài nguyên cần tạo là một tệp tin (File).
- `-Path .\CLAUDE.md`: Đường dẫn vị trí tệp tin tại thư mục hiện hành (`.\`).
- `-Force`: Cưỡng chế tạo mới hoặc ghi đè nếu file đã tồn tại mà không ném cảnh báo.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
agent-skills/
└── 📄 CLAUDE.md              <-- [MỚI TẠO chính xác từ lệnh New-Item] File Hiến pháp ngữ cảnh của dự án
```

---

### 2.6. Kiểm tra tính toàn vẹn và chẩn đoán sức khỏe hệ thống (Health Check)

Claude Code tích hợp sẵn công cụ bác sĩ chẩn đoán (`doctor`) để quét toàn bộ hệ thống mạng, quyền hạn tệp tin, và tính hợp lệ của API Key.

#### ⚡ Lệnh Terminal chạy chẩn đoán:
```powershell
claude doctor
```

#### 🔬 Giải phẫu chi tiết lệnh Terminal:
- `claude`: Lệnh gọi trình điều hành Claude Code CLI.
- `doctor`: Lệnh con thực hiện bài kiểm tra sức khỏe toàn diện:
  1. Kiểm tra kết nối HTTPS đến máy chủ Anthropic (`https://api.anthropic.com`).
  2. Xác thực tính hợp lệ của token API Key.
  3. Kiểm tra quyền ghi/đọc của Terminal và phiên bản Git hiện hành.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
C:\Users\<Tên_User>\.claude/
└── 📄 config.json            <-- [MỚI TẠO hoặc CẬP NHẬT] Ghi nhận trạng thái cấu hình hệ thống hợp lệ
```

[⬆️ Quay lại Mục lục](#mục-lục)

---

## 3. Cách sử dụng trong thực tế chi tiết

---

### 3.1. Giai đoạn 0: Thiết lập Hiến pháp dự án `CLAUDE.md` chuẩn công nghiệp

#### 🧠 Tư duy logic:
Tại sao `CLAUDE.md` lại là chìa khóa sống còn của Claude Code?  
Mô hình AI không có trí nhớ vĩnh viễn giữa các phiên làm việc. Nếu không có `CLAUDE.md`, mỗi lần bạn mở terminal lên, Claude sẽ như một "nhân viên mới vào công ty ngày đầu tiên": không biết dự án dùng package manager nào (`pnpm` hay `npm`), không biết lệnh chạy test là gì, và rất dễ sinh mã vi phạm các quy tắc đặt tên của team.  
`CLAUDE.md` chính là bản **Onboarding tài liệu hóa** giúp Claude tự động nạp toàn bộ luật chơi ngay ở giây đầu tiên.

#### 📝 Cấu hình thực tế file `CLAUDE.md`:
Mở file `CLAUDE.md` trong thư mục gốc và thiết lập nội dung chuẩn mực:

```markdown
# HIẾN PHÁP DỰ ÁN VÀ QUY CHUẨN KỸ THUẬT CHO CLAUDE

## 1. Lệnh Xây dựng và Kiểm thử (Build & Test Commands)
- Cài đặt thư viện: `pnpm install` (Tuyệt đối không dùng npm/yarn)
- Chạy môi trường phát triển: `pnpm dev`
- Kiểm tra kiểu dữ liệu: `pnpm tsc --noEmit`
- Chạy bộ kiểm thử tự động: `pnpm test`
- Chạy kiểm tra tiếp cận: `pnpm test:a11y`

## 2. Tiêu chuẩn Mã nguồn Bắt buộc (Code Style Guidelines)
- **Ngôn ngữ:** TypeScript Strict Mode (`noImplicitAny: true`).
- **Xử lý State:** Tuyệt đối không dùng cụm biến boolean rời rạc (`isLoading`, `isError`). Bắt buộc dùng Discriminated Unions (`status: 'idle' | 'loading' | 'success' | 'error'`).
- **CSS & UI:** Dùng Tailwind CSS kết hợp hàm trợ năng `cn()` từ `src/lib/utils.ts`. Không nối chuỗi template literal thủ công.
- **Tiếp cận (a11y):** Tuân thủ chuẩn WCAG 2.1 AA. Mọi nút bấm tương tác phải hỗ trợ phím Tab và nhãn `aria-label`.

## 3. Quy trình Làm việc (Workflow Protocol)
- **Chế độ Kế hoạch:** Với mọi tính năng mới hoặc tái cấu trúc, bắt buộc trình bày kế hoạch tóm tắt (Plan) và chờ người dùng xác nhận trước khi sửa file.
- **Chốt chặn phản biện:** Nếu đề bài có điểm mù hoặc thiếu thông tin về trường hợp biên (Edge Cases), bắt buộc đặt câu hỏi làm rõ trước khi thực thi.
```

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
agent-skills/
└── 📄 CLAUDE.md              <-- [CẬP NHẬT] Đã điền 3 điều khoản cốt lõi về lệnh thực thi, code style và protocol
```

---

### 3.2. Giai đoạn 1: Giao việc chuẩn 4 yếu tố & Kỹ thuật Phản biện (Challenge Protocol)

#### 🧠 Tư duy logic:
Tại sao "Vibe Prompting" (ra lệnh 1 dòng chung chung) luôn tạo ra code rác?  
Khi bạn bảo: *"Hãy làm cho tôi trang Dashboard"*, bạn đang ép AI phải tự đoán mò 90% các thông số còn lại. Kết quả là nó sẽ chọn ngẫu nhiên thư viện biểu đồ, tự bịa ra cấu trúc dữ liệu, và sinh ra giao diện không ăn khớp với hệ thống của bạn.  
Quy chuẩn Kỹ sư yêu cầu áp dụng **Khung 4 Yếu tố (Framework G-I-C-D)**:
1. **Goal (Mục tiêu):** Làm cái gì, giải quyết bài toán nghiệp vụ nào?
2. **Input (Dữ liệu đầu vào):** Dữ liệu mẫu lấy từ đâu, cấu trúc interface ra sao?
3. **Constraints (Ràng buộc):** Công nghệ bắt buộc, giới hạn thư viện, cấm dùng cái gì?
4. **Done Criteria (Tiêu chí hoàn thành):** Thế nào là xong? (Có test pass, không có lỗi linter).

Kết hợp cùng **Kỹ thuật Contrastive Prompting (Mẫu ưng vs Mẫu không ưng)** và **Chốt chặn phản biện (Challenge Gate)**.

#### ⚡ Mẫu câu lệnh thực tế gửi cho Claude Code:

```text
Tôi cần xây dựng tính năng Bộ đếm Thời gian Countdown Timer Component.

[1. MỤC TIÊU (GOAL)]
Tạo một component đếm ngược thời gian từ số giây cho trước về 0, có các nút: Bắt đầu, Tạm dừng, Đặt lại.

[2. DỮ LIỆU ĐẦU VÀO (INPUT)]
Props: { initialSeconds: number; onFinish?: () => void; autoStart?: boolean }

[3. RÀNG BUỘC (CONSTRAINTS)]
- Sử dụng React 19 + TypeScript.
- Dùng useReducer thay vì nhiều useState riêng lẻ.
- MẪU ƯNG (DO): Dùng custom hook useCountdown để tách biệt 100% logic khỏi giao diện hiển thị.
- MẪU KHÔNG ƯNG (DON'T): Tuyệt đối không dùng setInterval trần trong useEffect mà không dọn dẹp (cleanup function); không hardcode màu sắc bừa bãi.

[4. TIÊU CHÍ HOÀN THÀNH (DONE CRITERIA)]
- Viết file test với vitest kiểm tra đầy đủ: chạy đếm ngược đúng nhịp, tạm dừng không mất thời gian, và gọi hàm onFinish khi về 0.
- Lệnh `pnpm test` chạy thành công 100%.

CHỐT CHẶN PHẢN BIỆN: Trước khi viết bất kỳ dòng mã nào, nếu phát hiện điểm nào trong yêu cầu chưa tối ưu hoặc có nguy cơ gây rò rỉ bộ nhớ (memory leak), hãy chất vấn lại tôi trước!
```

#### 🔬 Cơ chế xử lý của Claude:
Nhận được prompt này, Claude sẽ không hấp tấp code ngay. Nó sẽ kích hoạt "Chốt chặn phản biện" và hỏi lại bạn:  
*"Thưa Tech Lead, nếu người dùng chuyển tab trình duyệt khiến trình duyệt giảm tốc độ chạy của background timer (tab throttling), bạn có muốn dùng kỹ thuật so sánh timestamp (`Date.now()`) thay vì trừ đều mỗi giây để đảm bảo thời gian chính xác tuyệt đối không?"*  
Đây chính là sự khác biệt giữa một thợ gõ phím và một Kỹ sư AI cao cấp.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
> Ở bước này, Claude phản biện trên màn hình chat, chuẩn hóa tư duy trước khi sinh file.
```text
(Workspace chưa bị thay đổi file - Tránh được 100% rác mã nguồn)
```

---

### 3.3. Giai đoạn 2: Tác chiến với Plan Mode (Bản vẽ thiết kế trước khi gõ mã)

#### 🧠 Tư duy logic:
Tại sao Kỹ sư Trưởng luôn bắt buộc bật **Plan Mode**?  
Trong một dự án thực tế, một tính năng có thể chạm vào 5-10 file (Schema, Model, Service, Route, UI, Test). Nếu để AI tự ý chỉnh sửa file ngay lập tức, nó có thể sửa xong file thứ nhất, đến file thứ ba thì nhận ra sai kiến trúc và quay lại sửa lại file thứ nhất. Điều này gây lãng phí token cực lớn và tạo ra các commit vỡ nát.  
**Plan Mode** buộc Claude phải đóng vai trò Software Architect: đưa ra bản vẽ chi tiết các file sẽ tạo, các hàm sẽ viết, mối quan hệ phụ thuộc giữa chúng. Chỉ khi bạn gõ duyệt (Approve), Claude mới được phép chạm vào ổ đĩa.

#### ⚡ Thao tác kích hoạt Plan Mode trong Claude Code:
Trong cửa sổ terminal Claude Code:
- Bấm tổ hợp phím: `Shift + Tab` (để chuyển từ chế độ Direct Execution sang **Plan Mode**).
- Hoặc gõ lệnh: `/plan` kèm theo yêu cầu tính năng.

#### 🔬 Cấu trúc Bản vẽ Kiến trúc mà Claude sẽ trả về để bạn duyệt:

```markdown
### ARCHITECTURAL BLUEPRINT: COUNTDOWN TIMER MODULE

1. Phân tích Tác động (Impact Analysis):
   - Module độc lập, không làm ảnh hưởng các components hiện hữu.
   
2. Danh sách tệp tin cần tạo mới:
   - `src/hooks/useCountdown.ts`: Custom hook chứa State Machine (idle | running | paused | finished).
   - `src/components/ui/countdown/Countdown.tsx`: Giao diện hiển thị nút bấm và đồng hồ.
   - `src/components/ui/countdown/Countdown.test.tsx`: Bài kiểm thử hành vi logic.
   - `src/components/ui/countdown/index.ts`: Barrel export.

3. Thuật toán cốt lõi:
   - Sử dụng `requestAnimationFrame` kết hợp Delta Time (hoặc `Date.now()`) để chống lệch nhịp khi trình duyệt rơi vào trạng thái ngủ.

Bạn có đồng ý với bản vẽ thiết kế này không? [Y/n]
```

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
Sau khi bạn duyệt `Y`:
```text
agent-skills/
└── 📁 src/
    ├── 📁 hooks/
    │   └── 📄 useCountdown.ts        <-- [MỚI TẠO chính xác từ quá trình thực thi Plan]
    └── 📁 components/ui/countdown/
        ├── 📄 Countdown.tsx          <-- [MỚI TẠO]
        ├── 📄 Countdown.test.tsx     <-- [MỚI TẠO]
        └── 📄 index.ts               <-- [MỚI TẠO]
```

---

### 3.4. Giai đoạn 3: Chiến thuật "Dùng Tướng và Dùng Quân" & Tối ưu hóa Context Window

#### 🧠 Tư duy logic:
Tại sao không nên dùng Claude 3.5 Opus hoặc Sonnet cho mọi tác vụ?  
- **Model cấp cao (Opus / Sonnet 3.5 - Vị Tướng):** Có năng lực suy luận trừu tượng phi thường, thích hợp cho việc lập kế hoạch kiến trúc (Plan Mode), phân tích các bug hóc búa, hoặc review thiết kế hệ thống. Tuy nhiên, giá token cao hơn và độ trễ phản hồi lớn hơn.
- **Model siêu nhẹ (Haiku 3.5 - Người Lính):** Tốc độ phản hồi cực nhanh (gần như tức thì), chi phí token siêu rẻ (tiết kiệm đến 80-90%). Cực kỳ thích hợp cho các tác vụ mang tính chất lặp lại cơ bắp: viết mã mock dữ liệu, đổi tên biến hàng loạt, bổ sung comment JSDoc, hoặc format code.

Song song đó, việc bảo vệ **Context Window (Cửa sổ Ngữ cảnh)** là yếu tố sống còn:
- Sau 15-20 lượt trao đổi, lịch sử chat sẽ đầy ắp các đoạn code cũ. Càng nhiều token thừa, AI càng dễ bị ảo giác và mất tập trung.
- Lệnh `/compact`: Yêu cầu Claude tóm tắt toàn bộ bối cảnh quan trọng, nén lịch sử và dọn sạch rác bộ nhớ.
- Lệnh `/btw`: Dùng khi bạn đang làm việc dở dang nhưng muốn hỏi một câu hỏi bên lề (ví dụ: *"Cú pháp regex kiểm tra UUID v4 viết thế nào nhỉ?"*). Lệnh `/btw` sẽ trả lời câu hỏi của bạn nhưng **không lưu câu hỏi đó vào Context dài hạn**, giữ cho bàn làm việc luôn ngăn nắp.

#### ⚡ Thao tác trong Claude Code Terminal:

```text
# Dọn dẹp nén bộ nhớ sau khi hoàn thành một module:
/compact

# Hỏi câu hỏi phụ không làm ô nhiễm context:
/btw "Trong TypeScript 5.x, cờ verbatimModuleSyntax khác gì so với isolatedModules?"
```

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
> Thao tác tác động vào bộ nhớ đệm RAM của phiên làm việc (Session Context Memory), giúp giảm tải token từ ~80k tokens xuống còn ~8k tokens.
```text
(Context Window được dọn sạch, phiên làm việc phục hồi tốc độ tối đa)
```

---

### 3.5. Giai đoạn 4: Phân quyền và điều phối Sub-agents xử lý đa nhiệm

#### 🧠 Tư duy logic:
Chuyện gì xảy ra nếu bạn yêu cầu Claude chính: *"Hãy rà soát bảo mật cho 200 file trong thư mục src/"*?  
Trợ lý chính sẽ đọc lần lượt 200 file, nhanh chóng làm cạn kiệt Context Window (200k tokens), và sau đó bắt đầu quên mất các chỉ dẫn ban đầu.  
**Kỹ thuật Senior: Phân quyền cho Sub-agents (Trợ lý phụ).**  
Claude chính sẽ tạo ra các tiến trình phụ chạy ngầm độc lập. Mỗi Sub-agent được giao một phân vùng thư mục riêng, tự đọc, tự phân tích, và chỉ gửi bản tóm tắt ngắn gọn cuối cùng về cho Claude chính.

```mermaid
flowchart TD
    MainAgent["👑 Claude Code (Primary Agent)"] -->|Phân việc Task 1| Sub1["👷 Sub-Agent A (Quét controllers/)"]
    MainAgent -->|Phân việc Task 2| Sub2["👷 Sub-Agent B (Quét services/)"]
    MainAgent -->|Phân việc Task 3| Sub3["👷 Sub-Agent C (Quét repositories/)"]
    Sub1 -->|Báo cáo tóm tắt 5 dòng| MainAgent
    Sub2 -->|Báo cáo tóm tắt 3 dòng| MainAgent
    Sub3 -->|Báo cáo tóm tắt 4 dòng| MainAgent
    MainAgent -->|Tổng hợp kết quả sạch sẽ| Lead["👤 Tech Lead"]
```

#### ⚡ Câu lệnh kích hoạt điều phối Sub-agent:

```text
Hãy khởi tạo 3 sub-agents độc lập chạy song song:
- Sub-agent 1: Quét toàn bộ file trong src/components để tìm các phần tử button thiếu thuộc tính aria-label.
- Sub-agent 2: Quét toàn bộ file trong src/hooks để kiểm tra các hook thiếu cleanup function trong useEffect.
- Sub-agent 3: Quét toàn bộ file css để tìm các class hardcode màu hex không dùng Design Tokens.
Yêu cầu: Mỗi sub-agent chỉ gửi lại danh sách file vi phạm và vị trí dòng, không dump toàn bộ code.
```

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
(Bộ nhớ chính được giữ nguyên vẹn, 3 tiến trình phụ tự hủy sau khi hoàn thành nhiệm vụ)
```

---

### 3.6. Giai đoạn 5: Giao khoán tự trị khép kín bằng lệnh `/goal` (Self-Healing Loop)

#### 🧠 Tư duy logic:
Đây là đỉnh cao của năng lực AI Agent: **Giao khoán tự trị (Autonomous Execution with Self-Healing)**.  
Thay vì bạn phải ngồi canh màn hình và gõ *"Tiếp tục đi", "Lỗi rồi sửa lại đi"*, bạn sử dụng lệnh `/goal`.  
Bạn thiết lập:
1. Đích đến cuối cùng.
2. Lệnh kiểm chứng (Test/Lint command).  
Claude sẽ tự động bước vào vòng lặp khép kín: **Viết code $\rightarrow$ Chạy Test $\rightarrow$ Nếu Thất bại $\rightarrow$ Đọc Stacktrace $\rightarrow$ Tự sửa mã $\rightarrow$ Chạy lại Test** cho đến khi tất cả các bài kiểm tra chuyển sang màu xanh (Green/Passed).

#### ⚡ Lệnh Terminal kích hoạt giao khoán tự trị:
Trong Claude Code:

```text
/goal "Xây dựng toàn bộ tính năng useCountdown và Countdown component. Tiêu chí hoàn thành duy nhất: Chạy lệnh `pnpm test src/components/ui/countdown` đạt 100% Pass, không có bất kỳ cảnh báo TypeScript nào khi chạy `pnpm tsc --noEmit`. Hãy tự động lặp lại quy trình sửa lỗi cho đến khi pass toàn bộ mà không cần dừng lại hỏi tôi."
```

#### 🔬 Cơ chế Vòng lặp Tự phục hồi (Self-Healing Engine):
1. **Lượt 1:** Claude tạo file `Countdown.tsx` và chạy test $\rightarrow$ Báo lỗi: *"Cannot find module Countdown.test.tsx"*.
2. **Lượt 2:** Claude tự động tạo file test `Countdown.test.tsx` và chạy test $\rightarrow$ Báo lỗi: *"Timer does not tick because fake timers were not advanced"*.
3. **Lượt 3:** Claude phân tích lỗi, thêm `vi.useFakeTimers()` và `vi.advanceTimersByTime(1000)` vào test $\rightarrow$ Chạy test: **4/4 Tests Passed**.
4. **Lượt 4:** Claude tự chạy `pnpm tsc --noEmit` $\rightarrow$ Không có lỗi kiểu dữ liệu.
5. **Hoàn thành:** Claude thông báo bàn giao kết quả cuối cùng.

#### 📂 MINH HỌA SỰ TIẾN HÓA CỦA THƯ MỤC:
```text
src/components/ui/countdown/
├── 📄 Countdown.tsx          <-- [TỰ ĐỘNG SỬA & HOÀN THIỆN qua vòng lặp /goal]
└── 📄 Countdown.test.tsx     <-- [TỰ ĐỘNG CẬP NHẬT test cases đạt chuẩn 100% Pass]
```

[⬆️ Quay lại Mục lục](#mục-lục)

---

## 4. Sổ tay lỗi kinh điển cần tránh (Lessons Learned)

Dưới đây là 5 sai lầm nguy hiểm nhất mà các lập trình viên thường mắc phải khi ứng dụng Claude Agent, kèm theo giải pháp chuẩn mực:

---

### ⚠️ Sai lầm 1: "Vibe Prompting" - Ra lệnh một dòng thiếu bối cảnh
- **Hiện tượng:** Gõ prompt cộc lốc: *"Viết cho tôi API đăng nhập"*.
- **Hậu quả:** Claude sẽ tự chọn thư viện ngẫu nhiên (chỗ dùng `jsonwebtoken`, chỗ dùng `jose`), dùng thuật toán băm yếu (MD5 hoặc bcrypt đời cũ), và cấu trúc mã không theo bất kỳ kiến trúc chuẩn nào của dự án.
- **Giải pháp:** Luôn áp dụng **Khung 4 Yếu tố (Goal - Input - Constraints - Done Criteria)** kết hợp cung cấp **Mẫu ưng** và **Mẫu không ưng**.

---

### ⚠️ Sai lầm 2: Bỏ mặc Context Window phình to dẫn đến "Ảo giác Amnesia"
- **Hiện tượng:** Giữ một phiên làm việc kéo dài suốt 3 ngày, hỏi hàng trăm câu hỏi từ chuyện vá lỗi CSS đến kiến trúc Database.
- **Hậu quả:** Khi số lượng token vượt quá 100k-150k tokens, khả năng chú ý (Attention Mechanism) của mô hình bị suy giảm nghiêm trọng. Claude bắt đầu "quên" các điều khoản đã cam kết trong `CLAUDE.md`, tự tiện sửa đè lên code của người khác và sinh ra code rác.
- **Giải pháp:** Sau khi hoàn thành trọn vẹn một tính năng, luôn chủ động gõ lệnh `/compact` hoặc khởi động một phiên làm việc mới bằng cách đóng và mở lại terminal.

---

### ⚠️ Sai lầm 3: Cấp quyền nguy hiểm mù quáng (`--dangerously-skip-permissions`)
- **Hiện tượng:** Để tránh việc Claude liên tục hỏi xin phép trước khi chạy lệnh shell, lập trình viên bật cờ bỏ qua kiểm tra quyền trên môi trường cá nhân không có sandbox.
- **Hậu quả:** Nếu Claude bị ảo giác hoặc nhận một câu lệnh xóa file nhầm, nó có thể chạy các lệnh phá hủy như `rm -rf /` hoặc xóa sạch dữ liệu database local mà không có cơ hội phục hồi.
- **Giải pháp:** Chỉ cho phép tự động chạy các lệnh an toàn (như `git status`, `pnpm test`, `ls`). Các lệnh ghi đè file hàng loạt hoặc can thiệp hệ thống bắt buộc phải duy trì cơ chế phê duyệt thủ công của con người (Human-in-the-loop).

---

### ⚠️ Sai lầm 4: Bỏ qua Plan Mode và cho phép sửa file trực tiếp
- **Hiện tượng:** Vừa nảy ra ý tưởng là yêu cầu Claude sửa code ngay trên các file chính.
- **Hậu quả:** Claude sửa dở dang một nửa file thì gặp lỗi kiến trúc, khiến toàn bộ mã nguồn của dự án rơi vào trạng thái gãy đổ (broken state), không thể compile hay chạy dev server được nữa.
- **Giải pháp:** Luôn kích hoạt **Plan Mode** (`Shift + Tab` hoặc `/plan`). Bản vẽ thiết kế phải được bạn xem xét và gõ duyệt thì mới cho phép máy móc bắt tay vào đục đẽo code.

---

### ⚠️ Sai lầm 5: Gõ phím thủ công dài dòng thay vì dùng Voice-to-Text
- **Hiện tượng:** Cố gắng gõ những câu prompt ngắn cụt lủn vì lười gõ bàn phím, khiến thông tin truyền tải bị thiếu hụt nghiêm trọng.
- **Hậu quả:** Con người gõ phím với tốc độ trung bình 40-60 từ/phút nhưng có thể nói với tốc độ 150-180 từ/phút. Gõ phím cụt làm mất đi toàn bộ các sắc thái nghiệp vụ, điều kiện biên và cảm quan mong muốn.
- **Giải pháp:** Sử dụng công cụ chuyển giọng nói thành văn bản (Voice Dictation - ví dụ phím tắt `Win + H` trên Windows hoặc tính năng Voice trên Mac). Nói liên tục trong 1 phút để truyền tải toàn bộ bối cảnh dự án, liếc mắt kiểm tra lại rồi bấm gửi.

[⬆️ Quay lại Mục lục](#mục-lục)

---

## 5. Bảng tra cứu nhanh lệnh claude-agent-master (Cheat Sheet)

### 📌 Bảng 1: Bảng tra cứu lệnh Terminal CLI (`claude-code`)

| Lệnh Terminal | Bóc tách cú pháp cốt lõi | Mục đích kỹ thuật | Tác động hệ thống |
| :--- | :--- | :--- | :--- |
| `npm install -g @anthropic-ai/claude-code` | `install -g`: Cài toàn cục<br>`@anthropic-ai/claude-code`: Gói chính thức | Cài đặt Claude Code CLI vào hệ điều hành | Tạo file thực thi `claude` trong PATH toàn cục |
| `claude` | Khởi chạy chương trình không tham số | Bắt đầu một phiên làm việc Agentic tương tác tại thư mục hiện hành | Đọc `CLAUDE.md`, nạp ngữ cảnh và mở giao diện prompt |
| `claude "câu lệnh"` | Truyền trực tiếp prompt qua tham số chuỗi | Chạy một tác vụ đơn lẻ (Single Shot) rồi tự động thoát | Thực thi tác vụ nhanh không cần duy trì session |
| `claude doctor` | Chẩn đoán sức khỏe hệ sinh thái | Kiểm tra kết nối mạng, API Key, quyền hạn thư mục và Git | In báo cáo kiểm tra tình trạng hệ thống |
| `claude --model claude-3-5-haiku-latest` | `--model`: Chỉ định model thực thi | Ép Claude Code sử dụng model siêu nhẹ để tiết kiệm token và tăng tốc | Giảm 90% chi phí token cho các tác vụ đơn giản |
| `claude --print` | In toàn bộ luồng suy luận ra stdout | Dùng cho việc tích hợp Claude Code vào CI/CD pipeline hoặc script shell | Xuất kết quả dạng văn bản thuần |

---

### 📌 Bảng 2: Bảng tra cứu các lệnh Slash Commands trong Claude Code

| Lệnh Slash | Khi nào nên sử dụng? | Chức năng thực thi | Lợi ích cho Kỹ sư |
| :--- | :--- | :--- | :--- |
| `/compact` | Sau khi hoàn thành một module hoặc khi chat quá 15 lượt | Nén toàn bộ lịch sử trò chuyện, loại bỏ code thừa | Giảm dung lượng token, ngăn chặn tình trạng ảo giác |
| `/btw <câu hỏi>` | Khi cần hỏi câu hỏi phụ ngoài lề về cú pháp/lý thuyết | Trả lời câu hỏi tức thì nhưng **không lưu vào ngữ cảnh dài hạn** | Giữ bàn làm việc (Context Window) luôn tinh gọn |
| `/plan` hoặc `Shift+Tab` | Trước khi xây dựng tính năng mới hoặc tái cấu trúc lớn | Chuyển sang Chế độ Kế hoạch: lập bản vẽ kiến trúc, không sửa file | Kiểm soát 100% kiến trúc trước khi tốn tài nguyên code |
| `/goal <mục tiêu>` | Khi muốn giao khoán trọn gói kèm bài test | Kích hoạt vòng lặp tự trị: code $\rightarrow$ test $\rightarrow$ tự sửa lỗi | Tự động hóa hoàn toàn, lập trình viên không cần ngồi canh |
| `/cost` | Cuối buổi làm việc hoặc sau tác vụ nặng | Hiển thị chi tiết số lượng token đã dùng và chi phí USD đã tiêu tốn | Quản lý ngân sách dự án minh bạch, chính xác |
| `/init` | Khi mới vào một repository hoàn toàn mới | Quét toàn bộ repo và tự động sinh ra file `CLAUDE.md` ban đầu | Tiết kiệm 30 phút nghiên cứu cấu trúc dự án |
| `/clear` | Khi muốn đổi sang một bài toán nghiệp vụ hoàn toàn khác | Xóa sạch bộ nhớ phiên hiện tại, bắt đầu trang giấy mới | Tránh lẫn lộn ngữ cảnh giữa 2 tính năng không liên quan |

---

### 📌 Bảng 3: Ma trận Prompt Giao việc Chuẩn mực 4 Yếu tố (Template)

```text
================================================================================
          KHUNG GIAO VIỆC KỸ THUẬT CHUẨN MỰC (FRAMEWORK G-I-C-D)
================================================================================

1. [GOAL - MỤC TIÊU]:
   - Tôi muốn xây dựng/tối ưu: [Tên tính năng hoặc vấn đề cần xử lý]
   - Giá trị mang lại: [Giải thích ngắn gọn tại sao cần làm]

2. [INPUT - DỮ LIỆU ĐẦU VÀO]:
   - Cấu trúc dữ liệu / Schema: [Định nghĩa TypeScript Interface hoặc JSON mẫu]
   - Đường dẫn các file liên quan: [src/.../a.ts, src/.../b.ts]

3. [CONSTRAINTS - RÀNG BUỘC]:
   - Công nghệ bắt buộc: [React 19 / TypeScript Strict / Tailwind CSS]
   - MẪU ƯNG (DO): [Liệt kê các thói quen tốt: Clean code, tách hook, dùng cn()]
   - MẪU KHÔNG ƯNG (DON'T): [Liệt kê các điều cấm kỵ: Cấm dùng any, cấm boolean flag soup]

4. [DONE CRITERIA - TIÊU CHÍ HOÀN THÀNH]:
   - Bộ kiểm thử: [Phải có unit test chạy bằng vitest pass 100%]
   - Linter: [Chạy `pnpm tsc --noEmit` và `pnpm lint` không có lỗi]

🛑 CHỐT CHẶN PHẢN BIỆN (CHALLENGE GATE):
   "Trước khi tạo hoặc sửa bất kỳ file nào, hãy bật Plan Mode để trình bày bản vẽ
    kiến trúc. Nếu có bất kỳ câu hỏi nào về logic biên (edge cases), hãy hỏi lại tôi ngay!"
================================================================================
```

[⬆️ Quay lại Mục lục](#mục-lục)
