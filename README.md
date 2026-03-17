***Cơ cấu bảo mật của thẻ Mifare Classic***

*1. CƠ CẤU TỔ CHỨC BỘ NHỚ EEPROM*
  Trong cấu trúc của 1 thẻ Mifare bất kỳ thì bộ nhớ Eeprom là phần đóng vai trò quan trọng nhất trong hoạt động lưu trữ dữ liệu và cơ cấu bảo mật của thẻ
  Hình dưới đây là cấu trúc của bộ nhớ Eeprom trên 1 thẻ Mifare Classic
<img width="1350" height="679" alt="1" src="https://github.com/user-attachments/assets/874724f8-e633-49d1-8b95-d085bc57ef9c" />

  Bộ nhớ Eeprom có dung lượng 1 KByte được chia thành 16 Sectors hay vùng nhớ. Các Sector này được đánh số từ 0 đến 15, được truy xuất không theo thứ tự nghĩa là các 
  vùng nhớ được đọc/ghi một các độc lập, không rang buộc với nhau. Trong mỗi Sector lại được chia thành 4 Block (được đánh số từ 0 đến 3) chứa dữ liệu người dùng và các khóa 
  bảo mật, dung lượng của mỗi Block là 16 Bytes được đánh số từ trái sang phải từ Byte 0 đến 15. 
  
  Trong mỗi Sector, có một Block rất quan trọng, nếu nhìn theo không gian sắp xếp của thẻ thì Block này nằm ở vị trí cao nhất và được đánh số cuối cùng trong các Block, nhà 
  sản xuất gọi là Sector Trailer. Mỗi Sector Trailer gồm 3 thành phần chính là 2 mã bảo mật có thể lập trình được gọi là Key A(nằm từ Byte 0 đến Byte 5) và Key B(nằm từ Byte 10 
  đến Byte 15) để truy cập đọc/ghi vào chính Sector mà nó quy định. Và thành phần còn lại là AccessBit (nằm từ Byte 6 đến Byte 9) có khả năng quy định chức năng cho Key A, B và 
  khả năng truy xuất thông tin của thẻ. Duy chỉ có Sector 0 là đặc biệt hơn so với 15 Sector còn lại, đó là việc nó có 1 Block của nhà sản xuất gọi là Manafacturer Block. Block này được nhà   sản xuất ghi mã Serial Number vào nhằm phân biệt các thẻ với nhau và các mã Serial Number này là duy nhất.
  
  Người lập trình không được ghi, xóa dữ liệu vào Block này để tránh việc bị lỗi Sector này (theo khuyến cáo của nhà sản xuất).
  Như vậy, mỗi Sector sẽ sử dụng được 3 Block để phục vụ cho việc lưu trữ(Data Block),trong đó Sector 0 là chỉ sử dụng được 2 Block lưu trữ (do 1 Block nhà sản xuất đã sử dụng để ghi số Seri lên). 
  Như vậy, qua phép tính đơn giản chúng ta có thể biết được tổng số Block có thể sử dụng cho việc lưu trữ dữ liệu là: 2 + 15x3 = 47 Block, mỗi Block có độ lớn 16 Bytes nên độ lớn dữ liệu   tối đa là 47x16 = 752 Bytes.


*2. CƠ CẤU BẢO MẬT CỦA THẺ MIFARE*
  Sau khi nắm rõ được cơ cấu tổ chức bên trong của thẻ Mifare thì việc tìm hiểu các cách thức bảo mật của thẻ sẽ trở nên rất dễ dàng. 
  Đối với 1 thẻ Mifare Classic thì có 4 cách để bảo mật thông tin trên thẻ
        • Dựa vào UID độc nhất của mỗi thẻ. Như đã nói ở trên, mỗi thẻ Mifare đều có 1 Seri 
      Number độc nhất do nhà sản xuất quy định và chúng ta sẽ không thể thay đổi và tác 
      động vào. Do đó trong những yêu cầu đơn giản, cần bảo mật nhưng không phải là 
      tuyệt đối thì ta có thể sử dụng chính UID này như là mã bảo mật. Ví dụ như các bãi 
      giữ xe trong các trường học, bệnh viện, siêu thị… 
      • Dựa vào thuật toán truy xuất. Khi nhà sản xuất phát hành các loại thẻ thì đồng thời 
      họ cũng cung cấp cho người dùng datasheet đi kèm, dựa vào data shet này chúng ta 
      có thể biết được lược đồ thời gian truy câp vào thẻ. Từ đó có cách lập trình đọc/ghi 
      thẻ đúng đắn. Mỗi nhà loại thẻ có một lưu đồ thuật toán khác nhau nên chỉ có đầu 
      đọc được thiết kế cho loại thẻ đó mới có thể đọc/ghi dữ liệu trên thẻ được. Đáng tiếc 
      rằng thuật toán truy xuất vào thẻ thì có thể bị sao chép nên phương án bảo mật này 
      không được đánh giá cao.  
      • Dựa vào KeyA, key B: đây là hai khóa bảo mật được đặt trong Trailer Block của 
      mỗi Block, được coi như chìa khóa mở ra thông tin trên mỗi Block của thẻ vì khi 
      lập trình, người dùng buộc phải khai báo đúng hai khóa này trước khi tác động vào 
      thông tinh trên thẻ. Phương án này được đánh giá khá cao bởi người dùng thẻ Mifare 
      chỉ sau Access Bit. 
      • Dựa vào Access Bit: đây là phương án bào mật cao nhất trên thẻ Mifare. Access Bit 
      là tổ hợp các bộ ba bit điều khiển chức năng của Key A, Key B cũng như khả năng 
      được xuất hiện của dữ liệu trên thẻ

      Trong đây sẽ chỉ đi sâu vào cách thức dùng key A và key B
      Hình ảnh cấu trúc của 1 Sector Trailer và vị trí của Key A,B và Acceccbit
      
