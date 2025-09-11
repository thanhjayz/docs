

># ========== Next.js ========= 
React là 1 thư viện dùng để xây dựng giao diện ng dùng
Nextjs là react framework dùng cho công nghiệp, 1 mô hình sử dụng thư viện react có sẵn
Học lại next với lựa chọn js thay vì ts
># ========== Cấu hình dự án ====
>## ========= Tạo project =======

Phải cài đặt nodejs để tạo môi trường phát triển cho nextjs


Tại 1 đường dẫn, chạy lệnh terminal để tạo ra 1 project nextjs:

`npx create-next-app Project_name`

`npx create-next-app@lastest Project_name`


># ========= Component =======
component là 1 thành phần rời có thể tái sử dụng như 1 thẻ trong html

tạo thư mục components song song với app, các file component thường được đặt tên bằng chữ hoa đầu tiên, đuôi là .jsx, ví dụ `Header.jsx`

để sử dụng cần import component đó vào, đặt lại tên cho component gọi vào và bắt buộc phải dùng chữ hoa
`import Header from @/components/Header` 

chỗ này đặt tên gì cũng đc, tên này sẽ là tên thẻ component cần chèn, để gọi component vào vị trí cần chèn, dùng như thẻ htlm `<Header />` hoặc `<Header> <Header/>` nếu như component chứa  `{children}`

>## ========= tổ chức components ======== 

có thể tạo 1 thư mục component đại diện: ví dụ Header, trong thư mục này chứa index.jsx, và các components khác chỉ nằm trong header này
1 thư mục shared chứa các component nhỏ như  button, warning....
1 thư mục Svg chứa các ảnh svg, làm cho code clean
```
/components
  ├── Header
  │   ├── index.jsx           // Xuất component Header chính
  │   ├── NavBar.jsx          // Component NavBar
  │   ├── Logo.jsx            // Component Logo
  │   └── SearchBar.jsx       // Component SearchBar
  ├── Shared                  // Thư mục chứa các component dùng chung
  │   ├── Button.jsx          // Component Button
  │   ├── Warning.jsx         // Component Warning
  │   └── InputField.jsx      // Component InputField
  ├── Svg                     // Thư mục chứa các ảnh SVG
  │   ├── Icon1.svg           // SVG Icon 1
  │   ├── Icon2.svg           // SVG Icon 2
  │   └── Logo.svg            // Logo SVG
  └── Footer.jsx 
```

>## ========= Component sẵn ======= 

nextjs hỗ trợ các component sẵn và ko thể tự ý sử dụng thẻ html tương ứng
>### Link đến 1 liên kết :
```
import Link from "next/link"
<Link href="/blog">Blog</Link>
```
>### Image :
```
import Image from 'next/image'
<Image
  src="/profile.png"
  width={500}
  height={500}
  alt="Picture of the author"
/>
```
Image ko cho phép các liên kết tên miền ngoài project, nếu muốn dùng ảnh từ nguồn ngoài, phải sửa file next.config.js
```
module.exports = {
  images: {
    domains: ['example.com', 'another-domain.com'],
  },
};
```
nếu là file next.config.mjs
```
const nextConfig = {
    images: {
      domains: ['encrypted-tbn0.gstatic.com'], // Thêm domain ở đây
    },
  };
  
  export default nextConfig;
  
```



># ========= Props ======== 

truyền dữ liệu cho component con :__ để truyền dữ liệu vào component con(pass data), thì componenent mà nhận dữ liệu phải khai báo props 
ví dụ: export default function Header(props: any)
sử dụng dữ liệu đc truyền: {props.name}
để truyền dữ liệu: giả sử có biến const name = "thanh", truyền nó dưới dạng 1 attribute
`<Header name= {name}` hoặc truyền vào giá trị string thì `address={"Đà Nẵng"}` `message={"a message from home"} />`

Ở trong component con, ta gọi lại tập hợp các biến đó, cú pháp `{props.children}`

>## ========= Key props ======== 

 là 1 dạng props khác được tích hợp sẵn, nghĩa là có sẵn, được định nghĩa sẵn, chỉ dùng lại theo quy tắc, dùng để truyền dữ liệu giống như props , key props attribute đc viết thẳng là key luôn, ví dụ: `<div key={số}> </div>` hoặc `key={"chữ"}`, thường là đại diện cho id của thẻ này, thường được dùng để phân biệt các thành phần, key chỉ đc xài 1 lần trong 1 thẻ, xài 2 hay nhiều key sẽ lỗi
truyền props nhiều component lồng nhau, nhiều hơn 2 component lồng nhau, thì các component sau từ component con đầu tiên trở lên nên truyền dưới dạng `{...props}`, level tiếp theo vẫn nhận props như cũ







># ========= Route =========
Route là định tuyến địa chỉ cho từng trang web
>Để đặt tên url, ta đặt tên folder, đường dẫn của folder tương ứng địa chỉ trình duyệt

__url :__ : tên thư mục sẽ tạo đường dẫn của địa chỉ, ví dụ thư mục dashboard sẽ tạo ra địa chỉ localhost/dashboard. thư mục lồng nhau tạo ra đường dẫn lồng nhau, ví dụ `localhost/dashboard/user`

__hide groups route :__ ví dụ auth sẽ chứa nhiểu url như login, register,... , nếu ta muốn ẩn group route auth thì đặt tên folder route đó là  `(auth)`

__url động :__ địa chỉ động, địa chỉ không cố định mà tùy vào trang đó, ví dụ: product/id, để tạo url động cần đặt tên theo quy tắc `url_name`, ví dụ `productId`

__url động lồng nhau :__ để cho phép nhiều url động lồng nhau ta dùng cú pháp `...url_name`, ví dụ `...multi_url`, khi ta lấy param sẽ trả về cả cụm url về sau, params lúc này sẽ ở dạng mảng, phải dùng hàm map duyệt mảng
file page.jsx sẽ là file nội dung của đường dẫn đó

__lấy url :__ để trích xuất url động, ta dùng params
```
export default function Page({ params })
    params.slug <- sẽ chứa url
```
nếu url động lồng nhau:
```
const slugs = params.slug || [];
  slugs.map((slug, index) => (
    <li key={index}>Post Slug: {slug}</li>)
  )
```
không thể dùng useRouter từ next/route và next/navigate vì next/route chỉ hoạt động ở phía client, next/navigate thì có getpathName thì lấy hết đường dẫn, trong khi ta chỉ cần phần đường dẫn động, vì vậy chỉ còn slug

__private folder :__ là thư mục mà hệ thống sẽ không cho vào router, chỉ cần đặt tên thư mục đó _name, thêm dấu gạch dưới vào trước tên, đây là nơi lưu các chức năng ( ví dụ dùng 1 file để format date )

>## =========== Page file ============

có nhiều loại page, đã được quy ước sẵn: Not Found Page, layout, template, loading, error ...

__page.jsx :__ file page.jsx trong mỗi thư mục url, tương ứng triển khai giao diện cho trang đó của địa chỉ đó

__layouts.jsx :__ 1 trang giao diện bao bọc lấy route hiện tại, tạo file tên layout.jsx, layout sẽ phải chứa children để truyền page vào, layout có thể lồng nhau
```
export default function DashboardLayout({children}) {
  return <section>{children}</section>
}
```
__templates.jsx.jsx :__ 1 trang nằm bên trong layout, nhưng lại bao bọc bên ngoài trang page, khi dùng layout, nó áp dụng lên tất cả các trang dùng chung 1 trang, chí dụ có 1 form, khi ta điền vào, khi qua 1 trang khác cũng dùng chung layout thì dữ liệu đã điền trong form sẽ ko bị xóa đi mà vẫn ở trang mới, vì vậy ta dùng template để các trang luôn load lại template , loại bỏ hết state cũ

__loading page.jsx :__  1 file màn hình load khi trang đang tải, đặt tên là loading.jsx

__not-found.jsx :__ nằm ở cùng thư mục cần tạo trang not-found, tạo fine not-found.jsx
not found page ko chỉ dùng để chặn link lạ, mà còn dùng để chuyển hướng khi link ko hợp lệ, ví dụ:
import { notFound } from "next/navigation";
ở 1 chỗ nào đó có sử dụng hàm notFound() để chuyển hướng về trang notFound, ví dụ liên kết đến link sản phẩm ko tồn tại

__error page.jsx__ file error.jsx để hiển thị ra lỗi khi trang báo lỗi
```
export default function ErrorBoudary(error) {
  return <div>page error, decription error: {error}</div>
}
```


># ===== Route handle - API routes =====
Trong nextjs , việc xử lý các phương thức HTTP request như GET, POST,... đc thực hiện thông qua api route

Ứng với mỗi đường dẫn, ta đã có file route.js để handle đóng vai trò như 1 api, nhưng trong thư mục app/api, ta có thể tạo api ở đây, để tránh xung đột giữa route.js và page.jsx nằm cùng folder thì họ thường để file route.js vào thư mục api

Các hàm trong route là các hàm ứng với các phương thức request như GET, POST,PUT, PATCH, DELETE

__Cách dùng:__
+ Get: đường dẫn cơ bản đến trang page
+ Post: lấy dữ liệu từ db dựa vào req
+ Put: lưu dữ liệu vào db
+ Patch: thay đổi dữ liệu trong db
+ Delete: xóa dữ liệu trong db


>Để triển khai các hàm này, ta triển khai như sau:
```
import { NextResponse } from 'next/server';
export async function GET() {
   return NextResponse.json({ message: 'Hello, world!' });
}
export async function POST(req) {
  const data = await req.json();
  new data = data + thay đổi data

  return new Response(JSON.stringify(data), {
    status: 200,
    headers: { 'Content-Type': 'text/plain' },
  });
}
```
ở đây , server trả về dữ liệu, ta có thể xài: Response hoặc NextResponse

Response là một phần của Web API tiêu chuẩn và có thể được sử dụng trong bất kỳ môi trường nào hỗ trợ Fetch API hoặc tương tự.

NextResponse là một phần của Next.js và tích hợp sâu hơn với các tính năng của Next.js như middleware và API routes.



`new Response(body, options)` cung cấp các tính năng cơ bản của một phản hồi HTTP. Body là nội dung của phản hồi.

options là một đối tượng chứa các tùy chọn như status, headers,...

Phản hồi được tạo ra bằng Response hoàn toàn tương thích với các tiêu chuẩn web API.

`NextResponse` cung cấp các phương thức tiện ích như json(), redirect(), và rewrite() giúp việc xử lý phản hồi và điều hướng dễ dàng hơn trong môi trường Next.js


 Xử lý api tuyến đường động
