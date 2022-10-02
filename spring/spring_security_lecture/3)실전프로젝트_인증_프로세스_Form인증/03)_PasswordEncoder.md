# PasswordEncoder

- 비밀번호를 안전하게 암호화 하도록 제공

    => 평문인 비밀번호를 안전하게 암호화 하여 저장한다.

- 생성

    - `PasswordEncode pwEncoder = PasswordEncoderFactories.createDelegatingPasswordEncoder()`

    ```java
        public static PasswordEncoder createDelegatingPasswordEncoder() {
            String encodingId = "bcrypt";
            Map<String, PasswordEncoder> encoders = new HashMap<>();
            encoders.put(encodingId, new BCryptPasswordEncoder());
            encoders.put("ldap", new org.springframework.security.crypto.password.LdapShaPasswordEncoder());
            encoders.put("MD4", new org.springframework.security.crypto.password.Md4PasswordEncoder());
            encoders.put("MD5", new org.springframework.security.crypto.password.MessageDigestPasswordEncoder("MD5"));
            encoders.put("noop", org.springframework.security.crypto.password.NoOpPasswordEncoder.getInstance());
            encoders.put("pbkdf2", new Pbkdf2PasswordEncoder());
            encoders.put("scrypt", new SCryptPasswordEncoder());
            encoders.put("SHA-1", new org.springframework.security.crypto.password.MessageDigestPasswordEncoder("SHA-1"));
            encoders.put("SHA-256",
                    new org.springframework.security.crypto.password.MessageDigestPasswordEncoder("SHA-256"));
            encoders.put("sha256", new org.springframework.security.crypto.password.StandardPasswordEncoder());
            encoders.put("argon2", new Argon2PasswordEncoder());
           
            return new DelegatingPasswordEncoder(encodingId, encoders);
        }
    ```


    -  여러개의 PasswordEncoder 유형을 선언한 뒤, 상황에 맞게 선택하여 사용할 수 있도록 지원하는 Encoder 다.

<br>

- 암호화 포맷 : `{id}encodedPassword`

    - ex) `{bcrypt}@#!%rewrwe132sds1!@#421123.!@#421231@.13DDgfher`

    - 알고리즘 종류 (id 종류) : `bcyrpt, noop, bpkdf2, scrypt, sha256`

    - 기본 포맷은 `bcrypt` 방식으로 사용함

<br>

- 인터페이스

    - encode(password)

        - 패스워드 암호화

    - matches (rawPassword, encodedPassword)

        - 패스워드 비교