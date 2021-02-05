.. _contract-instance:

========================
Các trường hợp sử dụng hợp đồng thông minh
========================

.. todo::

   - Làm rõ cách các trường hợp liên quan đến hợp đồng thông minh liên quan đến các module
     (ví dụ: ngay bây giờ nó nói rằng một phiên bản là một module + trạng thái nhưng
     hình dưới đây cho thấy các trường hợp dưới dạng hợp đồng + trạng thái).
   - Quyết định chính xác chúng ta nên định nghĩa các module hợp đồng thông minh (như khái niệm riêng của nó
     hoặc như các module Wasm), và liệu chúng ta có nên nói về chúng hay không.
   - Quyết định xem chúng ta có nên có một ví dụ mã cụ thể hay không và liệu nó có nên
     ở trạng thái Wasm hoặc Rust hoặc có thể là mã giả.
   - Xem xét có một bức tranh giải thích mối quan hệ giữa các module và các cá thể.

** Ví dụ hợp đồng thông minh ** là một module hợp đồng thông minh cùng với
trạng thái cụ thể và số lượng mã thông báo GTU.
Nhiều phiên bản hợp đồng thông minh có thể được tạo từ cùng một module.
Ví dụ: đối với hợp đồng: ref: `bid <auction>`, có thể có nhiều trường hợp, mỗi trường hợp
một dành riêng để đấu thầu cho một mặt hàng cụ thể và với những người tham gia của chính nó.

Các phiên bản hợp đồng thông minh có thể được tạo từ hợp đồng thông minh đã triển khai: ref: `
module <contract-module> `thông qua giao dịch` `init '' gọi ra
chức năng được yêu cầu trong module hợp đồng thông minh. Chức năng này có thể có một
tham số.
Kết quả cuối cùng của nó được yêu cầu là trạng thái hợp đồng thông minh ban đầu của
ví dụ.

.. Ghi chú::

   Một trường hợp hợp đồng thông minh thường chỉ được gọi là * trường hợp *.

.. graphviz ::
   : align: center
   : caption: Ví dụ về module hợp đồng thông minh chứa hai hợp đồng thông minh:
             Ký quỹ và huy động vốn từ cộng đồng. Mỗi hợp đồng có hai trường hợp.

    digraph G {
       rankdir="BT"

       subgraph cluster_0 {
           label = "Module";
           labelloc=b;
           node [fillcolor=white, shape=note]
           "Crowdfunding";
           "Escrow";
       }

       subgraph cluster_1 {
           label = "Instances";
           style=dotted;
           node [shape=box, style=rounded]
           House;
           Car;
           Gadget;
           Boardgame;
       }

       House:n -> Escrow;
       Car:n -> Escrow;
       Gadget:n -> Crowdfunding;
       Boardgame:n -> Crowdfunding;
   }

Trạng thái của một trường hợp hợp đồng thông minh
==================================

Trạng thái của một phiên bản hợp đồng thông minh bao gồm hai phần, do người dùng xác định
nêu rõ và số lượng GTU mà hợp đồng nắm giữ, tức là * số dư * của nó. Khi nào
đề cập đến trạng thái, chúng tôi thường chỉ có nghĩa là trạng thái do người dùng xác định. Lý giải cho việc
coi số GTU một cách riêng biệt là GTU chỉ có thể được chi tiêu và
nhận được theo các quy tắc của mạng, ví dụ: hợp đồng không thể tạo
hoặc phá hủy mã thông báo GTU.

.. _contract-instance-init-on-chain:

Khởi tạo hợp đồng thông minh trên chuỗi
=======================================

Mọi hợp đồng thông minh phải chứa một chức năng để tạo hợp đồng thông minh
các trường hợp. Một hàm như vậy được gọi là * init function *.

Để tạo một phiên bản hợp đồng thông minh, một tài khoản sẽ gửi một giao dịch đặc biệt với
tham chiếu đến module hợp đồng thông minh đã triển khai và tên của
hàm init để sử dụng cho việc khởi tạo.

Giao dịch cũng có thể bao gồm một lượng GTU, được thêm vào số dư
của phiên bản hợp đồng thông minh. Một tham số cho hàm được cung cấp như một phần
của giao dịch dưới dạng một mảng byte.

Tóm lại, giao dịch bao gồm:

- Tham chiếu đến module hợp đồng thông minh.
- Tên của hàm init.
- Tham số cho hàm init.
- Số lượng GTU cho ví dụ.

Hàm init có thể báo hiệu rằng nó không muốn tạo một phiên bản mới
với các thông số đó. Nếu hàm init chấp nhận các tham số, nó sẽ đặt
lên trạng thái ban đầu của cá thể và số dư của nó. Ví dụ được đưa ra
địa chỉ trên chuỗi và tài khoản đã gửi giao dịch trở thành chủ sở hữu
của trường hợp. Nếu hàm từ chối, không có phiên bản nào được tạo và chỉ
giao dịch để cố gắng tạo phiên bản có thể nhìn thấy trên chuỗi.

.. Xem thêm::

   Xem: hướng dẫn ref: `initialize-contract` để biết cách khởi tạo một
   hợp đồng trong thực tế.

Trạng thái phiên bản
==============

Mỗi phiên bản hợp đồng thông minh đều có trạng thái riêng được thể hiện trên chuỗi
dưới dạng một mảng các byte. Phiên bản sử dụng các chức năng do máy chủ cung cấp
môi trường để đọc, ghi và thay đổi kích thước trạng thái.