```
export async function GET(req, {params}) {
  //từ đây lấy param
  const url_dynamic = params.slug
  return NextResponse.json("something");
}
```
__URL Query Parameters :__

 trích xuất url , lọc url, tạm chưa hiểu đc, trong codevolution

__Redirects in Route Handlers:__

 chuyển hướng trang, dùng khi...
 

__auto redirect :__ tự động chuyển hướng đến 1 trang nào đó
```
"use client"
import { useRouter } from "next/navigation"

const router = useRoute();
router.push("/home")
```
## JSON 
Json tương đồng với các đối tượng của js, nhưng js ko hiểu đc json , vì vậy, cần chuyển đổi giữa json và js

`.json(): ` là một phương thức của đối tượng Response trong Fetch API

`JSON.stringify(): ` chuyển đổi từ đối tượng JavaScript thành chuỗi JSON



># ====== Hook và Event Handling ====
># ====== Event handle ==========

khi lướt web có rất nhiều sự kiện, ta cần bắt sự kiện đó, ta sẽ dùng các event handle để bắt sự kiện và liên kết nó vào 1 hàm, khi sự kiện được kích hoạt sẽ thực thi hàm, ví dụ click chuột,...

`onClick={handleClick}`khi click

`onChange={handleChange}` khi nó bị thay đổi nội dung hoăc trạng thái ví dụ như sửa form hoăc disble 1 cái nút,...

`onSubmit={handleSubmit}` khi submit form

`onClick=((param1, param2)=>{console.log("arrow function")})` khi dùng arrow function

tất cả đều đc viết nằm trong thuộc tính của thẻ html
># ======= HOOK ==========

useState là 1 phần của reac hook, cái mà cho phép ta điều khiển các chức năng component hay sử dụng và có sẵn

có 5 hook hay sử dụng:

State hook: `useState()` và `useReduce()` giúp quản lý trạng thái của 1 biến dữ liệu

Effect hook:` useEffect()` kết nối với các hệ thống bên ngoài như api

Context hook: `useContext()`

Reference hook: `useRef()` tham chiếu các thẻ html

Performance hook: `useMemo()`,` useCallback()` cải thiện hiệu suất bằng cách ngăn chặn những thứ ko cần thiết 

hay sử dụng: `useState()`, `useRef()`, `useEffect()`
>## ======= State ========
useState để quản lý trạng thái của dữ liệu

tạo state: để tạo trạng thái mặc định

`const [like, setLike] = useState("")` ở đây ta truyền giá trị mặc định vào hoặc để chuỗi trống(số, chữ, true false,...), usestate sẽ trả về biến like và hàm setLike

`[value, setValue] = useState(0)`

ví dụ sử dụng: `onChange = {(e)=> setValue(e.target.value)}`
>## ======= Effect ====== 

Là cách React thay thế cho các lifecycle methods (componentDidMount, componentDidUpdate, componentWillUnmount) trong class component.

Dùng để thực hiện các thao tác như: fetch API, đăng ký event listeners, thao tác với DOM, v.v.

useEffect(() => {
  // Code thực hiện side effect ở đây
  
  return () => {
    // Cleanup function (optional)
  };
}, [dependencies]); // Mảng dependencies


`const [like, setLike] = useState("")` ở đây ta truyền giá trị mặc định vào hoặc để chuỗi trống(số, chữ, true false,...), usestate sẽ trả về biến like và hàm setLike

`[value, setValue] = useState(0)`

ví dụ sử dụng: `onChange = {(e)=> setValue(e.target.value)}`
>## ======= Reduce =======

useState để quản lý trạng thái của dữ liệu

tạo state: để tạo trạng thái mặc định

`const [like, setLike] = useState("")` ở đây ta truyền giá trị mặc định vào hoặc để chuỗi trống(số, chữ, true false,...), usestate sẽ trả về biến like và hàm setLike

`[value, setValue] = useState(0)`

ví dụ sử dụng: `onChange = {(e)=> setValue(e.target.value)}`
># ======= Client - Server =====
__Server :__ Mặc định, tất cả các thành phẩn của nextjs đc coi là server component. Server component nghĩa là nó cái trang đó (cái component đó đã được chạy, biên dịch, gắn dữ liệu trên server và trả về cả trang hoàn chỉnh cho client). Lợi ích là bảo mật hơn, SEO tốt hơn, ko phải truy cập vào máy chủ nữa. Nhược điểm là không thể tương tác, ví dụ ko có state, các lệnh console.log đc chạy hết trên máy chủ nên người dùng ko thấy log, ko test được.
__Client :__ Là thành phẩn client component, server gửi 1 gói xuống cho ng dùng biên dịch và chạy, nó chứa code tương tác chạy với ng dùng đc( nó có quyền truy cập đầy đủ vào các api của trình duyệt có sẵn, các api này cho phép tương tác )


>## ====== CSR (Client side rendering) =====
Ở react, server gửi về 1 thẻ div root và 1 file bundle.js javascript, từ đó, toàn bộ trang đc render ở phía ng dùng bằng cách chạy file javascript đó, tạo ra trang html đầy đủ, nếu có dữ liệu, js sẽ chạy ở phía ng dùng, lấy dữ liệu và ghép dần vào. Đây chính là csr

cách dùng: dùng "use client" ở đầu file jsx, đây là chỉ thị (directive) trong Nextjs dùng để chỉ định component này sẽ chạy ở client thay vì server (nghĩa là chuyển thành CSR). Sau đó dùng hook, ví dụ use effect, state,... dữ liệu sẽ đc 

>## ====== ISR ( side rendering) =====


>## ====== Pre rendering =====
“render” và “hydrate”
Render nghĩa là tạo ra giao diện HTML
Hydrate là bước xảy ra sau khi HTML đã được pre-render. Nó là quá trình React "gắn lại" logic JavaScript (event handler, state, v.v.) vào HTML tĩnh đã được gửi từ server.

Ở nextjs, server đã render từng trang html trên server, server gửi file html này về cho client hiển thị, đây chính là pre-rendering, giúp cải thiện hiệu suất và SEO tốt hơn, pre-rendering là mặc định của nextjs
NextJS hỗ trợ 2 kiểu là static generation và server side rendering
>### ====== SSG (Static side generation) ======
Render lúc build (chạy next build), nghĩa là chạy một lần duy nhất trước khi bạn triển khai (deploy) website lên server. HTML được tạo ở server, nhưng tạo sẵn từ trước, không phải lúc có request. Phù hợp cho dữ liệu ít thay đổi, hoặc có thể cập nhật định kỳ. 
Mặc định của Next.js là Static Generation (SSG)
`Cách dùng: Dùng getStaticProps hoặc getStaticPaths trong Next.js`
Dữ liệu được fetch và embed (nhúng dữ liệu vào html) trực tiếp vào HTML ngay từ server trước khi gửi về client
>### ======= SSR (Server side rendering) =======

Render mỗi lần có request từ người dùng. HTML được tạo ở server, ngay tại thời điểm người dùng truy cập. Chậm hơn SSG – vì phải render mỗi lần
`Cách dùng:	Dùng getServerSideProps trong Next.js`

# ======= services - actions =======
Người dùng Next.js thường tạo thêm hai thư mục services/ và actions/ để tổ chức mã nguồn cho rõ ràng và dễ bảo trì hơn

services/ — Xử lý logic gọi API, kết nối bên ngoài, chứa các hàm truy xuất với dữ liệu bên ngoài: gọi API, truy vấn database, tương tác Firebase, Supabase, Prisma, v.v.

actions/ — Xử lý sự kiện phía client hoặc server, chứa các các hàm logic ứng dụng, ví dụ: xử lý form, submit, trigger API nội bộ, login, logout, v.v.
Bên trong các actions có thể gọi service
```
// actions/authActions.js
'use server';
import { createUser, getUserByEmail } from '@/services/userService';
```



## ======== Session - jwt =========
Session 
Người dùng gửi thông tin đăng nhập đến máy chủ, máy chủ xác minh thông tin đăng nhập so với database, nếu đúng, server tạo ra 1 session, server lưu trữ session này trên database, session này bao gồm sessionID, userID, lifetime ( vòng đời ),... , server gửi lại 1 cookie về cho client chứa sessionID gọi là session token, từ đây, mỗi lần gửi request lên server, client sẽ đính kém cookie chứa sessionID ( session token ) lên server, server kiểm tra sessionID so với sessionID trên database, nếu mà đúng thì sẽ xử lý yêu cầu. sessionID này có thể coi như token. Lợi thế session là thu hồi session trên server đơn giản. Bất cứ khi nào ng dùng lấy thông tin từ session như userID, name, avata, ... thì client sẽ gửi request đến /api/auth/session, lấy data tương ứng session token

JWT
Người dùng gửi thông tin đăng nhập đến máy chủ, máy chủ xác minh thông tin đăng nhập so với database, nếu đúng, server tạo ra 1 mã thông báo jwt ( gọi là 1 token ), server sẽ kí jwt bằng 1 khóa bí mật, server gửi cookie chứa token này cho client lưu trữ,  mỗi lần client gửi request lên server sẽ gửi kèm token này, server xác minh chữ kí jwt, nếu đúng, sẽ xử lý yêu cầu, dùng nó để xác thực và ủy quyền. server ko cần lưu trữ token. Tất cả dữ liệu cần thiết đã đc chứa trong token và lưu ở máy khách, điều này làm cho jwt không có trạng thái.
Một JWT gồm 3 phần <Header>.<Payload>.<Signature>
Ví dụ: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjMiLCJuYW1lIjoiNguyá»…n Váº¡n AIn0.sfdsk2JH93Jhd-saf82jskfjd...
Khi client gửi JWT lên, server Giải mã header và payload ( chỉ cần base64 decode đã có sẵn → không cần chữ ký secret ).
Sau đó, kiểm tra chữ ký (signature) bằng cách: Dùng chính secret (mà server đã cấu hình) để ký lại <Header>.<Payload> . So sánh chữ ký đó với Signature được client gửi lên

1 số tài liệu đề coi cả 2 là session, nhưng 1 cái là session database, 1 cái là session jwt

