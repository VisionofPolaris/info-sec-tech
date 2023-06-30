# Ch13 数字签名

# 对比

<aside>
🔥 Explain the differences between conventional signatures and digital signatures.

</aside>

1. 包含性：传统的签名包含在文件中，是文件的一部分；数字签名把签名当作一个单独的文件发送。
2. 验证方法：传统的签名接收者需要将文件签名与档案里的签名副本进行比对；数字签名接收者需要使用验证技术来验证信息和签名。
3. 关系：传统签名和文件之间是一对多的关系，数字签名和信息之间是一对一的关系。
4. 二重性：传统签名中，已签文件的副本可以和档案中原始签名相区别，数字签名中除了时间因素以外，在文件上没有这种区别。

# 过程

![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled.png)

1. 数字签名需要有一个公钥系统，签名者用私钥签名，验证者用公钥验证。而在密码系统中发送者使用接收者的公钥加密，接收者使用私钥解密。
2. 长消息用非对称密钥系统效率低，解决的办法是签信息摘要，验证者验证信息摘要。

# 服务

1. 数字签名可以直接提供信息身份验证、信息完整性、不可否认性（运用可信组织）在内的安全服务。
    
    <aside>
    🔥 How to provide nonrepudiation service by using a trusted center and signature?
    
    </aside>
    
    ![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled%201.png)
    
    在发送者和验证者之间建立一个可信的常设的第三方组织，签名者从信息中创建签名，并把这个信息、签名、他的身份、接收方的身份发送给该可信组织。该组织用公钥验证这一信息是来自签名者的，然后将发送方、接收方以及带时间戳的信息副本保存在档案中。再用自己的私钥创建另一个签名，再把信息、新签名、双方的身份发送给验证者。验证者用可信组织的公钥验证信息。如果签名方否认，该中心就可以出示保存的副本，如果与验证方接收到的相同，则不能抵赖。
    
2. 数字签名不提供保密性，如果要求机密性，信息和签名要用密码系统加密/解密。
    
    ![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled%202.png)
    

# 攻击

## 攻击类型

- 唯密钥攻击（key-only attack) - 唯密文攻击
- 已知信息攻击（known-message attack）- 已知明文攻击
- 选择信息攻击（chosen-message attack）- 选择明文攻击

## 伪造类型

- 存在性伪造（Existential Forgery）：合法的信息/签名对，但是内容是计算机随机的，不能被理解。
- 选择性伪造（Selective Forgery）：用攻击者选择的内容，伪造签名。

# RSA数字签名方案

<aside>
🔥 1. Make a distinction between private key and public keys as used in digital signatures and public and private keys as used in a cryptosystem for confidentiality.
2. The scheme of RSA digital signature scheme.

</aside>

RSA可以用于密码系统提供保密性，也可以用于数字签名。数字签名改变了私钥和公钥的作用：

- 使用的是发送者的私钥和公钥，而在密码系统中，使用的是接收者的公钥和私钥；
- 发送者用私钥在文件上签名，接收者用公钥验证这个文件，而在密码系统中，发送者用公钥加密，接收者用私钥解密。

![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled%203.png)

1. 密钥生成：与RSA完全相同。选择两个素数$p$、$q$，计算$n=p \times q$，计算欧拉函数$\phi (n) = (p-1)\times (q-1)$，在$Z_{\phi (n) ^*}$中选择一个数$e$，求乘法逆$d = e^{-1} \mod \phi (n)$。得到公钥$(e,n)$和私钥$d$。
2. 签名和验证
    - 签名：用私钥从信息中签名$S=M^d \mod n$，将信息和签名发给验证者；
    - 验证：接受签名S和消息M，创建消息的副本$M’=S^e \mod n$，再比较M和M‘的值。如果两个值同余，则接收该消息。
        
        证明：$M’\equiv M \mod n \Rightarrow S^e \equiv M \mod n \Rightarrow M^{d\times e} \mod n$。又$d\times e \equiv 1 \mod \phi (n)$，由欧拉定理，同余成立。
        
    
    ![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled%204.png)
    
3. 对信息摘要的RSA签名：签名者用一个双方都同意的hash函数创建摘要$D=h(M)$，再用私钥进行加密$S=D^d \mod n$，将S和M发送给验证方。验证方首先恢复摘要$D’=S^e \mod n$，再将接受的信息通过hash算法$D=h(M)$，最后比较D和D’如果模n同余则接收消息。这时，数字签名的敏感性依赖于哈希算法的强度。
    
    ![Untitled](Ch13%20%E6%95%B0%E5%AD%97%E7%AD%BE%E5%90%8D%20eacd9d67b5fe4cecaacbb189442a2e83/Untitled%205.png)
    
    # 变化
    
    1. 时间戳签名：避免签的文件被对手重放。
    2. 盲签名：文件需要签名但不让签名者知道内容。