.. Xem thêm::

   Xem: ref: `host-functions-state` để tham khảo các hàm này.

Trạng thái hợp đồng thông minh có kích thước giới hạn. Hiện tại giới hạn hợp đồng thông minh
trạng thái là 16KiB.

.. Xem thêm::

   Kiểm tra: ref: `resource-Accounting` để biết thêm về điều này.

Tương tác với một phiên bản
============================

Hợp đồng thông minh có thể hiển thị không hoặc nhiều chức năng để tương tác với
thể hiện, được gọi là * hàm nhận *.

Cũng giống như với các hàm init, các hàm nhận được kích hoạt bằng cách sử dụng
các giao dịch chứa một số GTU cho hợp đồng và một đối số
tới hàm dưới dạng byte.

Tóm lại, một giao dịch tương tác với hợp đồng thông minh bao gồm:
- Địa chỉ đến phiên bản hợp đồng thông minh.
- Tên của chức năng nhận.
- Tham số cho chức năng nhận.
- Số lượng GTU cho ví dụ.

.. _contract-instance-action:

Ghi nhật ký sự kiện
==============

.. todo::

   Giải thích các sự kiện là gì và tại sao chúng hữu ích.
   Diễn đạt lại/làm rõ "giám sát các sự kiện".

Các sự kiện có thể được ghi lại trong quá trình thực hiện các chức năng của hợp đồng thông minh. Đây là
trường hợp cho cả hai hàm init và nhận. Nhật ký được thiết kế cho
sử dụng ngoài chuỗi, để các tác nhân bên ngoài chuỗi có thể giám sát các sự kiện và
phản ứng với chúng. Các hợp đồng thông minh hoặc bất kỳ tác nhân nào khác trên
chuôi. Sự kiện có thể được ghi lại bằng một chức năng do máy chủ cung cấp
Môi trường.

.. Xem thêm::

   Xem: ref: `host-functions-log` để tham khảo về hàm này.

Các bản ghi sự kiện này được người làm bánh giữ lại và đưa vào tóm tắt giao dịch.

Ghi nhật ký một sự kiện có một chi phí liên quan, tương tự như chi phí ghi vào
trạng thái của hợp đồng. Trong hầu hết các trường hợp, chỉ cần đăng nhập một vài byte vào sẽ có ý nghĩa
giảm chi phí.

.. _action-description:

Mô tả hành động
===================

Một hàm nhận trả về một * mô tả các hành động * sẽ được thực hiện bởi
môi trường chủ trên chuỗi.

Các hành động khả thi mà một hợp đồng có thể tạo ra là:

- ** Chấp nhận ** là một hành động nguyên thủy luôn thành công.
- ** Chuyển đơn giản ** GTU từ phiên bản sang tài khoản được chỉ định.
- ** Gửi **: gọi hàm nhận của phiên bản hợp đồng thông minh được chỉ định,
  và tùy chọn chuyển một số GTU từ phiên bản gửi đến phiên bản nhận
  ví dụ.

Nếu một hành động không thực thi, hàm nhận sẽ được hoàn nguyên, để lại
trạng thái và số dư của phiên bản không thay đổi. Tuy nhiên,

- giao dịch kích hoạt chức năng nhận (không thành công) vẫn được thêm vào chuỗi và
- chi phí giao dịch, bao gồm cả chi phí thực hiện hành động không thành công,
  được trừ vào tài khoản gửi.

Xử lý nhiều mô tả hành động
---------------------------------------

Bạn có thể xâu chuỗi các mô tả hành động bằng cách sử dụng tổ hợp ** và **.
Chuỗi mô tả hành động `` A '' ** và ** `` B ''

1) Thực hiện `` A ''.
2) Nếu `` A '' thành công, hãy thực thi `` B ''.
3) Nếu `` B '' không thành công, toàn bộ chuỗi hành động không thành công (và kết quả của `` A '' được hoàn nguyên).

Xử lý lỗi
---------------

Sử dụng tổ hợp ** hoặc ** để thực hiện một hành động trong trường hợp hành động trước đó không thành công.
Mô tả hành động `` A '' ** hoặc ** `` B ''

1) Thực hiện `` A ''.
2) Nếu `` A '' thành công, dừng thực thi.
3) Nếu `` A '' không thành công, hãy thực thi `` B ''.

.. graphviz ::
   : align: center
   : caption: Ví dụ về mô tả hành động cố gắng chuyển sang Alice
             và sau đó Bob, nếu bất kỳ điều nào trong số này không thành công, nó sẽ cố gắng chuyển sang
             Charlie thay vào đó.

digraph G {
       node [color=transparent]
       or1 [label = "Or"];
       and1 [label = "And"];
       transA [label = "Transfer x to Alice"];
       transB [label = "Transfer y to Bob"];
       transC [label = "Transfer z to Charlie"];

       or1 -> and1;
       and1 -> transA;
       and1 -> transB;
       or1 -> transC;
   }

.. Xem thêm::

   Xem: ref: `host-functions-action` để tham khảo cách tạo
   các hành động.

Toàn bộ cây hành động được thực thi ** nguyên tử ** và dẫn đến các bản cập nhật
cho tất cả các trường hợp và tài khoản có liên quan, hoặc, trong trường hợp bị từ chối, đối với khoản thanh toán
để thực hiện, nhưng không có thay đổi nào khác. Tài khoản đã gửi lần khởi tạo
giao dịch trả tiền cho việc thực hiện toàn bộ cây.