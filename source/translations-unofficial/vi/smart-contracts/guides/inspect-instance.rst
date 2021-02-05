.. _inspect-instance:

=======================================
Kiểm tra phiên bản hợp đồng thông minh
=======================================

Hướng dẫn này sẽ chỉ cho bạn cách kiểm tra phiên bản hợp đồng thông minh.
Kiểm tra một phiên bản sẽ cho bạn biết tên, chủ sở hữu, tham chiếu module, số dư,
trạng thái và các chức năng nhận:

Chuẩn bị
=========

Đảm bảo rằng bạn đang :ref:`running a node<run-a-node>` bằng cách sử dụng phiên bản mới nhất: ref:`Concordium software <downloads>'và bạn có
phiên bản hợp đồng thông minh trên chuỗi để kiểm tra.

.. Xem thêm::
   Để biết cách triển khai module hợp đồng thông minh, hãy xem :ref:`deploy-module` và
   cách tạo một thể hiện :ref: `initialize-contract`.

Kiểm tra
=========

Để kiểm tra hoặc hiển thị thông tin về phiên bản hợp đồng thông minh với
chỉ mục địa chỉ ``0'', chạy lệnh sau:

.. code-block:: console

   $concordium-client contract show 0

Đầu ra phải tương tự như sau:

.. code-block:: console

   Contract:        my_contract
   Owner:           '4Lh8CPhbL2XEn55RMjKii2XCXngdAC7wRLL2CNjq33EG9TiWxj' (default)
   ModuleReference: 'd121f262f3d34b9737faa5ded2135cf0b994c9c32fe90d7f11fae7cd31441e86'
   Balance:         0.000000 GTU
   State:
       {
           "first_field": 0,
           "second_field": 42
       }
   Functions:
    - receive_one
    - receive_two

.. Xem thêm::

   Để biết thêm thông tin về địa chỉ phiên bản hợp đồng, hãy xem
   :ref:`reference-on-chain.

Mức độ chi tiết của việc kiểm tra phụ thuộc vào việc lệnh ``show'' có
truy cập vào một :ref:`contract schema <contract-schema>`.
Nếu lược đồ được nhúng, nó sẽ được sử dụng ngầm định.
Nếu không, một lược đồ có thể được cung cấp bằng cách sử dụng tham số``--schema/path/to/schema.bin''.

.. Ghi chú::

   Tệp giản đồ được cung cấp bằng tham số ``--schema'' sẽ được ưu tiên
   qua một lược đồ nhúng.

.. Xem thêm::

   :ref:`Đọc thêm về lý do và cách sử dụng lược đồ hợp đồng thông minh <contract-schema>'.