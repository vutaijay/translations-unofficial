.. Trả lời cho:
    - Hợp đồng thông minh là gì?
    - Tại sao sử dụng hợp đồng thông minh?
    - Các trường hợp sử dụng là gì?
    - Các trường hợp không sử dụng là gì?

.. _Giới thiệu:

===============================
Giới thiệu về hợp đồng thông minh
===============================

Hợp đồng thông minh là một đoạn mã do người dùng cung cấp được gửi tới Concordium
blockchain, được sử dụng để xác định hành vi không trực tiếp là một phần của lõi
giao thức. Hợp đồng thông minh được thực hiện bởi các nút trong mạng Concordium
theo quy tắc xác định trước. Quá trình thực hiện của chúng hoàn toàn minh bạch và tất cả
các nút phải đồng ý về kết quả của việc thực thi chỉ dựa trên công khai
thông tin có sẵn.

Một hợp đồng thông minh có thể nhận, giữ và gửi GTU, nó có thể quan sát một số
các khía cạnh của chuỗi và duy trì trạng thái của chính nó. Hợp đồng thông minh luôn
được thực thi như một phản hồi cho các hành động ** bên ngoài **, ví dụ: một tài khoản gửi một
thông điệp. Trong thực tế, các hợp đồng thông minh thường sẽ là một phần nhỏ của hợp đồng lớn hơn
hệ thống, kết hợp chức năng on và off-chain. Một ví dụ về off-chain
chức năng có thể là một máy chủ gọi hợp đồng thông minh dựa trên
dữ liệu từ thế giới thực, chẳng hạn như giá cổ phiếu hoặc thông tin thời tiết.

Hợp đồng thông minh để làm gì?
=============================

Hợp đồng thông minh có thể làm giảm lượng tin tưởng cần thiết vào bên thứ ba, trong một số trường hợp
loại bỏ nhu cầu về bên thứ ba đáng tin cậy, trong các trường hợp khác, giảm
và do đó làm giảm mức độ tin tưởng cần thiết vào chúng.

Bởi vì các hợp đồng thông minh được thực thi hoàn toàn minh bạch, theo cách
bất kỳ ai có quyền truy cập vào một nút đều có thể xác minh, chúng có thể rất hữu ích để đảm bảo
thỏa thuận giữa các bên.

.. _bán đấu giá:

Ví dụ về hợp đồng thông minh đấu giá
------------------------------

Một trường hợp sử dụng cho các hợp đồng thông minh có thể là để tổ chức một cuộc đấu giá; đây chúng tôi lập trình
hợp đồng thông minh để chấp nhận các giá thầu khác nhau từ bất kỳ ai và giúp nó theo dõi
của người trả giá cao nhất.
Khi phiên đấu giá kết thúc, hợp đồng thông minh sẽ gửi GTU của người thắng thầu cho người bán và tất cả các giá thầu khác trở lại. Người bán sau đó sẽ gửi mặt hàng cho người chiến thắng.

Hợp đồng thông minh thay thế vai trò chính của đấu giá viên. Hợp đồng chính nó
chỉ điều chỉnh phần đấu thầu và việc phân phối trên chuỗi các GTU. Nó sẽ
cũng có thể cần một số logic để hoàn trả cho người trả giá cao nhất nếu người bán
không thực hiện nghĩa vụ của họ. Điều này rất có thể có nghĩa là hợp đồng
cần hỗ trợ một số khái niệm bằng chứng rằng người bán đã thực sự đáp ứng
nghĩa vụ, hoặc một số cách để người trả giá cao nhất nộp đơn khiếu nại. Thông minh
hợp đồng không thể tự động giải quyết các vấn đề trong thế giới thực này, và tốt nhất
giải pháp có thể sẽ phụ thuộc vào các chi tiết cụ thể của cuộc đấu giá.

Hợp đồng thông minh * không * để làm gì?
===================================

Hợp đồng thông minh là một công nghệ rất thú vị và mọi người vẫn đang tìm kiếm
cách để tận dụng chúng.
Tuy nhiên, có một số trường hợp hợp đồng thông minh không phải là một giải pháp tốt.

Một trong những lợi thế chính của hợp đồng thông minh là sự tin tưởng vào mã
thực thi và để đạt được điều này, một số lượng lớn các nút trong mạng blockchain
phải thực hiện cùng một mã và đảm bảo sự đồng ý của kết quả.
Đương nhiên, điều này trở nên tốn kém so với việc chạy cùng một mã trên một nút
trong một số dịch vụ đám mây.

Trong trường hợp hợp đồng thông minh phụ thuộc vào các tính toán nặng nề, nó có thể
có thể chuyển phép tính này ra khỏi hợp đồng thông minh và có
hợp đồng chỉ thực hiện một số phần chính của tính toán, sử dụng mật mã
kỹ thuật để đảm bảo các phần khác được thực hiện chính xác.

Cuối cùng, điều quan trọng cần nhớ là hợp đồng thông minh không có quyền riêng tư và
** mọi thứ ** hợp đồng thông minh có quyền truy cập đều có thể truy cập được cho mọi người khác
trên mạng Concordium, có nghĩa là rất khó để xử lý dữ liệu nhạy cảm trong
hợp đồng thông minh. Trong một số trường hợp, có thể sử dụng các công cụ mật mã để
không làm việc trực tiếp với dữ liệu, mà thay vào đó để các hợp đồng thông minh hoạt động với
các khái niệm bắt nguồn như mã hóa và cam kết, ẩn dữ liệu thực tế.

Vòng đời của hợp đồng thông minh
==============================

Hợp đồng thông minh lần đầu tiên được triển khai cho chuỗi như một phần của hợp đồng: ref: `
mô-đun <contract-module> `. Sau đó, một hợp đồng thông minh có thể được * khởi tạo * thành
lấy a: ref: `smart contract instance <contract-instances>`. Cuối cùng là một thông minh
cá thể hợp đồng có thể được cập nhật nhiều lần theo logic của riêng nó.