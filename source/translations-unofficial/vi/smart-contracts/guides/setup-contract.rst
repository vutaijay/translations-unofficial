.. Điểm nổi bật :: toml

.. Thiết lập hợp đồng:

===================================
Thiết lập một dự án hợp đồng thông minh
===================================

Hợp đồng thông minh trong Rust được viết như một thùng thư viện Rust thông thường.
Thư viện sau đó được biên dịch thành Wasm bằng cách sử dụng Rust target
``wasm32-unknown-unknown'' và vì nó chỉ là một thư viện Rust, chúng ta có thể sử dụng
Cargo_ để quản lý người phụ thuộc.

Để thiết lập một dự án hợp đồng thông minh mới, trước tiên hãy tạo một thư mục dự án. Phía trong
thư mục dự án chạy như sau trong một thiết bị đầu cuối:

.. code-block:: console

   $cargo init --lib

Điều này sẽ thiết lập một dự án thư viện Rust mặc định bằng cách tạo một vài tệp và
các thư mục.
Thư mục của bạn bây giờ sẽ chứa tệp ``Cargo.toml'' và ``src''
thư mục và một số tệp ẩn.

Để có thể xây dựng Wasm, chúng ta cần thông báo cho hàng hóa đúng ``crate-type''.
Điều này được thực hiện bằng cách thêm phần sau vào file ``Cargo.toml'' ::

   [lib]
   crate-type = ["cdylib", "rlib"]

Thêm thư viện tiêu chuẩn hợp đồng thông minh
==========================================

Bước tiếp theo là thêm ``concordium-std'' làm phụ thuộc.
Nó là một thư viện cho Rust chứa các macro thủ tục và các hàm cho
viết các hợp đồng thông minh nhỏ và hiệu quả.

Thư viện được thêm vào bằng cách mở ``Cargo.toml`` và thêm dòng
``concordium-std = "*" `` (tốt nhất là thay thế `*` bằng phiên bản mới nhất của `concordium-std`_) trong
phần ``[dependencies]`` ::

   [dependencies]
   concordium-std = "0,4"

Tài liệu về thùng có thể được tìm thấy trên docs.rs.

.. Ghi chú::

   Nếu bạn muốn sử dụng phiên bản sửa đổi của thùng này, bạn sẽ phải sao chép
   kho lưu trữ với ``concordium-std`` và có điểm phụ thuộc
   thay vào đó, bằng cách thêm phần sau vào ``Cargo.toml`` ::

      [dependencies]
      concordium-std = {path = "./path/to/concordium-std"}

.. Rust: https://www.rust-lang.org/
.. Cargo: https://doc.rust-lang.org/cargo/
.. rustup: https://rustup.rs/
.. repository: https://gitlab.com/Concordium/concordium-std
.. docs.rs: https://docs.rs/crate/concordium-std/
.. `concordium-std`: https://docs.rs/crate/concordium-std/

Đó là nó! Bây giờ bạn đã sẵn sàng để phát triển hợp đồng thông minh của riêng mình.