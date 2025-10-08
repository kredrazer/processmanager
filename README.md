# 🧠 ProcessManagerOS - Operating System Mini Project

## 📘 Giới thiệu
Dự án **ProcessManagerOS** là một hệ điều hành mini được mô phỏng nhằm hiểu sâu về các khái niệm nền tảng trong **Hệ điều hành (Operating System)** như:
- Quản lý tiến trình (Process Management)
- Lập lịch (Scheduling)
- Ngắt (Interrupts)
- Gọi hệ thống (System Calls)
- Đa nhiệm đơn giản (Cooperative Multitasking)

Mục tiêu là **xây dựng từ bootloader đến kernel** có thể quản lý và chuyển đổi giữa các tiến trình giả lập.

---

## 🗂️ Cấu trúc thư mục dự án

```
ProcessManagerOS/
│
├── boot/
│   ├── boot.asm          # Bootloader (nạp kernel)
│   └── print.asm         # Hàm in ký tự ra màn hình (int 0x10)
│
├── kernel/
│   ├── kernel.c          # Entry chính của kernel
│   ├── process.c         # Quản lý tiến trình (PCB)
│   ├── scheduler.c       # Lập lịch tiến trình
│   ├── syscall.c         # Bảng system call
│   ├── timer.c           # Bộ định thời (timer tick)
│   └── interrupts.c      # Handler ngắt giả lập
│
├── include/
│   ├── kernel.h
│   ├── process.h
│   ├── scheduler.h
│   ├── syscall.h
│   ├── timer.h
│   └── types.h
│
├── tools/
│   ├── build_image.sh    # Tạo file .img
│   └── run_qemu.sh       # Chạy QEMU
│
├── Makefile
├── README.md
└── PROJECT_PLAN.md
```

---

# 🚀 PROJECT_PLAN.md

## 🎯 Mục tiêu tổng thể
- Xây dựng hệ điều hành nhỏ có thể **chạy trong QEMU**.
- Hiểu rõ cơ chế **bootloader – kernel – scheduler – process – interrupt**.
- Áp dụng lý thuyết môn **Hệ điều hành (Operating Systems)**.

---

## 🧩 Giai đoạn 1: Nền tảng hệ thống (System Foundation)

### ✅ Mục tiêu
Tạo được file `.img` chạy được trong QEMU, in ra dòng chữ từ kernel.

### 🔧 Công việc chi tiết

| Thành phần | Nhiệm vụ | Người phụ trách | Study Task |
|-------------|-----------|------------------|-------------|
| **Bootloader (boot.asm)** | Tạo bootloader nạp kernel từ sector 2 | Tuấn | Học về Real Mode, BIOS Interrupt, FAT12 |
| **print.asm** | Viết hàm in ký tự ra màn hình | Tuấn | Tìm hiểu interrupt 0x10 và text mode |
| **kernel.c** | Tạo entry chính, in chuỗi “Kernel started” | Triệu | Hiểu về GDT, chuyển từ ASM sang C |
| **build_image.sh** | Ghép boot + kernel thành file .img | Minh | Học lệnh `dd`, `objcopy`, và `ld` |
| **run_qemu.sh** | Script chạy thử trên QEMU | Tuấn | Tìm hiểu `qemu-system-x86_64` và các tham số |

### 🧪 Kết quả mong đợi
- Tạo thành công `os.img`
- Khi chạy `./tools/run_qemu.sh`, màn hình hiển thị:  
  **"Kernel started successfully!"**

---

## 🧩 Giai đoạn 2: Quản lý tiến trình & lập lịch (Process & Scheduling)

### ✅ Mục tiêu
Kernel có thể tạo nhiều tiến trình, chuyển đổi CPU giữa các tiến trình, và xử lý ngắt thời gian (timer tick).

### 🔧 Công việc chi tiết

| Thành phần | Nhiệm vụ | Người phụ trách | Study Task |
|-------------|-----------|------------------|-------------|
| **process.c** | Xây dựng cấu trúc PCB (Process Control Block) | Tuấn | Ôn về PCB, Stack riêng từng process |
| **scheduler.c** | Tạo bộ lập lịch Round Robin | Tuyên | Học thuật toán lập lịch cơ bản (RR, FCFS) |
| **timer.c** | Cấu hình timer tick (giả lập) | Tuấn | Tìm hiểu PIT 8253 và ngắt IRQ0 |
| **interrupts.c** | Xử lý ngắt và lưu/khôi phục context | Tuấn | Hiểu cơ chế IDT, ISR, context switch |
| **syscall.c** | Tạo bảng system call | Tuấn | Nắm rõ cơ chế trap và int 0x80 |

### 🧪 Kết quả mong đợi
- Hệ thống có ít nhất **2 tiến trình chạy luân phiên**.
- Có thể in ra console:
  ```
  [Process 1] Running...
  [Process 2] Running...
  ```
- Sau mỗi timer tick, tiến trình được chuyển đổi.

---

## ⚙️ Công cụ sử dụng
- **NASM** – assembler cho bootloader.
- **GCC (Cross-compiler)** – biên dịch code kernel.
- **QEMU** – giả lập hệ thống.
- **Makefile** – tự động hóa build.

---

## 📖 Kế hoạch học tập (Study Roadmap)

| Chủ đề | Nội dung cần học | Giai đoạn |
|--------|------------------|------------|
| BIOS & Bootloader | Cách BIOS nạp boot sector, FAT12 | Giai đoạn 1 |
| QEMU & Image | Cách tạo và nạp `.img` | Giai đoạn 1 |
| Assembly cơ bản | Real Mode, Segment, Interrupt | Giai đoạn 1 |
| Kernel & Memory Map | Cách bootloader nạp kernel | Giai đoạn 1 |
| PCB & Scheduling | Process Control Block, Round Robin | Giai đoạn 2 |
| Interrupt & Context Switch | ISR, IDT, Timer tick | Giai đoạn 2 |
| System Call | int 0x80, bảng syscall | Giai đoạn 2 |

---

## 🧱 Kết quả cuối cùng
- File `os.img` chạy được trên QEMU.
- Có tiến trình giả lập luân phiên nhau.
- Hiểu sâu các phần chính của hệ điều hành thực tế.

---

🧑‍💻 **Tác giả:** Nguyễn Anh Tuấn  
📅 **Ngày tạo:** {datetime.now().strftime("%Y-%m-%d")}


