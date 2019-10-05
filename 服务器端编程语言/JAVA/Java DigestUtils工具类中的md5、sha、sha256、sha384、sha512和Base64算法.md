* ### maven 引入apache的jar
  ```java
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.6</version>
        </dependency>
  ```
* ### 使用代码
  ```java
  private static void digest(){
		String target = "changeme";
		String md5Hex = DigestUtils.md5Hex(target);
		String shaHex = DigestUtils.shaHex(target);
		String sha256Hex = DigestUtils.sha256Hex(target);
		String sha384Hex = DigestUtils.sha384Hex(target);
		String sha512Hex = DigestUtils.sha512Hex(target)
		byte[] encode = Base64.encodeBase64(target.getBytes(), true);
		byte[] decode = Base64.decodeBase64(encode);
		System.out.println(target + " md5Hex:     " + md5Hex);
		System.out.println(target + " shaHex:     " + shaHex);
		System.out.println(target + " sha256Hex:     " + sha256Hex);
		System.out.println(target + " sha384Hex:     " + sha384Hex);
		System.out.println(target + " sha512Hex:     " + sha512Hex);
		System.out.println("Base64 Encode:     " + new String(encode));
		System.out.println("Base64 Decode:     " + new String(decode));

	}
  ```