Header (Tiêu đề), Ví dụ:
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```
Payload (Nội dung) chứa các "claims" — thông tin về người dùng hoặc dữ liệu khác. Ví dụ:
```
{
  "sub": "1234567890",
  "name": "Nguyễn Văn A",
  "admin": true,
  "iat": 1516239022
}
```
# ======= auth =======
nextjs có 2 dạng xác thực phổ biến là nextAuth.js và auth.js
NextAuth.js là một thư viện xác thực (authentication) dành riêng cho Nextjs. chỉ dùng được với Nextjs, không dùng được cho các framework khác như Remix, SvelteKit, Nuxt..  

Auth.js là phiên bản mới ( tức là phiên bản đa nền tảng thay thế cho NextAuth.js cũ ). Là "kế nhiệm" chính thức của NextAuth.js. Hỗ trợ nhiều framework: Next.js, SvelteKit, Nuxt, Astro, SolidStart, Express, v.v. Cách sử dụng và cấu hình rất giống NextAuth.js, chỉ khác ở cách tích hợp vào từng framework.

# ======== Auth.js =======
ta sẽ tập trung vào auth.js
tài liệu doc mới nhất của Auth.js, sẽ thiếu vắng những phần như: session config, jwt config, callbacks như jwt(), session(), signIn(), events, pages, adapter, v.v. vì Auth.js (mới) đã chia nhỏ các tính năng thành các package riêng biệt, không còn gộp vào @next-auth/react hoặc next-auth như trước nữa. vẫn có thể cấu hình session, callbacks, jwt... như trước

## ======== Cài đặt =======
cài đặt thư viện auth.js bằng lệnh npm: ` npm install next-auth@beta `
## ======== Cài đặt biến môi trường =======

biến môi trường là 1 biến bắt buộc để mã hóa token. Nó là 1 chuỗi 24 kí tự ngẫu nhiên 
để sinh biên môi trường tự động, dùng lệnh ` npx auth secret ` hoặc ` npm exec auth secret `
hoặc tự tạo trong file .env 1 biến môi trường ` AUTH_SECRET="This is an example" ` 

tiếp đến là cặp biến của nhà cung cấp xác thực ủy quyền bên thứ 3
AUTH_[PROVIDER]_ID=
AUTH_[PROVIDER]_SECRET=
ví dụ:
```
# Google
AUTH_GOOGLE_ID=123
AUTH_GOOGLE_SECRET=123
 
# GitHub
AUTH_GITHUB_ID=123
AUTH_GITHUB_SECRET=123
```

Khi triển khai web lên sản phẩm, sẽ phải cáu hình lại tên dự án thay vì tên localhost chạy trên máy cục bộ

` NEXTAUTH_URL=https://example.com `

## ======== Cấu hình ============
Tạo 1 file auth.js tại root, tại đây, ta sẽ viết cấu hình cho xác thực
```
import NextAuth from "next-auth"
// Tùy vào phương thức xác thực mà bạn import nhà cung cấp dịch vụ xác thực
import Google from "next-auth/providers/google"
import GitHub from "next-auth/providers/github"
import Credentials from "next-auth/providers/credentials"
//import bộ kết nối database
import { PrismaAdapter } from "@auth/prisma-adapter"
import { prisma } from "@/prisma"
 

export const { handlers, signIn, signOut, auth } = NextAuth({

  // khai báo provider, là nhà cung cấp xác minh đăng nhập bạn sử dụng
  providers: [

    // xác minh đăng nhập bằng nhà cung cấp dịch vụ bên thứ 3 ủy nhiệm, cú pháp mặc đinh 
    Github,

    // Nếu bạn sử dụng tên biến môi trường trong file .env  tùy chỉnh, thì phải chỉ cho provider hiểu theo cách thủ công
    Google ({
      clientId: "YOUR_CLIENT_ID",
      clientSecret: "YOUR_CLIENT_SECRET",
    }),


    //xác minh đăng nhập bằng tài khoản mật khẩu truyền thống gọi là Credentials
    Credentials({

      // nextauth và authjs cung cấp 1 form đăng nhập có sẵn, được liên kết với đối tượng credentials
      // đối tượng credentials dùng để khai báo cấu trúc của form đăng nhập để NextAuth.js (hoặc NextAuth) biết bạn muốn người dùng nhập những gì.
      credentials: {
        email:  { label: "Tên đăng nhập", type: "text", placeholder: "nhập email tại đây" },
        password: {},
      },
      // nếu tự dùng không dùng form login mẫu có sẵn, mà tự viết, thì phải tự viết chức năng gửi credentials đến NextAuth thông qua hàm signIn() mà NextAuth cung cấp. Sẽ được bày ở dưới

      // Viết hàm xác minh đăng nhập
      authorize: async (credentials) => {
        let user = null
        //viết logic để hash password
        // viết logic để kiểm tra đăng nhập, kiểm tra user có tồn tại, đúng ra là được viết trong services hoặc action
        user = await getUserFromDb(credentials.email, pwHash)
        if (!user) {
          // No user found, so this is their first attempt to login
          // Optionally, this is also the place you could do a user registration
          throw new Error("Invalid credentials.")
        }
        return user
      },
        
      // adapter dùng để chỉ định bộ kết nối DB
      adapter: PrismaAdapter(prisma),

      // Thiết lập cấu hình session
      session: {
        strategy: "jwt" | "database"; // thiết lập xem phương pháp sessionlaf gì, mặc định là jwt, nếu có adapter, mặc định là db
        generateSessionToken: () => string; // thiết lập tùy biến token, là 1 chuỗi UUID ngẫu nhiên hoặc chuỗi string theo mặc định hoặc  CUID
        maxAge: number; // thời gian vòng đời session
        updateAge: number; // Tần suất reset vòng đời session
      };

      // thiết lập cấu hình jwt
      jwt: {
        maxAge: 60 * 60 * 24 * 7, thời gian vòng đời , tính bằng giây
        encode: async ({ token, secret }) => {
          return jwtSign(token, secret); // tuỳ biến mã hoá
        },
        decode: async ({ token, secret }) => {
          return jwtVerify(token, secret); // tuỳ biến giải mã
      }
      
      // Về callback trong jwt, khi có hành động gì đó xảy ra (user đăng nhập, tạo JWT, tạo session...), bạn có thể chèn code của mình vào
      callbacks: {
        
        jwt({ token, user }) {
          if (user) {
            token.role = user.role; // thêm role từ DB vào JWT
          }
          return token;
        }

        session({ session, token }) {
          session.user.role = token.role; // chuyển role từ JWT vào session
          return session;
        }

        async redirect({ url, baseUrl }) {
          return baseUrl + "/dashboard";
        }

        async signIn({ user, account, profile }) {
          if (user.email.endsWith("@example.com")) {
            return true; // cho phép đăng nhập
          }
          return false; // chặn lại
        }
      },
      





}




    }),
  ],
})





```








về next-auth

// Mặc định, nextauth và authjs dùng session jwt, nhưng nếu bạn chỉ định adapter, session mặc định sẽ là database, Vẫn có thể tùy chỉnh thành jwt nếu cần
session: { 

  // strategy dùng để chỉ định session dạng db hay jwt
  strategy: "database",

  // maxAage để thiết lập tuổi thọ vòng đời session, tính bằng giây
  maxAge: 30 * 24 * 60 * 60, // 30 days

  // tần suất thiết lập lại tuổi thọ của sesion
  updateAge: 24 * 60 * 60, // 24 hours

  // session token thường là 1 dãy UUID ngẫu nhiên hoặc chuỗi string, bạn có thể tùy biến token
  generateSessionToken: () => {
    return randomUUID?.() ?? randomBytes(32).toString("hex")
  }
}

  // nếu bạn chỉ định strategy là DB, jwt sẽ k chạy
jwt: {

  // tùy chỉnh tuổi thọ vòng đời session jwt, tihs bằng giây
  maxAge: 60 * 60 * 24 * 30,
  
  // Có thể định nghĩa lại mã hóa và giải mã theo cách riêng
  async encode() {},
  async decode() {},
}

// auth cung cấp các trang mẫu sẵn tích hợp như đăng nhập, đăng xuất, quên mk,....
// Bạn có thể tạo các trang của riêng bạn, khai báo nó cho auth hiểu, ghi đè lên trang mẫu sẵn.
pages: {
  signIn: '/auth/signin',
  signOut: '/auth/signout',
  error: '/auth/error', // Error code passed in query string as ?error=
  verifyRequest: '/auth/verify-request', // (used for check email message)
  newUser: '/auth/new-user' // New users will be directed here on first sign in (leave the property out if not of interest)
}

Callback là các hàm bất đồng bộ xảy ra ngay sau khi các hàm auth đc thực thi, bạn có thể tùy chỉnh chuyên sâu callback.
callbacks: {
  async signIn({ user, account, profile, email, credentials }) {
    return true
  },
  async redirect({ url, baseUrl }) {
    return baseUrl
  },
  async session({ session, token, user }) {
    return session
  },
  async jwt({ token, user, account, profile, isNewUser }) {
    return token
  }
}

// event là hàm chạy ở phí server, sau khi thực thi 1 hàm auth xác thực, dùng để ghi log, gửi email thông báo, cập nhật data BD phụ, giúp sửa lỗi
events: {
  async signIn(message) { /* on successful sign in */ },
  async signOut(message) { /* on signout */ },
  async createUser(message) { /* user created */ },
  async updateUser(message) { /* user updated - e.g. their email was verified */ },
  async linkAccount(message) { /* account (e.g. Twitter) linked to a user */ },
  async session(message) { /* session is active */ },
}




Auth.js mới được viết lại hoàn toàn để chạy trong môi trường "route handlers" của App Router, nơi không còn kiểu (req, res) như trong NextAuth.js (Pages Router).

Trong Auth.js, authorize() chỉ nhận 1 tham số duy nhất là credentials








## ======== Cấu hình Trình xử lý api ======

Tạo 1 file route tại ` ./app/api/auth/[...nextauth]/route.js `
Tệp này dùng để xử lý tuyến đường API, nextjs có sẵn các phương thức giao thức kết nối HTTP GET và POST sẵn, nhưng chưa bao gồm xác minh đăng nhập. trong khi đó, nextauth cung cấp hàm handlers là một hàm xử lý tất cả các phương thức HTTP cho bạn mà không cần phải viết riêng cho từng phương thức (GET, POST, v.v.). Và ta ko muốn mỗi lần dùng http lại phải gọi hàm handle, nên ta xuất handle cho GET và POST gốc luôn. 
```
import { handlers } from "@/auth"
export const { GET, POST } = handlers
```
## ======== Middleware =======
middleware.js
```
export { auth as middleware } from "@/auth"
```











# ======== Formik + Yup =======
Formik là một thư viện quản lý form nhẹ, linh hoạt và mạnh
Yup là một công cụ dùng để Kiểm tra dữ liệu theo schema mà bạn cấu hình
## ======== Cài đặt =======
Cài đặt  formik, chạy terminal `  npm install formik --save ` hoặc `  yarn add formik `
Cài đặt  Yup, chạy terminal `  npm install yup `
## ======== Sử dụng =======
Formik là thư viện React thuần client-side (dùng state, event, form xử lý trên trình duyệt) nên phải có "use client"

