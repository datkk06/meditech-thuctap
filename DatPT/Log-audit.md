# Tìm hiểu về log audit.

## 1. Log audit là gì?

- Audit hệ thống đơn giản là việc phân tích sâu về hệ thống với 1 mục tiêu cụ thể. Một quy trìnhcuộc audit được thực hiện bằng việc kiểm tra các vùng khác nhau của hệ thống đó, với đánh giá các vùng khác nhau của hệ thống.

- Một trong những chương trình audit phổ biến thường được sử dụng trên RHEL / CentOS Linux là auditd. ĐâyNó là một phương tiện để theo dõi các thông tin liên quan đến an ninh trên một hệ thống.: Aauditd sử dụng các quy tắc được cấu hình từ trước để thu thập một lượng lớn thông tin về các sự kiện đang xảy ra trên hệ thống và ghi lại chúng dưới dạng log.

- Nó có thể ghi lại các thông tin như ngày và thời gian, kiểu event và kết quả của event; user gây ra event, bất kỳ sửa đổi nào được thực hiện cho các tập tin /cơ sở dữ liệu, các event sử dụng các cơ chế xác thực hệ thống, chẳng hạn như PAM, LDAP, SSH và các loại khác.

- Auditd cũng theo dõi bất kỳ thay đổi nào được thực hiện đối với các tệp tin audit config hoặc bất kỳ truy cập nào vào audit log và bất kỳ nỗ lực nào để nhập hoặc xuất thông tin từ hệ thống cùng với rất nhiều thông tin liên quan đến an ninh khác.

## 2. Log audit quan trọng như thế nào.

- Tự chủ do không phải chạy bất cứ chương trình hay tiến trình nào từ bên ngoài hệ thống.

- Cho phép bạn xem bất kỳ hoạt động nào của hệ thống mà bạn muốn.

- Hỗ trợ việc phát hiện hoặc phân tích những nguy cơ tiềm ẩn của một hệ thống.

- Có khả năng hoạt động như một hệ thống IDS.

- Có thể làm việc với hệ thống IDS để cho phép phát hiện xâm nhập.

- Là một công cụ quan trọng trong forensics.

## 3. Các thành phần trong hệ thống.

Hệ thống audit có 2 thành phần chính đó là :

- user-space applications và các tiện ích/công cụ

- Hệ thống xử lý ở kernel-side –chấp nhận các yêu cầu từ các user-space applications và chuyển chúng qua ba loại bộ lọc, cụ thể: user, task, exit hoặc exclude.

Phần quan trọng nhất là user-space audit daemon (auditd) thu thập thông tin dựa trên các quy tắc được cấu hình từ trước, từ kernel và tạo ra các thông tin trong audit log: đường dẫn mặc định là /var/log/audit/audit.log.

Ngoài ra, audispd (là một daemon gửi đi các audit) là một multiplexor tương tác với auditd và gửi sự kiện đến các chương trình khác mà muốn thực hiện xử lý event theo thời gian thực.

Một số công cụ mà user-space sử dụng cho quản lý và truy xuất thông tin từ hệ thống audit:

- auditctl – 1 tiện ích để kiểm soát hệ thống kernel audit.

- ausearch – 1 tiện ích cho việc tìm kiếm các audit log với các event đặc biệt.

- aureport – 1 tiện ích để tạo các báo cáo về sự kiện được ghi lại.

## 4. Cài đặt audit trên CentOS.

- Kiểm tra xem các gói cài đặt của audit đã được cài đặt trên hệ thống hay chưa :

    ```sh
    rpm -qa | grep audit  audit-libs-python-2.7.6-3.el7.x86_64  audit-2.7.6-3.el7.x86_64  audit-libs-2.7.6-3.el7.x86_64
    ```

- Chạy lệnh sau để cài đặt audit :

    ```sh
    yum install audit -y
    ```

- Kiểm tra xem auditd có hoạt động hay không :

    ```sh
    systemctl is-enabled auditd
    ```

## 5. Tìm hiểu về cấu hình của Auditd.

File cấu hình chính `/etc/audit/auditd.conf` , sửa đổi các thông tin cấu hình ở đây sẽ giúp chúng ta có thể kiểm soát được các dịch vụ đang chạy, vị trí lưu log, số lượng log tối đa, định dạng log, cách xử lý khi đầy ổ cứng, rotation và nhiều tùy chọn khác.

- Thông tin về file config :

    ```sh
    #
    # This file controls the configuration of the audit daemon
    #

    local_events = yes
    write_logs = yes
    log_file = /var/log/audit/audit.log
    log_group = root
    log_format = RAW
    flush = INCREMENTAL_ASYNC
    freq = 50
    max_log_file = 8
    num_logs = 5
    priority_boost = 4
    disp_qos = lossy
    dispatcher = /sbin/audispd
    name_format = NONE
    ##name = mydomain
    max_log_file_action = ROTATE
    space_left = 75
    space_left_action = SYSLOG
    verify_email = yes
    action_mail_acct = root
    admin_space_left = 50
    admin_space_left_action = SUSPEND
    disk_full_action = SUSPEND
    disk_error_action = SUSPEND
    use_libwrap = yes
    ##tcp_listen_port = 60
    tcp_listen_queue = 5
    tcp_max_per_addr = 1
    ##tcp_client_ports = 1024-65535
    tcp_client_max_idle = 0
    enable_krb5 = no
    krb5_principal = auditd
    ##krb5_key_file = /etc/audit/audit.key
    distribute_network = no

    ```

## 6. Kiểm soát các rules của auditd.