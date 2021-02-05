.. Rust: https://www.rust-lang.org/
.. Cargo: https://doc.rust-lang.org/cargo/
.. rust-analyzer: https://github.com/rust-analyzer/rust-analyzer

.. Biên dịch module:

====================================
Biên dịch module hợp đồng thông minh Rust
====================================

Hướng dẫn này sẽ chỉ cho bạn cách biên dịch module hợp đồng thông minh được viết bằng Rust để
một module Wasm.

Chuẩn bị
===========

Đảm bảo đã cài đặt Rust and Cargo và cài đặt ``wasm32-unknown-unknown``
target, cùng với ``goods-concordium`` và mã nguồn Rust cho một
hợp đồng module, bạn muốn biên dịch.

.. Xem thêm::

   Để biết hướng dẫn về cách cài đặt các công cụ dành cho nhà phát triển, hãy xem
   :ref: `setup-tools`.

Biên dịch sang Wasm
=================

Để giúp xây dựng các module hợp đồng thông minh và tận dụng các tính năng
chẳng hạn như: ref:`contract schemas <contract-schema>`, chúng tôi khuyên bạn nên sử dụng
Công cụ ``row-concordium`` để xây dựng hợp đồng thông minh với Rust.

Để xây dựng một hợp đồng thông minh, hãy chạy:

.. code-block:: console

   $cargo concordium build

Điều này sử dụng Cargo để xây dựng, nhưng chạy các tối ưu hóa hơn nữa đối với kết quả.

.. Xem thêm::

   Để xây dựng lược đồ cho module hợp đồng thông minh, một số :ref:`further
   cần chuẩn bị <build-schema> `.

.. Ghi chú::

   Cũng có thể biên dịch trực tiếp bằng Cargo_ bằng cách chạy:

   .. code-block:: console

      $cargo build --target=wasm32-unknown-unknown [--release]

   Lưu ý rằng ngay cả với ``--release``, module Wasm được sản xuất bao gồm
   thông tin gỡ lỗi.

Xóa thông tin máy chủ khỏi bản dựng
====================================

module Wasm đã biên dịch có thể chứa thông tin từ việc xây dựng máy chủ
nhị phân; thông tin chẳng hạn như đường dẫn tuyệt đối của thư mục ``.carg ''.

Đối với hầu hết mọi người, đây không phải là thông tin nhạy cảm, nhưng điều quan trọng là
nhận thức về nó.

Trên Linux, các đường dẫn có thể được kiểm tra bằng cách chạy:

.. code-block:: console

   strings contract.wasm | grep /home/

.. rubric:: The solution

Giải pháp lý tưởng sẽ là loại bỏ hoàn toàn đường dẫn này, nhưng
Rất tiếc nói chung nó không phải là một nhiệm vụ tầm thường.

Có thể khắc phục sự cố bằng cách sử dụng tiền tố ``--remap-path-prefix`` khi biên dịch hợp đồng.
Trên các hệ thống giống Unix cờ có thể được chuyển trực tiếp đến ``cargo concordium``
lời gọi sử dụng biến môi trường ``RUSTFLAGS``:

.. code-block:: console

   $RUSTFLAGS="--remap-path-prefix=$HOME=" cargo concordium build

Điều này sẽ thay thế đường dẫn trang chủ của người dùng bằng chuỗi trống. Các con đường khác có thể
được ánh xạ theo cách tương tự. Nói chung, sử dụng ``--remap-path-prefix = from = to``
sẽ ánh xạ ``from '' thành ``to`` ở đầu bất kỳ đường dẫn nhúng nào.

Cờ cũng có thể được đặt vĩnh viễn trong tệp ``.cargo/config`` trong
thùng, dưới phần xây dựng:

.. code-block:: toml

   [build]
   rustflags = ["--remap-path-prefix=/home/<user>="]

trong đó `<user>` nên được thay thế bằng việc người dùng xây dựng module wasm.

Cảnh báo
-------

Ở trên có thể sẽ không khắc phục được sự cố nếu thành phần ``gỉ-src``
được cài đặt cho chuỗi công cụ Rust. Thành phần này cần thiết bởi một số công cụ Rust
chẳng hạn như bộ phân rust-analyzer.

.. Xem thêm::

   Báo cáo sự cố với tiền tố `--remap-path-prefix`` và ``gỉ-src``
   https://github.com/rust-lang/rust/issues/73167