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
  khả năng truy xuất thông tin của thẻ. Duy chỉ có Sector 0 là đặc biệt hơn so với 15 Sector còn lại, đó là việc nó có 1 Block của nhà sản xuất gọi là Manafacturer Block.                   Block này được nhà sản xuất ghi mã Serial Number vào nhằm phân biệt các thẻ với nhau và các mã Serial Number này là duy nhất.
  
  Người lập trình không được ghi, xóa dữ liệu vào Block này để tránh việc bị lỗi Sector này (theo khuyến cáo của nhà sản xuất).
  Như vậy, mỗi Sector sẽ sử dụng được 3 Block để phục vụ cho việc lưu trữ(Data Block),trong đó Sector 0 là chỉ sử dụng được 2 Block lưu trữ (do 1 Block nhà sản xuất đã sử dụng để ghi số    Seri lên). 
  Như vậy, qua phép tính đơn giản chúng ta có thể biết được tổng số Block có thể sử dụng cho việc lưu trữ dữ liệu là: 2 + 15x3 = 47 Block, mỗi Block có độ lớn 16 Bytes nên độ lớn           dữ liệu tối đa là 47x16 = 752 Bytes.


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
*2.3 Cách sử dụng Key A,B*
  Để truy xuất thông tin trên thẻ Mifare, sau khi nhận được tính hiệu có thẻ được đưa vào, đầu đọc sẽ yêu cầu người dùng xác thực với các mã bảo mật, nếu mã bảo mật được 
  cung cấp là đúng thì sẽ trả về “True” ngược lại trả về “False”. Dựa vào các trạng thái True/False đầu đọc sẽ tiếp tục thực hiện yêu cầu hay không. Khi mã bảo mật được cung cấp là đúng,   Đọc mã xác nhận trên thẻ và tiến hành các hoạt động đọc/ghi dữ liệu. Khi một thẻ mới được mua về và chuẩn bị được sử dụng thì việc thay đổi các khóa bảo mật nên được thực hiện. ở phần    này,  chỉ tác động với Key A, Key B để thấy được rõ vai trò của chúng, nên các Access Bit nên được giữ nguyên, các Access Bit sẽ được hướng dẫn ở phần sau.Việc thay đổi Key A, Key B      được thực hiện thông qua các bước chính như sau:
  <img width="1350" height="825" alt="10" src="https://github.com/user-attachments/assets/6982f3bb-edfa-418b-a839-5408a31bad45" />
  Việc đọc một vùng nhớ nào đó kể cả trailer block có thể thực hiện được một cách dễ dàng khi người dùng đã nắm được Key A, Key B của sector chứa Block cần truy cập. 
  từ giao diện của phần mềm Arduino ta gọi chương trình đọc dữ liệu một Block bất kỳ như sau: File >> Example >> Mfrc522 >> Rfid_defaut_keys 
  <img width="1350" height="897" alt="11" src="https://github.com/user-attachments/assets/b3dc32b1-1e2d-46cf-bed8-dbef52766433" />
  <img width="1350" height="887" alt="12" src="https://github.com/user-attachments/assets/4e5afaed-ec4f-4ef2-b610-456481a1d147" />
  <img width="1350" height="883" alt="13" src="https://github.com/user-attachments/assets/85cdedcb-650a-49d0-8d20-ada3557848b4" />

  Thứ nhất: mã xác thực gồm 6 bytes được điền vào mảng knownKeys[]. Người dùng phải điền đúng mã xác thực chứa trong Trailer Block của Sector cần truy cập. 
  Thứ hai: chọn loại mã cần xác thực, nếu mã xác thực điền vào là Key A thì lựa chọn : status =mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_A, block, key, &(mfrc522.uid)); 
  Ngược lại, nếu mã xác thực là Key B thì chọn dòng: status =mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_B, block, key, &(mfrc522.uid)); 
  thứ ba: chọn block cần đọc ở mục byte block. Block điền vào phải chứa trong Sector chữa trailer block cần yêu cầu truy cập. 
  Giả sử, nhóm thực hiện sẽ đọc dữ liệu Blcock 5 nằm trong sector 2, nhận dạng Key điền vào là Key A là Key mặc định của nhà sản xuất. kết quả có được như sau: 
  <img width="1350" height="679" alt="14" src="https://github.com/user-attachments/assets/17e48a8b-86a2-453c-a5e5-ebde9ddffa5f" />
  Việc ghi một vùng nhớ nào đó kể cả trailer block có thể thực hiện được một cách dễ dàng khi người dùng đã nắm được Key A, Key B của sector chứa Block cần truy cập. từ 
  giao diện của phần mềm Arduino ta nhập chương trình như sau:
  <img width="1350" height="679" alt="15" src="https://github.com/user-attachments/assets/b4c4d76d-63e3-416c-94e8-a7c0af0ad715" />
  <img width="1350" height="915" alt="16" src="https://github.com/user-attachments/assets/dc83aeb4-83b2-4a36-bf99-e52161b1f182" />
  <img width="1350" height="914" alt="17" src="https://github.com/user-attachments/assets/e1f8cd0f-bfc7-4623-ab22-558ae206586a" />
  <img width="1350" height="679" alt="18" src="https://github.com/user-attachments/assets/017d0586-1319-43a2-9594-8c4122469ee3" />
  Trong chương trình này có bốn điểm cần lưu ý: 
  Thứ nhất: mã xác thực gồm 6 bytes được điền vào mảng   key.keyByte[]. Người dùng phải điền đúng mã xác thực chứa trong Trailer Block của Sector cần truy cập. 
  Thứ hai: chọn loại mã cần xác thực, nếu mã xác thực điền vào là Key A thì lựa chọn: status =mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_A, block, key, &(mfrc522.uid)); 
  Ngược lại, nếu mã xác thực là Key B thì chọn dòng: status =mfrc522.PCD_Authenticate(MFRC522::PICC_CMD_MF_AUTH_KEY_B, block, key, &(mfrc522.uid)); 
  Thứ ba: chọn block cần đọc ở mục byte block. Block điền vào phải chứa trong Sector chứa trailer block cần yêu cầu truy cập. 
  Thứ tư: điền vào dữ liệu cần thay đổi vào mảng buffer. Nên điền hết tất cả các bytes trong mảng này mục đích để tránh sai lầm khi thay đổi Key A, Key B
  Kết quả thu được:
  <img width="1350" height="679" alt="19" src="https://github.com/user-attachments/assets/2ce1a447-ecfa-4fbf-a759-2f182ad02958" />
  Thử kiểm tra lại dữ liệu mới đã được ghi vào hay chưa, dùng chương trình Rfid_defaut_keys, kết quả thu được như sau:
  <img width="1350" height="679" alt="20" src="https://github.com/user-attachments/assets/3fbafdd8-b16c-4ff2-b7ae-301e021a3ac7" />