có 2 cách dùng formik là Sử dụng hook useFormik (Hook-based API), và Sử dụng component <Formik> (Component-based API)
### Cách 1: Sử dụng hook useFormik (Hook-based API)
```
"use client"
import { useFormik } from 'formik'; //import hook
 const SignupForm = () => {
   const formik = useFormik({      //gọi hook để sử dụng
     initialValues: {
       email: '',
       password: '',
     },
     onSubmit: values => {
       alert(JSON.stringify(values, null, 2));
     },
   });
   return (
     <form onSubmit={formik.handleSubmit}> //gán hook vào các attribute của thẻ html
       <label htmlFor="email">Email Address</label> 
       <input
         id="email"
         name="email"
         type="email"
         onChange={formik.handleChange} //gán hook vào các attribute của thẻ html
         value={formik.values.email} //gán hook vào các attribute của thẻ html
       />
       <input
         id="password"
         name="password"
         type="password"
         onChange={formik.handleChange} //gán hook vào các attribute của thẻ html
         value={formik.values.password} //gán hook vào các attribute của thẻ html
       />
       <button type="submit">Submit</button>
     </form>
   );
 };
return
...
  {formik.errors.lastName ? <div>{formik.errors.lastName}</div> : null}
```
cách sử dụng validate 
```
const validate = values => {
   const errors = {};
   if (!values.firstName) {
     errors.firstName = 'Required';
   } else if (values.firstName.length > 15) {
     errors.firstName = 'Must be 15 characters or less';
   }
 
   if (!values.lastName) {
     errors.lastName = 'Required';
   } else if (values.lastName.length > 20) {
     errors.lastName = 'Must be 20 characters or less';
   }
 
   if (!values.email) {
     errors.email = 'Required';
   } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
     errors.email = 'Invalid email address';
   }
 
 };
```
### Cách 2: Sử dụng component <Formik> (Component-based API), khai báo các props của Component
```
"use client" 
import React from 'react';
import { Formik } from 'formik';

const initialValues = {
  email: '',
  password: '',
}
const handleSubmit = values => {
  console.log(values)
}
const validationSchema = Yup.object({
  email: Yup.string().email("email không hợp lệ").required('Trường này là bắt buộc.'),
  password: Yup.string().required('Trường này là bắt buộc.')
})

const BasicExample = () => (
<div>
  <h1>My Form</h1>
  <Formik
  initialValues=initialValues           //khai báo props của component
  onSubmit=handleSubmit                 //khai báo props của component
  validationSchema={validationSchema}>  //khai báo props của component
    <Form>
      <Field
        type="email"  name="email"
        value={props.values.name}
      />
      <ErrorMessage component="p" name="email"  />
      <Field
        type="password"  name="password"
        value={props.values.password}
      />
      <ErrorMessage component="p" name="password"  />
      <button type="submit">Submit</button>
    </Form>
    
  </Formik>
</div>);
```
### Cách dùng yup
```
const validationSchema = Yup.object({
  email: Yup.string().email("email không hợp lệ").required('Trường này là bắt buộc.'),
  password: Yup.string().required('Trường này là bắt buộc.')
})

```
# ============================== swiper =============================
swiper là 1 thư viện giúp tạo slider cực nhanh, mạnh, và siêu nhẹ, cảm ứng rất mượt cho trình duyệt (nextjs) và cả ứng dụng mobile (react-native)
## ======== Cài đặt =======
chạy terminal ` npm install swiper `
## ======== Sử dụng =======
swiper đc sử dụng với nhiều farmework, nên mỗi fw có 1 cách dùng khác nhau, ta sẽ học với cách dùng với react/ nextjs
swiper là component client side, nên bắt buộc phải có ` use client `

// Import Swiper React components
import { Swiper, SwiperSlide } from 'swiper/react'; //Đây là các React component chính bắt buộc dùng để render Swiper trong React/Next.js.
import { Navigation } from 'swiper/modules' //Đây là module điều hướng, giúp Swiper biết cách xử lý nút bấm.

// Import Swiper styles
import 'swiper/css';

Từ đây là ta đã có thể sử dụng swiper cơ bản
Cấu trúc viết component swiper như sau: 
swiper có 2 cách viết, 1 là  Thuần HTML + JS + nối với đúng tên clas của Swiper để Swiper dò tìm
<!-- Slider main container -->
<div class="swiper">
  <!-- component chứaslider -->
  <div class="swiper-wrapper">
    <!-- Slides -->
    <div class="swiper-slide">Slide 1</div>
    <div class="swiper-slide">Slide 2</div>
    <div class="swiper-slide">Slide 3</div>
    ...
  </div>
  <!-- Nếu cần button điều khiển slider -->
  <div class="swiper-button-prev"></div>
  <div class="swiper-button-next"></div>
  <!-- Nếu cần phân trang -->
  <div class="swiper-pagination"></div>
  <!-- Nếu cần thanh trượt -->
  <div class="swiper-scrollbar"></div>
</div>
2 là dùng component Swiper có sẵn của react/nextjs
<Swiper modules={[Navigation, Pagination, Scrollbar]
 navigation
 pagination
 scrollbar }>
  <SwiperSlide>Slide 1</SwiperSlide>
  <SwiperSlide>Slide 2</SwiperSlide>
  ...
</Swiper>

## ============ Tùy biến slider swiper ============
<Swiper
  // modules={[mảng chứa các chức năng]}, dùng để bật các tính năng mở rộng, không bật thì không chạy, ví dụ: Navigation (nút điều khiển), Pagination(nút phân trang), Scrollbar(thanh trượt), Autoplay(tự động chạy), Free Mode(cảm ứng lướt), Accessibility (a11y - phím tắt).
  modules={[Navigation, Pagination, Scrollbar, A11y]}
  // props, hiểu là Parameters, dùng để tiết lập config chi tiết cho module, bao gồm module có sẵn mở rộng tự động và module bạn vừa bật ở trên, modules ở trên cũng là 1 props với giá trị là 1 mảng
  spaceBetween={50}
  slidesPerView={3}
  navigation
  pagination={{ clickable: true }}
  scrollbar={{ draggable: true }}
  onSwiper={(swiper) => console.log(swiper)}
  onSlideChange={() => console.log('slide change')}
>
quan hệ giữa props module và các props còn lại hiểu như : “Tôi đã cài đèn rồi (modules=[Navigation]), giờ tôi bật công tắc (navigation) để đèn sáng.”

## ===== các module thường dùng =====
🔹 Cấu hình slider
1. Navigation – Nút điều hướng trái/phải
2. Pagination – Dấu chấm phân trang
3. Autoplay – Tự động chuyển slide
4. Scrollbar – Thanh kéo (thường dùng trong mobile)
5. A11y – Trợ năng, hỗ trợ cảm ứng & bàn phím cơ bản
6. FreeMode – Trượt tự do, không snap vào vị trí slide
7. Keyboard – Điều khiển bằng bàn phím (trái/phải)
8. Mousewheel – Cuộn bằng chuột (dọc hoặc ngang)
9. Thumbs – Slider con để điều khiển slider chính (thường dùng cho gallery)
🔹 Hiệu ứng chuyển động
10. EffectFade – Chuyển mờ dần
11. EffectCoverflow – Hiệu ứng bìa sách (3D nghiêng)
12. EffectCube – Hiệu ứng khối 3D
13. Zoom – Phóng to nội dung (ít dùng trừ khi xem ảnh)
## ===== các props thường dùng ===== 
0. modules	                                                                  Khai báo các module cần dùng (Navigation, Pagination, Autoplay, ...)
1. slidesPerView          	number | 'auto'	                                  Số slide hiển thị trong 1 lần. 'auto' để tự động theo kích thước slide
2. spaceBetween           	number	                                          Khoảng cách giữa các slide (đơn vị: px)
3. loop	                    boolean	                                          Cho phép lặp vô hạn các slide
4. autoplay                 object	                                          Tự động chạy slide (VD: { delay: 3000 })
5. navigation	              boolean | object	                                Bật/tắt hoặc tùy chỉnh nút điều hướng trái/phải
6. pagination	              boolean | object	                                Hiển thị dấu chấm phân trang
7. scrollbar	              boolean | object	                                Hiển thị thanh cuộn (scrollbar)
8. freeMode	                boolean	                                          Trượt tự do (không snap vào từng slide)
9. effect	                  'slide' | 'fade' | 'cube' | 'coverflow' | 'flip'	Hiệu ứng chuyển slide
10. onSlideChange	          function	                                        Callback khi chuyển slide
11. onSwiper	              function	                                        Callback trả về instance Swiper sau khi khởi tạo
12. breakpoints	            object	                                          Cấu hình responsive theo kích thước màn hình
13. direction	              'horizontal' | 'vertical'	                        Chiều trượt của slide
## Các lỗi thường gặp
slideperview: auto không hoạt động, thẻ SwiperSlide bị width: full  ghi đè
 Tailwind className="w-[200px]" KHÔNG tác động trực tiếp được vì bạn đang style một React component, không phải thẻ HTML. className là 1 props, ko thể truyền vào dom thẻ cha, trong khi style co thể tryền xuyên qua dom.

># Moongoose
># Prisma
># Next-auth
># Supabase 

































## ======== Session - jwt =========
Session 
Người dùng gửi thông tin đăng nhập đến máy chủ, máy chủ xác minh thông tin đăng nhập so với database, nếu đúng, server tạo ra 1 session, server lưu trữ session này trên database, session này bao gồm sessionID, userID, lifetime ( vòng đời ),... , server gửi lại 1 cookie về cho client chứa sessionID gọi là session token, từ đây, mỗi lần gửi request lên server, client sẽ đính kém cookie chứa sessionID ( session token ) lên server, server kiểm tra sessionID so với sessionID trên database, nếu mà đúng thì sẽ xử lý yêu cầu. sessionID này có thể coi như token. Lợi thế session là thu hồi session trên server đơn giản. Bất cứ khi nào ng dùng lấy thông tin từ session như userID, name, avata, ... thì client sẽ gửi request đến /api/auth/session, lấy data tương ứng session token

