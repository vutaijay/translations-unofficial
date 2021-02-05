.. mô phỏng cục bộ:

===================================
Mô phỏng cục bộ các chức năng hợp đồng
===================================

Hướng dẫn này là về cách mô phỏng cục bộ một lệnh gọi của một số init hoặc
nhận chức năng từ module hợp đồng thông minh Wasm trong một ngữ cảnh nhất định và
câu lệnh.
Mô phỏng này rất hữu ích để kiểm tra một hợp đồng thông minh và kết quả trong
các kịch bản cụ thể.

.. Xem thêm::

   Để biết hướng dẫn về các bài kiểm tra đơn vị tự động, hãy xem: ref:`unit-test-contract`.

Chuẩn bị
===========

Đảm bảo rằng bạn đã cài đặt ``row-concordium``, nếu không làm theo hướng dẫn
:ref: `setup-tools`.
Bạn cũng sẽ cần một module hợp đồng thông minh trong Wasm để mô phỏng.

.. todo::

   Viết phần còn lại, khi nội dung lược đồ đã sẵn sàng.

Mô phỏng khởi tạo
========================

Để mô phỏng việc khởi tạo một phiên bản hợp đồng thông minh bằng cách sử dụng
``row-concordium``, hãy chạy lệnh sau:

.. code-block:: console

   $cargo concordium run init --module contract.wasm \
                               --contract "my_contract" \
                               --context init-context.json \
                               --amount 123456.789 \
                               --parameter-bin parameter.bin \
                               --out-bin state.bin

``init-context.json`` (được sử dụng với tham số ``--context``) là một tệp
chứa thông tin ngữ cảnh như trạng thái hiện tại của chuỗi,
người gửi giao dịch và tài khoản nào gọi chức năng này.
Một ví dụ về ngữ cảnh này có thể là:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "initOrigin": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
   }

.. Xem thêm::

   Để tham khảo ngữ cảnh, hãy xem: ref:`simulate-context`.


Mô phỏng các bản cập nhật
=========================

Để mô phỏng một bản cập nhật cho một trường hợp hợp đồng thông minh hợp đồng bằng
``cargo-concordium``, chạy:

.. code-block:: console

   $cargo concordium run update --module contract.wasm \
                                 --contract "my_contract" \
                                 --func "some_receive" \
                                 --context receive-context.json \
                                 --amount 123456.789 \
                                 --parameter-bin parameter.bin \
                                 --state-bin state-in.bin \
                                 --out-bin state-out.bin

``get-context.json`` (được sử dụng với tham số ``--context``) là một tệp
chứa thông tin ngữ cảnh như trạng thái hiện tại của chuỗi,
người gửi giao dịch, tài khoản nào gọi chức năng này và tài khoản nào
tài khoản hoặc địa chỉ đã gửi tin nhắn hiện tại.
Một ví dụ về ngữ cảnh này có thể là:

.. code-block:: json

   {
       "metadata": {
           "slotTime": "2021-01-01T00:00:01Z"
       },
       "invoker": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF",
       "selfAddress": {"index": 0, "subindex": 0},
       "selfBalance": "0",
       "sender": {
           "type": "account",
           "address": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
       },
       "senderPolicies": [{
           "identityProvider": 1,
           "createdAt": "202012",
           "validTo": "202109"
       }],
       "owner": "3uxeCZwa3SxbksPWHwXWxCsaPucZdzNaXsRbkztqUUYRo1MnvF"
   }


.. Xem thêm::

   Để tham khảo ngữ cảnh, hãy xem: ref:`simulate-context`.