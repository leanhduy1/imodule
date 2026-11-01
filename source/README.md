# Mã nguồn lab

Mỗi tệp `.zip` ở đây là **toàn bộ mã nguồn** của một bài thực hành Labtainer — đủ để dựng lại
image từ đầu, không phụ thuộc vào bản image đã đẩy lên Docker Hub.

Khác với các tệp `imodule-*.tar` ở thư mục gốc của repo: tệp `imodule` chỉ chứa phần cấu hình
(`config/`, `instr_config/`, `docs/`) đủ để **chạy** một bài mà image đã có sẵn trên registry.
Muốn **sửa và dựng lại** image thì phải có mã nguồn trong thư mục này.

## Cấu trúc mỗi gói

```
<tên-lab>/
├── config/            start.config, parameter.config, *-home_tar.list
├── instr_config/      results.config, goals.config, pregrade.sh
├── docs/              read_first.txt và tài liệu bài
├── dockerfiles/       Dockerfile.<lab>.<container>.student
└── <tên-container>/   một thư mục cho mỗi máy trong bài
    ├── _bin/          fixlocal.sh, treataslocal
    ├── _system/       tệp hệ thống chép vào image
    ├── home_tar/home.tar
    ├── sys_tar/sys.tar
    └── (dữ liệu bài: .pcap, .php, .sql, .py …)
```

Gói không đặt mật khẩu.

## Cách dựng lại một bài

```bash
cd $LABTAINER_DIR/labs
unzip <tên-lab>.zip
cd $LABTAINER_DIR/scripts/labtainer-student
./bin/rebuild -f <tên-lab>
```

Đã kiểm chứng thực tế với `net-wireshark-nat`: giải nén gói, `rebuild` dựng xong image tại chỗ và
`labtainer` khởi động container từ chính image vừa dựng.

## Lưu ý

- **`REGISTRY` trong `config/start.config` trỏ tới `labattt2025`**, tài khoản Docker Hub của khoa.
  Cả 42 image đã được chuyển sang đó. Bản cũ trên tài khoản cá nhân `leanh1` vẫn còn để dự phòng
  nhưng không còn được dùng.
- **Cấu hình nằm trong image, không chỉ trong imodule.** Lúc khởi động, container đọc
  `~/.local/config/parameter.config` có sẵn trong image, không đọc bản trong imodule. Nếu đổi tên
  bài hay tên container mà chỉ sửa imodule thì `HASH_REPLACE` sẽ nhắm vào container không tồn tại,
  dòng `MSV:` không được thay, và mục `personalization` luôn bị chấm N. Đổi tên thì phải dựng lại
  image (hoặc vá `.local/config` trong image) cho khớp.
- Tên bài đã đổi sang bộ tiền tố `net-` / `ops-`. Tên bài được nhúng vào tên thư mục container,
  tên tệp `Dockerfile.*` và `*-home_tar.list`, nên khi đổi tên phải sửa đồng bộ cả bốn chỗ.
- Các bài dẫn xuất từ bộ Labtainers của Trung tâm An ninh mạng và Tác chiến không gian mạng,
  Trường Sau đại học Hải quân Hoa Kỳ (Naval Postgraduate School).