JWT
Người dùng gửi thông tin đăng nhập đến máy chủ, máy chủ xác minh thông tin đăng nhập so với database, nếu đúng, server tạo ra 1 mã thông báo jwt ( gọi là 1 token ), server sẽ kí jwt bằng 1 khóa bí mật, server gửi cookie chứa token này cho client lưu trữ,  mỗi lần client gửi request lên server sẽ gửi kèm token này, server xác minh chữ kí jwt, nếu đúng, sẽ xử lý yêu cầu, dùng nó để xác thực và ủy quyền. server ko cần lưu trữ token. Tất cả dữ liệu cần thiết đã đc chứa trong token và lưu ở máy khách, điều này làm cho jwt không có trạng thái.
Một JWT gồm 3 phần <Header>.<Payload>.<Signature>
Ví dụ: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxMjMiLCJuYW1lIjoiNguyá»…n Váº¡n AIn0.sfdsk2JH93Jhd-saf82jskfjd...
Khi client gửi JWT lên, server Giải mã header và payload ( chỉ cần base64 decode đã có sẵn → không cần chữ ký secret ).
Sau đó, kiểm tra chữ ký (signature) bằng cách: Dùng chính secret (mà server đã cấu hình) để ký lại <Header>.<Payload> . So sánh chữ ký đó với Signature được client gửi lên

1 số tài liệu đề coi cả 2 là session, nhưng 1 cái là session database, 1 cái là session jwt

Header (Tiêu đề), Ví dụ:
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```
Payload (Nội dung) chứa các "claims" — thông tin về người dùng hoặc dữ liệu khác. Ví dụ:
```
{
  "sub": "1234567890",
  "name": "Nguyễn Văn A",
  "admin": true,
  "iat": 1516239022
}
```
# ======= auth =======
nextjs có 2 dạng xác thực phổ biến là nextAuth.js và auth.js
NextAuth.js là một thư viện xác thực (authentication) dành riêng cho Nextjs. chỉ dùng được với Nextjs, không dùng được cho các framework khác như Remix, SvelteKit, Nuxt..  

Auth.js là phiên bản mới ( tức là phiên bản đa nền tảng thay thế cho NextAuth.js cũ ). Là "kế nhiệm" chính thức của NextAuth.js. Hỗ trợ nhiều framework: Next.js, SvelteKit, Nuxt, Astro, SolidStart, Express, v.v. Cách sử dụng và cấu hình rất giống NextAuth.js, chỉ khác ở cách tích hợp vào từng framework.

# ======== Auth.js =======
ta sẽ tập trung vào auth.js
tài liệu doc mới nhất của Auth.js, sẽ thiếu vắng những phần như: session config, jwt config, callbacks như jwt(), session(), signIn(), events, pages, adapter, v.v. vì Auth.js (mới) đã chia nhỏ các tính năng thành các package riêng biệt, không còn gộp vào @next-auth/react hoặc next-auth như trước nữa. vẫn có thể cấu hình session, callbacks, jwt... như trước

## ======== Cài đặt =======
cài đặt thư viện auth.js bằng lệnh npm: ` npm install next-auth@beta `
## ======== Cài đặt biến môi trường =======

biến môi trường là 1 biến bắt buộc để mã hóa token. Nó là 1 chuỗi 24 kí tự ngẫu nhiên 
để sinh biên môi trường tự động, dùng lệnh ` npx auth secret ` hoặc ` npm exec auth secret `
hoặc tự tạo trong file .env 1 biến môi trường ` AUTH_SECRET="This is an example" ` 

tiếp đến là cặp biến của nhà cung cấp xác thực ủy quyền bên thứ 3
AUTH_[PROVIDER]_ID=
AUTH_[PROVIDER]_SECRET=
ví dụ:
```
# Google
AUTH_GOOGLE_ID=123
AUTH_GOOGLE_SECRET=123
 
# GitHub
AUTH_GITHUB_ID=123
AUTH_GITHUB_SECRET=123
```

Khi triển khai web lên sản phẩm, sẽ phải cáu hình lại tên dự án thay vì tên localhost chạy trên máy cục bộ

` NEXTAUTH_URL=https://example.com `

## ======== Cấu hình ============
Tạo 1 file auth.js tại root, tại đây, ta sẽ viết cấu hình cho xác thực
```
import NextAuth from "next-auth"
// Tùy vào phương thức xác thực mà bạn import nhà cung cấp dịch vụ xác thực
import Google from "next-auth/providers/google"
import GitHub from "next-auth/providers/github"
import Credentials from "next-auth/providers/credentials"
//import bộ kết nối database
import { PrismaAdapter } from "@auth/prisma-adapter"
import { prisma } from "@/prisma"
 

export const { handlers, signIn, signOut, auth } = NextAuth({

  // khai báo provider, là nhà cung cấp xác minh đăng nhập bạn sử dụng
  providers: [

    // xác minh đăng nhập bằng nhà cung cấp dịch vụ bên thứ 3 ủy nhiệm, cú pháp mặc đinh 
    Github,

    // Nếu bạn sử dụng tên biến môi trường trong file .env  tùy chỉnh, thì phải chỉ cho provider hiểu theo cách thủ công
    Google ({
      clientId: "YOUR_CLIENT_ID",
      clientSecret: "YOUR_CLIENT_SECRET",
    }),


    //xác minh đăng nhập bằng tài khoản mật khẩu truyền thống gọi là Credentials
    Credentials({

      // nextauth và authjs cung cấp 1 form đăng nhập có sẵn, được liên kết với đối tượng credentials
      // đối tượng credentials dùng để khai báo cấu trúc của form đăng nhập để NextAuth.js (hoặc NextAuth) biết bạn muốn người dùng nhập những gì.
      credentials: {
        email:  { label: "Tên đăng nhập", type: "text", placeholder: "nhập email tại đây" },
        password: {},
      },
      // nếu tự dùng không dùng form login mẫu có sẵn, mà tự viết, thì phải tự viết chức năng gửi credentials đến NextAuth thông qua hàm signIn() mà NextAuth cung cấp. Sẽ được bày ở dưới

      // Viết hàm xác minh đăng nhập
      authorize: async (credentials) => {
        let user = null
        //viết logic để hash password
        // viết logic để kiểm tra đăng nhập, kiểm tra user có tồn tại, đúng ra là được viết trong services hoặc action
        user = await getUserFromDb(credentials.email, pwHash)
        if (!user) {
          // No user found, so this is their first attempt to login
          // Optionally, this is also the place you could do a user registration
          throw new Error("Invalid credentials.")
        }
        return user
      },
        
      // adapter dùng để chỉ định bộ kết nối DB
      adapter: PrismaAdapter(prisma),

      // Thiết lập cấu hình session
      session: {
        strategy: "jwt" | "database"; // thiết lập xem phương pháp sessionlaf gì, mặc định là jwt, nếu có adapter, mặc định là db
        generateSessionToken: () => string; // thiết lập tùy biến token, là 1 chuỗi UUID ngẫu nhiên hoặc chuỗi string theo mặc định hoặc  CUID
        maxAge: number; // thời gian vòng đời session
        updateAge: number; // Tần suất reset vòng đời session
      };

      // thiết lập cấu hình jwt
      jwt: {
        maxAge: 60 * 60 * 24 * 7, thời gian vòng đời , tính bằng giây
        encode: async ({ token, secret }) => {
          return jwtSign(token, secret); // tuỳ biến mã hoá
        },
        decode: async ({ token, secret }) => {
          return jwtVerify(token, secret); // tuỳ biến giải mã
      }
      
      // Về callback trong jwt, khi có hành động gì đó xảy ra (user đăng nhập, tạo JWT, tạo session...), bạn có thể chèn code của mình vào
      callbacks: {
        
        jwt({ token, user }) {
          if (user) {
            token.role = user.role; // thêm role từ DB vào JWT
          }
          return token;
        }

        session({ session, token }) {
          session.user.role = token.role; // chuyển role từ JWT vào session
          return session;
        }

        async redirect({ url, baseUrl }) {
          return baseUrl + "/dashboard";
        }

        async signIn({ user, account, profile }) {
          if (user.email.endsWith("@example.com")) {
            return true; // cho phép đăng nhập
          }
          return false; // chặn lại
        }
      },
      





}




    }),
  ],
})





```








về next-auth

// Mặc định, nextauth và authjs dùng session jwt, nhưng nếu bạn chỉ định adapter, session mặc định sẽ là database, Vẫn có thể tùy chỉnh thành jwt nếu cần
session: { 

  // strategy dùng để chỉ định session dạng db hay jwt
  strategy: "database",

  // maxAage để thiết lập tuổi thọ vòng đời session, tính bằng giây
  maxAge: 30 * 24 * 60 * 60, // 30 days

  // tần suất thiết lập lại tuổi thọ của sesion
  updateAge: 24 * 60 * 60, // 24 hours

  // session token thường là 1 dãy UUID ngẫu nhiên hoặc chuỗi string, bạn có thể tùy biến token
  generateSessionToken: () => {
    return randomUUID?.() ?? randomBytes(32).toString("hex")
  }
}

  // nếu bạn chỉ định strategy là DB, jwt sẽ k chạy
jwt: {

  // tùy chỉnh tuổi thọ vòng đời session jwt, tihs bằng giây
  maxAge: 60 * 60 * 24 * 30,
  
  // Có thể định nghĩa lại mã hóa và giải mã theo cách riêng
  async encode() {},
  async decode() {},
}

// auth cung cấp các trang mẫu sẵn tích hợp như đăng nhập, đăng xuất, quên mk,....
// Bạn có thể tạo các trang của riêng bạn, khai báo nó cho auth hiểu, ghi đè lên trang mẫu sẵn.
pages: {
  signIn: '/auth/signin',
  signOut: '/auth/signout',
  error: '/auth/error', // Error code passed in query string as ?error=
  verifyRequest: '/auth/verify-request', // (used for check email message)
  newUser: '/auth/new-user' // New users will be directed here on first sign in (leave the property out if not of interest)
}

Callback là các hàm bất đồng bộ xảy ra ngay sau khi các hàm auth đc thực thi, bạn có thể tùy chỉnh chuyên sâu callback.
callbacks: {
  async signIn({ user, account, profile, email, credentials }) {
    return true
  },
  async redirect({ url, baseUrl }) {
    return baseUrl
  },
  async session({ session, token, user }) {
    return session
  },
  async jwt({ token, user, account, profile, isNewUser }) {
    return token
  }
}

// event là hàm chạy ở phí server, sau khi thực thi 1 hàm auth xác thực, dùng để ghi log, gửi email thông báo, cập nhật data BD phụ, giúp sửa lỗi
events: {
  async signIn(message) { /* on successful sign in */ },
  async signOut(message) { /* on signout */ },
  async createUser(message) { /* user created */ },
  async updateUser(message) { /* user updated - e.g. their email was verified */ },
  async linkAccount(message) { /* account (e.g. Twitter) linked to a user */ },
  async session(message) { /* session is active */ },
}




