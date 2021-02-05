.. Trả lời cho:
..
.. - Tại sao phải sử dụng lược đồ?
.. - Lược đồ là gì?
.. - Sử dụng lược đồ ở đâu?
.. - Một lược đồ được nhúng như thế nào?
.. - Tôi nên nhúng hay ghi vào tệp?
..

.. _` phần tùy chỉnh`: https://webassembly.github.io/spec/core/appcharge/custom.html
.. _`Triển khai trong Rust`: https://github.com/Concordium/concordium-contracts-common/blob/main/src/schema.rs

.. _Hợp đồng-lược đồ:

======================
Lược đồ hợp đồng thông minh
======================

Một lược đồ hợp đồng thông minh là một mô tả về cách thể hiện các byte trong một
biểu diễn có cấu trúc. Nó có thể được sử dụng bởi các công cụ bên ngoài khi hiển thị
trạng thái của một phiên bản hợp đồng thông minh và để chỉ định các tham số bằng cách sử dụng
biểu diễn có cấu trúc, chẳng hạn như JSON.

.. Xem thêm::

   Để được hướng dẫn về cách xây dựng module cho module hợp đồng thông minh trong
   Rust, xem: ref: `build-schema`.

Tại sao sử dụng module hợp đồng
=========================

Dữ liệu trên blockchain, chẳng hạn như trạng thái của một phiên bản và các thông số được truyền
đến các chức năng init và nhận, được tuần tự hóa thành một chuỗi các byte.
Việc tuần tự hóa được tối ưu hóa cho hiệu quả, thay vì khả năng đọc của con người.

.. todo::

   Cân nhắc viết lại mục này vì có thể hơi khó
   hiểu biết; đặc biệt, có thể chỉ nói rằng để thuận tiện, người dùng
   có thể chuyển dữ liệu chưa được công nghệ hóa vào một hàm miễn là chúng cũng cung cấp
   lược đồ giải thích cách (de) tuần tự hóa dữ liệu.

Thông thường những byte này có cấu trúc và cấu trúc này được biết đến với
hợp đồng như một phần của các chức năng của hợp đồng, nhưng bên ngoài các chức năng này, nó
có thể khó hiểu về các byte. Điều này đặc biệt xảy ra khi
kiểm tra trạng thái phức tạp của một phiên bản hợp đồng hoặc khi vượt qua phức tạp
tham số cho một chức năng hợp đồng thông minh. Trong trường hợp sau, các byte phải
hoặc được tuần tự hóa từ dữ liệu có cấu trúc hoặc được viết thủ công.

Giải pháp để tránh phân tích cú pháp thủ công các byte là nắm bắt thông tin này
trong * lược đồ hợp đồng thông minh *, mô tả cách tạo cấu trúc từ
byte và có thể được sử dụng bởi các công cụ bên ngoài.

.. Ghi chú::

    The ``concordium-client`` tool can use a schema to
   :ref:`serialize JSON parameters<init-passing-parameter-json>`
   và để giải mã trạng thái của các phiên bản hợp đồng sang JSON.

Sau đó, lược đồ được nhúng vào một module hợp đồng thông minh được triển khai
vào chuỗi, hoặc được ghi vào một tệp và được chuyển đi vòng ngoài chuỗi.

Bạn nên nhúng hay ghi vào một tập tin?
====================================

Liệu một lược đồ hợp đồng nên được nhúng hay ghi vào một tệp tin phụ thuộc vào
hoàn cảnh của bạn.

Nhúng lược đồ vào module hợp đồng thông minh sẽ phân phối lược đồ
cùng với hợp đồng đảm bảo lược đồ chính xác đang được sử dụng và cũng
cho phép bất kỳ ai sử dụng trực tiếp. Nhược điểm là module hợp đồng thông minh
trở nên lớn hơn về kích thước và do đó tốn kém hơn để triển khai.
Nhưng trừ khi hợp đồng thông minh sử dụng các loại rất phức tạp cho trạng thái và
tham số, kích thước của lược đồ có thể không đáng kể so với
kích thước của chính hợp đồng thông minh.

Có lược đồ trong một tệp riêng biệt cho phép bạn có lược đồ mà không
trả thêm byte khi triển khai.
Nhược điểm là thay vào đó bạn phải phân phối tệp lược đồ thông qua một số
kênh khác và đảm bảo rằng người dùng hợp đồng đang sử dụng đúng tệp với
hợp đồng thông minh.

Định dạng lược đồ
=================

.. todo::

   Làm rõ liệu chúng ta có nói về * bất kỳ * lược đồ trừu tượng nào mà người dùng có thể triển khai hay không,
   hoặc một lược đồ cụ thể do Concordium cung cấp. Sau đó, chỉ nói về cái này hay cái kia,
   hoặc ít nhất tách biệt rõ ràng cuộc thảo luận của những người.

Một lược đồ có thể chứa

- thông tin cấu trúc cho một module hợp đồng thông minh
- mô tả về trạng thái hợp đồng thông minh
- các tham số cho các chức năng init và nhận của một hợp đồng thông minh.

Mỗi mô tả này được gọi là * kiểu lược đồ *. Các loại lược đồ luôn
tùy chọn để đưa vào một lược đồ.

Hiện tại, các loại module được hỗ trợ được lấy cảm hứng từ những gì thường được sử dụng trong
ngôn ngữ lập trình Rust:

.. code-block:: rust

   enum Type {
       Unit,
       Bool,
       U8,
       U16,
       U32,
       U64,
       I8,
       I16,
       I32,
       I64,
       Amount,
       AccountAddress,
       ContractAddress,
       Timestamp,
       Duration,
       Pair(Type, Type),
       List(SizeLength, Type),
       Set(SizeLength, Type),
       Map(SizeLength, Type, Type),
       Array(u32, Type),
       Struct(Fields),
       Enum(List (String, Fields)),
   }

   enum Fields {
       Named(List (String, Type)),
       Unnamed(List Type),
       Empty,
   }



Ở đây, `` SizeLength '' mô tả số byte được sử dụng để mô tả chiều dài
của loại độ dài thay đổi, chẳng hạn như `` List ''.

.. code-block:: rust

   enum SizeLength {
       One,
       Two,
       Four,
       Eight,
   }

Để tham khảo về cách một kiểu lược đồ được tuần tự hóa thành các byte, chúng tôi tham khảo
trình đọc `thực hiện trong Rust`_.

.. _contract-schema-which-để chọn:

Nhúng các lược đồ trên chuỗi
==========================

Các lược đồ được nhúng vào các module hợp đồng thông minh bằng cách sử dụng `
section`_ tính năng của module Wasm.
Điều này cho phép module Wasm bao gồm một phần byte được đặt tên, điều này không
ảnh hưởng đến ngữ nghĩa của việc chạy module Wasm.

Tất cả các lược đồ được thu thập và thêm vào một phần tùy chỉnh có tên
`` concordium-schema-v1 ''.
Bộ sưu tập này là một danh sách các cặp, chứa tên của hợp đồng được mã hóa
trong UTF-8 và các byte lược đồ hợp đồng.