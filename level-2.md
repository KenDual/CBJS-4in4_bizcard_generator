# Behaviour:

* Đã thử 1 vài payload và nhận thấy là nếu payload quá dài sẽ bị cắt đi khúc sau, chỉ nhận 10 ký tự thôi

→ Vì vậy thay gì payload như level 1 là: `'; cat /se* #` sẽ đổi lại thành `';cat /* #`

* Có 2 lý do vì sao lại làm vậy:
  * Tránh bị cắt ký tự
  * Ở root dù sao cũng chỉ có 1 file duy nhất đó là flag, kiến trúc của linux (docker) thường sẽ là 100% dir

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/a59d9f06-add2-492c-992d-b1dc782ab87a/67b117af-09c7-4058-af09-5487193e6691?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy9hNTlkOWYwNi1hZGQyLTQ5MmMtOTkyZC1iMWRjNzgyYWI4N2EvNjdiMTE3YWYtMDljNy00MDU4LWFmMDktNTQ4NzE5M2U2NjkxIiwiaWF0IjoxNzg2OTUwOTY0LCJleHAiOjE3ODY5OTQxNjR9.MPCXdW11pGqcucvYXbMDzXZeKplTsYgGR_U8lmlGvr0)