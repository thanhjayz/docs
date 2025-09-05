
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