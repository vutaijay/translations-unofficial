.. danh sách các loại triển khai SchemaType: https://docs.rs/concordium-contracts-common/latest/concordium_contracts_common/schema/trait.SchemaType.html#foreign-impls
.. xây dựng lược đồ:

=======================
Xây dựng một lược đồ hợp đồng
=======================

Hướng dẫn này sẽ chỉ cho bạn cách xây dựng một lược đồ hợp đồng thông minh, cách xuất nó
vào một tệp và/hoặc nhúng lược đồ vào module hợp đồng thông minh, tất cả đều sử dụng
``cargo-concordium``.

Chuẩn bị
===========

Trước tiên, hãy đảm bảo rằng bạn đã cài đặt ``row-concordium`` và nếu chưa thì hướng dẫn
:ref:`setup-tools` sẽ giúp bạn.

Chúng tôi cũng cần mã nguồn Rust của hợp đồng thông minh mà bạn muốn xây dựng
lược đồ.

Thiết lập hợp đồng cho một lược đồ
==================================

Để xây dựng một lược đồ hợp đồng, trước tiên chúng ta phải chuẩn bị
hợp đồng xây dựng lược đồ.

Chúng tôi có thể chọn các phần của hợp đồng thông minh của chúng tôi để đưa vào lược đồ.
Các tùy chọn bao gồm một lược đồ cho trạng thái hợp đồng và/hoặc cho mỗi
các tham số của hàm init và hàm nhận.

Mỗi kiểu chúng ta muốn đưa vào lược đồ phải triển khai ``SchemaType``
đặc điểm. Điều này đã được thực hiện cho tất cả các kiểu cơ sở và một số kiểu khác (xem ``danh sách các kiểu triển khai SchemaType`_).
Đối với hầu hết các trường hợp khác, nó cũng có thể đạt được tự động, bằng cách sử dụng
``#[derive(SchemaType)]``::

   #[derive(SchemaType)]
   struct SomeType {
       ...
   }

Việc triển khai đặc điểm ``SchemaType'' theo cách thủ công chỉ yêu cầu chỉ định một
hàm, là một getter cho một ``schema::Type'', về cơ bản mô tả
cách biểu diễn kiểu này dưới dạng byte và cách biểu diễn nó.

.. todo::

   Tạo một ví dụ cho thấy cách triển khai thủ công ``SchemaType'' và liên kết
   đến nó từ đây.

Bao gồm cả trạng thái hợp đồng
------------------------

Để tạo và bao gồm lược đồ cho trạng thái hợp đồng, chúng tôi chú thích loại
với ``#[contract_state(contract = ...)]`` macro::

   #[contract_state(contract = "my_contract")]
   #[derive(SchemaType)]
   struct MyState {
       ...
   }

Hoặc đơn giản hơn nữa nếu trạng thái hợp đồng thuộc loại đã triển khai ``SchemaType'', ví dụ:u32::

    #[contract_state(contract = "my_contract")]
   type State = u32;

Bao gồm các tham số chức năng
-----------------------------

Để tạo và bao gồm lược đồ cho các tham số cho init và
nhận các chức năng, chúng tôi đặt thuộc tính ``parameter`` tùy chọn cho
``# [init (..)]`` - và ``#[receive(..)]``-macro::

    #[derive(SchemaType)]
   enum InitParameter { ... }

   #[derive(SchemaType)]
   enum ReceiveParameter { ... }

   #[init(contract = "my_contract", parameter = "InitParameter")]
   fn contract_init<...> (...){ ... }

   #[receive(contract = "my_contract", name = "my_receive", parameter = "ReceiveParameter")]
   fn contract_receive<...> (...){ ... }

Xây dựng lược đồ
===================

Bây giờ, chúng tôi đã sẵn sàng xây dựng lược đồ thực tế bằng cách sử dụng ``cargo-concordium'' và chúng tôi
có các tùy chọn để nhúng lược đồ và/hoặc ghi lược đồ vào một tệp.

.. Xem thêm::

   Để biết thêm về cái để chọn, hãy xem
   :ref:`here <contract-schema-which-to-choose>`.

Nhúng lược đồ
--------------

Để nhúng lược đồ vào module hợp đồng thông minh, chúng tôi thêm
``--schema-embed`` to the build command

.. code-block:: console

   $cargo concordium build --schema-embed

Nếu thành công, đầu ra của lệnh sẽ cho bạn biết tổng kích thước của
lược đồ theo byte.

Xuất tệp lược đồ
------------------------

Để xuất lược đồ thành một tệp, chúng ta có thể sử dụng ``--schema-out=FILE''
trong đó ``FILE'' là một đường dẫn của tệp để tạo:

.. code-block:: console

   $cargo concordium build --schema-out="/some/path/schema.bin"