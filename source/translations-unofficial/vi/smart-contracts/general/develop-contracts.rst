.. Được trả lời:
    - Viết một hợp đồng thông minh cần sử dụng gỉ?
    - Những phần cần thiết để viết một hợp đồng thông minh trong gỉ là gì?
        - Câu lệnh
             - Serialized
            - Schema
        - Init
        - Receive
    - Loại thử nghiệm nào có thể
    - Thực hành tốt nhất?
        - Đảm bảo số tiền 0
        - Đừng hoảng sợ
        - Tránh tính toán nặng

.. _Viết hợp đồng thông minh:

==================================
Phát triển hợp đồng thông minh trong Rust
==================================

Trên các hợp đồng thông minh blockchain concordium được triển khai dưới dạng modules Wasm, nhưng
Wasm được thiết kế chủ yếu như một mục tiêu biên dịch và không thuận tiện để
viết bằng tay.
Thay vào đó, chúng ta có thể viết các hợp đồng thông minh của mình bằng ngôn ngữ lập trình Rust_,
hỗ trợ tốt cho việc biên dịch sang Wasm.

Hợp đồng thông minh không nhất thiết phải được viết bằng Rust.
Đây chỉ đơn giản là SDK đầu tiên chúng tôi cung cấp.
Wasm được viết thủ công hoặc Wasm được biên dịch từ C, C ++, AssemblyScript_ và
những cái khác, đều có giá trị như nhau trên chuỗi, miễn là nó tuân thủ: ref: `Wasm
giới hạn mà chúng tôi áp đặt <wasm-limitations> `.

.. Xem thêm::

   Để biết thêm thông tin về các chức năng được mô tả bên dưới, hãy xem concordium_std_
   API để viết các hợp đồng thông minh trên chuỗi khối Concordium trong Rust.

.. Xem thêm::

   Xem: ref: `contract-module` để biết thêm thông tin về các modules hợp đồng thông minh.

Một modules hợp đồng thông minh được phát triển trong Rust dưới dạng thùng thư viện, sau đó
được biên dịch sang Wasm.
Để có được các bản xuất chính xác, thuộc tính `crate-type` phải được đặt thành
`` ["cdylib", "rlib"] `` trong tệp kê khai:

.. code-block:: text

   ...
   [lib]
   crate-type = ["cdylib", "rlib"]
   ...


Viết hợp đồng thông minh bằng cách sử dụng `` concordium_std ''
=================================================

Bạn nên sử dụng thùng `` concordium_std '', thùng này cung cấp
thêm trải nghiệm giống như Rust để phát triển modules hợp đồng thông minh và gọi điện
các chức năng máy chủ.

Thùng cho phép viết init và nhận các hàm như Rust đơn giản
các hàm được chú thích bằng `` # [init (...)] `` và `` # [receive (...)] `` tương ứng.

Đây là một ví dụ về hợp đồng thông minh triển khai bộ đếm:

.. code-block:: rust

   use concordium_std::*;

   type State = u32;

   #[init(contract = "counter")]
   fn counter_init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let state = 0;
       Ok(state)
   }

   #[receive(contract = "counter", name = "increment")]
   fn contract_receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       ensure!(ctx.sender().matches_account(&ctx.owner()); // Only the owner can increment
       *state += 1;
       Ok(A::accept())
   }

Có một số điều cần lưu ý:

.. todo::

   - Viết các yêu cầu theo cách dễ đọc hơn (ví dụ: chia đoạn văn thành các dấu đầu dòng phụ).
   - Những yêu cầu này phải là một phần của đặc tả được viết ở đâu đó,
     tức là, không chỉ là một phần của ví dụ này.

- Loại chức năng:

  * Một hàm init phải có kiểu `` & impl HasInitContext -> InitResult <MyState> ''
    trong đó `` MyState '' là một kiểu triển khai đặc điểm `` Serialize ''.
  * Một hàm nhận phải nhận tham số kiểu `` A: HasActions '',
    một tham số `` & impl HasReceiveContext '' và một tham số `` & mut MyState '' và trả về
    một `` ReceiveResult<A> ''.

