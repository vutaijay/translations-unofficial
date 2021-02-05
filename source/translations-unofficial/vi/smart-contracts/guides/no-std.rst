.. _no-std:

======================
Xây dựng bằng cách sử dụng ``no_std''
======================

Hướng dẫn này chỉ ra cách bật ``no_std'' cho hợp đồng thông minh trong rust của bạn,
có khả năng làm giảm kích thước của module Wasm thu được vài kilobyte.

Chuẩn bị
===========

Biên dịch ``concordium-std'' mà không có tính năng ``std'' yêu cầu sử dụng công cụ rust nightly, 
có thể được cài đặt bằng cách sử dụng ``rustup'':

.. code-block:: console

   $rustup toolchain install nightly

Thiết lập module cho ``no_std''
===============================

Thư viện ``concordium-std'' cho thấy tính năng ``std'', cho phép sử dụng
của thư viện tiêu chuẩn rust.
Tính năng này được bật theo mặc định.

Để vô hiệu hóa nó, người ta chỉ cần tắt các tính năng mặc định cho
``concordium-std'' trong phần phụ thuộc của module của bạn.

.. code-block:: rust

   [dependencies]
   concordium-std = { version: "=0.2", default-features = false }

Để có thể chuyển đổi giữa có và không có std, hãy thêm ``std'' vào
module riêng, cho phép tính năng ``std'' của ``concordium-std'':

.. code-block:: rust

   [features]
   std = ["concordium-std/std"]

Đây là thiết lập của các ví dụ hợp đồng thông minh, trong đó ``std'' cho mỗi
module hợp đồng thông minh được bật theo mặc định.

Xây dựng module
================

Để sử dụng chuỗi công cụ nightly, hãy thêm ``+nightly'' ngay sau
``cargo'':

.. code-block:: console

   $cargo +nightly concordium build

Nếu bạn muốn tắt các tính năng mặc định của module hợp đồng thông minh của riêng mình,
bạn có thể chuyển thêm đối số cho ``cargo'':

.. code-block:: console

   $cargo +nightly concordium build -- --no-default-features