# Reddit Place Script 2022

[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)

It's all gone now, thanks for the effort guys, after all, r/place 2022 is just an april fool joke?

## Disclaimer

This is a copy of rdeepak2002 version

## Thông tin

Script này sẽ vẽ hình ảnh vào r/place (<https://www.reddit.com/r/place/>).

## Tính năng

- Hỗ trợ chạy nhiều tài khoản cùng lúc
- Xác định thời gian có thể đặt pixel trở lại của từng tài khoản
- Tự nhận diện các pixel cùng màu và né chúng
- Tự động chuyển đổi màu của hình ảnh sang bảng màu của r/place
- Dễ dàng để sử dụng
- Hỗ trợ proxy SOCKS
- Không cần client id và secret code
- Đọc proxies từ proxies.txt

## Những thứ cần để chạy

- [Bản mới nhất của Python 3](https://www.python.org/downloads/)

## MacOSX
Nếu bạn dùng MacOS và gặp lỗi SSL_CERTIFICATE. Bạn có thể fix nó theo hướng dẫn này https://stackoverflow.com/questions/42098126/mac-osx-python-ssl-sslerror-ssl-certificate-verify-failed-certificate-verify  

## Hướng dẫn cài đặt

Thay username và password của bạn trong file config.json
ví dụ:
  "workers": {
    "username": {
      "password": "mật khẩu",
      "start_coords": [0, 0]
    },
    "username2": {
      "password": "mật khẩu",
      "start_coords": [0, 1]
    },...
  }
}
```
### Notes

- Bạn có thể đổi tên file target2.png bằng bất cứ file nào miễn đuôi tệp là .jpg hoặc .png
- Những pixel với bảng màu R, G, B là 69, 42, 0 sẽ bị skip, nó sẽ hữu dụng cho các hình tứ giác
- Nếu bạn sử dụng xác minh hai bước (2FA) thì hãy đổi "mật khẩu" thành "mật khẩu:XXXXXX" trong đó 'XXXXXX' là mã 2FA của bạn

## Chạy script

### Windows

```shell
chạy file start.bat hoặc startverbose.bat
```

### Unix-like (Linux, MacOS etc.)

```shell
chmod +x start.sh startverbose.sh
./start.sh or ./startverbose.sh
```

### You can get more logs (`DEBUG`) by running the script with `-d` flag

(không cần thiết nên không dịch)
`python3 main.py -d` or `python3 main.py --debug`

## Chạy nhiều tài khoản

Bạn có thể chạy thêm nhiều tài khoản bằng cách thêm các username

```json
{
  "image_path":"image.png",
  "image_start_coords": [741, 610],
  "thread_delay": 2,

  "workers": {
    "worker1username": {
      "password": "password",
      "start_coords": [0, 0]
    },
    "worker2username": {
      "password": "password",
      "start_coords": [0, 50]
    }
  }
}
```

Ở đây, tài khoản 1 sẽ vẽ từ toạ độ 0, 0 của target.png và tài khoản 2 sẽ vẽ từ toạ độ 0, 50 của target.png

Cái này sẽ hữu dụng nếu bạn muốn vẽ nhiều phần của target.png với nhiều tài khoản

## Các setting khác

Nếu gặp các lỗi về file json thì hãy kiểm tra lỗi chính tả, hoặc `config.json` cần fix. Hãy chắc rằng bạn add thêm dòng này vào trong file config.json

```json
{
    "thread_delay": 2,
    "unverified_place_frequency": false,
    "proxies": ["1.1.1.1:8080","2.2.2.2:1234"],
    "compact_logging": true
}
```

- thread_delay - Thêm độ trễ giữa lúc bắt đầu thêm pixel và kết thúc. Có thể sử dụng để tránh bot chạy đúng vào khi nó đang cooldown
- unverified_place_frequency - Đặt giới hạn pixel cho các tài khoản chưa được xác minh
- proxies - Dặt proxies để gửi các yêu cầu đến Reddit mà không bị ban. Proxies được chọn ngẫu nhiên mỗi lần gửi yêu cầu. Cũng có thể được sử dụng để tránh cooldown
- compact_logging - Loại bỏ bộ đếm ngược cho đến pixel tiếp theo

- Điểm mà bot không vẽ có thể sử dụng mà RGB (69, 42, 0) trong bất cứ phần nào của hình ảnh
- Chạy file startverbose.bat/.sh thì bạn sẽ có nhiều thông tin khi bot chạy hơn, có thể hữu dụng khi bạn đang sửa lỗi hoặc test
- Bạn cũng có thể add thêm proxies bằng cách ghi thêm "proxies" và cách chúng ra mỗi dòng.

# Tor

tor is can be used as an alternative to normal proxies. Note that currently, you cannot use normal proxies and tor at the same time.

```json
"using_tor": false,
"tor_port": 1881,
"tor_control_port": 9051,
"tor_password": "Passwort",
"tor_delay": 5,
"use_builtin_tor": true 
```
the config values are as follows:
- deactivates or activates tor
- sets the httptunnel port that should be used
- sets the tor control port
- sets the password (leave it as "Passwort" if you want to use the default binaries
- the delay that tor should receive to process a new connection
- whether the included tor binary should be used. It is preconfigured. If you want to use your own binary, make sure you configure it right.

note that when using the included binaries, only the tunnel port is explicitly set while starting tor.

<h3>If you want to use your own binaries, follow these steps:</h3>

- get tor standalone for your platform [here](https://www.torproject.org/download/tor/). For Windows just use the expert bundle. For MacOS you'll have to compile the binaries yourself or get them from somewhere else, which is both out of the scope of this guide.
- in your tor folder, create a file named ``torrc``. Copy [this](https://github.com/torproject/tor/blob/main/src/config/torrc.sample.in) into it.
- Search for ``ControlPort`` in your torrc file and uncomment it. change the port number to your desired control port.
- Decide on the password you want to use. Run ``tor --hash-password PASSWORD`` from a terminal in the folder with your tor executable, with "PASSWORD" being your desired password. copy the resulting hash.
- search for ``HashedControlPassword`` and uncomment it. paste the hash value you copied after it.
- decide on a port for your httptunnel. The default for this script is 1881.
- fill in your password, your httptunnel port and your control port in this script's ``config.json`` and enable tor with ``using_tor = true``.
- To start tor, run ``tor --defaults-torrc PATHTOTORRC --HttpTunnelPort TUNNELPORT``, with PATHTOTORRC being your path to the torrc file you created and TUNNELPORT being your httptunnel port.
- now run the script and (hopefully) everything should work.

license for the included tor binary:
> Tor is distributed under the "3-clause BSD" license, a commonly used
software license that means Tor is both free software and open source:
Copyright (c) 2001-2004, Roger Dingledine
Copyright (c) 2004-2006, Roger Dingledine, Nick Mathewson
Copyright (c) 2007-2019, The Tor Project, Inc.
Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are
met:
>- Redistributions of source code must retain the above copyright
notice, this list of conditions and the following disclaimer.
>- Redistributions in binary form must reproduce the above
copyright notice, this list of conditions and the following disclaimer
in the documentation and/or other materials provided with the
distribution.
>- Neither the names of the copyright owners nor the names of its
contributors may be used to endorse or promote products derived from
this software without specific prior written permission.

>THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
"AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## Docker

(không quan trọng, không dịch)
A dockerfile is provided. Instructions on installing docker are outside the scope of this guide.

To build: After editing your config.json, run `docker build . -t place-bot`. and wait for the image to build

You can now run with 

`docker run place-bot`


## Các đóng góp

Xem file [Contributing Guide](docs/CONTRIBUTING.md)
