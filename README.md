# PF162-Assignment 01

## 1. Giới thiệu bài toán
Các nhà nghiên cứu trong lĩnh vực sức khoẻ cộng đồng đã quan sát
và thấy rằng chiều cao của một người trưởng thành (lúc 18 tuổi) 
phụ thuộc lớn vào chiều cao của chính người đó lúc 2 tuổi. 
Việc xác định có hay không quan hệ như vậy có ý nghĩa quan trọng. 
Mỗi khi có quan hệ đó, người ta tập trung hơn trong việc sớm 
cải thiện chiều cao của trẻ em ở độ tuổi lên 2, cũng như có thể 
dự báo chiều cao của một người lúc trưởng thành. Do vậy, nhóm 
nghiên cứu đã thực hiện thí nghiệm để kiểm chứng quan điểm này. 

Để thực hiện thí nghiệm, người ta theo dõi chiều cao của `N` 
(đủ lớn) đối tượng (cá thể) từ lúc 2 tuổi đến lúc 18 tuổi, 
bằng cách đo chiều cao của từng đối tượng lúc 2 tuổi và lúc 
trưởng thành. Dựa trên số liệu thu thập được, nhóm nghiên cứu 
dùng một kỹ thuật được gọi là "tối ưu hoá" (optimization) để 
xác định mô hình dự báo chiều cao và kiểm chứng quan điểm trên. 
Kỹ thuật này sẽ được trình bày trong phần theo sau.

Gọi `x` là chiều cao của một đối tượng lúc 2 tuổi, và `t` là 
chiều cao của chính người đó lúc 18 tuổi. Cả `x` và `t` được 
đo cho từng đối tượng. Sau khi thu thập số liệu `(x, t)` của 
`N` đối tượng trong thí nghiệm, các nhà nghiên cứu đã quan sát 
sự phân phối dữ liệu của $N$ đối tượng. Từ quan sát đó, họ đi 
đến một giả thiết cụ thể hơn, đó là, chiều cao lúc trưởng thành 
(`y`) có thể được dự báo từ chiều cao lúc 2 tuổi (`x`) theo một 
mô hình tuyến tính như trong Phương trình (1). 
Do đó, nhiệm vụ của nhóm nghiên cứu là xác định các hệ số `a` và 
`b` của mô hình tuyến tính trong Phương trình (1).
	
	y = a * x + b                                           (1)
	
## 2. Phương pháp tối ưu hoá
### 2.1. Sai số dự báo
Với chiều cao `x` (lúc 2 tuổi) ở đầu vào, chiều cao lúc trưởng thành 
`y` là giá trị dự báo được tính từ Phương trình (1). Tuy nhiên, với 
các đối tượng trong thí nghiệm, chiều cao lúc trưởng thành thực sự 
đã được đo đạc rồi, đó là `t`.  Chiều cao thực sự này có thể sai 
lệch (nhỏ hơn hoặc lớn hơn) với chiều cao tính được từ mô hình 
dự báo `y`. Độ sai số được tính là `e = t - y`. Giá trị `e` có thể 
âm hay dương. Trong kỹ thuật, sai số thường là số không âm, để 
biểu thị độ lệch lớn hay nhỏ. Một trong các hàm có tính chất đó là 
**"tổng bình phương sai số"** (sum of squared error, SSE). 
Hàm này được định nghĩa trong Phương trình (2). 
	
    L(a,b) = 0.5 sum((t_i - y_i)^2)                         (2)
	       = 0.5 sum((t_i - a * x_i - b)^2) 
	
Vì `x` và `t` là các giá trị đã đo được từ `N` cá thể trong thí 
nghiệm, trong Phương trình (1), `a` và `b` là hai biến cần xác định. 
Nghĩa là, hàm sai số phụ trên các biến cần xác định là `a` và `b`. 

### 2.2. Gradient
Một cách hình thức, các số liệu sau đây là đã biết trước khi dùng 
giải thuật tối ưu
- `N` bộ số liệu `(x, t)`, là những số đo cho `N` cá thể trong 
thí nghiệm.
- Mô hình dự báo đã biết trước là có dạng như Phương trình (1). 
Trong mô hình này, có hai thông số cần xác định là `a` và `b`.
- Hàm sai số để biểu diễn tổng sai số dự báo đã biết trước, 
đó là `L(a, b)`, được định nghĩa trong Phương trình (2).