*2.4  Thay đổi khóa bảo mật Key A, Key B*
  Việc thay đổi Key A, Key B cũng chính là ghi dữ liệu nhưng đặc biệt hơn là phải thay đổi vào vùng nhớ của Trailer Block vì chỉ Trailer Block mới chứa các mã bảo mật này. 
  Một lưu ý quan trọng là khi thay chọn mảng để thay đổi, thì đặt vào đủ 16 byte của Trailer Block mặc dù có những Bytes không thay đổi ở đây là Access Bit. Tùy vào Access 
  Bit qui định cho từng Block thì sẽ tương ứng với Key nào được gọi trong quá trình ghi. Chương trình ghi ở phần này cũng là chương trình ghi đã đưa ở phần trước. Lưu đồ thay 
  đổi khóa bảo mật như sau: 
  <img width="1350" height="771" alt="21" src="https://github.com/user-attachments/assets/9422ab66-9f9f-46b1-ba3a-2e58256e7b52" />
  Giả sử, thực hiện sẽ ghi dữ liệu Blcock 7 là trailer Block của Sector 1, nhận dạng Key điền vào là Key B là Key mặc định của nhà sản xuất (chưa thay đổi). Dữ liệu 
  mới ghi vào là: Buffer[34] = {0x00, 0x11, 0x22, 0x33, 0x44, 0x55, 0xFF, 0x07, 0x80, 0x69, 0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB}
  Trong đó: Key A=0x00, 0x11, 0x22, 0x33, 0x44, 0x55, Key B=0x66, 0x77, 0x88, 0x99, 0xAA, 0xBB. Các Byte còn lại giữ nguyên bản như dữ liệu ở thẻ mới. Dùng chương trình ghi ta được kết     quả như sau:
  <img width="1350" height="679" alt="22" src="https://github.com/user-attachments/assets/a5b4e3f1-6d4a-4115-86f5-a69b1a4e2def" />
  Kết quả cho thấy: lần đầu quẹt thẻ, đầu đọc sẽ sử dụng Key cũ để ghi dữ liệu và đã thành công. Từ lần quẹt thử thứ 2 vì mã bảo mật đã được thay đổi nên có lỗi trả về. Như 
  vậy, việc thay đổi Key A, Key B diễn ra thành công. Bây giờ muốn đọc dữ liệu trong Trailer Block hoặc bất kỳ Block nào trong Sector 1 thì phải gọi đúng key mới. Giả sử, đọc dữ liệu 
  từ Block 5, xác thực với Key A. kết quả như sau: 
  <img width="1350" height="679" alt="23" src="https://github.com/user-attachments/assets/beb9de03-18d7-4bb1-82eb-4d175df313d2" />
  Dữ liệu đọc về được từ Block 5 đúng với dữ liệu đã ghi ở mục “Cách sử dụng Key A, Key B”. Chung quy lại: mã bảo mật là rất quan trọng trong tất cả các hệ thống, nó cần 
  được giữ bí mật để phòng tránh mất dữ liệu. ở thử Mifare chúng ta có thể dùng tới 16 bộ KeyA, Key B khác nhau để bảo mật dữ liệu. Điều này đối với một hệ thống thông thường 
  thì đã rất tốt. 













