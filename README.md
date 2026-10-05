\# Bài 4: Mô phỏng quy trình Hotfix \& Gitflow thực tế



\## 1. Mục tiêu



Bài thực hành mô phỏng quy trình Gitflow khi hệ thống production phát sinh lỗi nghiêm trọng cần sửa gấp.



Các mục tiêu chính:



\- Tạo nhánh `hotfix` trực tiếp từ `main`.

\- Sửa lỗi khẩn cấp trên nhánh hotfix.

\- Merge hotfix trở lại `main`.

\- Tạo tag phiên bản mới `v1.0.1`.

\- Merge hotfix vào `develop` để nhánh phát triển tương lai cũng nhận được bản sửa lỗi.



\## 2. Khởi tạo repository



Khởi tạo Git:



```bash

git init

git branch -M main



Tạo file app.txt với nội dung ban đầu:

version=1.0.0

user\_data=visible



Commit phiên bản production:

git add .

git commit -m "release: stable v1.0.0"



Tạo tag phiên bản:

git tag -a v1.0.0 -m "Release 1.0.0"



3\. Tạo nhánh develop

Tạo nhánh phát triển:

git checkout -b develop



Sửa file app.txt thành:

version=1.1.0-dev

user\_data=visible

new\_feature=in-progress



Commit:

git add .

git commit -m "feat: develop new feature"



Lúc này:

\- main: chứa phiên bản production ổn định.

\- develop: chứa tính năng đang phát triển.

4\. Tạo nhánh hotfix từ main

Chuyển về nhánh main:

git checkout main



Tạo nhánh sửa lỗi khẩn cấp:

git checkout -b hotfix/v1.0.1



Hotfix được tạo trực tiếp từ main vì đây là bản vá dành cho phiên bản production hiện tại.

5\. Sửa lỗi trên nhánh hotfix

Sửa file app.txt thành:

version=1.0.1

user\_data=hidden



Trong đó:

user\_data=hidden



đại diện cho việc sửa lỗi lộ dữ liệu người dùng.

Commit bản sửa lỗi:

git add .

git commit -m "fix: prevent user data exposure"



6\. Merge hotfix vào main

Chuyển về nhánh main:

git checkout main



Merge hotfix:

git merge --no-ff hotfix/v1.0.1 -m "merge hotfix v1.0.1 into main"



Sau khi merge thành công, tạo tag phiên bản mới:

git tag -a v1.0.1 -m "Release Hotfix 1.0.1"



Kiểm tra tag:

git tag



Kết quả mong đợi:

v1.0.0

v1.0.1



Tag v1.0.1 đánh dấu phiên bản production đã được vá lỗi.

7\. Merge hotfix vào develop

Sau khi hotfix đã được đưa lên production, cần đồng bộ bản sửa lỗi vào nhánh phát triển.

Chuyển sang develop:

git checkout develop



Merge hotfix:

git merge --no-ff hotfix/v1.0.1 -m "merge hotfix v1.0.1 into develop"



Nếu có conflict trong app.txt, cần giữ cả:

\- Bản sửa lỗi từ hotfix.

\- Tính năng đang phát triển trên develop.

Nội dung cuối có thể là:

version=1.1.0-dev

user\_data=hidden

new\_feature=in-progress



Sau đó:

git add app.txt

git commit



8\. Kiểm tra các nhánh và tag

Chạy:

git branch -a

git tag



Kết quả có thể giống:

\* develop

&#x20; hotfix/v1.0.1

&#x20; main



Danh sách tag:

v1.0.0

v1.0.1



Ảnh minh chứng:

branches-and-tags.png



9\. Kiểm tra lịch sử Gitflow

Chạy:

git log --graph --oneline --all --decorate



Kết quả sẽ thể hiện nhánh hotfix được tách từ main, sau đó merge vào cả main và develop.

Ví dụ:

\*   xxxx (HEAD -> develop) merge hotfix v1.0.1 into develop

|\\

| \* xxxx fix: prevent user data exposure

\* | xxxx feat: develop new feature

|/

| \*   xxxx (tag: v1.0.1, main) merge hotfix v1.0.1 into main

| |\\

| |/

|/|

| \* xxxx fix: prevent user data exposure

|/

\* xxxx (tag: v1.0.0) release: stable v1.0.0



Ảnh minh chứng:

gitflow-hotfix-graph.png



10\. Sơ đồ Gitflow

&#x20;                 develop

&#x20;                    |

&#x20;                    o  feat: develop new feature

&#x20;                    |

&#x20;                    |\\

&#x20;                    | \\

&#x20;                    |  o  fix: prevent user data exposure

&#x20;                    | /   hotfix/v1.0.1

&#x20;                    |/

main        o--------o

&#x20;           |        |

&#x20;        v1.0.0   v1.0.1



Quy trình tổng quát:

main

&#x20; |

&#x20; +---- hotfix/v1.0.1

&#x20; |          |

&#x20; |          +---- sửa lỗi

&#x20; |          |

&#x20; |          +---- merge vào main

&#x20; |          |

&#x20; |          +---- merge vào develop

&#x20; |

develop



11\. Xóa nhánh hotfix sau khi hoàn tất

Sau khi hotfix đã được merge vào cả main và develop, có thể xóa nhánh local:

git branch -d hotfix/v1.0.1



Kiểm tra:

git branch



Kết quả:

\* develop

&#x20; main



Nhánh hotfix không còn cần thiết vì bản sửa lỗi đã được tích hợp vào các nhánh chính.

12\. Giải thích quy trình Hotfix

Trong Gitflow, nhánh hotfix được sử dụng khi phiên bản production đang chạy gặp lỗi nghiêm trọng cần sửa ngay.

Hotfix phải được tạo từ:

main



thay vì:

develop



vì develop có thể chứa các tính năng chưa hoàn thiện và chưa sẵn sàng để deploy.

Sau khi sửa lỗi:

hotfix

&#x20;  |

&#x20;  +----> main

&#x20;  |

&#x20;  +----> develop



Việc merge vào main giúp sửa lỗi trên production.

Việc merge vào develop đảm bảo lỗi đó không xuất hiện trở lại trong phiên bản tương lai.

13\. Ý nghĩa của tag v1.0.1

Tag:

v1.0.1



được tạo sau khi merge hotfix vào main.

Lệnh sử dụng:

git tag -a v1.0.1 -m "Release Hotfix 1.0.1"



Tag giúp đánh dấu chính xác commit được sử dụng để phát hành phiên bản 1.0.1.

Có thể kiểm tra bằng:

git tag



hoặc:

git show v1.0.1



14\. Các lệnh chính đã sử dụng

git init

git branch -M main



git add .

git commit -m "release: stable v1.0.0"



git tag -a v1.0.0 -m "Release 1.0.0"



git checkout -b develop



git add .

git commit -m "feat: develop new feature"



git checkout main

git checkout -b hotfix/v1.0.1



git add .

git commit -m "fix: prevent user data exposure"



git checkout main

git merge --no-ff hotfix/v1.0.1 -m "merge hotfix v1.0.1 into main"



git tag -a v1.0.1 -m "Release Hotfix 1.0.1"



git checkout develop

git merge --no-ff hotfix/v1.0.1 -m "merge hotfix v1.0.1 into develop"



git branch -a

git tag



git log --graph --oneline --all --decorate

