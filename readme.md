# Docker
1. Định nghĩa
- là công nghệ cho chúng ta gói ứng dụng và toàn bộ thư viện, nền tàng thực thi lại thành một gói duy nhất, để thực thi trên bất kì môi trường nào dù là Linux hay MacOS
2. Image: 
- Docker đóng gói các ứng dụng, các gói ứng dụng được gọi là Image. Khi triển khai các Image chúng ta có Container
3. Container
- Container hoạt động như 1 máy ảo, tuy nhiên Container không phải là máy ảo vì Container không có kernel. Container chỉ là các tiến trình chạy trên Os và được quản lý bởi Docker.
4. Sự khác biệt giữa Container và Virtual machine
- Container ảo hóa ở tầng software appication, tận dụng lại phần kernel của máy chủ vật lý nên không tốn tài nguyên của máy, hoặc là chiếm rất ít
- Virtual Machine ảo hóa ở tầng hardware, phân chia tài nguyên vật lý cho các máy ảo. 
## Tại sao nên cài Docker
- Docker giúp cho việc đóng gói các ứng dụng một các dễ dàng (Thay vì cài Git hay maven, python.. chúng ta chỉ cần cài docker )
## Tính chất của Docker
- Immutable: bất biến
- Lightweight: Kích thước nhẹ
- Stateless: kém ổn định
## Các lệnh cơ bản của Docker
1. Cú pháp 
- Docker < component> < command>
- Trong đó < compoment> bao gồm: image, container, network, volume
- Trong đó < command> bao gồm: ls (list), run, exec, stop, pull, prune.
2. Các lệnh cơ bản
- Docker image pull: dùng để tải image từ registry
- Docker image pull < tag>: Do nhiều image có tag khác nhau để đánh dấu version nên dùng tag để xác định image nào, nếu không tag thì tự động là lastest.
- Docker image push: dùng để tải image từ local lên registry
- Docker image push < tag>: Ngược lại với 
- Docker image pull < tag>
- Docker image ls | docker images: liệt kê
- Docker image prune: xóa

- Docker container run < tên image>: 
- Docker container ls: liệt kê | ls -a: lấy tất cả đã xóa
- Docker container stop < containe_id>
- Docker container prune: xóa toàn bộ
- Docker container exec : chạy bất kì lệnh trong container 

3. Vòng đời

- Chạy 1 Image chúng ta có Container (sống) Container ở trạng thái UP khi Stop hoặc Kill Container ở trạng thái EXITED, khi prune hoặc rm Container ở trạng thái DEAD  

4. Thực thi lệnh bên trong Container
- Câu lệnh: 
- + docker exec < container_id> < command> 
- Thao táo từ xa đến các máy khác (Giao thức SSH đến các máy từ xa sử dụng exec shell hoặc -it):
- + docker exec -it < containe_id> sh
- + docker exec -it < containe_id> bash

5. Port mapping
- Câu lệnh: 
docker run -p < target_port>:< container_port>

6. Logs trace
- Câu lệnh: 
docker logs  -f < container_id>
7. Volume-Bind mount (lưu trữ trong Container)
- Khởi tạo volume:  docker volume create [volume_name]
- Chạy: docker run -v [local_dir/volume] : [container_dir]
# Tạo Dockerfile 
1. Dockerfile
- Dockerfile là một template cho phép chúng ta khởi tạo 1 Image của riêng mình
- Câu lệnh: docker build -t < image_name> : < tag> . 
- Dấu "." được gọi là Build Context, tất cả các nội dung được chứa trong dockfile được gọi là Build context
- Keywords: 
- FROM < Image>
- RUN   < Command>
- WORKDIR < directory> : Thư mục mặc định
- COPY < src> < dest>
- ADD < src/URl> < dest>
- EXPOSE < port>
- CMD ["command", "argrument1", "argrument2"...]

2. Quy trình tạo
- Image là một layer được sắp xếp theo thứ tự. Mỗi layer được dựa theo một lệnh trong Dockfile
- Với mỗi lệnh, Docker sẽ khởi tạo một Container tạm thời từ layer trước, sau đó thực thi các lệnh trong Container đó, lưu trữ nó lại thành một Container tạm thời. Cuối cùng sẽ loại bỏ Container đó vì không cần thiết.
- Để làm giảm kích thước của Image, chúng ta kết hợp nhiều câu lệnh sử dụng toán tử && và xóa dữ liệu sau mỗi lệnh đó
## Docker Compose
1. Docker-Compose.yaml
- Thành phần: 
- version: '3'
- servies = container: pg( tên services ): 
- + image: postgres:9.6-alpine
- + port:   - 5432:5432
- + volume: - pgdata:/var/lib/postgressql/pgdata
- + environment:
- + + POSTGRES_DB:
- + + POSTGRES_USERNAME:
- + + POSTGRES_PASSWORD:

- volume: pgdata:

2. Tạo image đơn giản với compose
- Câu lệnh:
- + docker-compose build < service_name>
- Khởi tạo toàn bộ services
- + docker-compose up
- Khởi tạo riêng một services
- + docker-compose up < service_name>
- Dừng services 
- + docker-compose stop < service_name>
- Tắt service
- + docker-compose down

## Tham khảo tại https://www.youtube.com/watch?v=yWCse8S2qsM



