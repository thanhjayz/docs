disable minimap:
settings.json => "editor.minimap.enabled": true

cài extention

1. Cài Prettier
   Mở VS Code → vào Extensions (Ctrl+Shift+X)

Tìm "Prettier - Code formatter" → Cài đặt.

2. Chọn Prettier làm formatter mặc định
   Mở Command Palette (Ctrl+Shift+P)

Gõ "Format Document With" → "Configure Default Formatter" → Chọn "Prettier - Code formatter".

3. Bật format khi lưu
   Vào File → Preferences → Settings (Ctrl+,), tìm:

editor.formatOnSave → Tick chọn ✅.

editor.defaultFormatter → chọn "esbenp.prettier-vscode".

Hoặc chỉnh trực tiếp trong file settings.json:

json
Sao chép
Chỉnh sửa
{
"editor.formatOnSave": true,
"editor.defaultFormatter": "esbenp.prettier-vscode",
"prettier.printWidth": 80, // max ký tự mỗi dòng
"prettier.tabWidth": 2, // số spaces mỗi tab
"prettier.singleQuote": true, // dùng nháy đơn
"prettier.semi": true // thêm dấu chấm phẩy
} 4. Dùng thủ công khi cần
Nếu muốn format ngay một file:

Shift+Alt+F (Windows) hoặc Shift+Option+F (Mac).