Auth.js mới được viết lại hoàn toàn để chạy trong môi trường "route handlers" của App Router, nơi không còn kiểu (req, res) như trong NextAuth.js (Pages Router).

Trong Auth.js, authorize() chỉ nhận 1 tham số duy nhất là credentials








## ======== Cấu hình Trình xử lý api ======

Tạo 1 file route tại ` ./app/api/auth/[...nextauth]/route.js `
Tệp này dùng để xử lý tuyến đường API, nextjs có sẵn các phương thức giao thức kết nối HTTP GET và POST sẵn, nhưng chưa bao gồm xác minh đăng nhập. trong khi đó, nextauth cung cấp hàm handlers là một hàm xử lý tất cả các phương thức HTTP cho bạn mà không cần phải viết riêng cho từng phương thức (GET, POST, v.v.). Và ta ko muốn mỗi lần dùng http lại phải gọi hàm handle, nên ta xuất handle cho GET và POST gốc luôn. 
```
import { handlers } from "@/auth"
export const { GET, POST } = handlers
```
## ======== Middleware =======
middleware.js
```
export { auth as middleware } from "@/auth"
```




## ======== Cài đặt =======
## ======== Cài đặt =======
## ======== Cài đặt =======










































# ======= services - actions =======
Người dùng Next.js thường tạo thêm hai thư mục services/ và actions/ để tổ chức mã nguồn cho rõ ràng và dễ bảo trì hơn

services/ — Xử lý logic gọi API, kết nối bên ngoài, chứa các hàm truy xuất với dữ liệu bên ngoài: gọi API, truy vấn database, tương tác Firebase, Supabase, Prisma, v.v.

actions/ — Xử lý sự kiện phía client hoặc server, chứa các các hàm logic ứng dụng, ví dụ: xử lý form, submit, trigger API nội bộ, login, logout, v.v.
Bên trong các actions có thể gọi service
```
// actions/authActions.js
'use server';
import { createUser, getUserByEmail } from '@/services/userService';
```

# ======== Formik + Yup =======











# ======== swiper =======
swiper là 1 thư viện giúp tạo slider cực nhanh, mạnh, và siêu nhẹ, cảm ứng rất mượt cho trình duyệt (nextjs) và cả ứng dụng mobile (react-native)
## ======== Cài đặt =======
chạy terminal ` npm install swiper `
## ======== Sử dụng =======
swiper đc sử dụng với nhiều farmework, nên mỗi fw có 1 cách dùng khác nhau, ta sẽ học với cách dùng với react/ nextjs
swiper là component client side, nên bắt buộc phải có ` use client `

// Import Swiper React components
import { Swiper, SwiperSlide } from 'swiper/react'; //Đây là các React component chính bắt buộc dùng để render Swiper trong React/Next.js.
import { Navigation } from 'swiper/modules' //Đây là module điều hướng, giúp Swiper biết cách xử lý nút bấm.

// Import Swiper styles
import 'swiper/css';

Từ đây là ta đã có thể sử dụng swiper cơ bản
Cấu trúc viết component swiper như sau: 
swiper có 2 cách viết, 1 là  Thuần HTML + JS với đúng tên clas của Swiper để Swiper dò tìm
<!-- Slider main container -->
<div class="swiper">
  <!-- component chứaslider -->
  <div class="swiper-wrapper">
    <!-- Slides -->
    <div class="swiper-slide">Slide 1</div>
    <div class="swiper-slide">Slide 2</div>
    <div class="swiper-slide">Slide 3</div>
    ...
  </div>

  <!-- Nếu cần button điều khiển slider -->
  <div class="swiper-button-prev"></div>
  <div class="swiper-button-next"></div>
  
  <!-- Nếu cần phân trang -->
  <div class="swiper-pagination"></div>


  <!-- Nếu cần thanh trượt -->
  <div class="swiper-scrollbar"></div>
</div>
2 là dùng component Swiper có sẵn của react/nextjs
<Swiper modules={[Navigation, Pagination, Scrollbar]
 navigation
 pagination
 scrollbar }>
  <SwiperSlide>Slide 1</SwiperSlide>
  <SwiperSlide>Slide 2</SwiperSlide>
  ...
</Swiper>

## ============ Tùy biến slider swiper ============
<Swiper
  // modules={[mảng chứa các chức năng]}, dùng để bật các tính năng mở rộng, không bật thì không chạy, ví dụ: Navigation (nút điều khiển), Pagination(nút phân trang), Scrollbar(thanh trượt), Autoplay(tự động chạy), Free Mode(cảm ứng lướt), Accessibility (a11y - phím tắt).
  modules={[Navigation, Pagination, Scrollbar, A11y]}
  // props, hiểu là Parameters, dùng để tiết lập config chi tiết cho module, bao gồm module có sẵn mở rộng tự động và module bạn vừa bật ở trên
  spaceBetween={50}
  slidesPerView={3}
  navigation
  pagination={{ clickable: true }}
  scrollbar={{ draggable: true }}
  onSwiper={(swiper) => console.log(swiper)}
  onSlideChange={() => console.log('slide change')}
>
Hiểu như : “Tôi đã cài đèn rồi (modules=[Navigation]), giờ tôi bật công tắc (navigation) để đèn sáng.”

## ===== các module thường dùng ===== 
Navigation	Nút trái/phải điều hướng
Pagination	Dấu chấm phân trang bên dưới slider
Scrollbar	Thanh kéo
Autoplay	Tự động chuyển slide
A11y	Hỗ trợ cảm ứng lướt
EffectFade	Hiệu ứng mờ dần khi chuyển slide
EffectCube	Hiệu ứng khối 3D
EffectCoverflow	Hiệu ứng bìa sách
Zoom	Phóng to nội dung
Thumbs	Slider con để điều khiển slider chính
Keyboard	Điều khiển bằng bàn phím
Mousewheel	Điều khiển bằng cuộn chuột
FreeMode	Trượt tự do, không ràng buộc vị trí slide
Grid	Hiển thị lưới 2D

## ===== các parameter thường dùng ===== 
autoplay: {
   delay: 5000,
 }
## Các lỗi thường gặp
 Tailwind className="w-[200px]" KHÔNG tác động trực tiếp được vì bạn đang style một React component, không phải thẻ HTML.















 # Next.js 
# Tạo dự án
next tạo dự án theo phiên bản next hiện tại trong node
npx create-next-app Project_name 
tạo dự án theo phiên bản next mới nhất
npx create-next-app@lastest Project_name
terminal sẽ hỏi :
Would you like to use TypeScript, chọn Yes để vừa dùng js vừa dùng typeScript đc
Would you like to use Esline, chọn Yes
Would you like to use Tailwin CSS, chọn Yes để tích hợp thư viện Tailwin vào, code css nhanh hơn
Would you like to use 'src' directory,để lựa chọn có lồng thư mục App vào trong thư mục src ko, thư mục App là toàn bộ App của mình(route, component,...)
Would you like to customize the default import alias, chọn no để nó mặc định cho dễ hiểu
# route
route là định tuyến địa chỉ cho từng trang web của hệ thống
Tùy vào next js từng phiên bản sẽ route khác nhau
Các đinh tuyến sẽ nằm trong thư mục App, các file định tuyến cố định luôn cđc đặt tên page.tsx hoặc page.js, ở đây dùng tsx cho chuẩn và dễ nâng cấp
Địa chỉ của định tuyến đó dựa vào thư mục chứa nó
ở phiên bản 12 trở xuống, các file định tuyến đc đặt tên index.js, index.tsx
# nested route
nested route là định tuyến lồng nhau, để lồng nhau các địa chỉ web
để tạo định tuyến lồng nhau, ta tạo các thư mục lồng nhau, trong các thư mục lồng nhau đó phải chứa file định tuyến đc đặt tên là page.tsx
App/products/page.tsx
App/about/page.tsx
App/contatc/page.tsx
App/about/page.tsx
các file định tuyến page.tsx phải có export default, có thể đặt tên hàm khác tên thư mục chứa file routing
export default function page() {
  return (
    <div>page</div>
  )
}
# dynamic route
nested dynamic route là định tuyến động, nghĩa là định tuyến không có đường dẫn cố định tại 1 phần của định tuyến
tình huống: Tạo đường dẫn cho nhiều sản phẩm khác nhau, mỗi sản phẩm có 1 id, tương ứng 1 đường dẫn, ta không thể tạo từng đường dẫn cho từng sản phẩm mà phải tạo 1 đoạn đường dẫn động, linh hoạt, có thể thay đổi theo từng sản phẩn mà đáp ứng tất cả sản phẩm
để tạo dynamic route, ta thay đổi tên của folder route đó dưới dạng [dynamic_route_name( or folder_name)]
để lấy phần tên (id) đường dẫn động đó, ta sử dụng props params

export default function productDetail( {params}: {
    params: {
        productId: string;
    }
}) {
    console.log({params})
  return (
    <>
        Viewing product {params.productId}
    </>
  )
}

# nested dynamic route
nested dynamic rout là định tuyến động vào nhau
/category/[category]/subcategory/[subcategory]
hoặc định tuyến động lồng nhau nằm cạnh nhau 
/category/[category][subcategory]
# catch all segments
phần định tuyến động có thể được mở rộng, để bắt tất cả các phần định tuyến tiếp theo nằm sau \ bằng cách thêm dấu ba chấm bên trong dấu ngoặc [...segmentName]
tình huống: Người dùng nhập vào 1 đường dẫn nhiều thành phần, dài, nằm ngoài phạm vi route có sẵn, mình cần bắt phần dãy liên kết đó vào 1 mảng, vd:
/category/shoe/nikes/size43/thin/black/... more and more/...
thì t đặt tên phần định tuyến sẽ bắt tất cả định tuyến phía sau bằng tên [...dynamic_route_name]
để bắt tất cả phần định tuyến phía sau và đưa vào 1 mảng, ta dùng props params 

