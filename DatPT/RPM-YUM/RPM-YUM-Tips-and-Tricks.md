# RPM/YUM - Tips and Tricks.

## 1. Tại sao chúng ta lại cần RPM package.

Ban đầu tất cả các phần mềm được sử dụng trên Linux là có sẵn dưới dạng file tar. Tar files bao gồm tất cả các files có liên quan đến phần mềm. Người sử dụng sẽ giải nén file tar , biên dịch phần mềm và sau đó là cài đặt phần mềm. 2 điều trên có những yếu điểm như sau :

- Khi một phần mềm được cài đặt lên rất khó để theo dõi các tập tin được cài đặt trong đường dẫn khác nhau trong hệ thống tập tin và sẽ gặp khó khăn trong việc gỡ bỏ hoặc cập nhật phần mềm.

- Nếu ứng dụng yêu cầu thêm những phần mềm hoặc các phụ trợ khác (dependency) thì chúng ta phải tự tay cài những thứ này.

## 2. RPM package là gì?

RPM được sinh ra để giải quyết 2 yếu điểm ở phần 1 khi tất cả các các phần mềm và metadata của nó được đóng gói thành 1 file gọi là file RPM.

RPM file bao gồm phiên bản, danh sách các filess, các gói phụ trợ, mô tả về package,.... Nếu một ứng dụng được cài đặt bằng file RPM nó dễ dàng theo dõi phiên bản của phần mềm , các gói phụ trợ và đường dẫn files.

## 3. Câu lệnh với RPM.

List ra các gói đã được cài đặt bằng RPM ta sử dụng lệnh :

    ```sh
    rpm -qa
    ```

Kiểm tra một gói cụ thể nào đó được cài đặt bằng RPM trên hệ thống :

```sh
rpm -qa [package_name]

# example

rpm -qa httpd
httpd-2.4.6-67.el7.centos.6.x86_64
```

Kiểm tra gói phần mềm thông qua một đường dẫn đã cài đặt từ trước :

```sh
rpm -qf [file_path]

# example

rpm -qf /etc/httpd
httpd-2.4.6-67.el7.centos.6.x86_64
```

List tất cả các file trong một package :

```sh
rpm -ql <package name>

# example 

rpm -ql yum
/etc/logrotate.d/yum
/etc/yum
/etc/yum.conf
/etc/yum.repos.d
/etc/yum/fssnap.d
/etc/yum/pluginconf.d
/etc/yum/protected.d
/etc/yum/vars
/etc/yum/version-groups.conf
/usr/bin/yum
...................
/var/lib/yum/history
/var/lib/yum/plugins
/var/lib/yum/uuid
/var/lib/yum/yumdb

```

Kiểm tra các gói phụ trợ trong RPM package :

```sh
rpm -qR <package name>

# example

rpm -qR yum
/usr/bin/python
config(yum) = 3.4.3-154.el7.centos
cpio
diffutils
pygpgme
pyliblzma
python >= 2.4
python(abi) = 2.7
python-iniparse
python-sqlite
python-urlgrabber >= 3.10-8
pyxattr
rpm >= 0:4.11.3-22
rpm-python
rpmlib(CompressedFileNames) <= 3.0.4-1
rpmlib(FileDigests) <= 4.6.0-1
rpmlib(PayloadFilesHavePrefix) <= 4.0-1
yum-metadata-parser >= 1.1.0
yum-plugin-fastestmirror
rpmlib(PayloadIsXz) <= 5.2-1
```

## 4.  YUM commands.

Yum là một command line tool để quản lý RPM package và các repositories. 

Để cài đặt RPM package chúng ta thực hiện lệnh sau dưới quyền user root :

```sh
yum install [package_name]

# example

yum install httpd
```

- Lệnh trên sẽ thực hiện cài đặt httpd  package cùng với tất cả các gói phụ trợ . Nếu bất kỳ gói phụ trợ nào không được tìm thấy nó sẽ thất bại với thông báo lỗi `dependency error`.

- Trong thời gian cài đặt nó sẽ cho chúng ta những tùy chọn để tải package về `Is this ok [y/d/N]` ý nhĩa của các tùy chọn như sau : Với d thì là mode download only, chỉ download về và không cài đặt, và file rpm sẽ được lưu trong đường dẫn `/etc/cache/rpm`; tùy chọn y sẽ download và tiến hành cài đặt; N là không download. Ngoài ra chúng ta cũng có thể tiến hành download nhiều package cùng lúc bằng cách sử dụng lệnh :

    ```sh
    yum install [pkg1] [pkg2] ... [pkgn]
    ```

