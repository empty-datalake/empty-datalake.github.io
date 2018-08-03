---
toc: true
toc_sticky: true
title: "Ubuntu에서 Jekyll 구동하기"
date: 2018-08-03 23:00:00 +0900
categories: 
    - Jekyll 
tags: 
    - Ubuntu
    - Jekyll
---

## 들어가며

Jekyll을 설치하지 않아도 Github에 push하면 블로그의 내용을 확인하는건 가능하다. 단지 push를 위해서 준비해야하는 것들과 컴파일 시간이 필요하고 컴파일에 실패하는 경우도 생기는게 문제다.

그래서 ubuntu에 설치해서 블로그 내용을 수정하면 바로 확일 할 수 있도록 설정하기로 했다. 다른 PC에서는 막힘 없이 진행되서 포스팅 할 생각이 없었는데 막히는 부분이 하나 나와서 포스팅 하기로 했다. 개인의 설정에 따라서 문제가 좀 다르게 나올 수 있을것 같다.

## 우분투에서 설치하기

### ruby 설치하기

Jekyll은 ruby를 사용해서 구동이 된다. 따라서 ruby가 없는 환경에서는 먼저 설치해 주어야 한다.

```bash
sudo apt-get install ruby ruby-dev build-essential
```

일반사용자용 RubyGem 디렉토리 설정하기

```bash
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### Jekyll은 설치하기

```bash
gem install jekyll bundler
```

```jekyll과``` ```bundler``` 까지 설치가 되었으면 jekyll용 ```Gemfile이``` 있는 git repo의 ```root```로 이동한뒤 필요한 의존성을 마저 설치하기 위해서 아래 명령어를 실행한다.

```bash
bundle install
```

## Jekyll은 실행하기

서버시작. 4000번 포트가 기본으로 할당 되는 것으로 보인다.

```bash
jekyll serve
```

```_drafts의``` 내용을 미리 보고 싶은 경우에는 ```--draft```를 추가한다.

```bash
jekyll serve --draft
```

변경사항이 자동으로 로드되게 하려면 ```--licereload```를 추가한다.

```bash
jekyll serve --livereload
```

## Troubleshooting

```bundle install``` 을 수행하는 과정에서 ```nokogirl``` 를 설치하면서 오류가 오류가 발생해면서 설치에 실패하게 되었다. 열심히 구글의 도움을 받아서 검색하던중 유사한 오류를 발견했다. 오류 메시지는 대충 아래의 내용을 포함하고 있었다.

```bash
Installing nokogiri 1.6.8 with native extensions

Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /tmp/bundler20160905-11974-ln6hq2nokogiri-1.6.8/gems/nokogiri-1.6.8/ext/nokogiri
/usr/bin/ruby2.3 -r ./siteconf20160905-11974-klmref.rb extconf.rb
Using pkg-config version 1.1.7
checking if the C compiler accepts ... yes
Building nokogiri using packaged libraries.
Using mini_portile version 2.1.0
checking for gzdopen() in -lz... no
zlib is missing; necessary for building libxml2
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
    --with-opt-dir
    --without-opt-dir
    --with-opt-include
    --without-opt-include=${opt-dir}/include
    --with-opt-lib
    --without-opt-lib=${opt-dir}/lib
    --with-make-prog
    --without-make-prog
    --srcdir=.
    --curdir
    --ruby=/usr/bin/$(RUBY_BASE_NAME)2.3
    --help
    --clean
    --use-system-libraries
    --enable-static
    --disable-static
    --with-zlib-dir
    --without-zlib-dir
    --with-zlib-include
    --without-zlib-include=${zlib-dir}/include
    --with-zlib-lib
    --without-zlib-lib=${zlib-dir}/lib
    --enable-cross-build
    --disable-cross-build

To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /tmp/bundler20160905-11974-ln6hq2nokogiri-1.6.8/extensions/x86_64-linux/2.3.0/nokogiri-1.6.8/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /tmp/bundler20160905-11974-ln6hq2nokogiri-1.6.8/gems/nokogiri-1.6.8 for inspection.
Results logged to /tmp/bundler20160905-11974-ln6hq2nokogiri-1.6.8/extensions/x86_64-linux/2.3.0/nokogiri-1.6.8/gem_make.out
```

nokogirl을 빌드하는 과정에서 무언가 라이브러리가 부족하다는 뉘앙스의 메시지가 눈에 보였다. Ubuntu 18.04를 설치하면서 minimal install을 선택한 덕분인것 같다. 그럼 설치해보자.

```bash
sudo apt-get install zlib1g-dev
```

이제 다시 ```bundle install``` 을 실행해보자. 

**된다**.

이제 문제는 마주치지 말자. 끝.

## References

- [우분투에서 설치하기](https://jekyllrb-ko.github.io/docs/installation/#ubuntu)
- [https://github.com/ev3dev/ev3dev.github.io/issues/229](https://github.com/ev3dev/ev3dev.github.io/issues/229)