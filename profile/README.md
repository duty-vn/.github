# System architecture

```mermaid
graph LR
A[Client] --> B[Traefix]
B --> C[Backend]
B --> D[Frontend]
C --> E[svc-auth]
C --> F[svc-gov]
C --> G[svc-...]
E --> Z[(PostgreSQL)]
F --> ZZ[(PostgreSQL)]
F --> ZZZ[(Redis)]
D --> H[fe-on]
D --> I[fe-hr]
D --> J[fe-...]
```

Khi vào ứng dụng, người dùng sẽ vào [on.duty.vn](https://on.duty.vn) để login và chọn business, sau khi chọn business sẽ dc chuyển sang trang dành cho business đó `business_name.duty.vn`.
Mỗi phần nhỏ của ứng dụng sẽ được traefix điều hướng sang các microservice frontend khác nhau, ví dụ:
```
business_name.duty.vn/hr -> fe-hr
business_name.duty.vn/tax/report -> fe-tax
business_name.duty.vn/workspace -> fe-workspace
```
Mỗi microservice backend sẽ có một database riêng lưu những dữ liệu liên quan, nếu một microservice khác cần dữ liệu đó sẽ gọi grpc qua để lấy, ko lưu cùng một dữ liệu ở 2 microservice.

# Git flow

```mermaid
gitGraph
   commit
   commit tag: "v1.0"
   commit
   branch task-branch
   checkout task-branch
   commit
   commit
   checkout main
   merge task-branch
   commit tag: "v2.0"
   commit
```

Khi làm một tính năng mới, tạo một branch với tên ở định dạng `mã-task-tên-tính-năng`, ví dụ `pwf-1410-new-hr-tab`.
Sau khi xong tính năng tạo PR để merge vào `main`.
Khi cần release thì sẽ tạo một release mới để build artifact.
