.. _Cài đặt các công cụ:

=============================
Cài đặt các công cụ để phát triển hợp đồng thông minh.
=============================

Trước khi có thể bắt đầu phát triển các hợp đồng thông minh, chúng tôi cần thiết lập
Môi trường.

Rust và Cargo
==============

Đầu tiên, hãy cài đặt rustup`, sẽ cài đặt cả Rust và Cargo trên
máy của bạn.
Sau đó sử dụng ``rustup'' để cài đặt Wasm target, được sử dụng để biên dịch:

.. code-block:: console

   $rustup target add wasm32-unknown-unknown

Cargo Concordium
================

Cargo Concordium là công cụ để phát triển các hợp đồng thông minh cho Concordium
chuỗi khối.
Nó có thể được sử dụng cho :ref:`compiling <compile-module>` và
:ref:`testing <unit-test-contract>` hợp đồng thông minh và cho phép các tính năng như
:ref:`xây dựng các lược đồ hợp đồng <build-schema>`.

.. todo::

   Thêm liên kết để thử nghiệm và lược đồ.

Cargo Concordium được phân phối như một phần của gói :ref: `Concordium software <downloads>`.
Công cụ nên được đặt trong PATH của bạn.

Để biết mô tả về cách sử dụng Cargo Concordium chạy:

.. code-block:: console

   $cargo concordium --help

Concordium software
===================

Công cụ để triển khai và tương tác với các hợp đồng thông minh là
:ref:`concordium-client <concordium_client>`. Nó được phân phối như một phần của
:ref:`Concordium software<downloads <downloads> `.

.. Ghi chú::

   Để triển khai các modules hợp đồng thông minh và tương tác với chuỗi, hãy đảm bảo
   mà bạn là :ref: `đang chạy một nút <run-a-node>`.

.. Rust: https://www.rust-lang.org/
.. Cargo: https://doc.rust-lang.org/cargo/
.. cài đặt rustup: https://rustup.rs/
.. crates.io: https://crates.io/