# Kitura-Install
IBM-Kitura-installation(설치하는법)



## 스위프트 설치 (Ubuntu; APT-based linux)
1. Update your local package index first.
    ```bash
    sudo apt-get update && sudo apt-get upgrade
    ```
    
2. Install Swift dependencies on linux:
    ```bash
    sudo apt-get install clang libicu-dev
    ```
    
3. Install Swift depending on your platform on the follow [link](https://swift.org/download) (The latest version are recommended).

4. After installation of Swift, check your PATH environment variable whether Swift binary path is set or not. If it is not set execute below.:
    ```bash
    export PATH=/스위프트가 설치된 경로/usr/bin:"${PATH}"
    ```
    
    More details : 'Linux' on [here](https://swift.org/download)

## .bashrc 변경 
    1. Edit .bashrc  
    
      ```bash
      export PATH=$HOME/swift/usr/bin:"${PATH}"
      export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
      source .bashrc
      ```
## 서브 모듈 설치

  1. 서브 모듈 설치
  
    ```bash
    sudo apt-get install autoconf libtool pkg-config libhttp-parser-dev \
    libcurl4-openssl-dev libhiredis-dev libblocksruntime-dev \
    libkqueue-dev libpthread-workqueue-dev libbsd-dev
    ```
    
  2. libdispatch Patch and PCRE(중요)

    ```bash
    git clone https://github.com/apple/swift-corelibs-libdispatch.git
    ```
    디스패치 빌드후 설치 
    ```bash
    cd swift-corelibs-libdispatch
    sh ./autogen.sh
    ./configure  --with-swift-toolchain=<스위프트가 설치된 경로>/usr --prefix=<스위프트가 설치된 경로>/usr
    make
    make install
    ```
    PCRE 다운로드후 압축해제
    
    ```bash
    cd ~
    curl -O "http://ftp.exim.org/pub/pcre/pcre2-10.20.tar.gz"
    tar zxvf pcre2-10.20.tar.gz
    ```
    
     PCRE 설치
    
    ```bash
    cd pcre2-10.20
    ./configure
    make
    sudo make install
    ```

## Kitura 설치 
  1. 프로젝트 디렉토리 만들기
  ```bash
  mkdir firstProject
  ```
  2. 스위프트 프로젝트 설정

  ```bash
  cd myFirstProject
  swift build --init
  ``` 
  3. 패키지 메니저 설정(package.swift)

   ```swift
  import PackageDescription

  let package = Package(
      name: "firstProject",
      dependencies: [
          .Package(url: "https://github.com/IBM-Swift/Kitura.git", majorVersion: 0, minor: 5)
      ])
  ``` 
  4. 실행 코드 입력 (Source/main.swift)
    ```swift
    import Kitura
    import KituraNet
    import KituraSys

    let router = Router()

    router.get("/") {
      request, response, next in
      response.status(HttpStatusCode.OK).send("Hello, World!")
      next()
    }

    let server = HttpServer.listen(8090, delegate: router)
    Server.run()
  ``` 
  5. 프로젝트 빌드
  
  ```bash
  swift build -Xcc -fblocks
  ``` 
  6. 실행
  ```bash
  .build/debug/myFirstProject
  ``` 
  
  7. Install Swift depending on your platform on the follow [https://localhost:8090](https://localhost:8090) 
    
