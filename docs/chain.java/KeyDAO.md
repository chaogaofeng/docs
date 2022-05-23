# 账户私钥存储

为[账户私钥管理](chain.java/KeyService.md)提供存储方式，提供存储、读取、删除及加密、解密私钥的功能。

已实现的存储方式有：

- 内存Map

```java
import com.glodnet.chain.keys.impl.DefaultKeyDAOImpl;
```

## 存储私钥

```java
/**
* Save the encrypted private key to app
*
* @param name Name of the key
* @param key The encrypted private key object
* @throws KeyException if the save fails.
*/
void write(String name, Key key) throws KeyException;
```

## 读取私钥

```java
/**
* Get the encrypted private key by name
*
* @param name Name of the key
* @return The encrypted private key object or null
*/
Key read(String name);
```

## 删除私钥

```java
/**
* Delete the key by name
* @param name Name of the key
* @throws KeyException if the deletion fails.
*/
void delete(String name) throws KeyException;
```

## 加密私钥

```java
/**
* Optional function to encrypt the private key by yourself. Default to AES Encryption
*
* @param privKey The plain private key
* @param password The password to encrypt the private key
* @return The encrypted private key
* @throws KeyException if encrypt failed
*/
default byte[] encrypt(byte[] privKey, String password) throws KeyException;
```

## 解密私钥

```java
/**
* Optional function to decrypt the private key by yourself. Default to AES Decryption
*
* @param encrptedPrivKey The encrpted private key
* @param password The password to decrypt the private key
* @return The plain private key
* @throws KeyException if decrypt failed
*/
byte[] decrypt(byte[] encrptedPrivKey, String password) throws KeyException;
```