- Chú thích `` # [init (contract = "counter")] `` đánh dấu chức năng của nó
  được áp dụng dưới dạng hàm init của hợp đồng có tên là `` bộ đếm ''.
  Cụ thể, điều này có nghĩa là đằng sau hậu trường, macro này tạo ra
  hàm với chữ ký bắt buộc và tên `` init_counter ''.

- `` # [nhận (hợp đồng = "bộ đếm", tên = "số gia tăng")] `` deserializes và
  cung cấp trạng thái được thao tác trực tiếp.
  Phía sau, chú thích này cũng tạo ra một hàm được xuất với tên
  `` counter.increment '' có chữ ký bắt buộc và thực hiện tất cả
  boilerplate của trạng thái deserializing thành loại yêu cầu `` Trạng thái ''.

.. Ghi chú::

   Lưu ý rằng quá trình giải mã hóa không phải là không có chi phí và trong một số trường hợp,
   người dùng có thể muốn kiểm soát chi tiết hơn việc sử dụng các chức năng máy chủ.
   Đối với những trường hợp sử dụng như vậy, các chú thích hỗ trợ tùy chọn `` low_level '', có
   ít chi phí hơn, nhưng yêu cầu nhiều hơn từ người dùng.

.. todo::

   - Mô tả cấp thấp
   - Giới thiệu khái niệm về các chức năng máy chủ trước khi sử dụng chúng trong phần ghi chú ở trên


Trạng thái và thông số có thể nối tiếp
---------------------------------

.. todo :: Làm rõ nghĩa là trạng thái được hiển thị tương tự như `` File '';
   tốt hơn là không đề cập đến `` File ''.

Trên chuỗi, trạng thái của một cá thể được biểu diễn dưới dạng một mảng byte và được hiển thị
trong giao diện tương tự như giao diện `` Tệp '' của thư viện chuẩn Rust.

Điều này có thể được thực hiện bằng cách sử dụng đặc điểm `` Serialize '' chứa (de-) serialization
chức năng.

Thùng `` concordium_std '' bao gồm đặc điểm này và triển khai cho
hầu hết các loại trong thư viện tiêu chuẩn Rust.
Nó cũng bao gồm các macro cho phái sinh
đặc điểm cho cấu trúc do người dùng xác định và
enums.

.. code-block:: rust

   use concordium_std::*;

   #[derive(Serialize)]
   struct MyState {
       ...
   }

Điều này cũng cần thiết cho các tham số để bắt và nhận các hàm.

.. Ghi chú::

   Nói một cách chính xác, chúng tôi chỉ cần giải mã byte thành kiểu tham số của chúng tôi,
   nhưng rất tiện lợi khi có thể sắp xếp các kiểu khi viết các bài kiểm tra đơn vị.

.. _ Làm việc với các tham số:

Làm việc với các tham số
-----------------------

Các tham số cho hàm init và hàm nhận giống như ví dụ
trạng thái, được biểu diễn dưới dạng mảng byte.
Trong khi các mảng byte có thể được sử dụng trực tiếp, chúng cũng có thể được giải mã hóa thành
Dữ liệu có cấu trúc.

Cách đơn giản nhất để giải mã hóa một tham số là thông qua hàm `get ()` _ của
đặc điểm `Get`_.

Ví dụ, hãy xem hợp đồng sau, trong đó tham số
`` ReceiveParameter '' được giải mã trên dòng được đánh dấu:

.. code-block:: rust
   :emphasize-lines: 24

   use concordium_std::*;

   type State = u32;

   #[derive(Serialize)]
   struct ReceiveParameter{
       should_add: bool,
       value: u32,
   }

   #[init(contract = "parameter_example")]
   fn init(
       _ctx: &impl HasInitContext,
   ) -> InitResult<State> {
       let initial_state = 0;
       Ok(initial_state)
   }

   #[receive(contract = "parameter_example", name = "receive")]
   fn receive<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let parameter: ReceiveParameter = ctx.parameter_cursor().get()?;
       if parameter.should_add {
           *state += parameter.value;
       }
       Ok(A::accept())
   }