export default function docs({params}: {
    params:{
        slug: string[];
    };
}) {
    if(params.slug?.length===2)
    return (
        <h1>Viewing docs for feature {params.slug[0]} and concept {params.slug[1]}</h1>
    )
    if(params.slug?.length===1)
    return (
        <h1>Viewing docs for feature {params.slug[0]}</h1>
    )
    return (
        <h1>Viewing docs file</h1>
    )  
}
# Not Found Page
Page không đc tìm thấy là page chưa đăng ký trên route
để truy cập vào Not Found Page, ta nhập 1 địa chỉ ko tồn tại, vd: /billing
để custom Page Not Found, ta tạo file not-found.tsx trong app( thư mục root ( gốc ) ), phải đúng đường dẫn và đúng tên thư mục
tại đây, t có thể custom page tùy ý như 1 file page.tsx bình thường, có export default function, return
# Private Folders
Tạo thư mục trong App với tên có gạch dưới ở ký tự đầu tiên: _folder_Name, route sẽ không bao gồm thư mục này, cho dù trong này có chứa page.tsx thì route này vẫn không đc thực thi(không đc cho là tồn tại, chỉ là 1 folder chứa các file)
# Route Groups
Nhóm nhiều route vào 1 folder route, vd:
route auth chứa route register, login, forgot-password
Điều này tạo cho đường dẫn có cùng 1 đoạn url, tạo ra sự phức tạp, ta sẽ dùng ngoặc đơn route group này lại để ẩn đoạn url này
(auth) , /auth/login -> /login
# Layouts file
Trang giao diện bố cục được áp dụng cho toàn bộ các route nằm cùng file layout.tsx này trở đi, ko áp dụng cho các trang parent ở ngoài
mặc định sẽ luôn có 1 file layout.tsx nằm ở App, ta có thể chỉnh sửa file này, áp dụng lên toàn bộ trang web
file layout.tsx phải luôn 1 có props children để có thể truyền lại bố cục này vào props cho các trang con áp dụng lại
nếu xóa file layout.tsx tại App, next sẽ tự tạo lại
nếu muốn tạo file layout cho 1 group route nào đó, phải tạo thủ công và copy lại function của  layout gốc (copy export function và tự thay đổi nội dung return), ví dụ tạo layout cho trang auth
export default function authLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      <h1>This is our Auth page</h1>
      {children}
    </section>
  )
}

