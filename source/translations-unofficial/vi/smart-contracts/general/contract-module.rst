.. _hợp đồng - module:

======================
Các module hợp đồng thông minh
======================

Hợp đồng thông minh được triển khai trên chuỗi trong * module hợp đồng thông minh *.

.. Ghi chú::

   module hợp đồng thông minh thường được gọi đơn giản là * module *.

Một module có thể chứa một hoặc nhiều hợp đồng thông minh, cho phép chia sẻ mã
trong số các hợp đồng và có thể tùy chọn chứa: :ref:`contract schemas
<contract-schema>`.

.. graphviz::
   :align: center
   :caption: A smart contract module containing two smart contracts.

   digraph G {
       subgraph cluster_0 {
           node [fillcolor=white, shape=note]
           label = "Module";
           "Crowdfunding";
           "Escrow";
       }
   }

module phải độc lập và chỉ có danh sách nhập khẩu hạn chế
cho phép tương tác với chuỗi.
Chúng được cung cấp bởi môi trường chủ và có sẵn cho
hợp đồng bằng cách nhập một module có tên là `` concordium ''.

.. Xem thêm::

   Kiểm tra: ref: `host-functions` để tham khảo đầy đủ.

Ngôn ngữ trên chuỗi
=================

Trên chuỗi khối Concordium, ngôn ngữ hợp đồng thông minh là một tập hợp con của `Web
Assembly`_ (nói ngắn gọn là Wasm) được thiết kế để trở thành một trình biên dịch di động
mục tiêu và được chạy trong môi trường hộp cát. Điều này hữu ích vì 
hợp đồng thông minh sẽ được điều hành bởi những người làm bánh trong mạng lưới, những người không nhất thiết phải am hiểu code.
Wasm là một ngôn ngữ cấp thấp và việc viết bằng tay là không thực tế. Thay vào đó một
có thể viết các hợp đồng thông minh bằng một ngôn ngữ cấp cao hơn, sau đó
được biên dịch sang Wasm.

.. _wasm-giới hạn:

Hạn chế
-----------

.. todo::

   Thêm các giới hạn khác, chẳng hạn như phần bắt đầu ...

Môi trường blockchain rất đặc biệt theo nghĩa là mỗi nút phải
có thể thực hiện hợp đồng theo cùng một cách, sử dụng chính xác
lượng tài nguyên. Nếu không, các nút sẽ không đạt được sự đồng thuận về
trạng thái của chuỗi. Vì lý do này, hợp đồng thông minh cần phải được viết trong một
tập hợp con của Wasm.

Dấu phẩy động
^^^^^^^^^^^^^^^^^^^^^^^

Mặc dù Wasm có hỗ trợ dấu phẩy động, nhưng hợp đồng thông minh là
không được phép sử dụng chúng. Lý do cho điều này là các dấu phẩy động Wasm
có thể có giá trị `` NaN`` ("không phải số") đặc biệt mà việc xử lý có thể dẫn đến không xác định.

Hạn chế áp dụng tĩnh, có nghĩa là các hợp đồng thông minh không thể chứa
các loại dấu phẩy động, cũng như không thể chứa bất kỳ hướng dẫn nào liên quan đến
các giá trị điểm.


Triển khai
==========

Triển khai một module vào chuỗi có nghĩa là gửi mã phân đoạn module dưới dạng
giao dịch với mạng Concordium. Nếu * hợp lệ * giao dịch này sẽ
bao gồm trong một khối. Giao dịch này, như mọi giao dịch khác, có
chi phí liên quan. Chi phí dựa trên kích thước của mã bytecode và được tính phí
để kiểm tra tính hợp lệ của module và lưu trữ trên chuỗi.

Bản thân việc triển khai không thực thi
hợp đồng thông minh. Để thực thi, trước tiên người dùng phải tạo một * thể hiện * của hợp đồng.

.. Xem thêm::

   Xem: ref: `contract-instance` để biết thêm thông tin.

.. _smart-Contract-on-chain:

.. _smart-Contract-on-the-chain:

.. _contract-on-chain:

.. _contract-on-the-chain:

Hợp đồng thông minh trên chuỗi
===========================

Hợp đồng thông minh trên chuỗi là một tập hợp các chức năng được xuất từ ​​một
module. Cơ chế cụ thể được sử dụng cho việc này là xuất `Web Assembly`_
phần. Hợp đồng thông minh phải xuất một chức năng để khởi tạo mới
phiên bản và có thể xuất không hoặc nhiều hàm để cập nhật phiên bản.

Vì module hợp đồng thông minh có thể xuất các chức năng cho nhiều
hợp đồng, chúng tôi liên kết các chức năng bằng cách sử dụng lược đồ đặt tên:

- `` init_ <contract-name> '': Hàm khởi tạo hợp đồng thông minh phải
  bắt đầu bằng `` init_ '' theo sau là tên của hợp đồng thông minh. Hợp đồng
  chỉ được bao gồm các ký tự chữ và số hoặc dấu câu ASCII và không được
  được phép chứa ký hiệu ``. ''.

- `` <contract-name>. <receive- functions-name> ``: Các hàm để tương tác với
  hợp đồng thông minh được bắt đầu bằng tên hợp đồng, theo sau là dấu ``. '' và
  tên cho chức năng. Tương tự như đối với hàm init, tên hợp đồng không được phép
  để chứa ký hiệu ``. ''.

.. Ghi chú::

   Nếu bạn phát triển các hợp đồng thông minh sử dụng Rust và `` concordium-std '',
   macro thủ tục `` # [init (...)] `` và `` # [nhận (...)] `` thiết lập
   sơ đồ đặt tên đúng.

.. _Web Assembly: https://webassembly.org/