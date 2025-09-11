==========synchronous - asynchronous: đồng bộ - không đồng bộ===========
java script là 1 ngôn ngữ synchronous (đồng bộ), blocking (chặn(có thể là đóng)), và single-threaded(đơn luồng)
synchronous: code đc thực thi từ trên xuống dưới, mỗi thời điểm thực thi 1 dòng
blocking: dù thế nào thì javascript cũng luôn thực thi theo thứ tự từ trên xuống, từ trước đến sau, vd hàm a phải thực thi 1 chức năng chuyên sâu tốn 1ph, javascript sẽ chạy cho xong hàm a ms qua hàm b
vấn đề: 
trong 1 nhà hàng ,các khách hàng gọi món , ta không thể xử lý nguyên liệu xong nấu từng món lặp đi lặp lại, mà ta nên xử lý bất đồng bộ , trong lúc chờ món này đang trong quá trình nấu thì ta xử lý món khác, nấu món khác,...
khi chạy web, ta thường truy xuất dữ liệu, xử lý dữ liệu, render html + dữ liệu, mà quá trình truy xuất dữ liệu khá lâu khiến ng dùng chờ, nên t muốn javascript chạy không đồng bộ để có thể làm việc khác như render html rỗng trước, như cấu trúc tính năng cốt lõi next js render html cùng lúc xử lý truy vấn database, vì vậy, chúng ta cần javascript có hành vi không đồng bộ để xử lý

=========SetTimeout===========
setTimeout() là 1 hàm thực thi 1 khối lệnh 1 lần sau  khoảng thời gian xác định, hàm này là hàm không đồng bộ, khi chạy đến hàm này, trong lúc code trong hàm đc xử lý, nó sẽ chạy tiếp các phần tiếp theo ở ngoài hàng, đa luồng
setTimeout(function(hàm cần chạy), duration(khoảng thời gian mili giây muốn chờ), param1, param2,...) các param là các tham số truyền vào hàm
vd:
function greet(name){
    console.log('hello $(name)')
}
setTimeout(greet, 2000, 'Vishwas')
    ->log 'hello Vishwas' to the consolo after 2 second (đầu ra: xuất nội dung sau thời gian chờ 2 giây)
muốn hủy thời gian chờ và chạy luôn, ta dùng hàm clearTimeout()
=========setInterval()============
thực thi 1 khối lệnh lặp lại trong một khoảng thời gian
setInterval(function(hàm cần chạy), duration(khoảng thời gian mili giây muốn chờ), param1, param2,...) 
clearInterval() ngừng hàm setInterval ở trên
và các hàm vừa học ở trên đều là các hàm không đồng bộ 
==========CALLBACK============
callback()
function như 1 class object, điều này có nghĩa function như 1 object, function có thể được truyền dưới dạng đối số cho 1 hàm khác, cũng có thể được trả về dưới dạng giá trị từ hàm khác (gán vào 1 biến)
hàm nhận hàm callback làm đối số được gọi là higher order function (hàm bao bọc bên ngoài, nằm trên nên đc xếp bậc cao hơn)

function greet(name){
    console.log("hello ${name}")
}
function higherOrderFunction (callback){
    const name = 'wishwas'
    callback(name)
}
higherOrderFunction(greet)

synchronous callbacks: callback chỉ là truyền hàm này vào hàm khác, code vẫn chạy theo thứ tự từ trên xuống 1 cách đồng bộ, chờ hàm con chạy xong mới chạy tiếp, để callback chạy không đồng bộ, ta phải sử dụng các hàm có cơ chế không đồng bộ, ví dụ lồng hàm callback vào hàm setTimeout
==========Promises()=============
promises là 1 đối tượng sẽ đc ta khai báo ra, nó là 1 hàm không đồng bộ. Đối tượng này là 1 "lời hứa", khi nào ta cần nó thực hiện lời hứa thì ta sẽ gọi hàm then() để lấy kết quả mà nó đã hứa sẽ làm giùm mình, nó ko hẳn là 1 lời hứa mà là 1 lời cam đoan nhận 1 công việc mà ta giao cho nó

tạo 1 đối tượng Promise và đối tượng này có constructor là 1 callback, và callback này chứa  resolve và reject, bên trong hàm promise này ta xử lý các điều kiện, nếu thành công, promise trả về hàm resolve, không thì trả về hàm Reject, vậy hàm resolve và reject này là 2 hàm có sẵn của promise, promise truyền 2 hàm này vào callback và trong callback ta dùng lại, cách truyền trên phần callback ở trên có 1 ví dụ truyền dữ liệu vào hàm callback

