.. triển khai module:

==============================
Triển khai module hợp đồng thông minh
==============================

Hướng dẫn này sẽ chỉ cho bạn cách triển khai module hợp đồng thông minh *on-chain* và
cách đặt tên cho nó.

Chuẩn bị
===========

Đảm bảo rằng bạn đang: :ref:`running a node<run-a-node>` bằng cách sử dụng phiên bản mới nhất: ref:`Concordium software <downloads>'và
bạn có: ref:`smart-contract module <setup-tools>` sẵn sàng được triển khai.

Vì việc triển khai module hợp đồng thông minh được thực hiện dưới dạng giao dịch,
bạn cũng sẽ cần thiết lập ``concordium-client'' với tài khoản
đủ GTU để thanh toán cho giao dịch.

.. Ghi chú::

   Chi phí của giao dịch phụ thuộc vào quy mô của hợp đồng thông minh
   module. ``concordium-client'' hiển thị chi phí và yêu cầu xác nhận
   trước khi nó thực hiện bất kỳ giao dịch nào.

Triển khai
==========

Để triển khai module hợp đồng thông minh ``my_module.wasm'' bằng tài khoản
với tên tài khoản, hãy chạy lệnh sau:

.. code-block:: console

   $concordium-client module deploy my_module.wasm --sender account_name

.. Ghi chú::

   Tùy chọn --sender có thể bị bỏ qua nếu tài khoản "mặc định" được sử dụng. Để ngắn gọn, chúng tôi sẽ làm như vậy trong phần sau.

Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Module successfully deployed with reference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'

Ghi lại tham chiếu module vì nó được sử dụng khi tạo hợp đồng thông minh
các trường hợp.

.. Xem thêm::

   Để biết hướng dẫn về cách khởi tạo hợp đồng thông minh từ một module đã triển khai, hãy xem
   :ref:`initialize-contract`.

   Để biết thêm thông tin về tham chiếu module, hãy xem :ref:`reference-on-chain`.

.. đặt tên cho module:

Đặt tên một module
===============

Một module có thể được đặt một bí danh cục bộ hoặc * name* để tham chiếu đến nó
dễ dàng hơn.
Tên chỉ được lưu trữ cục bộ bởi ``concordium-client'', và không
hiển thị trên chuỗi.

.. Xem thêm::

   Để được giải thích về cách thức và vị trí của tên và các cài đặt cục bộ khác
   được lưu trữ, xem :ref:`local-settings`.

Để thêm tên trong quá trình triển khai, tham số ``--name'' được sử dụng.
Ở đây, chúng tôi đang đặt tên cho module là ``my_deployed_module'':

.. code-block:: console

   $concordium-client module deploy my_module.wasm --name my_deployed_module

Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Module successfully deployed with reference: '9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2' (my_deployed_module).

Các module cũng có thể được đặt tên bằng lệnh ``name''.
Để đặt tên cho một module đã triển khai với tham chiếu
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2'' như
``some_deployed_module'', chạy lệnh sau:

.. code-block:: console

   $concordium-client module name \
             9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
             --name some_deployed_module

Đầu ra phải tương tự như sau:

.. code-block:: console

   Module reference 9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 was successfully named 'some_deployed_module'.