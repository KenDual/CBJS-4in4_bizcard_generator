* Phát hiện vấn đề khi đọc code black box đó là ở `type=figlet` dev đã dùng dấu double thay vì single quote ở đoạn cuối.

```bash
echo 'Hello $username' | cowsay -n ; figlet "Hello $username" <-- chỗ này dính chưởng
```

* Tại sao ở đây dính command injection:
  * Vì trong Unix command dấu double quote có thể thực thi được lệnh còn single quote thì không:
  * PoC:

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/19573a03-c679-4993-88a7-79c2f24b196a/41bbb114-a55c-4d32-98df-2e395b22910e?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy8xOTU3M2EwMy1jNjc5LTQ5OTMtODhhNy03OWMyZjI0YjE5NmEvNDFiYmIxMTQtYTU1Yy00ZDMyLTk4ZGYtMmUzOTViMjI5MTBlIiwiaWF0IjoxNzg2OTUxMDA5LCJleHAiOjE3ODY5OTQyMDl9.lGCzlknHXF5orYtT9Jyhumau8G0QkWakwqfJF4v6rsM)

# Build payload:

* Chí mạng ở chỗ là regex đã chặn hầu hết các ký tự cần thiết để đọc secret_file ở root đó là (và dấu slash `/`):

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/492acc8c-1aba-4e1a-8e52-a67a0bdd6879/4448ac87-0b94-43bf-8efd-d519befdbc76?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy80OTJhY2M4Yy0xYWJhLTRlMWEtOGU1Mi1hNjdhMGJkZDY4NzkvNDQ0OGFjODctMGI5NC00M2JmLThlZmQtZDUxOWJlZmRiYzc2IiwiaWF0IjoxNzg2OTUxMDA5LCJleHAiOjE3ODY5OTQyMDl9.OfvKqzPNTTdxYPtUGezDdE2mBh9knRIMpLeTawZVHF4)

Nhưng vẫn chưa lọc kỹ vì vẫn còn ký tự chưa được lọc trong command injection đó là dấu `` ` ``, từ đó em có thể dùng các sự thay thế như sau:

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/cbd1f3a4-95bc-4941-85e3-2e1297a46aba/8a988ba4-e3c9-4806-9926-6e699af5dc5b?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy9jYmQxZjNhNC05NWJjLTQ5NDEtODVlMy0yZTEyOTdhNDZhYmEvOGE5ODhiYTQtZTNjOS00ODA2LTk5MjYtNmU2OTlhZjVkYzViIiwiaWF0IjoxNzg2OTUxMDA5LCJleHAiOjE3ODY5OTQyMDl9.R17Je3kW_5BaRi-TNFf3wOnFdQ6R1LQOUFHnjFU-ARc)

* `<space>` → `<tab>`
* dùng command substitution: ``` `` ```

![](https://uploads.linear.app/b471c4ef-0f5a-4e1c-b641-7b7285ddb9ac/d25234b9-8197-4e92-aeaa-aa3c1a85947f/bb642ee2-e9ea-4bad-a092-f33e47bc7912?signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJwYXRoIjoiL2I0NzFjNGVmLTBmNWEtNGUxYy1iNjQxLTdiNzI4NWRkYjlhYy9kMjUyMzRiOS04MTk3LTRlOTItYWVhYS1hYTNjMWE4NTk0N2YvYmI2NDJlZTItZTllYS00YmFkLWEwOTItZjMzZTQ3YmM3OTEyIiwiaWF0IjoxNzg2OTUxMDA5LCJleHAiOjE3ODY5OTQyMDl9.ZFyO_jJHKuQ26xeKGMZ5dJMpaNjrJPAlAnQT52fBruY)