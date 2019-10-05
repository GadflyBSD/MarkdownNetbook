> 加密方式大致分为两种，对称加密和非对称加密。对称加密是最快速、最简单的一种加密方式，加密（encryption）与解密（decryption）用的是同样的密钥（secret key）。非对称加密为数据的加密与解密提供了一个非常安全的方法，它使用了一对密钥，公钥（public key）和私钥（private key）。私钥只能由一方安全保管，不能外泄，而公钥则可以发给任何请求它的人。因此安全性大大提高。

- ### 对称加密
  > 所谓对称加密算法即：加密和解密使用相同密钥的算法。常见的有DES、3DES、AES、PBE等加密算法，这几种算法安全性依次是逐渐增强的。
  - #### DES加密
    > DES是一种对称加密算法，是一种非常简便的加密算法，但是密钥长度比较短。DES加密算法出自IBM的研究，后来被美国政府正式采用，之后开始广泛流传，但是近些年使用越来越少，因为DES使用56位密钥，以现代计算能力，24小时内即可被破解。虽然如此，在某些简单应用中，我们还是可以使用DES加密算法.
    ```java
    import org.apache.commons.codec.binary.Base64;
    import javax.crypto.Cipher;
    import javax.crypto.KeyGenerator;
    import javax.crypto.SecretKey;
    import javax.crypto.spec.SecretKeySpec;
    import java.security.Key;
    import java.security.NoSuchAlgorithmException;
    import java.security.SecureRandom;
    import java.util.logging.Level;
    import java.util.logging.Logger;
    import static javax.crypto.Cipher.getInstance;
    public class DESUtil {
    	private static final String KEY_ALGORITHM = "DES";
    	private static final String DEFAULT_CIPHER_ALGORITHM = "DES/ECB/PKCS5Padding";//默认的加密算法
    
    	/**
    	 * DES 加密操作
    	 *
    	 * @param content 待加密内容
    	 * @param key 加密密钥
    	 * @return 返回Base64转码后的加密数据
    	 */
    	public static String encrypt(String content, String key) {
    		try {
    			Cipher cipher = getInstance(DEFAULT_CIPHER_ALGORITHM);// 创建密码器
    			byte[] byteContent = content.getBytes("utf-8");
    			cipher.init(Cipher.ENCRYPT_MODE, getSecretKey(key));// 初始化为加密模式的密码器
    			byte[] result = cipher.doFinal(byteContent);// 加密
    			return Base64.encodeBase64String(result);//通过Base64转码返回
    		} catch (Exception ex) {
    			Logger.getLogger(DESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	/**
    	 * DES 解密操作
    	 *
    	 * @param content
    	 * @param key
    	 * @return
    	 */
    	public static String decrypt(String content, String key) {
    		try {
    			//实例化
    			Cipher cipher = getInstance(DEFAULT_CIPHER_ALGORITHM);
    			//使用密钥初始化，设置为解密模式
    			cipher.init(Cipher.DECRYPT_MODE, getSecretKey(key));
    			System.out.println(cipher.doFinal(Base64.decodeBase64(content)));
    			//执行操作
    			byte[] result = cipher.doFinal(Base64.decodeBase64(content));
    			return new String(result, "utf-8");
    		} catch (Exception ex) {
    			Logger.getLogger(DESUtil.class.getName()).log(Level.SEVERE, key, ex);
    		}
    		return null;
    	}
    
    	/**
    	 * 生成加密秘钥
    	 *
    	 * @return
    	 */
    	private static Key getSecretKey(final String key) {
    		//返回生成指定算法密钥生成器的 KeyGenerator 对象
    		KeyGenerator kg = null;
    		try {
    			kg = KeyGenerator.getInstance(KEY_ALGORITHM);
    			SecureRandom secureRandom = SecureRandom.getInstance("SHA1PRNG") ;
    			secureRandom.setSeed(key.getBytes());
    			kg.init(56, secureRandom);  //DES 要求密钥长度为 56
    			SecretKey secretKey = kg.generateKey(); //生成一个密钥
    			return new SecretKeySpec(secretKey.getEncoded(), KEY_ALGORITHM);// 转换为DES专用密钥
    		} catch (NoSuchAlgorithmException ex) {
    			Logger.getLogger(DESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	public static void main(String[] args) {
    		String content = "hello,您好";
    		String key = "sde@5f98H*^hsff%dfs$r344&df8543*er";
    		System.out.println("content:" + content);
    		String s1 = DESUtil.encrypt(content, key);
    		System.out.println("s1:" + s1);
    		System.out.println("s2:"+ DESUtil.decrypt(s1, key));
    	}
    }
    ```
  - #### 3DES加密
    > 3DES是一种对称加密算法，在 DES 的基础上，使用三重数据加密算法，对数据进行加密，它相当于是对每个数据块应用三次 DES 加密算法。由于计算机运算能力的增强，原版 DES 密码的密钥长度变得容易被暴力破解；3DES 即是设计用来提供一种相对简单的方法，即通过增加 DES 的密钥长度来避免类似的攻击，而不是设计一种全新的块密码算法这样来说，破解的概率就小了很多。缺点由于使用了三重数据加密算法，可能会比较耗性能。
    ```java
    import org.apache.commons.codec.binary.Base64;

    import javax.crypto.Cipher;
    import javax.crypto.KeyGenerator;
    import javax.crypto.SecretKey;
    import javax.crypto.spec.SecretKeySpec;
    import java.security.NoSuchAlgorithmException;
    import java.security.SecureRandom;
    import java.util.logging.Level;
    import java.util.logging.Logger;
    
    public class TripDESUtil {
    	private static final String KEY_ALGORITHM = "DESede";
    	private static final String DEFAULT_CIPHER_ALGORITHM = "DESede/ECB/PKCS5Padding";//默认的加密算法
    
    	/**
    	 * DESede 加密操作
    	 *
    	 * @param content 待加密内容
    	 * @param key 加密密钥
    	 * @return 返回Base64转码后的加密数据
    	 */
    	public static String encrypt(String content, String key) {
    		try {
    			Cipher cipher = Cipher.getInstance(DEFAULT_CIPHER_ALGORITHM);// 创建密码器
    			byte[] byteContent = content.getBytes("utf-8");
    			cipher.init(Cipher.ENCRYPT_MODE, getSecretKey(key));// 初始化为加密模式的密码器
    			byte[] result = cipher.doFinal(byteContent);// 加密
    			return Base64.encodeBase64String(result);//通过Base64转码返回
    		} catch (Exception ex) {
    			Logger.getLogger(TripDESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	/**
    	 * DESede 解密操作
    	 *
    	 * @param content
    	 * @param key
    	 * @return
    	 */
    	public static String decrypt(String content, String key) {
    		try {
    			//实例化
    			Cipher cipher = Cipher.getInstance(DEFAULT_CIPHER_ALGORITHM);
    			//使用密钥初始化，设置为解密模式
    			cipher.init(Cipher.DECRYPT_MODE, getSecretKey(key));
    			//执行操作
    			byte[] result = cipher.doFinal(Base64.decodeBase64(content));
    			return new String(result, "utf-8");
    		} catch (Exception ex) {
    			Logger.getLogger(TripDESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    
    		return null;
    	}
    
    	/**
    	 * 生成加密秘钥
    	 *
    	 * @return
    	 */
    	private static SecretKeySpec getSecretKey(final String key) {
    		//返回生成指定算法密钥生成器的 KeyGenerator 对象
    		KeyGenerator kg = null;
    		try {
    			kg = KeyGenerator.getInstance(KEY_ALGORITHM);
    			SecureRandom secureRandom = SecureRandom.getInstance("SHA1PRNG") ;
    			secureRandom.setSeed(key.getBytes());
    			kg.init(secureRandom);  //DESede
    			SecretKey secretKey = kg.generateKey(); //生成一个密钥
    			return new SecretKeySpec(secretKey.getEncoded(), KEY_ALGORITHM);// 转换为DESede专用密钥
    		} catch (NoSuchAlgorithmException ex) {
    			Logger.getLogger(TripDESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	public static void main(String[] args) {
    		String content = "hello,您好";
    		String key = "sde@5f98H*^hsff%dfs$r344&df8543*er";
    		System.out.println("content:" + content);
    		String s1 = TripDESUtil.encrypt(content, key);
    		System.out.println("s1:" + s1);
    		System.out.println("s2:"+ TripDESUtil.decrypt(s1, key));
    	}
    }
    ```
  - #### AES加密
    > AES是一种对称加密算法，在 DES 的基础上，使用三重数据加密算法，对数据进行加密，它相当于是对每个数据块应用三次 DES 加密算法。由于计算机运算能力的增强，原版 DES 密码的密钥长度变得容易被暴力破解；3DES 即是设计用来提供一种相对简单的方法，即通过增加 DES 的密钥长度来避免类似的攻击，而不是设计一种全新的块密码算法这样来说，破解的概率就小了很多。缺点由于使用了三重数据加密算法，可能会比较耗性能。
    ```java
    import org.apache.commons.codec.binary.Base64;
    import javax.crypto.Cipher;
    import javax.crypto.KeyGenerator;
    import javax.crypto.SecretKey;
    import javax.crypto.spec.SecretKeySpec;
    import java.security.NoSuchAlgorithmException;
    import java.security.SecureRandom;
    import java.util.logging.Level;
    import java.util.logging.Logger;
    
    public class AESUtil {
    	private static final String KEY_ALGORITHM = "AES";
    	private static final String DEFAULT_CIPHER_ALGORITHM = "AES/ECB/PKCS5Padding";//默认的加密算法
    
    	/**
    	 * AES 加密操作
    	 *
    	 * @param content 待加密内容
    	 * @param key 加密密钥
    	 * @return 返回Base64转码后的加密数据
    	 */
    	public static String encrypt(String content, String key) {
    		try {
    			Cipher cipher = Cipher.getInstance(DEFAULT_CIPHER_ALGORITHM);// 创建密码器
    			byte[] byteContent = content.getBytes("utf-8");
    			cipher.init(Cipher.ENCRYPT_MODE, getSecretKey(key));// 初始化为加密模式的密码器
    			byte[] result = cipher.doFinal(byteContent);// 加密
    			return Base64.encodeBase64String(result);//通过Base64转码返回
    		} catch (Exception ex) {
    			Logger.getLogger(AESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	/**
    	 * AES 解密操作
    	 *
    	 * @param content
    	 * @param key
    	 * @return
    	 */
    	public static String decrypt(String content, String key) {
    		try {
    			//实例化
    			Cipher cipher = Cipher.getInstance(DEFAULT_CIPHER_ALGORITHM);
    			//使用密钥初始化，设置为解密模式
    			cipher.init(Cipher.DECRYPT_MODE, getSecretKey(key));
    			System.out.println(Base64.decodeBase64(content));
    			//执行操作
    			byte[] result = cipher.doFinal(Base64.decodeBase64(content));
    			System.out.println(result);
    			return new String(result, "utf-8");
    		} catch (Exception ex) {
    			Logger.getLogger(AESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    
    		return null;
    	}
    
    	/**
    	 * 生成加密秘钥
    	 *
    	 * @return
    	 */
    	private static SecretKeySpec getSecretKey(final String key) {
    		//返回生成指定算法密钥生成器的 KeyGenerator 对象
    		KeyGenerator kg = null;
    		try {
    			kg = KeyGenerator.getInstance(KEY_ALGORITHM);
    			SecureRandom secureRandom = SecureRandom.getInstance("SHA1PRNG") ;
    			secureRandom.setSeed(key.getBytes());
    			kg.init(128, secureRandom); //AES 要求密钥长度为 128
    			SecretKey secretKey = kg.generateKey(); //生成一个密钥
    			return new SecretKeySpec(secretKey.getEncoded(), KEY_ALGORITHM);// 转换为AES专用密钥
    		} catch (NoSuchAlgorithmException ex) {
    			Logger.getLogger(AESUtil.class.getName()).log(Level.SEVERE, null, ex);
    		}
    		return null;
    	}
    
    	public static void main(String[] args) {
    		String content = "hello,您好";
    		String key = "sde@5f98H*^hsff%dfs$r344&df8543*er";
    		System.out.println("content:" + content);
    		String s1 = AESUtil.encrypt(content, key);
    		System.out.println("s1:" + s1);
    		System.out.println("s2:" + AESUtil.decrypt(s1, key));
    
    	}
    }
    ```
