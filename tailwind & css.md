
========================================================================================
dùng scss với module.css đôi khi vẫn sẽ làm cho child element bị override(ghi đè)
vd:
.parent{
    p{
        blue;
    }
}
<parent>
    <p>dòng này sẽ có màu xanh</p>
    <childComponent> trong này có thẻ p nào cũng sẽ bị ảnh hưởng theo </childComponent>
</parent>
========================================================================================
1 thẻ có margin sẽ đẩy cả thẻ fixed xuống
<header>thẻ này có position: fixed</header>
<main>thẻ này có margintop: 100px<main>
header sẽ bị đảy xuống 100px theo dù là fixed, dùng top: 0px trong global.css để ko tự động bị đẩy xuống
========================================================================================
2 thẻ flex lồng nhau, flex dọc lồng trong flex ngang
<div> <-thẻ này có display flex
    <div> <-thẻ này có display flex, flex-direction: comlumn
    thẻ này sẽ bị kéo dãn theo chiều dọc đến 100%
    đẻ fix, ta ko để 2 thẻ flex bọc nhau, div-flex -> div -> div-flex
    hoặc dùng align-items: flex-start ở flex con
    </div>
</div>

hiểu đơn giản hơn, thuộc tính align-items mặc định giá trị là stretch, row hay column cũng 
vậy
========================================================================================
để ảnh vừa với thẻ img chứa nó, dùng object-fit:cover
========================================================================================
không thể thay đổi màu của input checkbox và radio checkbox vì trình duyệt sẽ dựa vào accent-color
accent-color: #4ab9f8; <- màu xanh này sáng nên cái trình duyệt sẽ để checked màu trắng
accent-color: #1a89c8; <- màu xanh này tối nên cái trình duyệt sẽ để checked màu đen
cùng 1 màu xanh nhưng độ sáng tối 1 chút quyết định màu của checked
========================================================================================
box-shadow: h v blur spread color Optional
h: độ lệch theo chiều ngang của bóng, dương lệch phải, âm lệch trái
v: độ lệch theo chiều dọc của bóng, dương lệch trên, âm lệch dưới
blur: độ làm mờ của bóng()
spread: độ phóng to của bóng, mặc định bóng to bằng thẻ 
tất cả 4 thuộc tính trên đều dùng đơn vị px
color: màu của bóng

nếu bóng to quá thì nên giảm opacity của color
Optional: inset(bóng nằm trong content phần tử), initial( bóng nằm ngoài)
có thể multi bóng, ví dụ:
box-shadow: inset 0 1px 1px rgba(0, 0, 0, .075), 0 0 8px rgba(92, 172, 237, 0.7);
# ========================================Các vấn đề về flex box================================================
1. text bị co lại
nội dung text của các con của 1 flex cha sẽ bị ép xuống dòng nếu kéo giãn màn hình hẹp lại (responsive)
dùng min-width: fit-content cho text con, các con sẽ tự căn chỉnh không gian nhỏ nhất mà text content không bị xuống dòng
độ ưu tiên: căn chỉnh child element cùng 1 dòng với text content sao cho nhỏ nhất để ko xuống dòng -> khi ko thể thu hẹp đc nữa thì text con đó sẽ xuống dòng
các phương pháp:   
    white-space: nowrap;       /* ⛔ Không cho text xuống dòng */ thường dùng
    overflow: hidden;          /* Cắt phần tràn ra */
    text-overflow: ellipsis;   /* Dấu "..." nếu quá dài */
    min-width: 0;              /* Cho phép co lại đúng / thường dùng / Trong Flexbox, mặc định min-width của item là auto, nên nội dung sẽ không được co lại đủ để dùng ellipsis. min-width: 0 = cho phép co nhỏ hơn nội dung thật(Cho phép co nhỏ hết cỡ, bất chấp nội dung) → để text-overflow: ellipsis hoạt động.
    min-width: fit-content     /* Cho phép co lại đúng / 	Không bao giờ co nhỏ hơn nội dung / thường dùng
    flex-shrink: 1;            /* Co chiều ngang nếu thiếu chỗ */
2. flex box:

flex column đôi khi làm cho các phần tử con bị kéo dãn, do mặc định  justify của nó là strech, nên nó bị dãn, ta dùng justify - start cho cha hoặc justify self cho con là nó ko bị giãn nữa,

Grow - Basis - shrink

flex-grow không hoạt động là do flex-basis đang ở mặc định là auto
flex-basis: auto
Kích thước cơ sở: Tính theo nội dung hoặc giá trị width.
Tương tác với flex-grow: Chia không gian sau khi tính kích thước nội dung.
flex-basis: 0
Kích thước cơ sở: Bỏ qua kích thước của nội dung, kích thước cơ sở là 0.	
Tương tác với flex-grow:	Chia đều không gian dựa trên flex-grow.	



