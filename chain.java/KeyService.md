# 账户私钥管理 
提供生成私钥、导入助记词、导出私钥、导入私钥、签名等功能。如何存储请参考[账户私钥存储](chain.java/KeyDAO.md)

> 助记词可以推导出私钥， 私钥无法推出助记词。请一定备份助记词！！！

已实现的椭圆曲线算法有：

- secp256k1

```java
import com.glodnet.chain.keys.impl.DefaultKeyServiceImpl;
IKeyService keyService = new DefaultKeyServiceImpl(null); // secp256k1 椭圆曲线算法 null默认为DefaultKeyDAOImpl
```

- sm2 

```java
import com.glodnet.chain.keys.impl.SM2KeyServiceImpl;
IKeyService keyService = new SM2KeyServiceImpl(null); // sm2 椭圆曲线算法 null默认为DefaultKeyDAOImpl
```

## 随机生成私钥

```java
/**
* Creates a new key
*
* @param name     Name of the key
* @param password Password for encrypting the keystore
* @return Bech32 address and mnemonic
*/
Mnemonic mnemonic = keyService.addKey("test", "123456"); // 随机生成账户私钥, 返回助记词、链地址
String mnemonicStr = mnemonic.getMnemonic(); // 助记词, 24个英文字符
String address = mnemonic.getAddress(); // 链地址
```

## 导入助记词

```java
/**
* Recovers a key
*
* @param name         Name of the key
* @param password     Password for encrypting the keystore
* @param mnemonic     Mnemonic of the key
* @param derive       Derive a private key using the default HD path (default: true)
* @param index        The bip44 address index (default: 0)
* @param saltPassword A passphrase for generating the salt, according to bip39
* @return Bech32 address
*/
String mnemonic = "apology false junior asset sphere puppy upset dirt miracle rice horn spell ring vast wrist crisp snake oak give cement pause swallow barely clever";
String address = keyService.recoverKey("test1", "12345678", mnemonic, true, 0, ""); // 组记词恢复账户私钥

```

## 导出私钥keystore文件

```java
/**
* Exports keystore of a key
*
* @param name                 Name of the key
* @param keyPassword          Password of the key
* @param keystorePassword     Password for encrypting the keystore
* @param destinationDirectory Directory for Keystore file to export
* @return Keystore json
*/
String fileName = keyService.exportKeystore("test", "123456", "12345678", new File("/tmp/")); // 导出账户私钥的keystore格式文件
```

## 导入私钥keystore文件

```java
/**
* Imports a key from keystore
*
* @param name             Name of the key
* @param keyPassword      Password of the key
* @param keystorePassword Password for encrypting the keystore
* @param keystore         Keystore json
* @return Bech32 address
*/
String address = keyService.importFromKeystore("test2", "123456", "12345678", "/tmp/UTC--2022-05-19T08-43-03.216801000Z--gnc19wah2874d9tlxz8acqfxahnzpx0n8796mys2l4.key"); // keystore格式文件恢复账户私钥
```

## 私钥签名

```java
/**
* Get privKey by name
*
* @param name     Name of the key
* @param password Password of the key
* @param signdoc  content of the signed
* @return signed
* @throws KeyException If error occurs
*/
String signed = keyService.sign("test2", "123456", "hello");
```

## 删除私钥

```java
/**
* Deletes a key
*
* @param name     Name of the key
* @param password Password of the key
*/
keyService.deleteKey("test2", "123456");
```

## 其他

```java
/**
* Gets address of a key
*
* @param name Name of the key
* @return Bech32 address
* @throws KeyException If error occurs
*/
String showAddress(String name) throws KeyException;

/**
* Gets public key of a key
*
* @param name Name of the key
* @return bytes public key
* @throws KeyException If error occurs
*/
byte[] showPubKey(String name) throws KeyException;

/**
* Get privKey by name
*
* @param name     Name of the key
* @param password Password of the key
* @return {@link Key}
* @throws KeyException If error occurs
*/
Key getKey(String name, String password) throws KeyException;
```

