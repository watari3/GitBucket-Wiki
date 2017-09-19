postman
1)check status GET
http://core.wirtualus.com/user/login_status?_format=json
Response 0 or 1

2)login POST
http://core.wirtualus.com/user/login?_format=json

Body, Raw, JSON(Application)
{"name":"tester", "pass":"111111"}

Check Body
{
   "current_user": {
       "uid": "2",
       "name": "tester"
   },
   "csrf_token": "2uBtwoglnSy4D_z0h40hpjEE2G6mSIIWMu0QIJ9OFck",
   "logout_token": "r3SqEoMjo-BYj4Mr74ihyCZ9ogjks-rAHQp_hmKWsCM"
}

3)Logout POST
http://core.wirtualus.com/user/logout?_format=json&csrf_token=2uBtwoglnSy4D_z0h40hpjEE2G6mSIIWMu0QIJ9OFck&token=r3SqEoMjo-BYj4Mr74ihyCZ9ogjks-rAHQp_hmKWsCM

4)Get Token http://core.wirtualus.com/rest/session/token


5)Register Post
Header
X-CSRF-token=<Where we got in 4)>
Content-Type=application/json

{
"name":[{"value":"tester2"}],
"pass":[{"value":"111111"}],
"mail":[{"value":"chensihai@mail.com"}]
}