# Nested layout + group layout
Có thể tạo file layout.tsx áp dụng cho 1 số group route hoặc route riêng lẻ cụ thể. Điều này sẽ cho phép bạn tạo 1 hoặc 1 nhóm trang có sử dụng chung 1 layout
Tạo file layout.tsx tại folder group route hoặc route cần áp dụng layout
cấu trúc file layout này tương tự file layout gốc
# Routing Metadata
Biến có khả năng kế thừa trong nhiều layout
Biến này có thể sử dụng trong layout.tsx hay page.tsx
Biến này có các dạng tồn tại 1 đối tượng (object) tĩnh (static) hay 1 hàm (function) động (dynamic)
quy tắc: khi sử dụng trong layout.tsx, nó áp dụng lên tất cả các trang sau đó, nếu sử dụng trong page.tsx, nó chỉ áp dụng lên 1 trang đó. metadata được (hệ thống hiểu, đọc) read từ root level (cấp bậc gốc) đến final page level (ngọn cuối dùng)
page metadata sẽ ghi đè layout metadata nếu chúng trùng nhau
object metadata: 
export const metadata = {
    title: "Next.js",
    description: "Generated by Next.js",
    keywords: "Next.js tutorial"
}
function metadata: dynamic metadata dùng cho thông tin động như tham số tuyến đườn`g hiện tại, dữ liệu bên ngoài, segment(đọc ở trên), ví dụt trang product id cần title cho mỗi sản phẩm, nhưng tất cả sản phẩm lại cùng 1 layout
phải dùng  prop , tạo 1 biến params để ghi lại giá trị biến động, rồi gán biến động đó vào metadata, sau đó biến này mới đc đưa vào hàm export dèault
import { Metadata } from "next";

type Props = {
    params: {
        productId: string;
    }
}

export const generateMetadata = ({ params }: Props): Metadata =>{
    return {
        title: 'product ' + ${params.productId}
    }
}

export default function bookDetails({ params }: Props) {
  return (
    <>
    <h1>Details about product {params.productId}</h1>
    </>
  )
}

# Title Metadata
đối tượng title trong metadata có thêm 3 thuộc tính:
title: {
    absolute: "", //title này cố định và không thể bị sửa đổi, áp dụng cho tất cả các trang con, nếu trang con bị trang cha có thuộc tính template ghi đè, nó cũng không bị ghi đè
    default: "",//định danh title cho các trang con chưa có tiêu đề riêng
    template: "%s | Next.js tutorial" // sử dụng tiêu đề của trang con ghép vào %s, xong sử dụng toàn bộ chuỗi làm title
  },

# Link Component Navigation
Để điều hướng trong Nextjs, ta ko dùng thẻ a, mà phải dùng thẻ link, thẻ link là 1 thành phần component sẵn có của Nextjs
Cần import Link library vào
import Link from "next/link"
Sử dụng thẻ Link để tạo liên kết
<Link href="/about">About us</Link>
<Link href="/books">All books</Link>
Link to dynamic route
Có nhiều trang động, như sản phẩm 1, 2, 3,... để điều hướng đến trang động, ta điểu hướng đến biến chứa thông tin trang động đó
const productId = "book100"
<Link href="/books/{productId}">Book 100</Link>
nhưng ở next 13 đổ lên, next thay đổi bộ thư viện mã nguồn nền tảng, tăng tốc trải nghiệm mượt hơn nhưng bộ thư viện này chưa ổn định
hiện tại, khi viết {productId}, next chưa hiểu đây là đối số truyền vào, nên dẫn thẳng đến đường dẫn /books/{productId} như coi đây là 1 chuỗi
cách khắc phục tạm thời: dùng as
<Link
    href="/books/[productId]"
    as={`books/${productId}`}>
    book101
</Link>
quy tắc: trong thẻ <Link/>, props bắt buộc phải có:
href
props tự chọn có thể thêm vào:
as: dùng để phân tích, như ví dụ trên

passHref:
prefetch: dùng để bật tắt tải trước giao diện trang, mặc định là bật vì next mặc định tải trước trang, đây là tính năng của next, prefetch={false}
replace: giả sử đang đứng ở trang 1, nhấn vào đường dẫn sẽ đi đến trang 2, nhấn back sẽ quay lại trang 1, nhưng nếu dùng replace, nó sẽ thay thế trang 1 thành trang 2, khi nhấn back sẽ quay lại trang 0 (trang trước 1), trang 1 đã bị ghi đè lại vào lịch sử duyệt web
replace={false}
nâng cao: replace={product === '3' ? true : false}
scroll: đi tới trang sau nhưng còn cuộn tới 1 phần cụ thể của trang sau, dựa vào  hash ids(mã băm của component trong trang sau)
<Link href="https://refine.dev/blog/mui-datagrid-refine/#material-ui-datagrid-component">
  <a>Scroll to a specific section</a>
</Link>
locale: điều hướng ng dùng đến các phiên bản của trang web dựa trên ngôn ngữ, ví dụ: chúng tôi có thể có một blog được người dùng tiếng Anh và tiếng Pháp đọc và chúng tôi cần hiển thị nội dung blog bằng tiếng Anh cho người dùng tiếng Anh và bằng tiếng Pháp cho người dùng tiếng Pháp
cần đọc tài liệu về định cấu hình trang web: https://nextjs.org/docs/pages/building-your-application/routing/internationalization
shallow: Các đạo cụ cho phép chúng tôi cập nhật đường dẫn của trang hiện tại mà không cần chạy bất kỳ phương thức tìm nạp dữ liệu Next.js nào (nghĩa là , hoặc ). Đối tượng được cập nhật và liên kết với URL mới có thể được truy cập bởi đối tượng được cung cấp bởi useRouter hoặc withRouter.shallow getStaticProps getServerSideProps getInitialProps pathname query router
ví dụ + tài liệu: https://refine.dev/blog/next-js-link-component/#scroll-to-a-specific-section-in-a-webpage-using-hash-ids
https://github.com/vercel/next.js/blob/canary/examples/with-shallow-routing/pages/index.js

# Active Links
ta có phân tách 1 cụm link thành nhiều module:
const navLinks = [
  {name: "Register", href: "/register"},
  {name: "Login", href: "/login"},
  {name: "Forgot password", href: "forgot-password"}
]
trong hàm export:
{navLinks.map((link) => {
    return (
    <Link href={link.href} key={link.name}>
        {link.name}
    </Link>
    );
})}

Active link là xác định 1 liên kết đã truy cập và tùy biến lại giao diện, liên kết đó:
const isActive = pathname.startsWith(link.href)
tạo 1 biến isActive, dùng hàm pathname.startsWith(), truyền vào link hiện tại để kiểm tra link active chưa, hàm này trả về true/false
vd tùy biến:
classname = {isActive : "font-bold" : "text-blue"}

# Navigating Programmatically
tự động điều hướng ng dùng đến 1 trang web khác, ví dụ như sau khi bấm chức năng đặt hàng thì sẽ điểu hướng đến trang khác như trang chủ, lịch sử đặt hàng,... chứ ko phải là 1 liên kết sẽ đi đến trang đó
chức năng này liên quan đến thư viện hook  nên phải thêm dòng
"use client";
import useRouter from "next/navigation"
consr router = useRouter()
const handleClick = () => {
  //các câu lệnh xử lý trước khi chuyển hướng, ví dụ như gọi api đặt hàng
  router.push("/")
};

<button onclick={handleClick}>Đặt hàng</button>
# Templates------------------------------------
tình huống: chúng ta có cây route gồm auth bao gồm login, register, forgot password, file layout.tsx chứa bố cục chung cho 3 trang, và đường dẫn liên kết qua lại 3 trang, lúc này, khi ta bấm vào liên kết qua các trang đều là trang con (first child) của auth, next chỉ load lại phần child component, bố cục chung ko phải load, cả state,...
file template tương tự file layout, nhưng tái khởi động lại dữ liệu (state, effect), giữ nguyên hệ thống dom chứ ko tái build lại
tạo file template tương tự tạo file layout, có thể đổi tên file layout thành file template hoặc copy nội dung file layout vào file template, group route có thể vừa chứa file layout vừa chứa file template, trong đó file layout render trước rồi đến file template(template là con của layout, hay layout nằm ngoài, template nằm trong)

# Loading UI
như vậy, ta đã học qua page.tsx. layout.tsx. not-found.tsx, template.tsx, h sẽ học đến loading.tsx
tạo 1 giao diện trong khi chờ trạng thái tải của trang khác đc hiển thị(như phòng chờ), tại đây có thể tạo các giao diện chờ như bộ xương, vòng quay,..., tác dụng trấn an ng dùng, giúp họ yên tâm khi chờ phản hồi,...

# Error Handling
tiếp theo là file error.tsx
đôi khi, trình duyệt ko thể tránh khỏi lỗi, chẳng hạn tìm nạp dữ liệu bị lỗi

# Parallel Routes
Parallel Routes là các tuyến song song cho phép hiển thị đồng thời nhiều trang trong cùng 1 bố cục.
tình huống: ví dụ trang dashboard(quản trị của admin) phức tạp, gồm nhiều task (chứa các chức năng), cần hiển thị nhiều chế độ xem khác nhau
(ít dùng)
# Route Handlers
**Route** là dịnh tuyến đến 1 trang web, **Route Handlers** là tính năng giúp nắm đc quá trình định tuyến để custom (tùy chỉnh) nó (xử lý tuyến đường định tuyến)
ví dụ khi truy cập 1 trang web, trang web trả về 1 trang html, nhưng trang web thường kèm theo data, vậy chúng ta phải xử lý quá trình truy cập trang web đó để thực hiện các thao tác như truy xuất data, gán data vào trang web,...

tại folder router hiện tại, tạo file route.ts
quy tắc: file route.js sẽ phải chứa hàm GET, và đây là hàm async, bình thường thì next chỉ render mỗi hàm export default function của trang page html đó,  export là từ khóa giúp hàm này cũng sẽ đc thực thi, export là câu lệnh mình yêu cầu next, react cũng chạy function này
async là bất đồng bộ

export async function GET(){
  return new Response("Hello World")
  // hoặc là truy suất data,...
}

page sẽ trả về nội dung trong hàm Response là dòng chữ Hello World chứ ko phải page html, vì file page.tsx sẽ xung đột vs file route.ts, để ko xung đột, ta tạo folder api, và đưa file route.ts vào
*handling GET Request: ng dùng request cần trả về 1 trang(đôi khi cũng kèm theo data)

export async function GET(){
  return Response.json("hello"); <- có thể truyền vào 1 object
}

**Handling POST Request**: người dùng thường request lên với data kèm theo, ví dụ như chức năng tìm kiếm, data có thể là json,... vd: { text: hello world }
export async function POST(request: Request){
  const data = await request.json()
  const data2 = {
    text: data.text + 1
  }
  return new Response(JSON.stringify(data2),{
    header:{
      "Content-Type": "Application/json"
    },
    status: 201, 
  });
}
**Handling dynamic Request**: xử lý quá trình của 1 dynamic router (liên kết động, ví dụ liên kết đến 1 sản phẩm dựa trên id của sản phẩm đó(home/product/1))
export async function GET(request: Request, {param}: { param:{id: string} }) {
  return new Response ("Get handle");
}

**Handling PATCH Request**: giống như truy xuất dữ liệu, patch thường đc dùng để update 1 dữ liệu cụ thể, khác với put là put update dữ liệu hàng loạt
//localhost:3000/book/3 và có dữ liệu đầu vào json

vd: {
  text: "update book detail"
}

export async function PATCH(    
  request: Request,     
  {param}: {param: {id:string} }   
){   
  const {data} = await request.json()   
  //update data vào database    
  return new Response("PATCH handle")   
}    

**Handling Delete Request**: delete thường đc dùng để xóa dữ liệu
export async function DELETE(
  request: Request;
  {params}: {params: {id: string} }
){

}




# Rendering
Rendering là quá trình chuyển code của bạn thành giao diện người dùng, để hiểu rendering trong next, hãy hiểu rendering trong react.

1. **Client-side Rendering (CSR):** Với react, khi người dùng request lên sever, sever sẽ trả về file html chỉ chứa 1 thẻ div, và 1 file js chứa tất cả nội dung chi tiết html được 'đóng gói' lại(vì t ko code html mà t code jsx hoặc tsx, nên ta đang code giao diện trong js), sau đó máy khách sẽ biên dịch file js ra html giao diện ng dùng, chèn nó vào dom của phần tử div gốc (root), từ đây tạo ra giao diện người dùng thấy được. quá trình này đc hiểu là kết xuất ở phía máy khách (client-side rendering), vì máy khách phải biên dịch file js ra html.\
**Hạn chế:**\
Một thẻ div là ko tối ưu cho seo\
Cung cấp ít nội dung cho các công cụ tìm kiếm để lập chỉ mục cho công cụ tìm kiếm\
Hiệu năng thấp nếu các file đc lồng sâu\
2. **Server-side Rendering (SSR):** Server nhận request, server chịu trách nhiệm render full html, css, js => giao diện cho người dùng, sau đó mới gửi html định dạng đầy đủ về cho ng dùng, trình duyệt ng dùng nhanh chóng hiển thị giao diện hơn, cải thiện hiệu suất, sau đó trình duyệt gửi các gói không đồng bộ còn lại như dữ liệu, active link,... và cài vào giao diện vừa đc tải, quá trình này gọi là hydrat hóa
toàn bộ quá trình trên là **Pre-redering**, đc hiểu là render giao diện trên server. pre-rendering có 2 dạng: **Static site generation(SSG)** và **Server-side Rendering**.\
**Static site generation(SSG)**: ngay từ khi build web, ng dùng chưa request, next đã biên dịch sẵn file giao diện tsx ra html full đợi ng dùng tải về, tái sử dụng cho mọi request.\
**Server-side Rendering(SSR)**: khi người dùng request cụ thể 1 trang thì next mới biên dịch 1 trang đó cho 1 người dùng cụ thể để gửi về ng dùng đó.\
**Hạn chế**
SSR nếu có tìm nạp dữ liệu sẽ phải chờ dữ liệu nạp xong mới load đc giao diện và gửi giao diện về ng dùng => chậm. 
SSR phải tải về file js cần thiết (như thư viện), trước khi quá trình hydrat hóa
SSR tải js xuống trang html và hdrat hóa tree html đó (hydrat là biên dịch file js và gắn vào html)
Tất cả các coponent phải đc Hydrat hóa trước khi tương tác vs ng dùng
**Kiến trúc suspense SSR**\
Sử dụng component <suspense> để mở khóa 2 tính năng\
Html streaming on server\
Selective hydration on the client
# Server component
Mặc định tất cả component tạo ra đều ở trạng thái server component
# Client component




# Static Generation with getStaticProps
# Data fetching
# Pre-rendering
# React hook form
trong terminal
npm install react-hook-form

import { useForm, SubmitHandler } from "react-hook-form" <- khai báo SubmitHandler là để tí nữa  định nghĩa kiểu của hàm xử lý submit (submit handler) cho form của bạn.
nếu không có, phải khai báo cấu trúc của data là data: FormField

type FormField = {
  name: string,
  email: string,
  password: string,
} <-nếu là typeScript thì phải khai báo cấu trúc form

export ....
  
  const {
    control,
    register,
    handleSubmit, <-hàm này dùng để xử lý sự kiện submit>
    getValues,
    setError,
    formState:{errors}
  } = useForm<FormField>({
    mode: "onChange",
  });

const onSubmit: SubmitHandler<FormField> = async (data) => {
  try{
    //fetch data
    await console.log(data);
  } catch(error){
    setError("email", {
      message: "email này đã được sử dụng, hãy thử 1 email khác"
    })
  }
}

<form onSubmit={handleSubmit(onSubmit)}>
<input
  placeholder="Tên đầy đủ hoặc biệt hiệu"
  type="text"
  {...register("name",{
      required: "Trường này là bắt buộc",
      minLength: {
        value: 2,
        message: "Độ dài phải có ít nhất 2 kí tự"
      },
    })
  }
/>
nếu truyền vào componnent phải thay đổi register
<InputText
  placeholder="Tên đầy đủ hoặc biệt hiệu"
  type="text"
  error={errors.name?.message}
  register={         <-đầu kia nhận lại props: {...props.register}
    register("name",{
      required: "Trường này là bắt buộc",
      minLength: {
        value: 2,
        message: "Độ dài phải có ít nhất 2 kí tự"
      },
    })
  }
/>

dùng hookform - devtool để kiểm soát form
npm install -D @hookform/devtools
import { DevTool } from "@hookform/devtools";
trong export cho 1 thẻ <DevTool control={control} /> vào bên cạnh form, dưới và ngang hàng form
trên list đối tượng lấy ra từ useform khai báo thêm control, như trên



# Hooks - UseState
# Connect mongodb
1. Create the connection url (tạo chuỗi kết nối có chứa thông tin kết nối mongodb)

Tạo file .env ở cây thư mục gốc, tạo chuỗi kết nối mongodb:
MONGO_URI=mongodb+srv://<username(thanhjayz01)>:<password(okeconde123)>@cluster0.vrgnqps.mongodb.net/<dbname>
điền tên đăng nhập và pass monggodb vào, ko có dấu thẻ <>
2. Create connection function db.js (tạo file kết nối chứa các đoạn code kết nối, giống như laravel, new connection)
Tạo 1 thư mục chứa file kết nối để trông chuyên nghiệp, ví dụ thư mục utils chứa 2 thư mục con: config, model, muốn thêm tùy mình, tại thư mục config tạo file cấu hình kết nối đặt tên gì cũng dc, vd dbConfig.ts, dbconnect.ts, ...

import mongoose from "mongoose";
export  default async function connect() {
  try{
    await mongoose.connect(process.env.MONGO_URI!);
    console.log("MongoDB connected");
  }catch(error:any){  
      console.log(error)
  }
}

Tiếp theo ta tạo các file schema, đc hiểu là file model, dc đặt trong thư mục model hoặc gì tùy mình
tạo file User.ts
nếu ta thêm { timnestamps: true, } thì mongose sẽ tự động thêm biến createAt và updateAt vào model, khi cập nhật giá trị bằng hàm save(), updateOne(), findOneAndUpdate(), mongoose sẽ tự động cập nhật giá trị cho updateAt, tương tự với createAt

import mongoose, { Schema } from "mongoose";
const User = new Schema(
  {
    userName: {
        type:String,
        required:[true, "please write your full name"]
    },
    email: String,
    password: String,
  },
  {
    timnestamps: true,
  }
)

const User = mongoose.model.User || mongoose.model("User", User)
export default User;
hoặc viết liền là 
export default mongoose.model.User || mongoose.model("User", User);


3. Call the connection from api routes/nextjs
trong 1 file api route:

import connectMongoDB from "@/.../dbConfig"
import User from "@/.../User"

export async function POST(req){
  const {email, password} = await req.json();
  await connectMongoDB();
  //phải mở kết nối thì schama mới hoạt động đc , schema này có sẵn các hàm của mongodb, có thể xài các hàm tùy vào nhu cầu
  await User.create({email, passord})
  hoặc
  user = await User.find();
} 