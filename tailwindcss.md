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