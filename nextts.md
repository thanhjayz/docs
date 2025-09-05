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