Để remove một package bất kỳ ta thực hiện lệnh sau :

```sh
yum erase [package name]

# example

yum erase httpd
```

Để update một RPM package bất kỳ chúng ta thực hiện lệnh sau :

```sh
yum update [package_name]

# example

yum update httpd
```

Để update tất cả các gói RPMs trong hệ thống chúng ta thực hiện lệnh sau :

```sh
yum update
```

Để lấy thông  tin về một package ta thực hiện lệnh :

```sh
yum  info [package_name]

# example

yum info httpd
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.nhanhoa.com
 * epel: mirror.vinahost.vn
 * extras: mirrors.nhanhoa.com
 * updates: mirrors.nhanhoa.com
Installed Packages
Name        : httpd
Arch        : x86_64
Version     : 2.4.6
Release     : 67.el7.centos.6
Size        : 9.4 M
Repo        : installed
From repo   : updates
Summary     : Apache HTTP Server
URL         : http://httpd.apache.org/
License     : ASL 2.0
Description : The Apache HTTP Server is a powerful, efficient, and extensible
            : web server.

```

Để kiểm tra lịch sử của tất cả các tác vụ với yum trong hệ tống chúng ta có thể chạy lệnh dưới đây để kiểm tra được id của tác vụ cũng như là thời gian và ngày thực hiện :

```sh
yum history
```

CHúng ta có thể sử dụng `history` để thực hiện rollback bất kỳ tác vụ nào được thực hiện với yum :

```sh
yum history undo [transaction id]

# example

yum history undo 2 -y
```

Sử dụng lệnh sau để hiển thị cấu hình repo trong hệ thống :

```sh
yum repolist
```

Để xóa các dữ liệu cache lưu trữ trong `/var/cache/yum` sử dụng lệnh sau :

```sh
yum clean all
```

## 5. Làm thế nào để tạo một yum repo?

Chúng ta sẽ tiến hành test thử tạo một kho repo, 2 máy chủ sử dụng CentOS 7. Một máy đóng vai trò như một hệ thống máy chủ lưu trữ các repo và 1 máy sẽ đóng vai trò là máy khách.

### Cấu hình server.

Cài đặt httpd : 

```sh
yum install httpd -y
```

- Tạo một thư mục custom_repo trong `/var/www/html` phục vụ lưu trữ các repo, sau đó tiến hành coppy các files rpm vào đây.

    ```sh
    mkdir -p /var/www/html/custom_repo
    ```

Nếu chúng ta tạo một một kho repo để kiểm thử chúng ta có thể download rpms sử dụng yumdownloader, nó là một tool để download RPMs từ yum repo.

```sh
yumdownloader <pkg1> <pkg2> ....<pkgn>
```

- Tải package `createrepo` để  tạo yum repo :

    ```sh
    yum install createrepo -y 
    ```

- Sử dụng lệnh sau sẽ tạo ra repo data trong thư mục custom_repo. Sau khi xác minh được repo đã được tạo. Bật dịch vụ httpd lên :

    ```sh
    createrepo /var/www/html/custom_repo
    ls -l /var/www/html/custom_repo/repodata
    systemctl start httpd
    ```

CHúng ta có thể xác minh repo thông qua đường dẫn `IP-server/custom_repo`

### Cấu hình trên client.

CHúng ta có 2 cách để thêm repo vào hệ thống máy khách.

- Tạo một repo file `custom.repo` trong `/etc/yum/repos.d` và thêm nội dung vào đó :

    ```sh
    [custom_repo]
    name=custom_repo  
    baseurl=IP-server/custom_repo  
    enabled=1  
    gpgcheck=1  
    ```

    ```sh
    Name - Tên của repo 
    baseurl - Đường dẫn của repo  
    enabled - Để enabled repository đặt giá trị là 1. Để disable, 0.  
    gpgcheck - Để enable gpgcheck đặt giá trị là 1. Để disable, 0. 
    ```

- CHạy lệnh `yum-config-manager` để thêm repo, nó sẽ tự động tạo file repo trong `/etc/yum/repo.d`.

    ```sh
    yum-config-manager --add-repo http://IP-server/custom_repo
    ```

- Sau khi hoàn thành các thiết lập chúng ta tiến hành cài đặt :

    ```sh
    yum install [package name]

    # package name ở đây là tên của package trong repo
    ```




