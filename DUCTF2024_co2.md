# co2
### A group of students who don't like to do things the "conventional" way decided to come up with a CyberSecurity Blog post. You've been hired to perform an in-depth whitebox test on their web application.
### Author: n00b.master
Source code: https://github.com/Winz2910/DUCTF2024_WU/blob/201cf1fe4b91de0c9f787be303b728333bf7a19f/co2.zip

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/fbe0e80b-d65f-43c0-920f-2810297acdc1)

Challange là web app như kiểu một trang blog để mọi người đăng bài. Có các chức năng cơ bản như: đăng ký/đăng nhập, quản lí profile, đăng bài, feedback. Sau khi thử qua hết một lượt các tính năng thì mình bắt đầu nghiên cứu source code. 

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/c891d1b7-fe3c-4ebb-bb23-b797f950a26a)
![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/6295e149-8f16-4fc3-958b-b873bd94e58c)

Có vẻ các chức năng của app khá ổn, không có gì bất thường. Tuy nhiên tác giả đã cố ý để lại hint qua comment cho người chơi:

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/373b1296-d395-4a9d-bbd7-eb98fde69497)

Chính vì vậy mình bắt đầu focus vào test tính năng Feedback. 

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/e5f8b59d-293b-4fa6-b82e-e7ef69e9f58f)

Hàm `save_feedback()` thực hiện việc ghép dữ liệu từ người dùng vào một đối tượng của class Feedback thông qua hàm `merge()`. Tuy nhiên dễ thấy rằng data nhận từ người dùng không hề được xử lí trước khi merge nên hàm `save_feedback()` chính là nguyên nhân làm phát sinh lổ hổng. Dưới đây là code của hàm `merge()`:

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/3ae5e69a-d7bf-46e3-885c-1a4ca93917e2)

Trông có vẻ khá giống với lổ hổng Prototype Pollution trong ngôn ngữ JS nhưng với Python thì không tồn tại khái niệm này. Tuy nhiên điều đó không nghĩa là ta không thể khai thác nó một cách tương tự. Nhưng khai thác như vậy để làm gì? Đó là do muốn lấy được flag thì ta cần phải thỏa mãn điều kiện sau:

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/815b9501-1739-4473-8be0-d3f34ff1a068)
![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/78f89053-9ce7-436d-aa83-ff6b52d5a12f)

Ta cần thay đổi value của biến toàn cục `flag` thành `true` sau đấy truy cập đến end point `/get_flag` để lấy được cờ. Nhưng làm sao để can thiệp vào biến flag? Câu trả lời là lợi dùng hàm save_feedback() để đạt được điều đó. Python không tồn tại cái được gọi là Prototype nhưng chúng ta có "special attributes". Ví dụ như `__class__`, `__doc__`,...chúng là các thuộc tính đặc biệt trong mọi object trong Python. => ta cần tìm cách thay đổi value của biến toàn cục flag thành "true" thông qua việc truyền các thuộc tính đặc biệt vào hàm `save_feedback()`. Payload mà mình đã sử dụng:`{"__class__":{"__init__":{"__globals__":{"flag":"true"}}}}`
Payload mình sử dụng sau khi đã nghiên cứu bài phân tích trên trang Abdulrah33m's Blog: https://blog.abdulrah33m.com/prototype-pollution-in-python/

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/d9be1469-4f2f-4135-88ea-6a06d8e707ce)

Tóm lại đây là một challange liên quan đến Class Pollution trong Python phỏng theo cách khai thác Prototype Pollution trong ngôn ngữ JavaScript. Và mình đã lấy được flag khi truy cập end point `/get_flag`.

![image](https://github.com/Winz2910/DUCTF2024_WU/assets/117363798/cb2b4abb-224c-47f7-bb1d-f31fe8988396)