<img width="1350" height="679" alt="2" src="https://github.com/user-attachments/assets/4de226f1-546a-4b68-adff-f734889f5831" />

*2.1 Các công cụ hỗ trợ và thư viện sử dụng*
      Hiện nay, có rất nhiều công cụ hỗ trợ lập trình thẻ Mifare và đầu đọc Mfrc522 nhưng phổ biến nhất là trình biên dich Arduino, với phiên bản kit là Uno R3. Đây cũng là công 
      cụ lập trình chính trong bài viết này. Sau khi đã cài đặt được phần mềm Arduino, bước tiếp theo là tải bộ thư viện hỗ trợ sử dụng đầu đọc Mfrc522. 
      Tại giao diện phần mềm, ta làm như sau: chọn thẻ Sketch >> Include library >> Manage libraries >> nhập vào khung tìm kiếm Mfrc522 >> chọn Install. Phần mềm sẽ tự 
      động cập nhật bộ thư viện về máy tính của Người dùng.
<img width="783" height="599" alt="3" src="https://github.com/user-attachments/assets/8ba0e05e-bd07-48f9-a143-5dc6861ada49" />


      Sau khi cài đặt thành công thư viện Mfrc522, chúng ta có thể xem các chương trình trong thư viện vừa tải về. 
      Từ giao diện phần mềm, chọn tab File >> Example >> Mfrc522 >> chọn chương trình muốn mở. các phần chương trình được tạo sẵn gồm có: 
      Access Control, change UID, Dumpinfo, Rfid_defaut_key… thư viên này khá đầy đủ cho các nhu cầu sử dụng thẻ của người dùng. 
      Trước khi sử dụng tiến hành sử dụng thẻ, hãy đảm bảo rằng kết nối từ đầu đọc tới Arduino đã được thiết lập thành công. Để tránh hư hỏng thiết bị, đầu đọc, các chân kết                    nối từ Mfrc522 tới Arduino được mô tả như bảng sau:
<img width="794" height="599" alt="4" src="https://github.com/user-attachments/assets/660990a9-4c56-4189-8f3b-a62a2fba657d" />
*2.2 Xem Thông Tin Ban Đầu Trên Một Thẻ Mới*
  Một thẻ mifare khi mới mua về, dữ liệu bên trong chưa bị thay đổi, các khóa bảo mật Key A, Key A đang là mặc định của nhà sản xuất. Người dùng có thể đọc thông tinh 
  trên thẻ một cách toàn vẹn mà không gặp bất kỳ một trở ngại nào. Cách đọc thông tin như sau: từ giao diện phần mềm Arduino chọn chương trình Dumpinfo trong thư viên Mfrc522, 
  tiến hành biên dịch và nạp chương trình. Sau khi tải thành công chương trình, tiến hành quẹt thẻ. Để quan sát được dữ liệu trong thẻ, chúng ta cần mở cửa sổ Serial monitor từ tab 
  Tool trên thanh tác vụ. Dữ liệu thu được sua khi quẹt thẻ như sau: 
<img width="710" height="820" alt="6" src="https://github.com/user-attachments/assets/316fd004-ae3a-4f35-80f2-342a374bde0b" />
<img width="1350" height="802" alt="7" src="https://github.com/user-attachments/assets/e37ecad2-08f4-466e-a0b9-a963021ca8db" />
<img width="1350" height="710" alt="9" src="https://github.com/user-attachments/assets/93f69043-4a95-42c4-9402-ab2141561059" />

      Tại đây, chúng ta có thể thấy được UID, Key A, Key B trong mỗi Trailer sector. Một lưu ý nhỏ và rất quan trọng đó là các mặc định của nhà sản xuất thì Key A không được 
      xuất hiện trong tất cả các trường hợp. vì thế mặc dù mã bảo mật A, B mặc định là FFFFFFFFFFFF nhưng khi đọc thẻ thì chỉ mã B xuất hiện, còn mã A sẽ được trả về là dãy 
      số 0. Điều này được đánh giá rất cao trong cơ cấu bảo mật dùng Key A, Key B. Sở dĩ chương trình Dumpinfo đọc được tất cả các dữ liệu trong thẻ vì chương trình 
      này được người viết cài sẵn Key A, Key B mặc định, khi đọc thẻ, tất cả các Block được áp dụng hai Key này, chính vì lý do đó thẻ mới mua về thì chưa hề có bảo mật.


