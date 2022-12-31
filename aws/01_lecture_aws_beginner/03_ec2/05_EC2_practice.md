# EC2 실습

## 인스턴스를 간단한 웹서버로 만들기

1. root 권한 

``` 
$ sudo su
```

2. yum 업데이트

```
$ yum update -y
```

3. 아파치 설치

```
$ yum install httpd -y
```

4. 아파치 실행

```
$ service httpd start
```

5. 인스턴스가 리부트 되어도 아파치 자동 실행

```
$ chkconfig httpd on
```

6. 간단한 정적 페이지 만들기

```
$ cd /var/www/html
$ vi index.html
```

반드시 html 파일명은 index.html 이 되어야 한다. 