Giải thuật tối ưu hoá được trình bày ở đây là Gradient Descent (GD). 
GD có mục tiêu là tìm ra bộ tham số `a` và `b` mà tại đó hàm 
`L(a, b)` đạt giá trị cực tiểu cục bộ. Cực tiểu cục bộ nghĩa 
là cực tiểu nhưng có thể chưa phải là nhỏ nhất trong toàn bộ 
không gian tham số (`a` và `b`).
	
Giải thuật này dựa trên một tính chất của đạo hàm như sau. 
Nếu đạo hàm của một hàm `f(x)` nào đó là dương tại điểm `x_0` 
bất kỳ, thì hàm `f(x)` là hàm tăng tại điểm `x_0`. 
Nghĩa là `f(x_0 - \alpha * f'(x_0)) < f(x_0)`, với `\alpha > 0` 
và đủ nhỏ. Tương tự, nếu đạo hàm âm thì hàm số giảm, nghĩa là 
`f(x_0 + \alpha * f'(x_0)) < f(x_0)`. Tóm lại, **trên trục biến 
số `x`, tại điểm `x_0`, nếu ta di chuyển theo hướng ngược lại 
với dấu (hướng) của đạo hàm `f'(x_0)` thì hàm số sẽ giảm dần**. 
	
### 2.3. Gradient Descent
Tại bước thứ i với các tham số chứa trong vector `P_i`, nếu 
`P_(i+1)` được xác định từ phương pháp GD thì ta có 
`L(P_(i+1)) < L(P_i)`. 
Việc tìm cực trị được thực hiện đơn giản thông qua phép lặp.
	
- Gọi `P` là điểm nào đó trong mặt phẳng `(a, b)`. Điểm này có 
thể gán giá trị nào đó trước, hoặc ngẫu nhiên.
- Gán giá trị cho `\alpha`, giá trị thực dương.
- Lặp cho đến một điều kiện dừng nào đó, thực hiện các bước sau:
	+ Tính toán giá trị dự báo `y` cho tất cả các mẫu số liệu (`N` mẫu).
	+ Tính gradient vector.
	+ Chuẩn hoá gradient vector và thu được `g = [g_a, g_b]`
	+ Cập nhật lại toạ độ điểm `P`.
	
Những điểm cần chú ý trong giải thuật nêu trên:
- Điểm bắt đầu `P`: trong trường hợp tổng quát hơn, hàm lỗi 
(`L(a, b)`) có cực trị toàn cục và nhiều cực trị cục bộ, 
việc chạy thử nghiệm với các điểm bắt đầu khác nhau có thể 
là giải pháp để chọn điểm bắt đầu phụ hợp, và nó cho ra sai số 
dự báo tốt hơn. Ở bài tập lớn này, sinh viên sẽ chọn gán 
điểm bắt đầu từ thông số đọc từ tập tin đầu vào. 
- Hệ số `\alpha`: hệ số này có tên gọi là **hệ số học** 
(learning rate). Trong bài tập lớn, hệ số học cũng cho sẵn trong tập tin đầu vào.
- Điều kiện dừng của giải thuật. Có nhiều cách, trong bài tập lớn, 
điều kiện dừng là chạy đúng số lần lặp được quy định. 
Số này cho trong tập tin đầu vào.

## 3. Yêu cầu
Ở bài tập lớn này, sinh viên được yêu cầu viết một chương trình 
để thực hiện các công việc sau đây: đọc dữ liệu đầu vào và 
đánh giá mô hình dự đoán trong Phương trình (1) dựa vào các 
số liệu đọc được. Các phần theo sau sẽ mô tả các công việc chi tiết hơn.
	
