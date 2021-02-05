.. _ví dụ tương tác:

=======================================
Tương tác với một phiên bản hợp đồng thông minh
=======================================

Hướng dẫn này sẽ chỉ cho bạn cách tương tác với một phiên bản hợp đồng thông minh,
có nghĩa là kích hoạt một hàm nhận, khả năng, cập nhật trạng thái
ví dụ.

Chuẩn bị
===========

Đảm bảo rằng bạn đang :ref:`running a node<run-a-node>` bằng cách sử dụng phiên bản mới nhất: ref:`Concordium software <downloads>'và bạn có
phiên bản hợp đồng thông minh trên chuỗi để kiểm tra.

.. Xem thêm::
   Để biết cách triển khai module hợp đồng thông minh, hãy xem :ref:`deploy-module` và
   cách tạo một thể hiện :ref:`initialize-contract`.

Vì các tương tác với hợp đồng thông minh là các giao dịch, bạn cũng nên thực hiện
đảm bảo thiết lập ``concordium-client'' với tài khoản có đủ GTU để thanh toán
cho các giao dịch.

.. Ghi chú::

   Chi phí của giao dịch này phụ thuộc vào kích thước của các tham số được gửi đến
   hàm nhận và độ phức tạp của chính hàm.

Sự tương tác
===========

Để cập nhật một phiên bản có chỉ mục địa chỉ ``0'' bằng cách sử dụng không tham số
nhận hàm ``my_receive'' trong khi cho phép sử dụng tới 1000 năng lượng,
chạy lệnh sau:

.. code-block:: console

   $concordium-client contract update 0 --func my_receive --energy 1000

Nếu thành công, ta sẽ thấy tương tự như sau:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_receive'.

Truyền tham số ở định dạng JSON
---------------------------------

Một tham số ở định dạng JSON có thể được chuyển nếu :ref:`smart contract schema
<contract-schema>`được cung cấp dưới dạng tệp hoặc được nhúng trong module.
Lược đồ được sử dụng để tuần tự hóa JSON thành nhị phân.

.. Xem thêm::

   : ref:`Đọc thêm về lý do và cách sử dụng lược đồ hợp đồng thông minh
   <contract-schema> `.

Để cập nhật một phiên bản có chỉ mục địa chỉ ``0'' bằng cách sử dụng hàm nhận
``my_parameter_receive'' với tệp tham số ``my_parameter.json'' trong JSON
định dạng, chạy lệnh sau:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-json my_parameter.json


Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

Nếu không, một lỗi mô tả sự cố sẽ được hiển thị.
Các lỗi phổ biến được mô tả trong phần tiếp theo.

.. Xem thêm::

   Để biết thêm thông tin về địa chỉ phiên bản hợp đồng, hãy xem
   :ref:`reference-on-chain`.

.. Ghi chú::

   Nếu tham số được cung cấp ở định dạng JSON không phù hợp với loại
   được chỉ định trong lược đồ, một thông báo lỗi sẽ được hiển thị. Ví dụ:

      .. code-block:: console

       Error: Could not decode parameters from file 'my_parameter.json' as JSON:
       Expected value of type "UInt64", but got: "hello".
       In field 'first_field'.
       In {
           "first_field": "hello",
           "second_field": 42
       }.

.. Ghi chú::

   Nếu một module nhất định không chứa một lược đồ nhúng, nó có thể được cung cấp
   bằng tham số ``--schema/path/to/schema.bin''.

.. Ghi chú::

   GTU cũng có thể được chuyển sang hợp đồng trong quá trình cập nhật bằng cách sử dụng
   Tham số ``- amount AMOUNT''.

Truyền tham số ở định dạng nhị phân
-----------------------------------

Khi chuyển các tham số ở định dạng nhị phân,
:ref:`contract schema <contract-schema>` là không cần thiết.

Để cập nhật một phiên bản có chỉ mục địa chỉ ``0'' bằng cách sử dụng hàm nhận
``my_parameter_receive'' với tệp tham số ``my_parameter.bin'' ở dạng nhị phân
định dạng, chạy lệnh sau:

.. code-block:: console

   $concordium-client contract update 0 --func my_parameter_receive \
            --energy 1000 \
            --parameter-bin my_parameter.bin

Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Successfully updated contract instance {"index":0,"subindex":0} using the function 'my_parameter_receive'.

.. Xem thêm::

   Để biết thông tin về cách làm việc với các tham số trong hợp đồng thông minh, hãy xem
    :ref:`working-with-parameters`.

.. _parameter_cursor ():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get (): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read (): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8