- ### 非对称加密
  > 对极大整数做因数分解的难度决定了RSA算法的可靠性。换言之，对一极大整数做因数分解愈困难，RSA算法愈可靠。假如有人找到一种快速因数分解的算法的话，那么用RSA加密的信息的可靠性就肯定会极度下降。但找到这样的算法的可能性是非常小的。今天只有短的RSA钥匙才可能被强力方式解破。到目前为止，世界上还没有任何可靠的攻击RSA算法的方式。只要其钥匙的长度足够长，用RSA加密的信息实际上是不能被解破的。
  ```java
  import org.apache.commons.codec.binary.Base64;
  import javax.crypto.Cipher;
  import java.security.*;
  import java.security.interfaces.RSAPrivateKey;
  import java.security.interfaces.RSAPublicKey;
  import java.security.spec.PKCS8EncodedKeySpec;
  import java.security.spec.X509EncodedKeySpec;
  import java.util.HashMap;
  import java.util.Map;
  
  public class RSAUtil {
  	//非对称密钥算法
  	public static final String KEY_ALGORITHM = "RSA";
  
  	/**
  	 * 密钥长度，DH算法的默认密钥长度是1024
  	 * 密钥长度必须是64的倍数，在512到65536位之间
  	 */
  	private static final int KEY_SIZE = 512;
  	//公钥
  	private static final String PUBLIC_KEY = "RSAPublicKey";
  
  	//私钥
  	private static final String PRIVATE_KEY = "RSAPrivateKey";
  
  	/**
  	 * 初始化密钥对
  	 *
  	 * @return Map 甲方密钥的Map
  	 */
  	public static Map<String, Object> initKey() throws Exception {
  		//实例化密钥生成器
  		KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance(KEY_ALGORITHM);
  		//初始化密钥生成器
  		keyPairGenerator.initialize(KEY_SIZE);
  		//生成密钥对
  		KeyPair keyPair = keyPairGenerator.generateKeyPair();
  		//甲方公钥
  		RSAPublicKey publicKey = (RSAPublicKey) keyPair.getPublic();
  		//甲方私钥
  		RSAPrivateKey privateKey = (RSAPrivateKey) keyPair.getPrivate();
  		//将密钥存储在map中
  		Map<String, Object> keyMap = new HashMap<String, Object>();
  		keyMap.put(PUBLIC_KEY, publicKey);
  		keyMap.put(PRIVATE_KEY, privateKey);
  		return keyMap;
  	}
  
  
  	/**
  	 * 私钥加密
  	 *
  	 * @param data 待加密数据
  	 * @param key       密钥
  	 * @return byte[] 加密数据
  	 */
  	public static byte[] encryptByPrivateKey(byte[] data, byte[] key) throws Exception {
  		//取得私钥
  		PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(key);
  		KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
  		//生成私钥
  		PrivateKey privateKey = keyFactory.generatePrivate(pkcs8KeySpec);
  		//数据加密
  		Cipher cipher = Cipher.getInstance(keyFactory.getAlgorithm());
  		cipher.init(Cipher.ENCRYPT_MODE, privateKey);
  		return cipher.doFinal(data);
  	}
  
  	/**
  	 * 公钥加密
  	 *
  	 * @param data 待加密数据
  	 * @param key       密钥
  	 * @return byte[] 加密数据
  	 */
  	public static byte[] encryptByPublicKey(byte[] data, byte[] key) throws Exception {
  		//实例化密钥工厂
  		KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
  		//初始化公钥
  		//密钥材料转换
  		X509EncodedKeySpec x509KeySpec = new X509EncodedKeySpec(key);
  		//产生公钥
  		PublicKey pubKey = keyFactory.generatePublic(x509KeySpec);
  
  		//数据加密
  		Cipher cipher = Cipher.getInstance(keyFactory.getAlgorithm());
  		cipher.init(Cipher.ENCRYPT_MODE, pubKey);
  		return cipher.doFinal(data);
  	}
  
  	/**
  	 * 私钥解密
  	 *
  	 * @param data 待解密数据
  	 * @param key  密钥
  	 * @return byte[] 解密数据
  	 */
  	public static byte[] decryptByPrivateKey(byte[] data, byte[] key) throws Exception {
  		//取得私钥
  		PKCS8EncodedKeySpec pkcs8KeySpec = new PKCS8EncodedKeySpec(key);
  		KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
  		//生成私钥
  		PrivateKey privateKey = keyFactory.generatePrivate(pkcs8KeySpec);
  		//数据解密
  		Cipher cipher = Cipher.getInstance(keyFactory.getAlgorithm());
  		cipher.init(Cipher.DECRYPT_MODE, privateKey);
  		return cipher.doFinal(data);
  	}
  
  	/**
  	 * 公钥解密
  	 *
  	 * @param data 待解密数据
  	 * @param key  密钥
  	 * @return byte[] 解密数据
  	 */
  	public static byte[] decryptByPublicKey(byte[] data, byte[] key) throws Exception {
  		//实例化密钥工厂
  		KeyFactory keyFactory = KeyFactory.getInstance(KEY_ALGORITHM);
  		//初始化公钥
  		//密钥材料转换
  		X509EncodedKeySpec x509KeySpec = new X509EncodedKeySpec(key);
  		//产生公钥
  		PublicKey pubKey = keyFactory.generatePublic(x509KeySpec);
  		//数据解密
  		Cipher cipher = Cipher.getInstance(keyFactory.getAlgorithm());
  		cipher.init(Cipher.DECRYPT_MODE, pubKey);
  		return cipher.doFinal(data);
  	}
  
  	/**
  	 * 取得私钥
  	 *
  	 * @param keyMap 密钥map
  	 * @return byte[] 私钥
  	 */
  	public static byte[] getPrivateKey(Map<String, Object> keyMap) {
  		Key key = (Key) keyMap.get(PRIVATE_KEY);
  		return key.getEncoded();
  	}
  
  	/**
  	 * 取得公钥
  	 *
  	 * @param keyMap 密钥map
  	 * @return byte[] 公钥
  	 */
  	public static byte[] getPublicKey(Map<String, Object> keyMap) throws Exception {
  		Key key = (Key) keyMap.get(PUBLIC_KEY);
  		return key.getEncoded();
  	}
  
  	/**
  	 * @param args
  	 * @throws Exception
  	 */
  	public static void main(String[] args) throws Exception {
  		//初始化密钥
  		//生成密钥对
  		Map<String, Object> keyMap = RSAUtil.initKey();
  		//公钥
  		byte[] publicKey = RSAUtil.getPublicKey(keyMap);
  
  		//私钥
  		byte[] privateKey = RSAUtil.getPrivateKey(keyMap);
  		System.out.println("公钥：/n" + Base64.encodeBase64String(publicKey));
  		System.out.println("私钥：/n" + Base64.encodeBase64String(privateKey));
  
  		System.out.println("================密钥对构造完毕,甲方将公钥公布给乙方，开始进行加密数据的传输=============");
  		String str = "RSA密码交换算法";
  		System.out.println("/n===========甲方向乙方发送加密数据==============");
  		System.out.println("原文:" + str);
  		//甲方进行数据的加密
  		byte[] code1 = RSAUtil.encryptByPrivateKey(str.getBytes(), privateKey);
  		System.out.println("加密后的数据：" + Base64.encodeBase64String(code1));
  		System.out.println("===========乙方使用甲方提供的公钥对数据进行解密==============");
  		//乙方进行数据的解密
  		byte[] decode1 = RSAUtil.decryptByPublicKey(code1, publicKey);
  		System.out.println("乙方解密后的数据：" + new String(decode1) + "/n/n");
  
  		System.out.println("===========反向进行操作，乙方向甲方发送数据==============/n/n");
  
  		str = "乙方向甲方发送数据RSA算法";
  
  		System.out.println("原文:" + str);
  
  		//乙方使用公钥对数据进行加密
  		byte[] code2 = RSAUtil.encryptByPublicKey(str.getBytes(), publicKey);
  		System.out.println("===========乙方使用公钥对数据进行加密==============");
  		System.out.println("加密后的数据：" + Base64.encodeBase64String(code2));
  
  		System.out.println("=============乙方将数据传送给甲方======================");
  		System.out.println("===========甲方使用私钥对数据进行解密==============");
  
  		//甲方使用私钥对数据进行解密
  		byte[] decode2 = RSAUtil.decryptByPrivateKey(code2, privateKey);
  
  		System.out.println("甲方解密后的数据：" + new String(decode2));
  	}
  }
  ```