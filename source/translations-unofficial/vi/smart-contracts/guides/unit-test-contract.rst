.. _unit-test-contract:

============================
Đơn vị thử nghiệm một hợp đồng trong Rust
============================

Hướng dẫn này sẽ chỉ cho bạn cách viết các bài kiểm tra đơn vị cho một hợp đồng thông minh được viết bằng
Rust.
Để kiểm tra module Wasm hợp đồng thông minh, hãy xem: ref:`local-simulate`.

Hợp đồng thông minh trong Rust được viết dưới dạng thư viện và chúng tôi có thể kiểm tra đơn vị nó như một
thư viện bằng cách chú thích các hàm có thuộc tính ``# [test]``.

.. code-block:: rust

    // contract code
    ...

    #[cfg(test)]
    mod test {

        #[test]
        fn some_test() { ... }

        #[test]
        fn another_test() { ... }
    }


Việc chạy thử nghiệm có thể được thực hiện bằng cách sử dụng ``cargo``:

.. code-block:: console

   $cargo test

Theo mặc định, lệnh này biên dịch hợp đồng và kiểm tra mã máy cho
mục tiêu cục bộ của bạn (rất có thể là ``x86_64'') và chạy chúng.
Loại thử nghiệm này có thể hữu ích trong quá trình phát triển ban đầu và thử nghiệm chức năng đúng.
Để kiểm tra toàn diện, điều quan trọng là phải liên quan đến nền tảng mục tiêu, tức là
`Wasm32`.
Có một số khác biệt nhỏ giữa các nền tảng, có thể thay đổi
hành vi của một hợp đồng.
Một sự khác biệt là về kích thước của con trỏ, trong đó `Wasm32` sử dụng bốn byte
trái ngược với tám, phổ biến cho hầu hết các nền tảng.

Viết bài kiểm tra đơn vị
==================

Bài kiểm tra đơn vị thường tuân theo cấu trúc ba phần, trong đó bạn: thiết lập một số
trạng thái, chạy một số đơn vị mã và đưa ra xác nhận về trạng thái và đầu ra của
mật mã.

Nếu các hàm hợp đồng được viết bằng cách sử dụng ``# [init (..)]`` hoặc
``# [accept (..)]``, chúng tôi có thể kiểm tra các chức năng này trực tiếp trong bài kiểm tra đơn vị.

.. code-block:: rust

   use concordium_std::*;

   #[init(contract = "my_contract", payable, enable_logger)]
   fn contract_init(
      ctx: &impl HasInitContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
   ) -> InitResult<State> { ... }

   #[receive(contract = "my_contract", name = "my_receive", payable, enable_logger)]
   fn contract_receive<A: HasActions>(
      ctx: &impl HasReceiveContext<()>,
      amount: Amount,
      logger: &mut impl HasLogger,
      state: &mut State,
   ) -> ReceiveResult<A> { ... }

Kiểm tra sơ khai cho các đối số của hàm có thể được tìm thấy trong một module con của
``concordium-std`` được gọi là ``test_infra Structure``.

.. Xem thêm::

   Để biết thêm thông tin và ví dụ, hãy xem tài liệu thùng của
   concordium-std.

.. todo::

   Hiển thị thêm về cách viết bài kiểm tra đơn vị

Chạy thử nghiệm trong Wasm
=====================

Biên dịch các bài kiểm tra sang mã máy gốc là đủ cho hầu hết các trường hợp, nhưng nó
cũng có thể biên dịch các bài kiểm tra thành Wasm và chạy chúng bằng cách sử dụng chính xác
trình thông dịch được sử dụng bởi các nút.
Điều này làm cho môi trường thử nghiệm gần hơn với môi trường chạy trên chuỗi và có thể
trong một số trường hợp bắt nhiều lỗi hơn.

Công cụ phát triển ``row-concordium`` bao gồm một trình chạy thử nghiệm cho Wasm,
sử dụng cùng một trình thông dịch Wasm như một trình thông dịch được vận chuyển trong các nút Concordium.

.. Xem thêm::

   Để biết hướng dẫn về cách cài đặt ``row-concordium``, hãy xem: ref:`setup-tools`.

Bài kiểm tra đơn vị phải được chú thích bằng ``# [concordium_test]`` thay vì
``# [test]`` và chúng tôi sử dụng ``# [concordium_cfg_test]`` thay vì ``# [cfg (test)]``:

.. code-block:: rust

   // contract code
   ...

   #[concordium_cfg_test]
   mod test {

       #[concordium_test]
       fn some_test() { ... }

       #[concordium_test]
       fn another_test() { ... }
   }

``# [concordium_test]`` macro thiết lập các thử nghiệm của chúng tôi để chạy trong Wasm, khi
``concordium-std`` được biên dịch với tính năng ``wasm-test``, và nếu không
trở lại hoạt động giống như ``# [test]``, nghĩa là vẫn có thể chạy
kiểm tra đơn vị nhắm mục tiêu mã gốc bằng cách sử dụng ``kiểm tra hàng hóa``.

Tương tự, macro ``# [concordium_cfg_test]`` bao gồm module của chúng tôi khi xây dựng
``concordium-std`` với ``wasm-test`` sẽ hoạt động giống như ``# [test]``,
cho phép chúng tôi kiểm soát thời điểm đưa các thử nghiệm vào bản dựng.

Các thử nghiệm hiện có thể được xây dựng và chạy bằng cách sử dụng:

.. code-block:: console

   $cargo concordium test

Lệnh này biên dịch các bài kiểm tra cho Wasm với tính năng ``wasm-test`` được bật
cho ``concordium-std`` và sử dụng bộ chạy thử nghiệm từ ``row-concordium``.

.. cảnh báo::

   Thông báo lỗi từ ``panic! '', Và do đó cũng có các biến thể khác nhau
   của ``assert!``, được *không* hiển thị khi biên dịch sang Wasm.

   Thay vào đó, hãy sử dụng các biến thể ``fail!`` Và ``claim!'' Để xác nhận khi
   đang kiểm tra, vì những thông báo này báo lại thông báo lỗi cho người chạy thử *before*
   kiểm tra lỗi.
   Cả hai đều là một phần của ``concordium-std``.

.. todo::

   Sử dụng liên kết concordium-std: docs.rs/concordium-std khi thùng được xuất bản.