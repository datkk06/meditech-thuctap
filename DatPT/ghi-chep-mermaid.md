# Tổng hợp các ghi chép về mermaid.

## 1. Cài đặt.

Trong bài này sẽ hướng dẫn cài đặt mermaid trên CentOS 7.

- Thêm repo `yarn` :

    ```sh
    curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
    ```

- Nếu máy chưa cài nodejs chúng ta cần cấu hình thêm source NodeJS :

    ```sh
    curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -
    ```

- Sau đó , cài đặt `yarn` :

    ```sh
    yum install yarn -y
    ```
- Kiểm tra phiên bản của `yarn` để kiểm chứng việc cài đặt : 

    ```sh
    yarn --version
    ```

- Thêm package của `mermaid` :

    ```sh
    yarn add mermaid
    ```

[AsciiMath Documentation](http://asciimath.org/)

```mermaid
sequenceDiagram
    Alice ->> Bob: Hello Bob, how are you?
    Bob-->>John: How about you John?
    Bob--x Alice: I am good thanks!
    Bob-x John: I am good thanks!
    Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

    Bob-->Alice: Checking with John...
    Alice->John: Yes... John, how are you?
end
```