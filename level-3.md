* Phát hiện vấn đề khi đọc code black box đó là ở `type=figlet` dev đã dùng dấu double thay vì single quote ở đoạn cuối.

```bash
echo 'Hello $username' | cowsay -n ; figlet "Hello $username" <-- chỗ này dính chưởng
```

* Tại sao ở đây dính command injection:
  * Vì trong Unix command dấu double quote có thể thực thi được lệnh còn single quote thì không:
  * PoC:

![](images/level3-singlequote_vs_doublequote.png)

# Build payload:

* Chí mạng ở chỗ là regex đã chặn hầu hết các ký tự cần thiết để đọc secret_file ở root đó là (và dấu slash `/`):

![](images/level3-octaltable.png)

Nhưng vẫn chưa lọc kỹ vì vẫn còn ký tự chưa được lọc trong command injection đó là dấu `` ` ``, từ đó em có thể dùng các sự thay thế như sau:

![](images/level3-cyberchef.png)

* `<space>` → `<tab>`
* dùng command substitution: ``` `` ```

![](/images/level3-payloadfinal.png)