### Đọc dữ liệu đầu vào
Tập in đầu vào được đưa vào từ tham số dòng lệnh. 
Tập tin này chứa các tham số phục vụ quá trình tối ưu hoá như sau:
- num_iterations: số lần lặp trong giải thuật tối ưu GD.
- learning_rate: hệ số học
- start_a và start_b: điểm bắt đầu
- num_folds: cách chia tập dữ liệu để đánh giá, sẽ trình bày ở phần sau.
- eval: set ở `0` sẽ khiến chương trình không xuất output đánh giá mô
hình mà chỉ xuất ra kết quả ước lượng gồm `a`, `b`, và `e` (độ lỗi).

Theo sau phần các thông số đã mô tả là phần dữ liệu để đánh giá, 
đó là các cặp giá trị `(x, t)` được cho trong phần **"Data Samples"** 
của tập tin đầu vào. Hàng dữ liệu đầu tiên bắt đầu dòng thứ 12 
(tính từ 1). Mỗi dòng gồm 2 con số `x` và `t`, cách nhau 
khoảng trống.
	
Tập tin chứa thông số cấu hình thực thi được cho như sau:

	------------------------------------------------------------
	Traning and Validation Parameters 
	------------------------------------------------------------
	num_iterations:                      50
	learning_rate:                      0.1
	start_a:                              0
	start_b:                              0
	num_folds:                            3
	eval:                                 1

Tập tin chứa dữ liệu nhập có dạng như sau:

	------------------------------------------------------------
	Data samples 
	------------------------------------------------------------
	76.87       153.78
	83.76       167.48
	
### Đánh giá mô hình và kết xuất 
Gọi `M` là số mẫu dữ liệu được đọc vào. Mỗi mẫu bao gồm cặp giá trị 
`x` và `t`. Gọi `K` là giá trị đọc được từ thông số `num_folds`.
	
Việc đánh giá mô hình dự báo ở Phương trình (1) được thực hiện 
theo hướng dẫn sau đây.
- Chia `M` mẫu số liệu vào `K` ngăn (hay tập hợp). Vậy mỗi ngăn 
có trung bình `N = M/K`  (chia nguyên) mẫu dữ liệu; ngăn cuối cùng 
(ngăn thứ `K`) có thể chưa số mẫu dữ liệu ít hơn các ngăn khác 
nếu phép chia cho `K` không chẵn. Trong bài tập lớn này, sau khi 
đọc được `M` mẫu, sinh viên lưu vào một mảng **theo đúng thứ tự** 
như trong tập tin đầu vào. Như vậy, ngăn số 1 chứa các mẫu số liệu 
có **chỉ số** từ `0` đến `N-1`, ngăn số 2 chứa các mẫu số liệu có 
**chỉ số** từ `N` đến `2N-1`, v.v.
- Chương trình thực hiện `K` lần các công việc xác định mô hình 
và kiểm định mô hình theo hướng dẫn sau đây. Các lần này thực hiện 
này được đánh chỉ số từ 1 đến `K`. Ở lần thứ `k` (`1, 2, ..., K`), 
chương trình tiến hành các công việc sau.
    + Chương trình tách dữ liệu đầu vào thành 2 tập hợp, được gọi 
    là tập TRN và tập TST. Tập TRN gồm tất cả các mẫu dữ liệu 
    trong ngăn số `k`, và giả sử ngăn này có `N` mẫu dữ liệu. 
    Tập TST bao gồm tất cả các mẫu số liệu từ `K-1` ngăn còn lại, 
    ngăn này chứa `D` mẫu. Ở đó, `D = M - N`.
    + Chương trình sử dụng các mẫu dữ liệu trong tập TRN để 
	xác định các hệ số của mô hình dự báo trong Phương trình (1), 
	theo giải thuật được trình bày trong phần GD.
    + Chương trình dùng mô hình thu được để xác định sai số 
	dự báo trung bình cho các mẫu trong tập TST.
    + Chương trình xây dựng biểu đồ tần suất của độ lệch `e_i` 
    của các mẫu trong tập TST. `e_i` được định nghĩa là 
    `e_i = y_i - t_i`. Ỏ đó, `y_i` và `t_i` là giá trị dự báo 
    và giá trị đo được của cá thể thứ `i` trong tập TST, 
    `i=1, 2,..., M - N`. Giá trị `e_i` có thể âm hoặc dương. 
    Để tính được biểu đồ này, chương trình cần xác định cận trái 
    (`V_min`) và cận phải(`V_max`) của biểu đồ. 
    Biểu đồ tần suất của các giá trị `e_i` này bao gồm 
    10 khoảng cách đều. Các giá trị `e_i` nhỏ hơn `V_min`
    hoặc lớn hơn `V_max` thì không được tham gia vào phép tính 
    tần suất. Chia đoạn từ `V_min` đến `V_max` thành 10 
    khoảng bằng nhau và liên tục. Giá trị `e_i` thuộc khoảng 
    nào thì tăng số lần xuất hiện của khoảng tương ứng lên một. 
    Chuẩn hoá biểu đồ để tổng các giá trị trong biểu đồ là 1. 
    + Chương trình xuất ra một hàng số liệu cho 
    mỗi giá trị `k`. Hàng số liệu này gồm các số sau đây, từ 
    trái sang phải: hai giá trị của `a` và `b`, sai số dự báo 
    của `D` mẫu số liệu tính ở trên, và 10 giá trị biểu diễn 
    biểu đồ tần suất của các độ lệch. Như vậy, có 13 giá trị 
    trên cùng một hàng. Mỗi giá trị là một số thực định dạng 
    dấu chấm cố định (fixed), độ rộng là 7, có hai số đứng sau 
    dấu chấm thập phân và canh lề phải. Vì chương trình thực hiện 
    `K` lần kiểm định như vậy, nên trong tập tin đầu ra có `K` 
    dòng số liệu như đã mô tả (chưa kể các dòng văn bản, xem 
    tập tin mẫu).
	
