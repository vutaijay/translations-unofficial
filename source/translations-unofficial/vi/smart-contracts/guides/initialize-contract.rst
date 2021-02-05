.. khởi tạo hợp đồng:

====================================
Khởi tạo phiên bản hợp đồng thông minh
====================================

Hướng dẫn này sẽ chỉ cho bạn cách khởi tạo hợp đồng thông minh từ một
module hợp đồng thông minh với các tham số ở định dạng JSON hoặc nhị phân.
Ngoài ra, nó sẽ hiển thị cách đặt tên cho một thể hiện.

Chuẩn bị
===========

Đảm bảo rằng bạn đang :ref:`running a node<run-a-node>` bằng cách sử dụng phiên bản mới nhất: ref:`Concordium software <downloads>'và rằng bạn có một
contract :ref:`deployed <deploy-module>` trong một số module trên chuỗi.

Vì khởi tạo hợp đồng thông minh là một giao dịch, bạn cũng nên đảm bảo
thiết lập ``concordium-client'' với một tài khoản có đủ GTU để thanh toán
giao dịch.

.. Ghi chú::

   Chi phí của giao dịch này phụ thuộc vào kích thước của các tham số được gửi đến
   hàm init và độ phức tạp của chính hàm.

Khởi tạo
=========

Để khởi tạo một phiên bản của hợp đồng thông minh không tham số ``my_contract''
từ một module đã triển khai với tham chiếu
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2'' trong khi
cho phép sử dụng tới 1000 NRG, chạy lệnh sau:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --sender my_account \
            --contract my_contract \
            --energy 1000

Nếu thành công, đầu ra sẽ tương tự như sau:

.. todo::

   Update address format to ``<i, s>`` from ``{"index": i, "subindex": s}``
   (throughout the documentation).

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Nhìn thấy thông báo này có nghĩa là một phiên bản hợp đồng trên chuỗi mới đã được tạo
với địa chỉ được hiển thị.

.. Xem thêm::

   Để hiểu sâu hơn về khởi tạo hợp đồng, hãy xem
   :ref:`contract-instances-init-on-chain`.

   Để biết thêm thông tin về tham chiếu module và địa chỉ phiên bản,
   xem :ref:`reference-on-chain`.

   Sử dụng trực tiếp các tham chiếu module có thể không thuận tiện; đặt tên cho chúng, hãy xem
   :ref:`name-a-module`.

.. _init-passing-parameter-json:

Truyền tham số ở định dạng JSON
---------------------------------

Một tham số ở định dạng JSON có thể được chuyển nếu :ref:`smart contract schema
<contract-schema>` được cung cấp dưới dạng tệp hoặc được nhúng trong module.
Lược đồ được sử dụng để tuần tự hóa JSON thành nhị phân.

.. Xem thêm::

   :ref:`Đọc thêm về lý do và cách sử dụng lược đồ hợp đồng thông minh <contract-schema>'.

   :ref:`Các tham số cũng có thể được truyền ở định dạng nhị phân <init-passing-parameter-bin>`.

Để khởi tạo một phiên bản của hợp đồng ``my_parameter_contract'' từ
module với tài liệu tham khảo
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2'' với a
tệp tham số ``my_parameter.json'' ở định dạng JSON, hãy chạy lệnh sau:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-json my_parameter.json

Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

Nếu không, một lỗi mô tả sự cố sẽ được hiển thị.
Các lỗi phổ biến được mô tả trong phần tiếp theo.

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

   GTU cũng có thể được chuyển sang một phiên bản hợp đồng trong quá trình khởi tạo
   bằng cách sử dụng tham số ``--amount AMOUNT ''.


.. _init-pass-parameter-bin:

Truyền tham số ở định dạng nhị phân
-----------------------------------

Khi chuyển các tham số ở định dạng nhị phân, lược đồ hợp đồng  ref:`<contract-schema>`là không cần thiết.

Để khởi tạo một phiên bản của hợp đồng ``my_parameter_contract'' từ
module với tài liệu tham khảo
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2'' với
tệp tham số ``my_parameter.bin'' ở định dạng nhị phân, chạy lệnh sau:

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_parameter_contract \
            --energy 1000 \
            --parameter-bin my_parameter.bin


Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0}

.. Xem thêm::

   Để biết thông tin về cách làm việc với các tham số trong hợp đồng thông minh, hãy xem
   :ref:`working-with-parameters`.

.. đặt tên cho một trường hợp:

Đặt tên cho một phiên bản hợp đồng
==================================

Một trường hợp hợp đồng có thể được đặt một bí danh cục bộ hoặc *name*, điều này giúp
tham khảo nó dễ dàng hơn.
Tên chỉ được lưu trữ cục bộ bởi ``concordium-client'' và không hiển thị
trên dây chuỗi.

.. Xem thêm::

   Để được giải thích về cách thức và vị trí của tên và các cài đặt cục bộ khác
   được lưu trữ, xem :ref:`local-settings`.

Để thêm tên trong quá trình khởi tạo, tham số ``--name'' được sử dụng.

Ở đây, chúng tôi đang khởi tạo hợp đồng ``my_contract'' từ module đã triển khai
``9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2'' và đặt tên
nó ``my_name_contract'':

.. code-block:: console

   $concordium-client contract init \
            9eb82a01d96453dbf793acebca0ce25c617f6176bf7a564846240c9a68b15fd2 \
            --contract my_contract \
            --energy 1000 \
            --name my_named_contract


Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Contract successfully initialized with address: {"index":0,"subindex":0} (my_named_contract).

Các phiên bản hợp đồng cũng có thể được đặt tên bằng lệnh ``name''.
Để đặt tên một phiên bản có chỉ mục địa chỉ ``0'' là ``my_name_contract'', hãy chạy
lệnh sau:

.. code-block:: console

   $concordium-client contract name 0 --name my_named_contract

Nếu thành công, đầu ra sẽ tương tự như sau:

.. code-block:: console

   Contract address {"index":0,"subindex":0} was successfully named 'my_named_contract'.

.. Xem thêm::

   Để biết thêm thông tin về địa chỉ phiên bản hợp đồng, hãy xem
   :ref:`reference-on-chain`.

.. _parameter_cursor ():
   https://docs.rs/concordium-std/latest/concordium_std/trait.HasInitContext.html#tymethod.parameter_cursor
.. _get (): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _read (): https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html#method.read_u8