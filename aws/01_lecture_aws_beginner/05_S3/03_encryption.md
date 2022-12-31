# 암호화 (Encryption)

## S3 암호화

- 파일 업로드 / 다운로드시

    - SSL (Secure Socket Layer) / TLS (Transport Layer Security)

- 가만히 있을 시

    - SEE-S3

    - SSE-KMS

    - SSE-C

    (SSE : Server-side encryption)

## S3 암호화 과정

- x-amz-server-side-encryption-parameter 

    - 어떤 알고리즘을 사용하는지, 헤더에서 알려준다

- 암호화 된 put 요청 예시