Kết quả xuất ra màn hình như sau:

	---------------------------------------------------------------------------------
	Output of the validation 
	---------------------------------------------------------------------------------
	2.00   0.01   5.07   0.01   0.015   0.10   0.20   0.35   0.20   0.015   0.01
	2.00   0.01   5.07   0.01   0.015   0.10   0.20   0.35   0.20   0.015   0.01
	2.00   0.01   5.07   0.01   0.015   0.10   0.20   0.35   0.20   0.015   0.01

## Hiện thực
**Sinh viên được cung cấp các file sau:**
 - `main.cpp`: mã nguồn chính của chương trình
 - `commonLib.h`: file header chứa prototype các hàm dùng chung
 - `commonLib.cpp`: file hiện thực các hàm dùng chung khai báo trong `commonLib.h`
 - `linearRegression.h`: file header chứa các prototype cần thiết để xử lý yêu cầu bài toán
 - `linearRegression.cpp`: mã nguồn hiện thực các hàm xử lý yêu cầu bài toán.

__Sinh viên được cho các hàm sau:__
 - `loadData`: đọc dữ liệu từ file nhập.
 - `loadParams`: đọc cấu hình cho bài 
 
Đối với file __`linearRegression.h`__, sinh viên được cung cấp sẵn các định 
nghĩa cơ bản của những hàm xử lý sẽ được gọi trong chương trình. 
Sinh viên không được thay đổi prototype của các hàm này. Tuy nhiên,
sinh viên có quyền viết thêm các hàm xử lý cần thiết và hiện thực 
các hàm đó trong file `linearRegression.cpp`.

Sinh viên được phép tuỳ biến, chỉnh sửa `linearRegression.h` và 
`linearRegression.cpp`. Ngoài ra sinh viên tự hiện thực các chức 
năng load dữ liệu trong `commonLib.cpp` (file này sẽ được ghi đè 
trong quá trình chấm tự động trên hệ thống).

**Sinh viên không được sử dụng các thư viện nào khác ngoài các thư viện 
đã được dùng trong khung code được giao.**

## Build
Sinh viên thực hiện build bằng lệnh `make` từ command line trên Linux
và chạy file `pf162a01`. Cú pháp trên linux như sau:
> `./pf162a01 inputData.txt inputConfig.txt`

Đối với các bạn dùng VisualStudio trên Windows, các bạn có thể tạo một 
project và thêm các file mã nguồn vào. Nếu không bạn có thể sử dụng hệ 
thống CMake để tự sinh project VS cho mình.