flex-basis: 0
========================================Các vấn đề về position================================================
Chỉ khi dùng position: absolute hoặc fixed vào div con thì div con mới bị loại khỏi flow, không đẩy anh em của div cha nữa, và có thể nổi lên trên (tuỳ vào z-index). Nếu div con dùng dịch chuyển bản thân nó đi (top, left, right, bottom), thì nó cư xử như một div bình thường.
Ngay cả khi bạn có dịch nó (top: 10px), nó vẫn chiếm chỗ như cũ, chỉ thay đổi vị trí hiển thị – không ảnh hưởng đến anh em div cha
ví dụ: Vị trí gốc ban đầu của div con là tọa độ A. Khi bạn đặt top: 10px, nó dịch xuống hiển thị ở tọa độ B. Nhưng về luồng layout, nó vẫn chiếm chỗ tại A như cũ.  div con sẽ đè lên anh em div cha, tức là div con hiển thị phía trên, che khuất div anh em div cha

Header khi thu nhỏ dạng mobile, thì nav sẽ bị tách khỏi header, ẩn đi hoặc hiện ra phía dưới, đấy là dùng absolute cho nav đó để tách nó ra khỏi flow của header.

========================================Học Slider================================================
dù slider có phức tạp, nhưng về cơ bản, nó có 3 lớp chính:
[Container] (viewport) <!-- Có overflow: hidden để ẩn những ảnh bên ngoài khung nhìn. -->
    └── [List slide item] (nơi chứa tất cả item xếp ngang, Có thể di chuyển,thường là bằng transform: translateX(...) hoặc scrollLeft, v.v.)
            └── [item 1]
            └── [item 2]
            └── [item 3]
            ...
             │     ...
├── [← Prev Button]         ← Di chuyển lùi
├── [→ Next Button]         ← Di chuyển tới
└── [Pagination Buttons]    ← Chấm nhỏ để nhảy đến slide
3 cách phổ biến để tạo chuyển động cho slider:

1. Scroll-based (xài scrollbar) – đơn giản nhất
List item dài hơn Container, nên sẽ tạo thanh cuộn ngang (scrollbar).
Người dùng hoặc code sẽ cuộn ngang để xem các ảnh. Người dùng scroll bằng chuột hoặc bạn dùng JS để gọi .scrollLeft += 500 chẳng hạn để dịch trái phải
2. Transform-based (dịch chuyển vị trí) Dùng  transform: translateX(...).
.slider {
  display: flex;
  transition: transform 0.5s;
  transform: translateX(-500px); // dịch sang trái 1 slide
}
Khi nhấn nút "Next", JS sẽ thay đổi translateX → slider "trượt".
3. CSS Scroll Snap (hiện đại)
Dựa trên scrollbar nhưng tự động “dính” vào slide gần nhất sau khi scroll.
.container {
  scroll-snap-type: x mandatory;
  overflow-x: scroll;
  display: flex;
}

.slide {
  scroll-snap-align: start;
  flex-shrink: 0;
  width: 100%;
}
Khi scroll bằng tay hoặc JS, nó tự "bám dính" vào slide gần nhất.

========================================Tailwind css tutorial=================================

--------------------------------------|Extension ------------------------------------
Tailwind CSS IntelliSense: khi giữ chuột tại 1 className css, nó sẽ giải thích ý nghĩa khi đc viết trong css. 
    trong setting:
        tìm kiếm files Associations, set item: *.css, value: tailwindcss
        tìm kiếm quick suggestions, phần string, chọn on
Prettier - Code formatter: chưa biết
--------------------------------------|Plugin------------------------------------
npm -i -D prettier-plugin-tailwindcss

: 


--------------------------------------|Install------------------------------------
Để sử dụng tailwind css, ta phải có nodejs, sử dụng npm để cài đặt nó
tailwind css đc dùng trong nextjs và reactjs, và các dự án có file package.json
câu lệnh tải xuống: npm install tailwindcss
sau khi tải xuống, project sẽ đc thêm thư mục node_modules, file package-lock.json, và 1 dependence trong file package.json
@tailwind base;
@tailwind components;
@tailwind utilities;
--------------------------------------|How it work------------------------------------

--------------------------------------|Sử dụng đơn vị tùy chỉnh------------------------------------
dùng cú pháp -[] để tùy chỉnh kích thước
ví dụ: text-[14px]
--------------------------------------|------------------------------------