function parent(){
    let promise = new Promise(function(resolve, reject) {
        resolve("OK");
        reject("Error");
    });
}
parent.then(
  function(result) {console.log(result);},
  (error) => {console.log(error);}
);

hoặc

const getSomeThing = () => {
    return new Promise((resolve, reject) => {
1       //fetch something
        resolve("data")
        reject("error")
    });
}
getSomeThing.then((result) => {
        console.log(result);
    }
    .catch((error) => {
        console.log(error);
    });
);
cách dùng hàm then
Hàm then() là hàm gọi lời hứa chạy xử lý yêu cầu mà ta đã đăng ký trước đó, hàm then bắt truyền vào 1 callback và callback này phải nhận 1 tham số đầu vào làm dữ liệu để hàm then truyền cho, dữ liệu đó chính là kết quả trả về của lời hứa, cái mà ta truyền vào hàm resolve() và reject()
từ callback ta có thể tùy ý xử lý result như ví dụ trên
cách dùng hàm catch
tương tự hàm then(), hàm catch chạy khi kết quả trả về có lỗi ngoại lệ, ta phải khai báo hàm catch thì mới có thể dò lỗi
Xâu chuỗi lời hứa
Có thể xâu chuỗi promise với nhau bằng cách trong hàm then, ta lại bắt nó trả về 1 promise để nó lại nhận 1 yêu cầu 1 lần nữa, để ta biến cả cụm trả về đó thành 1 lời hứa, sau đó ta nối tiếp then vào đó
getSomeThing.then((result) => {
    console.log(result);
    return anotherPromise(result)            <- tại đây ta có thể gọi lại chính nó ( chính promise ban đầu để đệ quy)
}).then((result) => {
    console.log(result);
});
==========Fetch API============
Fetch là 1 hàm dùng tạo 1 yêu cầu request gửi đi và sau khi gửi đi, fetch nhận resolve trả về và đưa kết quả đó vào lời hứa và trả về lời hứa, ta có thể hiểu toàn bộ fetch là 1 lời hứa, ta dùng hàm then() để thực thi lời hứa như cách coi fetch là 1 promise, promise này cũng có resolve và reject, resolve sẽ trả về hàm then() và reject sẽ trả về hàm catch()
fetch("class/student.json").then((response) => {
    console.log("resolve ", response)
}).catch((error) => {
    console.log("reject ", error)
});


biến response trên chưa là 1 đối tượng json, web sẽ hiểu nó là 1 đoạn văn bản lộn xộn xuông dòng và các kí tự {} [], như json nhưng ko phải json, để đưa dữ liệu về json, ta dùng hàm .json(), hàm .json() này biến data kia thành đối tượng kiểu json, NHƯNG lại trảm về 1 lời hứa, ta phải dùng hàm then() để lấy dữ liệu từ lời hứa ra
response.json().then(data => {
    console.log(data)
});
hoặc
({
    return response.json()
}).then(data => {
    console.log(data)
});
===========Async & Await==========
Async & Await ko phải 1 đối tượng, 1 phương thức, hàm hay biến, đây là 1 cú pháp khai báo đc js thêm vào sau này, là 1 cách để định nghĩa không đồng bộ cho js hiểu
Async & Await rút gọn các cú pháp asynchronous và promise cho đỡ lộn xộn

Để định nghĩa 1 hàm là không đồng bộ, ta thêm cú pháp Async

const getToDo = async () => {
    //code
    return data;
}
hoặc 
async function getToDo(){
    //code
    return data;
}
khi ta gọi 1 hàm async, nó luôn trả về 1 promise, bất kể bên trong có gì
getTodo().then(data => console.log(data)).catch(...)...
Await sử dụng để chờ cho đoạn code thực thi xong
const response =  await fetch("some url");

if (response.status !== 200){
    throw new Error("can not fetch the data")
}
url sai thì code vẫn thực thi, trả về reject, nhưng hàm json() không hiểu đc reject nên sẽ gây ra lỗi khác, để check thủ công url, ta sử dụng if và throw, nếu lỗi sẽ dùng ngay lúc đó, trả về reject cho hàm bao trùm ngoài cùng luôn

const data = await response.json()
console.log(data);
nếu ko có await, test có thể rỗng vì hàm fetch là hàm không đồng bộ, nó có thể chưa kịp get data từ api mà đã bị gọi lấy dữ liệu, nên await ở đây sẽ bắt đoạn code chờ 1 cách không đồng bộ để lấy data, có thể hiểu và coi cả cụm là 1 promise

=========Event Loop==========

=========AJAX================

=========ES6================
=========Arows function================
ajax là viết tắt của asynchronous javascript and xml