Hàm nhận ở trên không hiệu quả ở chỗ nó giải mã
`` value '' ngay cả khi nó không cần thiết, tức là khi `` should_add '' là `` false ''.

Để có được nhiều quyền kiểm soát hơn và trong trường hợp này, hiệu quả hơn, chúng tôi có thể giải mã
bằng cách sử dụng đặc điểm `Read`_:

.. code-block:: rust
   :emphasize-lines: 7, 10

   #[receive(contract = "parameter_example", name = "receive_optimized")]
   fn receive_optimized<A: HasActions>(
       ctx: &impl HasReceiveContext,
       state: &mut State,
   ) -> ReceiveResult<A> {
       let mut cursor = ctx.parameter_cursor();
       let should_add: bool = cursor.read_u8()? != 0;
       if should_add {
           // Only decode the value if it is needed.
           let value: u32 = cursor.read_u32()?;
           *state += value;
       }
       Ok(A::accept())
   }

Lưu ý rằng ``value'' chỉ được giải mã hóa nếu `` should_add '' là
``true''.
Trong khi hiệu quả đạt được là tối thiểu trong ví dụ này, nó có thể có
tác động đáng kể cho các ví dụ phức tạp hơn.


Xây dựng modules hợp đồng thông minh với `` row-concordium ''
================================================== ========

Trình biên dịch Rust hỗ trợ tốt cho việc biên dịch sang Wasm bằng cách sử dụng
Mục tiêu `` wasm32-chưa biết-không xác định ''.
Tuy nhiên, ngay cả khi biên dịch với `` --release '', bản dựng kết quả bao gồm
phần lớn thông tin gỡ lỗi trong các phần tùy chỉnh, không hữu ích cho
hợp đồng thông minh trên chuỗi.

Để tối ưu hóa bản dựng và cho phép các tính năng mới như nhúng lược đồ, chúng tôi
khuyên bạn nên sử dụng `` row-concordium '' để xây dựng các hợp đồng thông minh.

.. Xem thêm::

   Để biết hướng dẫn về cách xây dựng bằng cách sử dụng `` row-concordium '', hãy xem
   : ref: `compile-module`.


Thử nghiệm hợp đồng thông minh
=======================

Bài kiểm tra đơn vị sơ khai
---------------------

Mô phỏng các cuộc gọi hợp đồng
-----------------------

Thực hành tốt nhất
==============

Đừng hoảng sợ
-----------

.. todo::

   Sử dụng bẫy để thay thế.

Tránh tạo ra các lỗ đen
--------------------------

Hợp đồng thông minh không bắt buộc phải sử dụng số lượng GTU được gửi đến nó và bằng
mặc định một hợp đồng thông minh không xác định bất kỳ hành vi nào để làm trống số dư
của một ví dụ, trong trường hợp ai đó đã gửi một số GTU.
Các GTU này sau đó sẽ vĩnh viễn * bị mất * và sẽ không có cách nào để khôi phục
chúng.

Do đó, cách thực hành tốt cho các hợp đồng thông minh không giao dịch với GTU,
để đảm bảo số lượng GTU đã gửi bằng 0 và từ chối mọi lời gọi
không phải.

Di chuyển các phép tính nặng ra khỏi chuỗi
---------------------------------


.. _Rust: https://www.rust-lang.org/
.. _Cargo: https://doc.rust-lang.org/cargo/
.. _AssemblyScript: https://github.com/AssemblyScript
.. _get (): https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html#tymethod.get
.. _Nhận: https://docs.rs/concordium-std/latest/concordium_std/trait.Get.html
.. _Đọc: https://docs.rs/concordium-std/latest/concordium_std/trait.Read.html
.. _concordium_std: https://docs.rs/concordium-std/latest/concordium_std/