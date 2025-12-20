# Network security#

> 原文：[https://4ed.computer-networking.info/syllabus/default/hosts/security.html](https://4ed.computer-networking.info/syllabus/default/hosts/security.html)

 In the early days, data networks were mainly used by researchers and security was not a concern. A few users were connected and capable of using the network. Almost all the devices attached to the network were openly accessible and users were trusted. As the utilization of the networks grew, security concerns started to appear. In universities, researchers and professors did not always trust their students and required some forms of access control. On standalone computers, the common access control mechanism is the password. A username is assigned to each user and when this user wants to access the computer, he or she needs to provide his/her username and his/her password. Most passwords are composed of a sequence of characters. The strength of the password is function of the difficulty of guessing the characters chosen by each user. Various guidelines have been defined on how to select a good password [[1]](#fpasswords). Some systems require regular modifications of the passwords chosen by their users.

When the first computers were attached to data networks, applications were developed to enable them to access to remote computers through the network. To authenticate the remote users, these applications have also relied on usernames and passwords. When a user connects to a distant computer, she sends her username through the network and then provides her password to confirm her identity. This authentication scheme is presented in the time sequence diagram below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Host A", linecolour=black],
> z [label="", linecolour=white],
> c [label="Host B", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "DATA.req(I'm Alice)\n\n" ] ,
> b>>c [ label = "", arcskip="1"];
> c=>d [ label = "DATA.ind(I'm Alice)\n\n" ];
> 
> d=>c [ label = "DATA.req(Password:)\n\n" ] ,
> c>>b [ label = "", arcskip="1"];
> b=>a [ label = "DATA.ind(Password:)\n\n" ];
> 
> a=>b [ label = "DATA.req(1234xyz$)\n\n" ] ,
> b>>c [ label = "", arcskip="1"];
> c=>d [ label = "DATA.ind(1234xyz$)\n\n" ];
> 
> d=>c [ label = "DATA.req(Access)\n\n" ] ,
> c>>b [ label = "", arcskip="1"];
> b=>a [ label = "DATA.ind(Access)\n\n" ];
> }](../Images/ee7754cb2df3b43180ffc3b27a683d83.png)<map id="155fbd15184cf522dc2c2ef20abe46b4639509c2" name="155fbd15184cf522dc2c2ef20abe46b4639509c2"></map>

Note

Alice and Bob

Alice and Bob are the first names that are used in examples for security techniques. They first appeared in a seminal paper by Diffie and Hellman [[DH1976]](../bibliography.html#dh1976). Since then, Alice and Bob are the most frequently used names to represent the users who interact with a network. Other characters such as Eve or Mallory have been added over the years. We will explain their respective roles later.

## Security threats[#](#security-threats "Link to this heading")

When analyzing security issues in computer networks, it is useful to reason about the capabilities of the attacker who wants to exploit some breach in the security of the network. There are different types of attackers. Some have generic capabilities, others are specific to a given technology or network protocol. In this section, we discuss some important threats that a network architect must take into account.

The first type of attacker is called the passive attacker. A passive attacker is someone able to observe and usually store the information (e.g. the packets) exchanged in a given network or subset of it (e.g. a specific link). This attacker has access to all the data passing through this specific link. This is the most basic type of attacker and many network technologies are vulnerable to such attacks. In the above example, a passive attacker could easily capture the password sent by Alice and reuse it later to be authenticated as Alice on the remote computer. This is illustrated in the figure below where we do not show anymore the `DATA.req` and `DATA.ind` primitives but only the messages exchanged. Throughout this chapter, we will always use Eve as a user who is able to eavesdrop the data passing in front of her.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Eve", linecolour=red],
> d [label="", linecolour=white],
> e [label="Bob", linecolour=black],
> f [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>e [ label = "I'm Alice\n\n", arcskip="1"];
> e=>f [ label = "" ];
> 
> e=>f [ label = "" ] ,
> e>>b [ label = "Password:\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>e [ label = "1234xyz$\n\n", arcskip="1"];
> e=>f [ label = "" ];
> 
> e=>f [ label = "" ] ,
> e>>b [ label = "Access\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/f4b5846cad3728586ed1ef7e20d24afb.png)<map id="17921862b047d560c7fe7db8be3c8ccaa1e34782" name="17921862b047d560c7fe7db8be3c8ccaa1e34782"></map>

In the above example, Eve can capture all the packets exchanged by Bob and Alice. This implies that Eve can discover Alice’s username and Alice’s password. With this information, Eve can then authenticate as Alice on Bob’s computer and do whatever Alice is authorized to do. This is a major problem from a security point of view. To prevent this attack, Alice should never send her password in clear over a network where someone could eavesdrop the information. In some networks, such as an open wireless network, an attacker can easily collect all the data sent by a particular user. In other networks, this is a bit more complex depending on the network technology used, but various software packages exist to automate this process. As will be described later, the best approach to prevent this type of attack is to rely on cryptographic techniques to ensure that passwords are never sent in clear.

Note

Pervasive monitoring

In the previous example, we have explained how Eve could capture data from a particular user. This is not the only attack of this type. In 2013, based on documents collected by Edward Snowden, the press revealed that several governmental agencies were collecting lots of data on various links that compose the global Internet [[Greenwald2014]](../bibliography.html#greenwald2014). Thanks to this massive amount of data, these governmental agencies have been able to extract lots of information about the behavior of Internet users. Like Eve, they are in a position to extract passwords, usernames and other privacy sensitive data from all the packets that they have captured. However, it seems that these agencies were often more interested in various meta data, e.g. information showing with whom a given user communicates than the actual data exchanged. These revelations have shocked the Internet community and the [Internet Engineering Task Force](https://www.ietf.org) that manages the standardization of Internet protocols has declared in [**RFC 7258**](https://datatracker.ietf.org/doc/html/rfc7258.html) that such pervasive monitoring is an attack that need to be countered in the development of new protocols. Several new protocols and extensions to existing ones are being developed to counter these attacks.

Eavesdropping and pervasive monitoring are not the only possible attacks against a network. Another type of attacker is the active attacker. In the literature, these attacks are often called Man in the middle or MITM attacks. Such attacks occur when one user, let us call him Mallory, has managed to configure the network so that he can both capture and modify the packets exchanged by two users. The simplest scenario is when Mallory controls a router that is on the path used by both Alice and Bob. For example, Alice could be connected to a WiFi access router controlled by Mallory and Bob would be a regular server on the Internet.

![Figure made with TikZ](../Images/c7ceeff3d8a02de071eac1377ed85bc7.png)

As Mallory receives all the packets sent by both Bob and Alice, he can modify them at will. For example, he could modify the commands sent by Alice to the server managed by Bob and change the responses sent by the server. This type of attack is very powerful and sometimes difficult to counter without relying on advanced cryptographic techniques.

The last type of attack that we consider in this introduction are the Denial of Service or DoS attacks. During such an attack, the attacker generates enough packets to saturate a given service and prevent it from operating correctly. The simplest Denial of Service attack is to send more packets that the bandwidth of the link that attaches the target to the network. The target could be a single server, a company or even an entire country. If these packets all come from the same source, then the victim can identify the attacker and contact the law enforcement authorities. In practice, such denial of service attacks do not originate from a single source. The attacker usually compromises a (possibly very large) set of sources and forces them to send packets to saturate a given target. Since the attacking traffic comes from a wide range of sources, it is difficult for the victim to locate the culprit and also to counter the attack. Saturating a link is the simplest example of Distributed Denial of Service (DDoS) attacks.

In practice, there is a possibility of denial of service attacks as soon as there is a limited resource somewhere in the network. This resource can be the bandwidth of a link, but it could also be the computational power of a server, its memory or even the size of tables used by a given protocol implementation. Defending against real DoS attacks can be difficult, especially if the attacker controls a large number of sources that are used to launch the attacks. In terms of bandwidth, DoS attacks composed of a few Gbps to a few tens of Gbps of traffic are frequent on the Internet. In 2015, [github.com](http://www.github.com) suffered from a distributed DoS that reached a top bandwidth of 400 Gbps according to some [reports](http://www.techworld.com/news/security/worlds-largest-ddos-attack-reached-400gbps-says-arbor-networks-3595715/).

When designing network protocols and applications that will be deployed on a large scale, it is important to take those DDoS attacks into account. Attackers use different strategies to launch DDoS attacks. Some have managed to gain control of a large number of sources by injecting malware on them. Others, and this is where protocol designers have an important role to play, simply exploit design flaws in some protocols. Consider a simple request-response protocol where the client sends a request and the server replies with a response. Often the response is larger or much larger than the request sent by the client. Consider that such a simple protocol is used over a datagram network. When Alice sends a datagram to Bob containing her request, Bob extracts both the request and Alice’s address from the packet. He then sends his response in a single packet destined to Alice. Mallory would like to create a DoS attack against Alice without being identified. Since he has studied the specification of this protocol, he can send a request to Bob inside a packet having Alice’s address as its source address. Bob will process the request and send his (large) response to Alice. If the response has the same size as the request, Mallory is producing a reflection attack since his packets are reflected by Bob. Alice would think that she is attacked by Bob. If there are many servers that operate the same service as Bob, Mallory could hide behind a large number of such reflectors. Unfortunately, the reflection attack can also become an amplification attack. This happens when the response sent by Bob is larger than the request that it has received. If the response is \(k\) times larger than the request, then when Mallory consumes 1 Gbps of bandwidth to send requests, his victim receives \(k\) Gbps of attack traffic. Such amplification attacks are a very important problem and protocol designers should ensure that they never send a large response before having received the proof that the request that they have received originated from the source indicated in the request.

## Cryptographic primitives[#](#cryptographic-primitives "Link to this heading")

Cryptography techniques have initially been defined and used by spies and armies to exchange secret information in manner that ensures that adversaries cannot decode the information even if they capture the message or the person carrying the message. A wide range of techniques have been defined. The first techniques relied on their secrecy to operate. One of the first encryption schemes is attributed to Julius Caesar. When he sent confidential information to his generals, he would encode each message by replacing each letter with another letter that is \(n\) positions after this letter in the alphabet. For example, the message SECRET becomes VHFUHW when encoded using Caesar’s cipher. This technique could have puzzled some soldiers during Caesar’s wars, but today even young kids can recover the original message from the ciphered one.

The security of the Caesar cipher depends on the confidentiality of the algorithm, but experience has shown that it is impossible to assume that an algorithm will remain secret, even for military applications. Instead, cryptographic techniques must be designed by assuming that the algorithm will be public and known to anyone. However, its behavior must be controlled by a small parameter, known as the key, that will only be known by the users who need to communicate secretly. This principle is attributed to Auguste Kerckhoff, a French cryptographer who first documented it :

> A cryptographic algorithm should be secure even if the attacker knows everything about the system, except one parameter known as the secret key.

This principle is important because it remains the basic assumption of all cryptographers. Any system that relies on the secrecy of its algorithm to be considered secure is doomed to fail and be broken one day.

With the Kerckhoff principle, we can now discuss a simple but powerful encryption scheme that relies on the XOR logic operation. This operation is easily implemented in hardware and is supported by all microprocessors. Given a secret, \(K\), it is possible to encode a message M by computing \(C_M = K \oplus M\). The receiver of this messages can recover the original message as since \(M = K \oplus (K \oplus M)\). This XOR operation is the key operation of the perfect cipher that is also called the Vernam cipher or the one-time pad. This cipher relies on a key that contains purely random bits. The encrypted message is then produced by XORing all the bits of the message with all the bits of the key. Since the key is random, it is impossible for an attacker to recover the original text (or plain text) from the encrypted one. From a security viewpoint, the one-time-pad is the best solution provided that the key is as long as the message.

Unfortunately, it is difficult to use this cipher in practice since the key must be as long as the message that needs to be transmitted. If the key is smaller than the message and the message is divided into blocks that have the same length as the key, then the scheme becomes less secure since the same key is used to decrypt different parts of the message. In practice, XOR is often one of the basic operations used by encryption schemes. To be usable, the deployed encryption schemes use keys that are composed of a small number of bits, typically 56, 64, 128, 256, …

A secret key encryption scheme is a perfectly reversible functions, i.e. given an encryption function E, there is an associated decryption function D such that \(\forall K, \forall M : D(K, E(K,M))=M\).

Various secret key cryptographic functions have been proposed, implemented and deployed. The most popular ones are :

> *   DES, the Data Encryption Standard that became a standard in 1977 and has been widely used by industry. It uses 56 bits keys that are not considered sufficiently secure nowadays since attackers can launch brute-force attacks by testing all possible keys. Triple DES combines three 56 bits keys, making the brute force attacks more difficult.
>     
>     
> *   RC4 is an encryption scheme defined in the late 1980s by Ron Rivest for RSA Security. Given the speed of its software implementation, it has been included in various protocols and implementations. However, cryptographers have identified several weaknesses in this algorithm. It is now deprecated and should not be used anymore [**RFC 7465**](https://datatracker.ietf.org/doc/html/rfc7465.html).
>     
>     
> *   AES or the Advanced Encryption Standard is an encryption scheme that was designed by the Belgian cryptographers Joan Daemen and Vincent Rijmen in 2001 [[DR2002]](../bibliography.html#dr2002). This algorithm has been standardized by the U.S. National Institute of Standards and Technology (NIST). It is now used by a wide range of applications and various hardware and software implementations exist. Many microprocessors include special instructions that ease the implementation of AES. AES divides the message to be encrypted in blocks of 128 bits and uses keys of length 128, 192 or 256 bits. The block size and the key length are important parameters of an encryption scheme. The block size indicates the smallest message that can be encrypted and forces the sender to divide each message in blocks of the supported size. If the message is larger than an integer number of blocks, then the message must be padded before being encrypted and this padding must be removed after decryption. The key size indicates the resistance of the encryption scheme against brute force attacks, i.e. attacks where the attacker tries all possible keys to find the correct one.

AES is widely used as of this writing, but other secret key encryption schemes continue to appear. ChaCha20, proposed by D. Bernstein is now used by several internet protocols [**RFC 7539**](https://datatracker.ietf.org/doc/html/rfc7539.html). A detailed discussion of encryption schemes is outside the scope of this book. We will consider encryption schemes as black boxes whose operation depends on a single key. A detailed overview of several of these schemes may be found in [[MVV2011]](../bibliography.html#mvv2011).

In the 1970s, Diffie and Hellman proposed in their seminal paper [[DH1976]](../bibliography.html#dh1976), a different type of encryption : public key cryptography. In public key cryptography, each user has two different keys :

> *   a public key (\(K_{pub}\)) that he can distribute to everyone
>     
>     
> *   a private key (\(K_{priv}\)) that he needs to store in a secure manner and never reveal to anyone

These two keys are generated together and they are linked by a complex mathematical relationship that is such that it is computationally difficult to compute \(K_{priv}\) from \(K_{pub}\).

A public key cryptographic scheme is a combination of two functions :

> *   an encryption function, \(E_{p}\), that takes a key and a message as parameters
>     
>     
> *   a decryption function, \(D_{p}\), that takes a key and a message as parameters

The public key is used to encrypt a message so that it can only be read by the intended recipient. For example, let us consider two users : Alice and Bob. Alice (resp. Bob) uses the keys \(A_{priv}\) and \(A_{pub}\) (resp. \(B_{priv}\) and \(B_{pub}\)). To send a secure message M to Alice, Bob computes \(CM=E_p(A_{pub},M)\) and Alice can decrypt it by using \(D_p(A_{priv},CM)=D_p(A_{priv},E_p(A_{pub},M))=M\).

Several public key encryption schemes have been proposed. Two of them have reached wide deployment :

> *   The Rivest Shamir Adleman (RSA) algorithm [[2]](#frsa) proposed in [[RSA1978]](../bibliography.html#rsa1978) that relies on modular exponentiation with large integers.
>     
>     
> *   The Elliptic Curve Cryptography techniques [[3]](#fecc) that rely on special properties of elliptic curves.

Another interesting property of public key cryptography is its ability to compute signatures that can be used to authenticate a message. This capability comes from the utilization of two different keys that are linked together. If Alice wants to sign a message M, she can compute \(SM=E_p(A_{priv},M)\). Anyone who receives this signed messaged can extract its content as \(D_p(A_{pub},SM)=D_p(A_{pub},E_p(A_{priv},M))=M\). Everyone can use \(A_{pub}\) to check that the message was signed by using Alice’s private key (\(A_{priv}\)). Since this key is only known by Alice, the ability to decrypt SM is a proof that the message was signed by Alice herself.

In practice, encrypting a message to sign it can be computationally costly, in particular if the message is a large file. A faster solution would be to summarize the document and only sign the summary of the document. A naive approach could be based on a checksum or CRC computed over the message. Alice would then compute \(C=Checksum(M)\) and \(SC=E_p(A_{priv},C)\). She would then send both M and SC to the recipient of the message who can easily compute C from SC and verify the authenticity of the message. Unfortunately, this solution does not protect Alice and the message’s recipient against a man-in-the-middle attack. If Mallory can intercept the message sent by Alice, he can easily modify Alice’s message and tweak it so that it has the same checksum as the original one. The CRCs, although more complex to compute, suffer from the same problem.

To efficiently sign messages, Alice needs to be able to compute a summary of her message in a way that makes prohibits an attacker from generating a different message that has the same summary. Cryptographic hash functions were designed to solve this problem. The ideal hash function is a function that returns a different number for every possible input. In practice, it is impossible to find such a function. Cryptographic hash functions are an approximation of this perfect summarization function. They compute a summary of a given message in 128, 160, 256 bits or more. They also exhibit the avalanche effect. This effect indicates that a small change in the message causes a large change in the hash value. Finally hash functions are very difficult to invert. Knowing a hash value, it is computationally very difficult to find the corresponding input message. Several hash functions have been proposed by cryptographers. The most popular ones are :

> *   MD5, originally proposed in [**RFC 1321**](https://datatracker.ietf.org/doc/html/rfc1321.html). It has been used in a wide range of applications. In 2010, attacks against MD5 were published and this hash function is now deprecated.
>     
>     
> *   SHA-1 is a cryptographic hash function that was standardized by the NIST in 1995\. It outputs 160 bits results. It is now used in a variety of network protocols.
>     
>     
> *   SHA-2 is another family of cryptographic hash functions designed by the NIST. Different variants of SHA-2 can produce has values of 224, 256, 384 or 512 bits.

Another important point about cryptographic algorithms is that often these algorithms require random numbers to operate correctly (e.g. to generate keys). Generating good random numbers is difficult and any implementation of cryptographic algorithms should also include a secure random number generator. [**RFC 4086**](https://datatracker.ietf.org/doc/html/rfc4086.html) provides useful recommendations.

## Cryptographic protocols[#](#cryptographic-protocols "Link to this heading")

We can now combine the cryptographic operations described in the previous section to build some protocols to securely exchange information. Let us first go back to the problem of authenticating Alice on Bob’s computer. We have shown earlier that using a simple password for this purpose is insecure in the presence of attackers.

A naive approach would be to rely on hash functions. Since hash functions are non-invertible, Alice and Bob could decide to use them to exchange Alice’s password in a secure manner. Then, Alice could be authenticated by using the following exchange.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Prove it\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Hash(passwd)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access granted\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/8ce774d886954a61fe3332c085b81a4f.png)<map id="e9d4cd4a36e66a6415dde6e83ecc95535122af78" name="e9d4cd4a36e66a6415dde6e83ecc95535122af78"></map>

Since the hash function cannot be inverted, an eavesdropper cannot extract Alice’s password by simply observing the data exchanged. However, Alice’s real password is not the objective of an attacker. The main objective for Mallory is to be authenticated as Alice. If Mallory can capture Hash(passwd), he can simply replay this data, without being able to invert the hash function. This is called a replay attack.

To counter this replay attack, we need to ensure that Alice never sends the same information twice to Bob. A possible mode of operation is shown below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:764192\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Hash(764192||passwd)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/d68b2f1363c49e52d5bce558f315b1ad.png)<map id="20f577439fc03098f4163306e3e3d06fd0e01a3f" name="20f577439fc03098f4163306e3e3d06fd0e01a3f"></map>

To authenticate herself, Alice sends her user identifier to Bob. Bob replies with a random number as a challenge to verify that Alice knows the shared secret (i.e. her password). Alice replies with the result of the computation of a hash function (e.g. SHA-1) over a string that is the concatenation between the random number chosen by Bob and Alice’s password. The random number chosen by Bob is often called a nonce since this is a number that should only be used once. Bob performs the same computation locally and can check the message returned by Alice. This type of authentication scheme has been used in various protocols. It prevents replay attacks. If Eve captures the messages exchanged by Alice and Bob, she cannot recover Alice’s password from the messages exchanged since hash functions are non-invertible. Furthermore, she cannot replay the hashed value since Bob will always send a different nonce.

Unfortunately, this solution forces Bob to store Alice’s password in clear. Any breach in the security of Bob’s computer would reveal Alice’s password. Such breaches unfortunately occur and some of them have led to the dissemination of millions of passwords.

A better approach would be to authenticate Alice without storing her password in clear on Bob’s computer. For this, Alice computes a hash chain as proposed by Lamport in [[Lamport1981]](../bibliography.html#lamport1981). A hash chain is a sequence of applications of a hash function (H) on an input string. If Alice’s password is P, then her 10 steps hash chain is : \(H(H(H(H(H(H(H(H(H(H(P))))))))))\). The result of this hash chain will be stored on Bob’s computer together with the value 10. This number is the maximum number of remaining authentications for Alice on Bob’s computer. To authenticate Alice, Bob sends the remaining number of authentications, i.e. 10 in this example. Since Alice knows her password, P, she can compute \(H^9(P)=H(H(H(H(H(H(H(H(H(P)))))))))\) and send this information to Bob. Bob computes the hash of the value received from Alice (\(H(H^9(P))\)) and verifies that this value is equal to the value stored in his database. It then decrements the number of authorized authentications and stores \(H^9(P)\) in his database. Bob is now ready for the next authentication of Alice. When the number of authorized authentications reaches zero, the hash chain needs to be reinitialized. If Eve captures \((H^n(P))\), she cannot use it to authenticate herself as Alice on Bob’s computer because Bob will have decremented its number of authorized authentications. Furthermore, given that hash functions are not invertible, Eve cannot compute \(H^{n-1}(P)\) from \(H^{n}(P)\).

The two protocols above prevent eavesdropping attacks, but not man-in-the-middle attacks. If Mallory can intercept the messages sent by Alice, he could force her to reveal \(H^n(P)\) and then use this information to authenticate as Alice on Bob’s computer. In practice, hash chains should only be used when the communicating users know that there cannot be any man-in-the-middle on their communication.

Public key cryptography provides another possibility to allow Alice to authenticate herself on Bob’s computer. Assume again that Alice and Bob know each other from previous encounters. Alice knows Bob’s public key (\(Bob_{pub}\)) and Bob also knows Alice’s key (\(Alice_{pub}\)). To authenticate herself, Alice could send her user identifier. Bob would reply with a random number encrypted with Alice’s public key : \(E_p(Alice_{pub},R)\). Alice can decrypt this message to recover R and sends \(E_p(Bob_{pub},R)\). Bob decrypts the nonce and confirms that Alice knows \(Alice_{priv}\). If an eavesdropper captures the messages exchanged, he cannot recover the value R which could be used as a key to encrypt the information with a secret key algorithm. This is illustrated in the time sequence diagram below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "E_p(Alice_{pub},R)", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Bob_{pub},R)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/f7eeac697813279f3277f20eb023de49.png)<map id="6027ccdd0a4dba418aa8843c8ac153fd6a422bd7" name="6027ccdd0a4dba418aa8843c8ac153fd6a422bd7"></map>

A drawback of this approach is that Bob is forced to perform two public key computations : one encryption to send the random nonce to Alice and one decryption to recover the nonce encrypted by Alice. If these computations are costly from a CPU viewpoint, this creates a risk of Denial of Service Attacks were attackers could try to access Bob’s computer and force it to perform such costly computations. Bob is more at risk than Alice in this situation and he should not perform complex operations before being sure that he is talking with Alice. An alternative is shown in the time sequence diagram below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/5d0252030a1e74f0aa10edc4572b381d.png)<map id="26127d2d1ccdbf50c7d1c6d967ea621292da9631" name="26127d2d1ccdbf50c7d1c6d967ea621292da9631"></map>

Here, Bob simply sends a random nonce to Alice and verifies her signature. Since the random nonce and the signature could be captured by an eavesdropper, they cannot be used as a secret key to encrypt further data. However Bob could propose a secret key and send it encrypted with Alice’s public key in response to the signed nonce that he received.

The solution described above works provided that Bob and Alice know their respective public keys before communicating. Otherwise, the protocol is not secure against man-in-the-middle attackers. Consider Mallory sitting in the middle between Alice and Bob and assume that neither Alice nor Bob knows the other’s public key.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> x [label="", linecolour=white],
> y [label="Mallory", textcolour="red", linecolour=red],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = " " ] ,
> b>>y [ label = "I'm Alice key=Alice_{pub}", arcskip="1" ];
> y>>c [ label = "I'm Alice key=Mallory_{pub}", textcolour="red", arcskip="1" ];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>y [ label = "E_p(Alice_{priv},R)", arcskip="1"];
> y>>c [ label = "E_p(Mallory_{priv},R)", textcolour="red",  arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/0638f4c9db9fea46091250d1fe48d4c9.png)<map id="1ee9ab4ade7fb05cf8774418033d3c5fb2944924" name="1ee9ab4ade7fb05cf8774418033d3c5fb2944924"></map>

In the above example, Alice sends her public key, (\(Alice_{pub}\)), in her first message together with her identity. Mallory intercepts the message and replaces Alice’s key with his own key, (\(Mallory_{pub}\)). Bob replies with a nonce, R. Alice then signs the random nonce to prove that she knows \(Alice_{priv}\). Mallory discards the information and instead computes \(E_p(Mallory_{priv},R)\). Bob now thinks that he is discussing with Alice while Mallory sits in the middle.

There are situations where symmetric authentication is required. In this case, each user must perform some computation with his/her private key. A possible exchange is the following. Alice sends her certificate to Bob. Bob replies with a nonce, \(R1\), and provides his certificate. Alice encrypts \(R1\) with her private key and generates a nonce, \(R2\). Bob verifies Alice’s computation and encrypts \(R2\) with his private key. Alice verifies the computation and both have been authenticated.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:R1", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R1),R2", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "E_p(Bob_{priv},R2)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/682391126620ce67dffa1f33ee972f1b.png)<map id="dc928c892b0c31362bbfde7cc4e2025e18374729" name="dc928c892b0c31362bbfde7cc4e2025e18374729"></map>

The protocol described above works, but it takes a long time for Bob to authenticate Alice and for Alice to authenticate Bob. A faster authentication could be the following.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice, R2", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:R1,E_p(Bob_{priv},R2)", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R1)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/d9c1e952550357ddf872d47493c46bd4.png)<map id="b006024cf4bbd5812ac6af177e9c613098b52e42" name="b006024cf4bbd5812ac6af177e9c613098b52e42"></map>

Alice sends her random nonce, \(R2\). Bob signs \(R2\) and sends his nonce : \(R1\). Alice signs \(R1\) and both are authenticated.

Now consider that Mallory wants to be authenticated as Alice. The above protocol has a subtle flaw that could be exploited by Mallory. This flaw can be exploited if Alice and Bob can act as both client and server. Knowing this, Mallory could operate as follows. Mallory starts an authentication with Bob faking himself as Alice. He sends a first message to Bob including Alice’s identity.

> ![msc {
> a [label="", linecolour=white],
> b [label="Mallory", textcolour="red", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice,RA", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:RB,E_p(Bob_{priv},RA)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/0129748f31a23c4c1ec10d426878fd81.png)<map id="b30892ea80e85d5226bffe1067bd495dabafda9e" name="b30892ea80e85d5226bffe1067bd495dabafda9e"></map>

In this exchange, Bob authenticates himself by signing the \(RA\) nonce that was sent by Mallory. Now, to authenticate as Alice, Mallory needs to compute the signature of nonce \(RB\) with Alice’s private key. Mallory does not know Alice’s key, but he could exploit the protocol to force Alice to perform the required computation. For this, Mallory can start an authentication to Alice as shown below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Mallory", textcolour="red",linecolour=black],
> z [label="", linecolour=white],
> c [label="Alice", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Mallory,RB", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:RX,E_p(Alice_{priv},RB)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/cd66745b390b62a38dfe355eb3a5cc12.png)<map id="48be047a9645ab6feb54e3f2716ac4f1e148d8e7" name="48be047a9645ab6feb54e3f2716ac4f1e148d8e7"></map>

In this example, Mallory has forced Alice to compute \(E_p(Alice_{priv},RB)\) which is the information required to finalize the first exchange and be authenticated as Alice. This illustrates a common problem with authentication schemes when the same information can be used for different purposes. The problem comes from the fact that Alice agrees to compute her signature on a nonce chosen by Bob (and relayed by Mallory). This problem occurs if the nonce is a simple integer without any structure. If the nonce includes some structure such as some information about Alice and Bob’s identities or even a single bit indicating whether the nonce was chosen by a user acting as a client (i.e. starting the authentication) or as a server, then the protocol is not vulnerable anymore.

To cope with some of the above mentioned problems, public-key cryptography is usually combined with certificates. A certificate is a data structure that includes a signature from a trusted third party. A simple explanation of the utilization of certificates is to consider that Alice and Bob both know Ted. Ted is trusted by these two users and both have stored Ted’s public key : \(Ted_{pub}\). Since they both know Ted’s key, he can issue certificates. A certificate is mainly a cryptographic link between the identity of a user and his/her public key. Such a certificate can be computed in different ways. A simple solution is for Ted to generate a file that contains the following information for each certified user :

> *   his/her identity
>     
>     
> *   his/her public key
>     
>     
> *   a hash of the entire file signed with Ted’s private key

Then, knowing Ted’s public key, anyone can verify the validity of a certificate. When a user sends his/her public key, he/she must also attach the certificate to prove the link between his/her identity and the public key. In practice, certificates are more complex than this. Certificates will often be used to authenticate the server and sometimes to authenticate the client.

A possible protocol could then be the following. Alice sends \(Cert(Alice_{pub},Ted)\). Bob replies with a random nonce.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Cert(Alice_{pub},Ted)", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = ""] ,
> b>>c [ label = "E_p(Alice_{priv},R)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/64eccedf6350698b6491b83ea9b24e73.png)<map id="3b6facf99117568ccbf6aa8408c70dddc156d855" name="3b6facf99117568ccbf6aa8408c70dddc156d855"></map>

Until now, we have only discussed the authentication problem. This is an important but not sufficient step to have a secure communication between two users through an insecure network. To securely exchange information, Alice and Bob need to both :

> *   mutually authenticate each other
>     
>     
> *   agree on a way to encrypt the messages that they will exchange

Let us first explore how this could be realized by using public-key cryptography. We assume that Alice and Bob have both a public-private key pair and the corresponding certificates signed by a trusted third party : Ted.

A possible protocol would be the following. Alice sends \(Cert(Alice_{pub},Ted)\). This certificate provides Alice’s identity and her public key. Bob replies with the certificate containing his own public key : \(Cert(Bob_{pub},Ted)\). At this point, they both know the other public key and could use it to send encrypted messages. Alice would send \(E_p(Bob_{pub},M1)\) and Bob would send \(E_p(Alice_{pub},M2)\). In practice, using public key encryption techniques to encrypt a large number of messages is inefficient because these cryptosystems require a large number of computations. It is more efficient to use secret key cryptosystems for most of the data and only use a public key cryptosystem to encrypt the random secret keys that will be used by the secret key encryption scheme.

## Key exchange[#](#key-exchange "Link to this heading")

When users want to communicate securely through a network, they need to exchange information such as the keys that will be used by an encryption algorithm even in the presence of an eavesdropper. The most widely used algorithm that allows two users to safely exchange an integer in the presence of an eavesdropper is the one proposed by Diffie and Hellman [[DH1976]](../bibliography.html#dh1976). It operates with (large) integers. Two of them are public, the modulus, p, which is prime and the base, g, which must be a primitive root of p. The communicating users select a random integer, \(a\) for Alice and \(b\) for Bob. The exchange starts as :

> *   Alice selects a random integer, \(a\) and sends \(A=g^{a} \mod p\) to Bob
>     
>     
> *   Bob selects a random integer, \(b\) and sends \(B=g^{b} \mod p\) to Alice
>     
>     
> *   From her knowledge of \(a\) and \(B\), Alice can compute \(Secret=B^{a} \mod p= (g^{b} \mod p) ^{a} \mod p=g^{a \times b} \mod p\)
>     
>     
> *   From is knowledge of \(b\) and \(A\), Bob can compute \(Secret=A^{b} \mod p=(g^{a} \mod p) ^{b} \mod p=g^{a \times b} \mod p\)

The security of this protocol relies on the difficulty of computing discrete logarithms, i.e. from the knowledge of \(A\) (resp. \(B\)), it is very difficult to extract \(\log(A)=\log(g^{a} \mod p)=a\) (resp. \(\log(B)=\log(g^{b} \mod p)=b\)).

An example of the utilization of the Diffie-Hellman key exchange is shown below. Before starting the exchange, Alice and Bob agree on a modulus (\(p=23\)) and a base (\(g=5\)). These two numbers are public. They are typically part of the standard that defines the protocol that uses the key exchange.

> *   Alice chooses a secret integer : \(a=8\) and sends \(A= g^{a} \mod p= 5^{8} \mod 23=16\) to Bob
>     
>     
> *   Bob chooses a secret integer : \(b=13\) and sends \(B= g^{b} \mod p=5^{13} \mod 23=21\) to Alice
>     
>     
> *   Alice computes \(S_{A}=B^{a} \mod p= 21^{8} \mod 23=3\)
>     
>     
> *   Bob computes \(S_{B}=A^{b} \mod p= 16^{13} \mod 23=3\)

Alice and Bob have agreed on the secret information \(3\) without having sent it explicitly through the network. If the integers used are large enough and have good properties, then even Eve who can capture all the messages sent by Alice and Bob cannot recover the secret key that they have exchanged. There is no formal proof of the security of the algorithm, but mathematicians have tried to solve similar problems with integers during centuries without finding an efficient algorithm. As long as the integers that are used are random and large enough, the only possible attack for Eve is to test all possible integers that could have been chosen by Alice and Bob. This is computationally very expensive. This algorithm is widely used in security protocols to agree on a secret key.

Unfortunately, the Diffie-Hellman key exchange alone cannot cope with man-in-the middle attacks. Consider Mallory who sits in the middle between Alice and Bob and can easily capture and modify their messages. The modulus and the base are public. They are thus known by Mallory as well. He could then operate as follows :

> *   Alice chooses a secret integer and sends \(A= g^{a} \mod p\) to Mallory
>     
>     
> *   Mallory generates a secret integer, \(m\) and sends \(M=g^{m} \mod p\) to Bob
>     
>     
> *   Bob chooses a secret integer and sends \(B=g^{b} \mod p\) to Mallory
>     
>     
> *   Mallory computes \(S_{A}=A^{m} \mod p\) and \(S_{B}=B^{m} \mod p\)
>     
>     
> *   Alice computes \(S_{A}=M^{a} \mod p\) and uses this key to communicate with Mallory (acting as Bob)
>     
>     
> *   Bob computes \(S_{B}=M^{b} \mod p\) and uses this key to communicate with Mallory (acting as Alice)

When Alice sends a message, she encrypts it with \(S_{A}\). Mallory decrypts it with \(S_{A}\) and encrypts the plaintext with \(S_{B}\). When Bob receives the message, he can decrypt it by using \(S_{B}\).

To safely use the Diffie-Hellman key exchange, Alice and Bob must use an authenticated exchange. Some of the information sent by Alice or Bob must be signed with a public key known by the other user. In practice, it is often important for Alice to authenticate Bob. If Bob has a certificate signed by Ted, the authenticated key exchange could be organized as follows.

> *   Alice chooses a secret integer : \(a\) and sends \(A= g^{a} \mod p\) to Bob
>     
>     
> *   Bob chooses a secret integer : \(b\), computes \(B= g^{b} \mod p\) and sends \(Cert(Bob,Bob_{pub},Ted), E_p(Bob_{priv},B)\) to Alice
>     
>     
> *   Alice checks the signature (with \(Bob_{pub}\)) and the certificate and computes \(S_{A}=B^{a} \mod p\)
>     
>     
> *   Bob computes \(S_{B}=A^{b} \mod p\)

This prevents the attack mentioned above since Mallory cannot create a fake certificate and cannot sign a value by using Bob’s private key. Given the risk of man-in-the-middle attacks, the Diffie-Hellman key exchange mechanism should never be used without authentication.

 ## The secure shell (ssh)[#](#the-secure-shell-ssh "Link to this heading")

One of the initial motivations for building computer networks was to allow users to access remote computers over the networks. In the 1960s and 1970s, the mainframes and the emerging minicomputers were composed of a central unit and a set of terminals connected through serial lines or modems. The simplest protocol that was designed to access remote computers over a network is probably [telnet](../glossary.html#term-telnet) [**RFC 854**](https://datatracker.ietf.org/doc/html/rfc854.html). [telnet](../glossary.html#term-telnet) runs over TCP and a telnet server listens on port 23 by default. The TCP connection used by telnet is bidirectional, both the client and the server can send data over it. The data exchanged over such a connection is essentially the characters that are typed by the user on the client machine and the text output of the processes running on the server machine with a few exceptions (e.g. control characters, characters to control the terminal like VT-100, …) . The default character set for telnet is the ASCII character set, but the extensions specified in [**RFC 5198**](https://datatracker.ietf.org/doc/html/rfc5198.html) support the utilization of Unicode characters.

From a security viewpoint, the main drawback of [telnet](../glossary.html#term-telnet) is that all the information, including the usernames, passwords and commands, is sent in cleartext over a TCP connection. This implies that an eavesdropper could easily capture the passwords used by anyone on an unprotected network. Various software tools exist to automate this collection of information. For this reason, [telnet](../glossary.html#term-telnet) is rarely used today to access remote computers. It is usually replaced by [ssh](../glossary.html#term-ssh) or similar protocols.

The secure shell protocol was designed in the mid 1990s by T. Ylonen to counter the eavesdropping attacks against [telnet](../glossary.html#term-telnet) and similar protocols [[Ylonen1996]](../bibliography.html#ylonen1996). [ssh](../glossary.html#term-ssh) became quickly popular and system administrators encouraged its usage. The original version of [ssh](../glossary.html#term-ssh) was freely available. After a few years, his author created a company to distribute it commercially, but other programmers continued to develop an open-source version of [ssh](../glossary.html#term-ssh) called [OpenSSH](http://www.openssh.com). Over the years, [ssh](../glossary.html#term-ssh) evolved and became a flexible applicable whose usage extends beyond remote login to support features such as file transfers, protocol tunneling, … In this section, we only discuss the basic features of [ssh](../glossary.html#term-ssh) and explain how it differs from [telnet](../glossary.html#term-telnet). Entire books have been written to describe [ssh](../glossary.html#term-ssh) in details [[BS2005]](../bibliography.html#bs2005). An overview of the protocol appeared in [[Stallings2009]](../bibliography.html#stallings2009).

The [ssh](../glossary.html#term-ssh) protocol runs directly above the TCP protocol. Once the TCP bytestream has been established, the client and the server exchange messages. The first message exchanged is an ASCII line that announces the version of the protocol and the version of the software implementation used by the client and the server. These two lines are useful when debugging interoperability problems and other issues.

The next message is the `SSH_MSG_KEX_INIT` message that is used to negotiate the cryptographic algorithms that will be used for the `ssh` session. It is very important for security protocols to include mechanisms that enable a negotiation of the cryptographic algorithms that are used. First, these algorithms provide different levels of security. Some algorithms might be considered totally secure and are recommended today while they could become deprecated a few years later after the publication of some attacks. Second, these algorithms provide different levels of performance and have different CPU and memory impacts.

In practice, an `ssh` implementation supports four types of cryptographic algorithms :

> *   key exchange
>     
>     
> *   encryption
>     
>     
> *   Message Authentication Code (MAC)
>     
>     
> *   compression

The [IANA](https://www.iana.org) maintains a [list of the cryptographic algorithms](http://www.iana.org/assignments/ssh-parameters/ssh-parameters.xhtml#ssh-parameters-16) that can be used by `ssh` implementations. For each type of algorithm, the client provides an ordered list of the algorithms that it supports and agrees to use. The server compares the received list with its own list. The outcome of the negotiation is a set of four algorithms [[4]](#fnull) that will be combined for this session.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "SSH-clientP-clientS comments", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "SSH-serverP-serverS comments", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "SSH_MSG_KEX_INIT", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "SSH_MSG_KEX_INIT", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/6c5012af9e2d96b7ddb2c31a55acbaec.png)<map id="6ae6d4bce6070be0fd3cb65a88d797c93f775d1d" name="6ae6d4bce6070be0fd3cb65a88d797c93f775d1d"></map>

This negotiation of the cryptographic algorithms allows the implementations to evolve when new algorithms are proposed. If a client is upgraded, it can announce a new algorithm as its preferred one even if the server is not yet upgraded.

Once the cryptographic algorithms have been negotiated, the key exchange algorithm is used to negotiate a secret key that will be shared by the client and the server. These key exchange algorithms include some variations over the basic algorithms. As an example, let us analyze how the Diffie-Hellman key exchange algorithm is used within the `ssh` protocol. In this case, each host has both a private and a public key. (Note that \(g\) is a generator for the subgroup of the Galois field of order \(p\), where \(p\) is a prime number, and || is the concatenation operator. For additional background information, see [[Schneier1996]](../bibliography.html#schneier1996).)

> *   the client generates the random number \(a\) and sends \(A=g^{a} \mod p\) to the server
>     
>     
> *   the server generates the random number \(b\). It then computes \(B=g^{b} \mod p\), \(K=B^{a} \mod p\) and signs with its private key \(hash(V_{Client} || V_{Server} || KEX\_INIT_{Client} || KEX\_INIT_{Server} || Server_{pub} || A || B || K )\) where \(V_{Server}\) (resp. \(V_{Client}\)) is the initial messages sent by the client (resp. server), \(KEX\_INIT_{Client}\) (resp. \(KEX\_INIT_{Server}\)) is the key exchange message sent by the client (resp. server) and \(A\), \(B\) and \(K\) are the messages of the Diffie-Hellman key exchange
>     
>     
> *   the client can recompute \(K=A^{b} \mod p\) and verify the signature provided by the server

This is a slightly modified authenticated Diffie-Hellman key exchange with two interesting points. The first point is that when the server authenticates the key exchange it does not provide a certificate. This is because `ssh` assumes that the client will store inside its cache the public key of the servers that it uses on a regular basis. This assumption is valid for a protocol like `ssh` because users typically use it to interact with a small number of servers, typically a few or a few tens. Storing this information does not require a lot of storage. In practice, most `ssh` clients will accept to connect to remote servers without knowing their public key before the connection. In this case, the client issues a warning to the user who can decide to accept or reject the key. This warning can be associated with a fingerprint of the key, either as a sequence of letters or as an ASCII art which can be posted on the web or elsewhere [[5]](#fdnsssh) by the system administrator of the server. If a client connects to a server whose public key does not match the stored one, a stronger warning is issued because this could indicate a man-in-the-middle attack or that the remote server has been compromised. It can also indicate that the server has been upgraded and that a new key has been generated during this upgrade.

The second point is that the server authenticates not only the result of the Diffie-Hellman exchange but also a hash of all the information sent and received during the exchange. This is important to prevent downgrade attacks. A downgrade attack is an attack where an active attacker modifies the messages sent by the communicating hosts (typically the client) to request the utilization of weaker encryption algorithms. Consider a client that supports two encryption schemes. The preferred one uses 128 bits secret keys and the second one is an old encryption scheme that uses 48 bits keys. This second algorithm is kept for backward compatibility with older implementations. If an attacker can remove the preferred algorithm from the list of encryption algorithms supported by the client, he can force the server to use a weaker encryption scheme that will be easier to break. Thanks to the hash that covers all the messages exchanged by the server, the downgrade attack cannot occur against `ssh`. Algorithm agility is a key requirement for security protocols that need to evolve when encryption algorithms are broken by researchers. This agility cannot be used without care and signing a hash of all the messages exchanged is a technique that is frequently used to prevent downgrade attacks.

Note

Single use keys

Thanks to the Diffie-Hellman key exchange, the client and the servers share key \(K\). A naive implementation would probably directly use this key for all the cryptographic algorithms that have been negotiated for this session. Like most security protocols, `ssh` does not directly use key \(K\). Instead, it uses the negotiated hash function with different parameters [[6]](#fsshkeys) to allow the client and the servers to compute six keys from \(K\) :

> *   a key used by the client (resp. server) to encrypt the data that it sends
>     
>     
> *   a key used by the client (resp. server) to authenticate the data that it sends
>     
>     
> *   a key used by the client (resp. server) to initialize the negotiated encryption scheme (if required by this scheme)

It is common practice among designers of security protocols to never use the same key for different purposes. For example, allowing the client and the server to use the same key to encrypt data could enable an attacker to launch a replay attack by sending to the client data that it has itself encrypted.

At this point, all the messages sent over the TCP connection will be encrypted with the negotiated keys. The `ssh` protocol uses messages that are encoded according to the Binary Packet Protocol defined in [**RFC 4253**](https://datatracker.ietf.org/doc/html/rfc4253.html). Each of these messages contains the following information :

> *   `length` : this is the length of the message in bytes, excluding the MAC and length fields
>     
>     
> *   `padding length` : this is the number of random bytes that have been added at the end of the message.
>     
>     
> *   `payload` : the data (after optional compression) passed by the user
>     
>     
> *   `padding` : random bytes added in each message (at least four) to ensure that the message length is a multiple of the block size used by the negotiated encryption algorithm
>     
>     
> *   `MAC` : this field is present if a Message Authentication Code has been negotiated for the session (in practice, using `ssh` without authentication is risky and this field should always be present). Note that to compute the MAC, an `ssh` implementation must maintain a message counter. This counter is incremented by one every time a message is sent and the MAC is computed with the negotiated authentication algorithm using the MAC key over the concatenation of the message counter and the cleartext message. The message counter is not transmitted, but the recipient can easily recover its value. The `MAC` is computed as \(mac = MAC(key, sequence\_number || unencrypted\_message)\) where the key is the negotiated authentication key.

Note

Authenticating messages with HMAC

ssh is one example of a protocol that uses Message Authentication Codes (MAC) to authenticates the messages that are sent. A naive implementation of such a MAC would be to simply use a hash function like SHA-1\. However, such a construction would not be safe from a security viewpoint. Internet protocols usually rely on the HMAC construction defined in [**RFC 2104**](https://datatracker.ietf.org/doc/html/rfc2104.html). It works with any hash function (H) and a key (K). As an example, let us consider HMAC with the SHA-1 hash function. SHA-1 uses 20 bytes blocks and the block size will play an important role in the operation of HMAC. We first require the key to be as long as the block size. Since this key is the output of the key generation algorithm, this is one parameter of this algorithm.

HMAC uses two padding strings : ipad (resp. opad) which is a string containing 20 times byte `0x36` (resp. byte `0x5C`). The HMAC is then computed as \(H[K \oplus opad, H(K \oplus ipad, data) ]\) where \(\oplus\) denotes the bitwise XOR operation. This computation has been shown to be stronger than the naive \(H(K,data)\) against some types of cryptographic attacks.

Among the various features of the `ssh` protocol, it is interesting to mention how users are authenticated by the server. The `ssh` protocol supports the classical username/password authentication (but both the username and the password are transmitted over the secure encrypted channel). In addition, `ssh` supports two authentication mechanisms that rely on public keys. To use the first one, each user needs to generate his/her own public/private key pair and store the public key on the server. To be authenticated, the user needs to sign a message containing his/her public key by using his/her private key. The server can easily verify the validity of the signature since it already knows the user’s public key. The second authentication scheme is designed for hosts that trust each other. Each host has a public/private key pair and stores the public keys of the other hosts that it trusts. This is typically used in environments such as university labs where each user could access any of the available computers. If Alice has logged on `computer1` and wants to execute a command on `computer2`, she can create an `ssh` session on this computer and type (again) her password. With the host-based authentication scheme, `computer1` signs a message with its private key to confirm that it has already authenticated Alice. `computer2` would then accept Alice’s session without asking for her credentials.

The `ssh` protocol includes other features that are beyond the scope of this book. Additional details may be found in [[BS2005]](../bibliography.html#bs2005).  ## Transport Layer Security[#](#transport-layer-security "Link to this heading")

The Transport Layer Security family of protocols were initially proposed under the name Secure Socket Layer (SSL). The first deployments used this name and many researchers still refer to this security protocol as SSL [[FKC1996]](../bibliography.html#fkc1996). In this chapter, we use the official name that was standardized by the IETF: TLS for Transport Layer Security.

The TLS protocol was designed to be usable by a wide range of applications that use the transport layer to reliably exchange information. TLS is mainly used over the TCP protocol. There are variants of TLS that operate over SCTP [**RFC 3436**](https://datatracker.ietf.org/doc/html/rfc3436.html) or UDP [**RFC 6347**](https://datatracker.ietf.org/doc/html/rfc6347.html), but these are outside the scope of this chapter.

A TLS session operates over a TCP connection. TLS is responsible for the encryption and the authentication of the SDUs exchanged by the application layer protocol while TCP provides the reliable delivery of this encrypted and authenticated bytestream. TLS is used by many different application layer protocols. The most frequent ones are HTTP (HTTP over TLS is called HTTPS), SMTP [**RFC 3207**](https://datatracker.ietf.org/doc/html/rfc3207.html) or POP and IMAP [**RFC 2595**](https://datatracker.ietf.org/doc/html/rfc2595.html), but proprietary application-layer protocols also use TLS [[AM2019]](../bibliography.html#am2019).

A TLS session can be initiated in two different ways. First, the application can use a dedicated TCP port number for application layer protocol x-over-TLS. This is the solution used by many HTTP servers that reserve port \(443\) for HTTP over TLS. This solution works, but it requires to reserve two ports for each application : one where the application-layer protocol is used directly over TCP and another one where the application-layer protocol is used over TLS. Given the limited number of TCP ports that are available, this is not a scalable solution. The table below provides some of the reserved port numbers for application layer protocols on top of TLS.

| Application | TCP port | TLS port |
| --- | --- | --- |
| POP3 | 110 | 995 |
| IMAP | 143 | 993 |
| NNTP | 119 | 563 |
| HTTP | 80 | 443 |
| FTP | 21 | 990 |

A second approach to initiate a TLS session is to use the standard TCP port number for the application layer protocol and define a special message in this protocol to trigger the start of the TLS session. This is the solution used for SMTP with the `STARTTLS` message. This extension to SMTP [**RFC 3207**](https://datatracker.ietf.org/doc/html/rfc3207.html) defines the new STARTTLS command. The client can issue this command to indicate to the server that it wants to start a TLS session as shown in the example below captured during a session on port 25.

[PRE0]

In the remaining parts of this chapter, we assume that the TLS session starts immediately after the establishment of the TCP connection. This corresponds to the deployments on web servers. We focus our presentation of TLS on this very popular use case. TLS is a complex protocol that supports other features than the one used by web servers. A more detailed presentation of TLS may be found in [[KPS2002]](../bibliography.html#kps2002) and [[Ristic2015]](../bibliography.html#ristic2015).

A TLS session is divided in two phases: the handshake and the data transfer. During the handshake, the client and the server negotiate the security parameters and the keys that will be used to secure the data transfer. During the second phase, all the messages exchanged are encrypted and authenticated with the negotiated algorithms and keys.

### The TLS handshake[#](#the-tls-handshake "Link to this heading")

When used to interact with a regular web server, the TLS handshake has three important objectives:

> 1.  Securely negotiate the cryptographic algorithms that will be used by the client and the server over the TLS session
>     
>     
> 2.  Verify that the client interacts with a valid server
>     
>     
> 3.  Securely agree on the keys that will be used to encrypt and authenticate the messages exchanged over the TLS session

The TLS handshake is a four-way handshake illustrated in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random], Certificate", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "E(K,MasterSecret), Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> b>>c [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> }](../Images/300581e55a5559d21fa138b6759650ca.png)<map id="6851383dfd7cb6f84e42dde9992fdd9303de35d3" name="6851383dfd7cb6f84e42dde9992fdd9303de35d3"></map>

In a nutshell, the client starts the TLS handshake by proposing a random nonce. The server replies with its random nonce and a certificate that binds its name to a public key. The client generates a MasterSecret that will be used later to derive the session keys and encrypts it with the public key of the server. It also generates a Finished message that contains a MAC of all the messages exchanged to allow the server to detect any modification of the messages sent by the client. The server also sends its own Finished message. At that point, the client and the server sent encrypted records thanks to the keys derived from the MasterSecret.

Let us first discuss the negotiation of the cryptographic algorithms and parameters. Like all security protocols, TLS includes some agility in its design since new cryptographic algorithms appear over the years and some older algorithms become deprecated once cryptanalysts find flaws. The TLS handshakes starts with the `ClientHello` message that is sent by the client. This message carries the following information :

> *   Protocol version number: this is the version of the TLS protocol supported by the client. The server should use the same version of the TLS protocol as the client, but may opt for an older version. Both versions 1.2 and 1.3 of TLS are deployed today. Older versions are being deprecated.
>     
>     
> *   Random number: security protocols rely on random numbers. The client sends a 32 bytes long random number where usually four of these bytes correspond to the client’s clock. This random number is used, together with the server’s random number, as a seed to generate the security keys.
>     
>     
> *   Cipher suites : this ordered list contains the set of cryptographic algorithms that are supported by the client, with the most preferred one listed first. In contrast with `ssh` that allows negotiating independent algorithms for encryption, key exchange and authentication, TLS relies on suites that combine these algorithms together. Many cryptographic suites have been defined for TLS. Various recommendations have been published on the security of some of these suites [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Compression algorithm : the client may propose the utilization of a specific compression algorithm (e.g. zlib). In theory, compressing the data before encrypting it is an intelligent way to reduce the amount of data exchanged. Unfortunately, its implementation in TLS has caused several security problems [[PHG2013]](../bibliography.html#phg2013). For this reason, compression is usually disabled in TLS [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Extensions : TLS supports various extensions in the `ClientHello` message. These extensions [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) are important to allow the protocol to evolve, but many of them go beyond the scope of this chapter.

Note

The `Server Name Indication (SNI)`

The `Server Name Indication (SNI)` extension defined in [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) is an important TLS extension for web servers. It is used by the client to indicate the name of the server that it wishes to contact. The IP address associated to this name has been queried from the DNS and used to establish the TCP connection. Why should the client indicate the server name in the TLS `ClientHello` ? The motivation is the same as for the `Host` header line in HTTP/1.0\. With the SNI extension, a single TLS server can support several web sites that use different domain names. Thanks to the SNI extension, the server knows the concerned domain name at the start of the TLS session. Without this extension, hosting providers would have been forced use one IP address per TLS-enabled server.

The server replies to the `ClientHello` with several messages:

> *   the `ServerHello` message that contains the protocol version chosen by the server (assumed to be the same as the client version in this chapter), the 32 random bytes chosen by the server, the Cipher Suite selected by the server from the list advertised by the client and a Session Id. This Session Id is an identifier which is chosen by the server. It identifies the TLS session and the security parameters (algorithms and keys) negotiated for this session. It is used to support session resumption.
>     
>     
> *   the `Certificate` message provides the certificate (or usually a chain of certificates) that binds a domain name to the public key used by the server. TLS uses the server certificates to authenticate the server. It relies on a Public Key Infrastructure that is composed of a set of root certification authorities that issue certificates to certification authorities that in the end issue certificates to servers. TLS clients are usually configured with the public keys of several root certification authorities and use this information to validate the certificates that they receive from servers. For historical reasons, the TLS certificates are encoded in ASN.1 format. The details of the ASN.1 syntax [[Dubuisson2000]](../bibliography.html#dubuisson2000) are outside the scope of this book.
>     
>     
> *   the `ServerKeyExchange` message is used by the server to transmit the information that is required to perform the key exchange. The content of this message is function of the selected key exchange algorithm.
>     
>     
> *   the `ServerHelloDone` indicates that the server has sent all the messages for the first phase of the handshake.

At this point, it is time to describe the TLS key exchange. TLS supports different key exchange mechanisms that can be negotiated as part of the selection of the cipher suite. We focus on two of them to highlight their differences:

> *   `RSA`. This key exchange algorithm uses the encryption capabilities of the RSA public-key algorithm. The client has validated the server’s public key thanks to the `Certificate` message. It then generates a (48 bytes) random number, encrypts it with the server public key and sends the encrypted number to the server in the `ClientKeyExchange` message. The server uses its private key to decrypt the random number. At this point, the client and the server share the same (48 bytes long) secret and use it to derive the secret keys required to encrypt and authenticate data in the second phase. With this key exchange algorithm, the server does not need to send a `ServerKeyExchange` message.
>     
>     
> *   `DHE_RSA`. This key exchange algorithm is the Ephemeral Diffie Hellman key exchange with RSA signatures to authenticate the key exchange. It operates as a classical authenticated Diffie Hellman key exchange. If this key exchange has been selected by the server, it sends its Diffie Hellman parameters in the `ServerKeyExchange` message and signs them with its private key. The client then continues the key exchange and sends the results of its own computation in the `ClientKeyExchange` message. `DHE_RSA` is thus an authenticated Diffie Hellman key exchange where the initial message is sent by the server (instead of the client as in our first example but since the protocol is symmetric, this does not matter).

An important difference between `DHE_RSA` and `RSA` is their reaction against attacks. `DHE_RSA` is considered by many to be stronger than `RSA` because it supports Perfect Forward Secrecy. This property is important against attackers that are able to eavesdrop all the (encrypted) data sent and received by a server. Consider that Terrence is such an attacker that has stored all the packets exchanged by Bob’s server during the last six months. If he manages, by any means, to obtain Bob’s private key, he will be able to decrypt all the keys used to secure the TLS sessions with Bob’s server during this period. With `DHE_RSA`, a similar attack is less devastating. If Terrence knows Bob’s private key, he will be able to launch a man-in-the-middle attack against future TLS sessions with Bob’s server. However, he will not be able to recover the keys used for all the past sessions that he captured.

Note

Perfect Forward Secrecy

Perfect Forward Secrecy (PFS) is an important property for key exchange protocols. A protocol provides PFS if its design guarantees that the keys used for former sessions will not be compromised even if the private key of the server is compromised. This is a very important property. `DHE_RSA` provides Perfect Forward Secrecy, but the `RSA` key exchange does not provide this property. In practice, `DHE_RSA` is costly from a computational viewpoint. Recent implementations of TLS thus prefer `ECDHE_RSA` or `ECDHE_ECDSA` when Perfect Forward Secrecy is required.

All the information required for the key exchange has now been transmitted. There are two important messages that will be sent by the client and the server to conclude the handshake and start the data transfer phase.

The client sends the `ChangeCipherSpec` message followed by the `Finished` message. The `ChangeCipherSpec` message indicates that the client has received all the information required to generate the security keys for this TLS session. This messages can also appear later in the session to indicate a change in the encryption algorithms that are used, but this usage is outside the scope of this book. The `Finished` message is more important. It confirms to the server that the TLS handshake has been performed correctly and that no attacker has been able to modify the data sent by the client or the server. This is the first message that is encrypted with the selected security keys. It contains a hash of all the messages that were exchanged during the handshake.

The server also sends a `ChangeCipherSpec` message followed by a `Finished` message.

Note

TLS Cipher suites

A TLS cipher suite is usually represented as an ASCII string that starts with TLS and contains the acronym of the key exchange algorithm, the encryption scheme with the key size and its mode of operation and the authentication algorithm. For example, `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256` is a TLS cipher suite that uses the `DHE_RSA` key exchange algorithm with 128 bits AES in GCM mode for encryption and SHA-256 for authentication. The official list of TLS cipher suites is maintained by IANA [[7]](#fianatls). The NULL acronym indicates that no algorithm has been specified. For example, `TLS_ECDH_RSA_WITH_NULL_SHA` is a cipher suite that does not use any encryption but still uses the `ECDH_RSA` key exchange and `SHA` for authentication.

### The TLS record protocol[#](#the-tls-record-protocol "Link to this heading")

The handshake is now finished. The client and the server will exchange authenticated and encrypted records. TLS defines different formats for the records depending on the cryptographic algorithms that have been negotiated for the session. A detailed discussion of these different types of records is outside the scope of this introduction. For illustration, we briefly describe one record format.

As other security protocols, TLS uses different keys to encrypt and authenticate records. These keys are derived from the MasterSecret that is either randomly generated by the client after the `RSA` key exchange or derived from the Diffie Hellman parameters after the `DH_RSA` key exchange. The exact algorithm used to derive the keys is defined in [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html).

A TLS record is always composed of four different fields :

> *   a Type that indicates the type of record. The most frequent type is application data which corresponds to a record containing encrypted data. The other types are handshake, change_cipher_spec and alert.
>     
>     
> *   a Protocol Version field that indicates the version of the TLS protocol used. This version is composed of two sub fields : a major and a minor version number.
>     
>     
> *   a Length field. A TLS record cannot be longer than 16,384 bytes.
>     
>     
> *   a TLSPlainText that contains the encrypted data

TLS supports several methods to encrypted records. The selected method depends on the cryptographic algorithms that have been negotiated for the TLS session. A detailed presentation of the different methods that can be used to produce the TLSPlainText from the user data is outside the scope of this book. As an example, we study one method: Stream Encryption. This method is used with cryptographic algorithms which can operate on a stream of bytes. The method starts with a sequence of bytes provided by the user application: the plain text. The first step is to compute the authentication code to verify the integrity of the data. For this, TLS computes \(MAC(SeqNum, Header, PlainText)\) using HMAC where SeqNum is a sequence number which is incremented by one for each new TLS record transmitted. The Header is the header of the TLS record described above and PlainText is the information that needs to be encrypted. Note that the sequence number is maintained at the two endpoints of the TLS session, but it is not transmitted inside the TLS record. This sequence number is used to prevent replay attacks.

Note

MAC-then-encrypt or Encrypt-then-MAC

When secure protocols use Message Authentication and Encryption, they need to specify how these two algorithms are combined. A first solution, which is used by the current version of TLS, is to compute the authentication code and then encrypt both the data and the authentication code. A drawback of this approach is that the receiver of an encrypted TLS record must first attempt to decrypt data that has potentially been modified by an attacker before being able to verify the authenticity of the record. A better approach is for the sender to first encrypt the data and then compute the authentication code over the encrypted data. This is the encrypt-then-MAC approach proposed in [**RFC 7366**](https://datatracker.ietf.org/doc/html/rfc7366.html). With encrypt-then-MAC, the receiver first checks the authentication code before attempting to decrypt the record.

### Improving TLS[#](#improving-tls "Link to this heading")

During the last two decades, the deployment of TLS has continued to grow. The early TLS servers were only used for critical services such as e-commerce websites or online banks. As CPU performance improved, it became much more cost-effective to use TLS to secure non-critical parts of web servers, including the delivery of HTML pages and even video services. There is now a growing number of applications that rely on TLS [[AM2019]](../bibliography.html#am2019).

In 2013, the statistics collected by the Firefox Telemetry project [[9]](#ftelemetry) revealed that 30% of the web pages loaded by Firefox users were done over HTTPS. In October 2019, 80% of the web pages are loaded over HTTPS. In six years, HTTPS became the dominant protocol to access web services. Another look at the deployment of HTTPS on web sites may be found in [[Helme2019]](../bibliography.html#helme2019).

Measurement studies that analyzed the evolution of TLS over the years have identified several important changes in the TLS ecosystem [[KRA2018]](../bibliography.html#kra2018). First, the preferred cryptographic algorithms have changed. While RC4 was used by 60% of the connections in 2012, its usage has dropped since 2015\. AES started to be deployed in 2013 and is now used for more than 90% of the connections. The deployed versions of TLS have also changed. TLS 1.0 and TLS 1.1 are now rarely used. The deployment of TLS 1.2 started in 2013 and reached 70% of the connections in 2015\. Version 1.3 of TLS, that is described below, is also widely deployed.

Another interesting fact is the key exchange schemes. In 2012, RSA was the dominant solution, used by more than 80% of the observed connections [[KRA2018]](../bibliography.html#kra2018). In 2013, Edward Snowden revealed the surveillance activities of several governments. These revelations had a huge impact on the Internet community. The IETF, which standardizes Internet protocols, considered in [**RFC 7258**](https://datatracker.ietf.org/doc/html/rfc7258.html) that such pervasive monitoring was an attack. Since then, several IETF working groups have developed solutions to counter pervasive monitoring. One of these solutions is to encourage Perfect Forward Security. Within TLS, this implies replacing RSA by an authenticated Diffie Hellman key exchange such as ECDHE. Measurements indicate that since summer 2014, ECDHE is more popular than RSA. In 2018, more than 90% of the observed TLS connections used ECDHE.

The last point is the difficulty of deploying TLS servers [[KMS2017]](../bibliography.html#kms2017). When TLS servers are installed, the system administrator needs to obtain certificates and configure a range of servers. Initially, getting certificates was complex and costly, but initiatives such as [https://letsencrypt.org](https://letsencrypt.org) have simplified this workflow.

In 2014, the IETF TLS working started to work on the development of version 1.3 of the TLS protocol. Their main objectives [[Rescorla2015]](../bibliography.html#rescorla2015) for this new version were:

> *   simplify the design by removing unused or unsafe protocol features
>     
>     
> *   improve the security of TLS by leveraging the lessons learned from TLS 1.2 and some documented attacks
>     
>     
> *   improve the privacy of the protocol
>     
>     
> *   reduce the latency of TLS

Since 2014, latency has become an important concern for web services. As access networks bandwidth continue to grow, latency is becoming a key factor that affects the performance of interactive web services. With TLS 1.2, the download of a web page requires a minimum of four round-trip-times, one to create the underlying TCP connection, one to exchange the ClientHello/ServerHello, one to exchange the keys and then one to send the HTTP GET and retrieve the response. This can be very long when the server is not near the client. TLS 1.3 aimed at reducing this handshake to one round-trip-time and even zero by placing some of the cryptographic handshake in the TCP handshake. This part will be discussed in the TCP chapter. We focus here on the reducing the TLS handshake to a single round-trip-time.

To simplify both the design and the implementations, TLS 1.3 uses only a small number of cipher suites. Five of them are specified in [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html) and `TLS_AES_128_GCM_SHA256` must be supported by all implementations. To ensure privacy, all cipher suites that did not provide Perfect Forward Secrecy have been removed. Compression has also been removed from TLS since several attacks on TLS 1.2 exploited its compression capability [**RFC 7457**](https://datatracker.ietf.org/doc/html/rfc7457.html).

Note

Enterprises, privacy and TLS

By supporting only cipher suites that provide Perfect Forward Secrecy in TLS 1.3, the IETF aims at protecting the privacy of users against a wide range of attacks. However, this choice has resulted in intense debates in some enterprises. Some enterprises, notably in financial organizations, have deployed TLS, but wish to be able to decrypt TLS traffic for various security-related activities. These enterprises tried to lobby within the IETF to maintain RSA-based cipher suites that do not provide Perfect Forward Secrecy. Their arguments did not convince the IETF. Eventually, these enterprises moved to ETSI, another standardization body, and convinced them to adopt entreprise TLS, a variant of TLS 1.3 that does not provide Perfect Forward Secrecy [[eTLS2018]](../bibliography.html#etls2018).

The TLS 1.3 handshake differs from the TLS 1.2 handshake in several ways. First, the TLS 1.3 handshake requires a single round-trip-time when the client connects for the first time to a server. To achieve this, the TLS designers look at the TLS 1.2 handshake in details and found that the first round-trip-time is mainly used to select the set of cryptographic algorithms and the cryptographic exchange scheme that will be used over the TLS session. TLS 1.3 drastically simplifies this negotiation by requiring to use the Diffie Hellman exchange with a small set of possible parameters. This means that the client can guess the parameters used by the server (i.e. the modulus, p and the base g) and immediately start the Diffie Hellman exchange. A simplified version of the TLS 1.3 handshake is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/e5ae4c8c184c7b414c018f2972f772de.png)<map id="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0" name="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0"></map>

There are several important differences with the TLS 1.2 handshake. First, the Diffie Hellman key exchange is required in TLS 1.3 and this exchange is initiated by the client (before having validated the server identity). To initiate the Diffie Hellman key exchange, the client needs to guess the modulus and the base that can be accepted by the server. Either the client uses standard parameters that most server supports or the client remembers the last modulus/base that it used with this particular server. If the client guessed incorrectly, the server replies with the parameters that it expects and one round-trip-time is lost. When the server sends its ServerHello, it already knows the session key. This implies that the server can encrypt all subsequent messages. After one round-trip-time, all data exchanged over the TLS 1.3 session is encrypted and authenticated. In TLS 1.3, the server certificate is encrypted with the session key, as well as the Finished message. The server signs the handshake to confirm that it owns the public key of its certificate. If the server wants to send application data, it can already encrypt it and send it to the client. Upon reception of the server Certificate, the client verifies it and checks the signature of the handshake and the Finished message. The client confirms the end of the handshake by sending its own Finished message. At that time, the client can send encrypted data. This means that the client only had to wait one round-trip-time before sending encrypted data. This is much faster than with TLS 1.2.

For some applications, waiting one round-trip-time before being able to send data is too long. TLS 1.3 allows the client to send encrypted data immediately after the ClientHello, without having to wait for the ServerHello message. At this point in the handshake, the client cannot know the key that will be derived by the Diffie Hellman key exchange. The trick is that the server and the client need to have previously agreed on a pre-shared-key. This key could be negotiated out of band, but usually it was exchanged over a previous TLS session between the client and the server. Both the client and the server can store this key in their cache. When the client creates a new TLS session to a server, it checks whether it already knows a pre-shared key for this server. If so, the client announces the identifier of this key in its ClientHello message. Thanks to this identifier, the server can recover the key and use it to decrypt the 0-rtt Encrypted record. A simplified version of the 0-rtt TLS 1.3 handshake [[8]](#fhandshake) is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/37315cb85580d2a0a41ccf2aa6b695ae.png)<map id="04ec071344c2fae0632e2d690c2782c9017b631e" name="04ec071344c2fae0632e2d690c2782c9017b631e"></map>

On the web, TLS clients use certificates to authenticate servers but the clients are not authenticated. However, there are environments such as enterprise networks where servers may need to authenticate clients as well. A popular deployment is to authenticate remote clients who wish to access the enterprise network through a Virtual Private Network service. Some of these services run above TLS (or more precisely a variant of TLS named DTLS that runs above UDP [[MoR2004]](../bibliography.html#mor2004) but is outside the scope of this chapter). In such services, each client is authenticated thanks to a public key and a certificate that is trusted by the servers. To establish a TLS session, such a client needs to prove that it owns the public key associated with the certificate. This is done by the server thanks to the CertificateRequest message. The TLS handshake becomes the following one:

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> 
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "CertificateRequest, Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Certificate, Sign(Kc, Handshake), Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/6b610706117d1538fe4793f16be928b1.png)<map id="36d37bda46d72f2f438e0e029b02b9c54e69fa82" name="36d37bda46d72f2f438e0e029b02b9c54e69fa82"></map>

The server sends a CertificatRequest message. The client returns its certificate and signs the Handshake with is private key. This confirms to the server that the client owns the public key indicated in its certificate.

There are many more differences between TLS 1.2 and TLS 1.3\. Additional details may be found in their respective specifications, [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html) and [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html).  ## Securing the Domain Name System[#](#securing-the-domain-name-system "Link to this heading")

The Domain Name System provides a critical service in the Internet infrastructure since it maps the domain names that are used by end users onto IP addresses. Since end users rely on names to identify the servers that they connect to, any incorrect information distributed by the DNS would direct end users’ connections to invalid destinations. Unfortunately, several attacks of this kind occurred in the past. A detailed analysis of the security threats against the DNS appeared in [**RFC 3833**](https://datatracker.ietf.org/doc/html/rfc3833.html). We consider three of these threats in this section and leave the others to [**RFC 3833**](https://datatracker.ietf.org/doc/html/rfc3833.html).

The first type of attack is eavesdropping. An attacker who can capture packets sent to a DNS resolver or a DNS server can gain valuable information about the DNS names that are used by a given end user. If the attacker can capture all the packets sent to a DNS resolver, he/she can collect a lot of meta data about the domain names used by the end user. Preventing this type of attack has not been an objective of the initial design of the DNS. There are currently discussions with the IETF to carry DNS messages over TLS sessions to protect against such attacks. However, these solutions are not yet widely deployed.

The second type of attack is the man-in-the-middle attack. Consider that Alice is sending DNS requests to her DNS resolver. Unfortunately, Mallory sits in front of this resolver and can capture and modify all the packets sent by Alice to her resolver. In this case, Mallory can easily modify the DNS responses sent by the resolver to redirect Alice’s packets to a different IP address controlled by Mallory. This enables Mallory to observe (and possibly modify) all the packets sent and received by Alice. In practice, executing this attack is not simple since DNS resolvers are usually installed in protected datacenters. However, if Mallory controls the WiFi access point that Alice uses to access the Internet, he could easily modify the packets on this access point and some software packages automate this type of attacks.

If Mallory cannot control a router on the path between Alice and her resolver, she could still launch a different attack. To understand this attack, it is important to correctly understand how the DNS protocol operates and the roles of the different fields of the DNS header which is reproduced in [Fig. 46](#fig-dns-header2).

[![../_images/dnsheader.svg](../Images/21a7bad41190434264bf8afe39a2de6a.png)](../_images/dnsheader.svg) 

Fig. 46 DNS header[#](#id39 "Link to this image") 

The first field of the header is the Identification field. When Alice sends a DNS request, she places a 16-bits integer in this field and remembers it. When she receives a response, she uses this Identification field to locate the initial DNS request that she sent. The response is only used if its Identification matches a pending DNS request (containing the same question).

Mallory has studied the DNS protocol and understands how it works. If he can predict a popular domain for which Alice will regularly send DNS requests, then he can prepare a set of DNS responses that map the name requested by Alice to an IP address controlled by Mallory instead of the legitimate DNS response. Each DNS response has a different Identification. Since there are only 65,536 values for the Identification field, it is possible for Mallory to send them to Alice hoping that one of them will be received while Alice is waiting for a DNS response with the same identifier. In the past, it was difficult to send 65,536 DNS responses quickly enough. However, with the high speed links that are available today, this is not an issue anymore. A second concern for Mallory is that he must be able to send the DNS responses as if they were coming directly from the DNS resolver. This implies that Mallory must be able to send IP packets that appear to originate from a different address. Although networks should be configured to prevent this type of attack, this is not always the case and there are networks where it is possible for a host to send packets with a different source IP address [[10]](#fspoof). If the attack targets a single end user, e.g. Alice, this is annoying for this user. However, if the attacker can target a DNS resolver that serves an entire company or an entire ISP, the impact of the attack can be much larger in particular if the injected DNS response carries a long TTL and thus resides in the resolver’s cache for a long period of time.

Fortunately, designers of DNS servers and resolvers have found solutions to mitigate this type of attack. The easiest approach would have been to update the format of the DNS requests and responses to include a larger Identifier field. Unfortunately, this elegant solution was not possible with the DNS because the DNS messages do not include any version number that would have enabled such a change. Since the DNS messages are exchanged inside UDP segments, the DNS developers found an alternate solution to counter this attack. There are two ways for the DNS library used by Alice to send her DNS requests. A first solution is to bind one UDP source port and always send the DNS requests from this source port (the destination port is always port `53`). The advantage of this solution is that Alice’s DNS library can easily receive the DNS responses by listening to her chosen port. Unfortunately, once the attacker has found the source port used by Alice, he only needs to send 65,536 DNS responses to inject an invalid response. Fortunately, Alice can send her DNS requests in a different way. Instead of using the same source port for all DNS requests, she can use a different source port for each request. In practice, each DNS request will be sent from a different source port. From an implementation viewpoint, this implies that Alice’s DNS library will need to listen to one different port number for each pending DNS request. This increases the complexity of her implementation. From a security viewpoint there is a clear benefit since the attacker needs to guess both the 16 bits Identifier and the 16 bits UDP source port to inject a fake DNS response. To generate all possible DNS responses, the attacker would need to generate almost \(2^{32}\) different messages, which is excessive in today’s networks. Most DNS implementations use this second approach to prevent these cache poisoning attacks.

These attacks affect the DNS messages that are exchanged between a client and its resolver or between a resolver and name servers. Another type of attack exploits the possibility of providing several resource records inside one DNS response. A frequent optimization used by DNS servers and resolvers is to include several related resource records in each response. For example, if a client sends a DNS query for an NS record, it usually receives in the response both the queried record, i.e. the name of the DNS server that serves the queried domain, and the IP addresses of this server. Some DNS servers return several NS records and the associated IP addresses. The cache poisoning attack exploits this DNS optimization.

Let us illustrate it on an example. Assume that Alice frequently uses the example.net domain and in particular the web server whose name is www.example.net. Mallory would like to redirect the TCP connections established by Alice towards www.example.net to one IP address that he controls. Assume that Mallory controls the mallory.net domain. Mallory can tune the DNS server of his domain and add special DNS records to the responses that it sends. An attack could go roughly as follows. Mallory forces Alice to visit the www.mallory.net web site. He can achieve this by sending a spam message to Alice or buying advertisements on a web site visited by Alice and redirect one of these advertisements to www.mallory.net. When visiting the advertisement, Alice’s DNS resolver will send a DNS request for www.mallory.net. Since Mallory control the DNS server, he can easily add in the response a AAAA record that associates www.example.net to the IP address controlled by Mallory. If Alice’s DNS library does not check the returned response, the cache entry for www.example.net will be replaced by the AAAA record sent by Mallory.

To cope with these security threats and improve the security of the DNS, the IETF has defined several extensions that are known as DNSSEC. DNSSEC exploits public-key cryptography to authenticate the content of the DNS records that are sent by DNS servers and resolvers. DNSEC is defined in three main documents [**RFC 4033**](https://datatracker.ietf.org/doc/html/rfc4033.html), [**RFC 4034**](https://datatracker.ietf.org/doc/html/rfc4034.html), [**RFC 4035**](https://datatracker.ietf.org/doc/html/rfc4035.html). With DNSSEC, each DNS zone uses one public-private key pair. This key pair is only used to sign and authenticate DNS records. The DNS records are not encrypted and DNSSEC does not provide any confidentiality. Other DNS extensions are being developed to ensure the confidentiality of the information exchanged between a client and its resolvers [**RFC 7626**](https://datatracker.ietf.org/doc/html/rfc7626.html). Some of these extensions exchange DNS records over a TLS session which provides the required confidentiality, but they are not yet deployed and outside the scope of this chapter.

DNSSEC defines four new types of DNS records that are used together to authenticate the information distributed by the DNS.

> *   the DNSKEY record allows storing the public key associated with a zone. This record is encoded as a TLV and includes a Base64 representation of the key and the identification of the public key algorithm. This allows the DNSKEY record to support different public key algorithms.
>     
>     
> *   the RRSIG record is used to encode the signature of a DNS record. This record contains several sub-fields. The most important ones are the algorithm used to generate the signature, the identifier of the public key used to sign the record, the original TTL of the signed record and the validity period for the signature.
>     
>     
> *   the DS record contains a hash of a public key. It is used by a parent zone to certify the public key used by one of its child zones.
>     
>     
> *   the NSEC record is used when non-existent domain names are queried. Its usage will be explained later

The simplest way to understand the operation of DNSSEC is to rely on a simple example. Let us consider the example.org domain and assume that Alice wants to retrieve the AAAA record for www.example.org using DNSSEC.

The security of DNSSEC relies on anchored keys. An anchored key is a public key that is considered as trusted by a resolver. In our example, we assume that Alice’s resolver has obtained the public key of the servers that manage the root zone in a secure way. This key has been distributed outside of the DNS, e.g. it has been published in a newspaper or has been received in a sealed letter.

To obtain an authenticated record for www.example.org, Alice’s resolver first needs to retrieve the NS which is responsible for the .org Top-Level Domain (TLD). This record is served by the DNS root server and Alice’s resolver can retrieve the signature (RRSIG record) for this NS record. Since Alice knows the DNSKEY of the root, she can verify the validity of this signature.

The next step is to contact ns.org, the NS responsible for the .org TLD to retrieve the NS record for the example.org domain. This record is accompanied by a RRSIG record that authenticates it. This RRSIG record is signed with the key of the .org domain. Alice’s resolver can retrieve this public key as the DNSKEY record for the .org, but how can it trust this key since it is distributed by using the DNS and could have been modified by attackers ? DNSSEC solves this problem by using the DS record that is stored in the parent zone (in this case, the root zone). This record contains a hash of a public key that is signed with a RRSIG signature. Since Alice’s resolver’s trusts the root key, it can validate the signature of the DS record for the .org domain. It can then retrieve the DNSKEY record for this domain from the DNS and compare the hash of this key with the DS record. If they match, the public key of the .org domain can be trusted. The same technique is used to obtain and validate the key of the example.org domain. Once this key is trusted, Alice’s resolver can request the AAAA record for www.example.org and validate its signature.

Thanks to the DS record, a resolver can validate the public keys of client zones as long as their is a chain of DS -> DNSKEY records from an anchored key. If the resolver trusts the public key of the root zone, it can validate all DNS replies for which this chain exists.

There are several details of the operation of DNSSEC that are worth being discussed. First, a server that supports DNSSEC must have a public-private key pair. The public key is distributed with the DNSKEY record. The private key is never distributed and it does not even need to be stored on the server that uses the public key. DNSSEC does not require the DNSSEC servers to perform any operation that requires a private key in real time. All the RRSIG records can be computed offline, possibly on a different server than the server that returns the DNSSEC replies. The initial motivation for this design choice was the CPU complexity of computing the RRSIG signatures for zones that contain millions of records. In the early days of DNSSEC, this was an operational constraint. Today, this is less an issue, but avoiding costly signature operations in real time has two important benefits. First, this reduces the risk of denial of service attacks since an attacker cannot force a DNSSEC server to perform computationally intensive signing operations. Second, the private key can be stored offline, which means that even if an attacker gains access to the DNSSEC server, it cannot retrieve its private key. Using offline signatures for the RRSIG records has some practical implications that are reflected in the content of this record. First, each RRSIG record contains the original TTL of the signed record. When DNS resolvers cache records, they change the value of the TTL of these cached records and then return the modified records to their clients. When a resolver receives a signed DNS record, it must replace the received TTL of the record with the original TTL (and check that the received TTL is smaller than the original one) before checking the signature. Second, the RRSIG records contain a validity period, i.e. a starting time and an ending time for the validity of the signature. This period is specified as two timestamps. This period is only the validity of the signature. It does not affect the TTL of the signed record and is independent from the TTL. In practice, the validity period is important to allow DNS server operators to update their public/private keys. When such a key is changed, e.g. because the private could have been compromised, there is some period of time during which records signed with the two keys coexist in the network. The validity period allows ensuring that old signatures do not remain in DNS caches for ever.

The last record introduced by DNSSEC is the NSEC record. It is used to authenticate a negative response returned by a DNS server. If a resolver requests a domain name that is not defined in the zone, the server replies with an error message. The designers of the original version of the DNS thought that these errors would not be very frequent and resolvers were not required to cache those negative responses. However, operational experience showed that queries for invalid domain names are more frequent than initially expected and a large fraction of the load on some servers is caused by repeated queries for invalid names. Typical examples include queries for invalid TLDs to the root DNS servers or queries caused by configuration errors [[WF2003]](../bibliography.html#wf2003). Current DNS deployments allow resolvers to cache those negative answers to reduce the load on the entire DNS [**RFC 2308**](https://datatracker.ietf.org/doc/html/rfc2308.html).

The simplest way to allow a DNSSEC server to return signed negative responses would be for the server to return a signed response that contains the received query and some information indicating the error. The client could then easily check the validity of the negative response. Unfortunately, this would force the DNSSEC server to generate signatures in real time. This implies that the private key must be stored in the server memory, which leads to risks if an attacker can take control of the server. Furthermore, those signatures are computationally complex and a simple denial of service attack would be to send invalid queries to a DNSSEC server.

Given the above security risks, DNSSEC opted for a different approach that allows the negative replies to be authenticated by using offline signatures. The NSEC record exploits the lexicographical ordering of all the domain names. To understand its usage, consider a simple domain that contains three names (the associated AAAA and other records that are not shown) :

[PRE1]

In this domain, the DNSSEC server adds three NSEC records. A RRSIG signature is also computed for each of these records.

[PRE2]

If a resolver queries delta.example.org, the server will parse its zone. If this name were present, it would have been placed, in lexicographical order, between the beta.example.org and the gamma.example.org names. To confirm that the delta.example.org name does not exist, the server returns the NSEC record for beta.example.org that indicates that the next valid name after beta.example.org is gamma.example.org. If the server receives a query for pi.example.org, this is the NSEC record for gamma.example.org that will be returned. Since this record contains a name that is before pi.example.org in lexicographical order, this indicates that pi.example.org does not exist.

Footnotes 

## Security threats[#](#security-threats "Link to this heading")

When analyzing security issues in computer networks, it is useful to reason about the capabilities of the attacker who wants to exploit some breach in the security of the network. There are different types of attackers. Some have generic capabilities, others are specific to a given technology or network protocol. In this section, we discuss some important threats that a network architect must take into account.

The first type of attacker is called the passive attacker. A passive attacker is someone able to observe and usually store the information (e.g. the packets) exchanged in a given network or subset of it (e.g. a specific link). This attacker has access to all the data passing through this specific link. This is the most basic type of attacker and many network technologies are vulnerable to such attacks. In the above example, a passive attacker could easily capture the password sent by Alice and reuse it later to be authenticated as Alice on the remote computer. This is illustrated in the figure below where we do not show anymore the `DATA.req` and `DATA.ind` primitives but only the messages exchanged. Throughout this chapter, we will always use Eve as a user who is able to eavesdrop the data passing in front of her.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Eve", linecolour=red],
> d [label="", linecolour=white],
> e [label="Bob", linecolour=black],
> f [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>e [ label = "I'm Alice\n\n", arcskip="1"];
> e=>f [ label = "" ];
> 
> e=>f [ label = "" ] ,
> e>>b [ label = "Password:\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>e [ label = "1234xyz$\n\n", arcskip="1"];
> e=>f [ label = "" ];
> 
> e=>f [ label = "" ] ,
> e>>b [ label = "Access\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/f4b5846cad3728586ed1ef7e20d24afb.png)<map id="17921862b047d560c7fe7db8be3c8ccaa1e34782" name="17921862b047d560c7fe7db8be3c8ccaa1e34782"></map>

In the above example, Eve can capture all the packets exchanged by Bob and Alice. This implies that Eve can discover Alice’s username and Alice’s password. With this information, Eve can then authenticate as Alice on Bob’s computer and do whatever Alice is authorized to do. This is a major problem from a security point of view. To prevent this attack, Alice should never send her password in clear over a network where someone could eavesdrop the information. In some networks, such as an open wireless network, an attacker can easily collect all the data sent by a particular user. In other networks, this is a bit more complex depending on the network technology used, but various software packages exist to automate this process. As will be described later, the best approach to prevent this type of attack is to rely on cryptographic techniques to ensure that passwords are never sent in clear.

Note

Pervasive monitoring

In the previous example, we have explained how Eve could capture data from a particular user. This is not the only attack of this type. In 2013, based on documents collected by Edward Snowden, the press revealed that several governmental agencies were collecting lots of data on various links that compose the global Internet [[Greenwald2014]](../bibliography.html#greenwald2014). Thanks to this massive amount of data, these governmental agencies have been able to extract lots of information about the behavior of Internet users. Like Eve, they are in a position to extract passwords, usernames and other privacy sensitive data from all the packets that they have captured. However, it seems that these agencies were often more interested in various meta data, e.g. information showing with whom a given user communicates than the actual data exchanged. These revelations have shocked the Internet community and the [Internet Engineering Task Force](https://www.ietf.org) that manages the standardization of Internet protocols has declared in [**RFC 7258**](https://datatracker.ietf.org/doc/html/rfc7258.html) that such pervasive monitoring is an attack that need to be countered in the development of new protocols. Several new protocols and extensions to existing ones are being developed to counter these attacks.

Eavesdropping and pervasive monitoring are not the only possible attacks against a network. Another type of attacker is the active attacker. In the literature, these attacks are often called Man in the middle or MITM attacks. Such attacks occur when one user, let us call him Mallory, has managed to configure the network so that he can both capture and modify the packets exchanged by two users. The simplest scenario is when Mallory controls a router that is on the path used by both Alice and Bob. For example, Alice could be connected to a WiFi access router controlled by Mallory and Bob would be a regular server on the Internet.

![Figure made with TikZ](../Images/c7ceeff3d8a02de071eac1377ed85bc7.png)

As Mallory receives all the packets sent by both Bob and Alice, he can modify them at will. For example, he could modify the commands sent by Alice to the server managed by Bob and change the responses sent by the server. This type of attack is very powerful and sometimes difficult to counter without relying on advanced cryptographic techniques.

The last type of attack that we consider in this introduction are the Denial of Service or DoS attacks. During such an attack, the attacker generates enough packets to saturate a given service and prevent it from operating correctly. The simplest Denial of Service attack is to send more packets that the bandwidth of the link that attaches the target to the network. The target could be a single server, a company or even an entire country. If these packets all come from the same source, then the victim can identify the attacker and contact the law enforcement authorities. In practice, such denial of service attacks do not originate from a single source. The attacker usually compromises a (possibly very large) set of sources and forces them to send packets to saturate a given target. Since the attacking traffic comes from a wide range of sources, it is difficult for the victim to locate the culprit and also to counter the attack. Saturating a link is the simplest example of Distributed Denial of Service (DDoS) attacks.

In practice, there is a possibility of denial of service attacks as soon as there is a limited resource somewhere in the network. This resource can be the bandwidth of a link, but it could also be the computational power of a server, its memory or even the size of tables used by a given protocol implementation. Defending against real DoS attacks can be difficult, especially if the attacker controls a large number of sources that are used to launch the attacks. In terms of bandwidth, DoS attacks composed of a few Gbps to a few tens of Gbps of traffic are frequent on the Internet. In 2015, [github.com](http://www.github.com) suffered from a distributed DoS that reached a top bandwidth of 400 Gbps according to some [reports](http://www.techworld.com/news/security/worlds-largest-ddos-attack-reached-400gbps-says-arbor-networks-3595715/).

When designing network protocols and applications that will be deployed on a large scale, it is important to take those DDoS attacks into account. Attackers use different strategies to launch DDoS attacks. Some have managed to gain control of a large number of sources by injecting malware on them. Others, and this is where protocol designers have an important role to play, simply exploit design flaws in some protocols. Consider a simple request-response protocol where the client sends a request and the server replies with a response. Often the response is larger or much larger than the request sent by the client. Consider that such a simple protocol is used over a datagram network. When Alice sends a datagram to Bob containing her request, Bob extracts both the request and Alice’s address from the packet. He then sends his response in a single packet destined to Alice. Mallory would like to create a DoS attack against Alice without being identified. Since he has studied the specification of this protocol, he can send a request to Bob inside a packet having Alice’s address as its source address. Bob will process the request and send his (large) response to Alice. If the response has the same size as the request, Mallory is producing a reflection attack since his packets are reflected by Bob. Alice would think that she is attacked by Bob. If there are many servers that operate the same service as Bob, Mallory could hide behind a large number of such reflectors. Unfortunately, the reflection attack can also become an amplification attack. This happens when the response sent by Bob is larger than the request that it has received. If the response is \(k\) times larger than the request, then when Mallory consumes 1 Gbps of bandwidth to send requests, his victim receives \(k\) Gbps of attack traffic. Such amplification attacks are a very important problem and protocol designers should ensure that they never send a large response before having received the proof that the request that they have received originated from the source indicated in the request.

## Cryptographic primitives[#](#cryptographic-primitives "Link to this heading")

Cryptography techniques have initially been defined and used by spies and armies to exchange secret information in manner that ensures that adversaries cannot decode the information even if they capture the message or the person carrying the message. A wide range of techniques have been defined. The first techniques relied on their secrecy to operate. One of the first encryption schemes is attributed to Julius Caesar. When he sent confidential information to his generals, he would encode each message by replacing each letter with another letter that is \(n\) positions after this letter in the alphabet. For example, the message SECRET becomes VHFUHW when encoded using Caesar’s cipher. This technique could have puzzled some soldiers during Caesar’s wars, but today even young kids can recover the original message from the ciphered one.

The security of the Caesar cipher depends on the confidentiality of the algorithm, but experience has shown that it is impossible to assume that an algorithm will remain secret, even for military applications. Instead, cryptographic techniques must be designed by assuming that the algorithm will be public and known to anyone. However, its behavior must be controlled by a small parameter, known as the key, that will only be known by the users who need to communicate secretly. This principle is attributed to Auguste Kerckhoff, a French cryptographer who first documented it :

> A cryptographic algorithm should be secure even if the attacker knows everything about the system, except one parameter known as the secret key.

This principle is important because it remains the basic assumption of all cryptographers. Any system that relies on the secrecy of its algorithm to be considered secure is doomed to fail and be broken one day.

With the Kerckhoff principle, we can now discuss a simple but powerful encryption scheme that relies on the XOR logic operation. This operation is easily implemented in hardware and is supported by all microprocessors. Given a secret, \(K\), it is possible to encode a message M by computing \(C_M = K \oplus M\). The receiver of this messages can recover the original message as since \(M = K \oplus (K \oplus M)\). This XOR operation is the key operation of the perfect cipher that is also called the Vernam cipher or the one-time pad. This cipher relies on a key that contains purely random bits. The encrypted message is then produced by XORing all the bits of the message with all the bits of the key. Since the key is random, it is impossible for an attacker to recover the original text (or plain text) from the encrypted one. From a security viewpoint, the one-time-pad is the best solution provided that the key is as long as the message.

Unfortunately, it is difficult to use this cipher in practice since the key must be as long as the message that needs to be transmitted. If the key is smaller than the message and the message is divided into blocks that have the same length as the key, then the scheme becomes less secure since the same key is used to decrypt different parts of the message. In practice, XOR is often one of the basic operations used by encryption schemes. To be usable, the deployed encryption schemes use keys that are composed of a small number of bits, typically 56, 64, 128, 256, …

A secret key encryption scheme is a perfectly reversible functions, i.e. given an encryption function E, there is an associated decryption function D such that \(\forall K, \forall M : D(K, E(K,M))=M\).

Various secret key cryptographic functions have been proposed, implemented and deployed. The most popular ones are :

> *   DES, the Data Encryption Standard that became a standard in 1977 and has been widely used by industry. It uses 56 bits keys that are not considered sufficiently secure nowadays since attackers can launch brute-force attacks by testing all possible keys. Triple DES combines three 56 bits keys, making the brute force attacks more difficult.
>     
>     
> *   RC4 is an encryption scheme defined in the late 1980s by Ron Rivest for RSA Security. Given the speed of its software implementation, it has been included in various protocols and implementations. However, cryptographers have identified several weaknesses in this algorithm. It is now deprecated and should not be used anymore [**RFC 7465**](https://datatracker.ietf.org/doc/html/rfc7465.html).
>     
>     
> *   AES or the Advanced Encryption Standard is an encryption scheme that was designed by the Belgian cryptographers Joan Daemen and Vincent Rijmen in 2001 [[DR2002]](../bibliography.html#dr2002). This algorithm has been standardized by the U.S. National Institute of Standards and Technology (NIST). It is now used by a wide range of applications and various hardware and software implementations exist. Many microprocessors include special instructions that ease the implementation of AES. AES divides the message to be encrypted in blocks of 128 bits and uses keys of length 128, 192 or 256 bits. The block size and the key length are important parameters of an encryption scheme. The block size indicates the smallest message that can be encrypted and forces the sender to divide each message in blocks of the supported size. If the message is larger than an integer number of blocks, then the message must be padded before being encrypted and this padding must be removed after decryption. The key size indicates the resistance of the encryption scheme against brute force attacks, i.e. attacks where the attacker tries all possible keys to find the correct one.

AES is widely used as of this writing, but other secret key encryption schemes continue to appear. ChaCha20, proposed by D. Bernstein is now used by several internet protocols [**RFC 7539**](https://datatracker.ietf.org/doc/html/rfc7539.html). A detailed discussion of encryption schemes is outside the scope of this book. We will consider encryption schemes as black boxes whose operation depends on a single key. A detailed overview of several of these schemes may be found in [[MVV2011]](../bibliography.html#mvv2011).

In the 1970s, Diffie and Hellman proposed in their seminal paper [[DH1976]](../bibliography.html#dh1976), a different type of encryption : public key cryptography. In public key cryptography, each user has two different keys :

> *   a public key (\(K_{pub}\)) that he can distribute to everyone
>     
>     
> *   a private key (\(K_{priv}\)) that he needs to store in a secure manner and never reveal to anyone

These two keys are generated together and they are linked by a complex mathematical relationship that is such that it is computationally difficult to compute \(K_{priv}\) from \(K_{pub}\).

A public key cryptographic scheme is a combination of two functions :

> *   an encryption function, \(E_{p}\), that takes a key and a message as parameters
>     
>     
> *   a decryption function, \(D_{p}\), that takes a key and a message as parameters

The public key is used to encrypt a message so that it can only be read by the intended recipient. For example, let us consider two users : Alice and Bob. Alice (resp. Bob) uses the keys \(A_{priv}\) and \(A_{pub}\) (resp. \(B_{priv}\) and \(B_{pub}\)). To send a secure message M to Alice, Bob computes \(CM=E_p(A_{pub},M)\) and Alice can decrypt it by using \(D_p(A_{priv},CM)=D_p(A_{priv},E_p(A_{pub},M))=M\).

Several public key encryption schemes have been proposed. Two of them have reached wide deployment :

> *   The Rivest Shamir Adleman (RSA) algorithm [[2]](#frsa) proposed in [[RSA1978]](../bibliography.html#rsa1978) that relies on modular exponentiation with large integers.
>     
>     
> *   The Elliptic Curve Cryptography techniques [[3]](#fecc) that rely on special properties of elliptic curves.

Another interesting property of public key cryptography is its ability to compute signatures that can be used to authenticate a message. This capability comes from the utilization of two different keys that are linked together. If Alice wants to sign a message M, she can compute \(SM=E_p(A_{priv},M)\). Anyone who receives this signed messaged can extract its content as \(D_p(A_{pub},SM)=D_p(A_{pub},E_p(A_{priv},M))=M\). Everyone can use \(A_{pub}\) to check that the message was signed by using Alice’s private key (\(A_{priv}\)). Since this key is only known by Alice, the ability to decrypt SM is a proof that the message was signed by Alice herself.

In practice, encrypting a message to sign it can be computationally costly, in particular if the message is a large file. A faster solution would be to summarize the document and only sign the summary of the document. A naive approach could be based on a checksum or CRC computed over the message. Alice would then compute \(C=Checksum(M)\) and \(SC=E_p(A_{priv},C)\). She would then send both M and SC to the recipient of the message who can easily compute C from SC and verify the authenticity of the message. Unfortunately, this solution does not protect Alice and the message’s recipient against a man-in-the-middle attack. If Mallory can intercept the message sent by Alice, he can easily modify Alice’s message and tweak it so that it has the same checksum as the original one. The CRCs, although more complex to compute, suffer from the same problem.

To efficiently sign messages, Alice needs to be able to compute a summary of her message in a way that makes prohibits an attacker from generating a different message that has the same summary. Cryptographic hash functions were designed to solve this problem. The ideal hash function is a function that returns a different number for every possible input. In practice, it is impossible to find such a function. Cryptographic hash functions are an approximation of this perfect summarization function. They compute a summary of a given message in 128, 160, 256 bits or more. They also exhibit the avalanche effect. This effect indicates that a small change in the message causes a large change in the hash value. Finally hash functions are very difficult to invert. Knowing a hash value, it is computationally very difficult to find the corresponding input message. Several hash functions have been proposed by cryptographers. The most popular ones are :

> *   MD5, originally proposed in [**RFC 1321**](https://datatracker.ietf.org/doc/html/rfc1321.html). It has been used in a wide range of applications. In 2010, attacks against MD5 were published and this hash function is now deprecated.
>     
>     
> *   SHA-1 is a cryptographic hash function that was standardized by the NIST in 1995\. It outputs 160 bits results. It is now used in a variety of network protocols.
>     
>     
> *   SHA-2 is another family of cryptographic hash functions designed by the NIST. Different variants of SHA-2 can produce has values of 224, 256, 384 or 512 bits.

Another important point about cryptographic algorithms is that often these algorithms require random numbers to operate correctly (e.g. to generate keys). Generating good random numbers is difficult and any implementation of cryptographic algorithms should also include a secure random number generator. [**RFC 4086**](https://datatracker.ietf.org/doc/html/rfc4086.html) provides useful recommendations.

## Cryptographic protocols[#](#cryptographic-protocols "Link to this heading")

We can now combine the cryptographic operations described in the previous section to build some protocols to securely exchange information. Let us first go back to the problem of authenticating Alice on Bob’s computer. We have shown earlier that using a simple password for this purpose is insecure in the presence of attackers.

A naive approach would be to rely on hash functions. Since hash functions are non-invertible, Alice and Bob could decide to use them to exchange Alice’s password in a secure manner. Then, Alice could be authenticated by using the following exchange.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Prove it\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Hash(passwd)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access granted\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/8ce774d886954a61fe3332c085b81a4f.png)<map id="e9d4cd4a36e66a6415dde6e83ecc95535122af78" name="e9d4cd4a36e66a6415dde6e83ecc95535122af78"></map>

Since the hash function cannot be inverted, an eavesdropper cannot extract Alice’s password by simply observing the data exchanged. However, Alice’s real password is not the objective of an attacker. The main objective for Mallory is to be authenticated as Alice. If Mallory can capture Hash(passwd), he can simply replay this data, without being able to invert the hash function. This is called a replay attack.

To counter this replay attack, we need to ensure that Alice never sends the same information twice to Bob. A possible mode of operation is shown below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:764192\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Hash(764192||passwd)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access\n\n", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/d68b2f1363c49e52d5bce558f315b1ad.png)<map id="20f577439fc03098f4163306e3e3d06fd0e01a3f" name="20f577439fc03098f4163306e3e3d06fd0e01a3f"></map>

To authenticate herself, Alice sends her user identifier to Bob. Bob replies with a random number as a challenge to verify that Alice knows the shared secret (i.e. her password). Alice replies with the result of the computation of a hash function (e.g. SHA-1) over a string that is the concatenation between the random number chosen by Bob and Alice’s password. The random number chosen by Bob is often called a nonce since this is a number that should only be used once. Bob performs the same computation locally and can check the message returned by Alice. This type of authentication scheme has been used in various protocols. It prevents replay attacks. If Eve captures the messages exchanged by Alice and Bob, she cannot recover Alice’s password from the messages exchanged since hash functions are non-invertible. Furthermore, she cannot replay the hashed value since Bob will always send a different nonce.

Unfortunately, this solution forces Bob to store Alice’s password in clear. Any breach in the security of Bob’s computer would reveal Alice’s password. Such breaches unfortunately occur and some of them have led to the dissemination of millions of passwords.

A better approach would be to authenticate Alice without storing her password in clear on Bob’s computer. For this, Alice computes a hash chain as proposed by Lamport in [[Lamport1981]](../bibliography.html#lamport1981). A hash chain is a sequence of applications of a hash function (H) on an input string. If Alice’s password is P, then her 10 steps hash chain is : \(H(H(H(H(H(H(H(H(H(H(P))))))))))\). The result of this hash chain will be stored on Bob’s computer together with the value 10. This number is the maximum number of remaining authentications for Alice on Bob’s computer. To authenticate Alice, Bob sends the remaining number of authentications, i.e. 10 in this example. Since Alice knows her password, P, she can compute \(H^9(P)=H(H(H(H(H(H(H(H(H(P)))))))))\) and send this information to Bob. Bob computes the hash of the value received from Alice (\(H(H^9(P))\)) and verifies that this value is equal to the value stored in his database. It then decrements the number of authorized authentications and stores \(H^9(P)\) in his database. Bob is now ready for the next authentication of Alice. When the number of authorized authentications reaches zero, the hash chain needs to be reinitialized. If Eve captures \((H^n(P))\), she cannot use it to authenticate herself as Alice on Bob’s computer because Bob will have decremented its number of authorized authentications. Furthermore, given that hash functions are not invertible, Eve cannot compute \(H^{n-1}(P)\) from \(H^{n}(P)\).

The two protocols above prevent eavesdropping attacks, but not man-in-the-middle attacks. If Mallory can intercept the messages sent by Alice, he could force her to reveal \(H^n(P)\) and then use this information to authenticate as Alice on Bob’s computer. In practice, hash chains should only be used when the communicating users know that there cannot be any man-in-the-middle on their communication.

Public key cryptography provides another possibility to allow Alice to authenticate herself on Bob’s computer. Assume again that Alice and Bob know each other from previous encounters. Alice knows Bob’s public key (\(Bob_{pub}\)) and Bob also knows Alice’s key (\(Alice_{pub}\)). To authenticate herself, Alice could send her user identifier. Bob would reply with a random number encrypted with Alice’s public key : \(E_p(Alice_{pub},R)\). Alice can decrypt this message to recover R and sends \(E_p(Bob_{pub},R)\). Bob decrypts the nonce and confirms that Alice knows \(Alice_{priv}\). If an eavesdropper captures the messages exchanged, he cannot recover the value R which could be used as a key to encrypt the information with a secret key algorithm. This is illustrated in the time sequence diagram below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "E_p(Alice_{pub},R)", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Bob_{pub},R)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/f7eeac697813279f3277f20eb023de49.png)<map id="6027ccdd0a4dba418aa8843c8ac153fd6a422bd7" name="6027ccdd0a4dba418aa8843c8ac153fd6a422bd7"></map>

A drawback of this approach is that Bob is forced to perform two public key computations : one encryption to send the random nonce to Alice and one decryption to recover the nonce encrypted by Alice. If these computations are costly from a CPU viewpoint, this creates a risk of Denial of Service Attacks were attackers could try to access Bob’s computer and force it to perform such costly computations. Bob is more at risk than Alice in this situation and he should not perform complex operations before being sure that he is talking with Alice. An alternative is shown in the time sequence diagram below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice\n\n", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R\n\n", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R)\n\n", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/5d0252030a1e74f0aa10edc4572b381d.png)<map id="26127d2d1ccdbf50c7d1c6d967ea621292da9631" name="26127d2d1ccdbf50c7d1c6d967ea621292da9631"></map>

Here, Bob simply sends a random nonce to Alice and verifies her signature. Since the random nonce and the signature could be captured by an eavesdropper, they cannot be used as a secret key to encrypt further data. However Bob could propose a secret key and send it encrypted with Alice’s public key in response to the signed nonce that he received.

The solution described above works provided that Bob and Alice know their respective public keys before communicating. Otherwise, the protocol is not secure against man-in-the-middle attackers. Consider Mallory sitting in the middle between Alice and Bob and assume that neither Alice nor Bob knows the other’s public key.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> x [label="", linecolour=white],
> y [label="Mallory", textcolour="red", linecolour=red],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = " " ] ,
> b>>y [ label = "I'm Alice key=Alice_{pub}", arcskip="1" ];
> y>>c [ label = "I'm Alice key=Mallory_{pub}", textcolour="red", arcskip="1" ];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>y [ label = "E_p(Alice_{priv},R)", arcskip="1"];
> y>>c [ label = "E_p(Mallory_{priv},R)", textcolour="red",  arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Access", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/0638f4c9db9fea46091250d1fe48d4c9.png)<map id="1ee9ab4ade7fb05cf8774418033d3c5fb2944924" name="1ee9ab4ade7fb05cf8774418033d3c5fb2944924"></map>

In the above example, Alice sends her public key, (\(Alice_{pub}\)), in her first message together with her identity. Mallory intercepts the message and replaces Alice’s key with his own key, (\(Mallory_{pub}\)). Bob replies with a nonce, R. Alice then signs the random nonce to prove that she knows \(Alice_{priv}\). Mallory discards the information and instead computes \(E_p(Mallory_{priv},R)\). Bob now thinks that he is discussing with Alice while Mallory sits in the middle.

There are situations where symmetric authentication is required. In this case, each user must perform some computation with his/her private key. A possible exchange is the following. Alice sends her certificate to Bob. Bob replies with a nonce, \(R1\), and provides his certificate. Alice encrypts \(R1\) with her private key and generates a nonce, \(R2\). Bob verifies Alice’s computation and encrypts \(R2\) with his private key. Alice verifies the computation and both have been authenticated.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:R1", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R1),R2", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "E_p(Bob_{priv},R2)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/682391126620ce67dffa1f33ee972f1b.png)<map id="dc928c892b0c31362bbfde7cc4e2025e18374729" name="dc928c892b0c31362bbfde7cc4e2025e18374729"></map>

The protocol described above works, but it takes a long time for Bob to authenticate Alice and for Alice to authenticate Bob. A faster authentication could be the following.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice, R2", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:R1,E_p(Bob_{priv},R2)", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "E_p(Alice_{priv},R1)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/d9c1e952550357ddf872d47493c46bd4.png)<map id="b006024cf4bbd5812ac6af177e9c613098b52e42" name="b006024cf4bbd5812ac6af177e9c613098b52e42"></map>

Alice sends her random nonce, \(R2\). Bob signs \(R2\) and sends his nonce : \(R1\). Alice signs \(R1\) and both are authenticated.

Now consider that Mallory wants to be authenticated as Alice. The above protocol has a subtle flaw that could be exploited by Mallory. This flaw can be exploited if Alice and Bob can act as both client and server. Knowing this, Mallory could operate as follows. Mallory starts an authentication with Bob faking himself as Alice. He sends a first message to Bob including Alice’s identity.

> ![msc {
> a [label="", linecolour=white],
> b [label="Mallory", textcolour="red", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Alice,RA", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:RB,E_p(Bob_{priv},RA)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/0129748f31a23c4c1ec10d426878fd81.png)<map id="b30892ea80e85d5226bffe1067bd495dabafda9e" name="b30892ea80e85d5226bffe1067bd495dabafda9e"></map>

In this exchange, Bob authenticates himself by signing the \(RA\) nonce that was sent by Mallory. Now, to authenticate as Alice, Mallory needs to compute the signature of nonce \(RB\) with Alice’s private key. Mallory does not know Alice’s key, but he could exploit the protocol to force Alice to perform the required computation. For this, Mallory can start an authentication to Alice as shown below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Mallory", textcolour="red",linecolour=black],
> z [label="", linecolour=white],
> c [label="Alice", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "I'm Mallory,RB", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "Challenge:RX,E_p(Alice_{priv},RB)", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/cd66745b390b62a38dfe355eb3a5cc12.png)<map id="48be047a9645ab6feb54e3f2716ac4f1e148d8e7" name="48be047a9645ab6feb54e3f2716ac4f1e148d8e7"></map>

In this example, Mallory has forced Alice to compute \(E_p(Alice_{priv},RB)\) which is the information required to finalize the first exchange and be authenticated as Alice. This illustrates a common problem with authentication schemes when the same information can be used for different purposes. The problem comes from the fact that Alice agrees to compute her signature on a nonce chosen by Bob (and relayed by Mallory). This problem occurs if the nonce is a simple integer without any structure. If the nonce includes some structure such as some information about Alice and Bob’s identities or even a single bit indicating whether the nonce was chosen by a user acting as a client (i.e. starting the authentication) or as a server, then the protocol is not vulnerable anymore.

To cope with some of the above mentioned problems, public-key cryptography is usually combined with certificates. A certificate is a data structure that includes a signature from a trusted third party. A simple explanation of the utilization of certificates is to consider that Alice and Bob both know Ted. Ted is trusted by these two users and both have stored Ted’s public key : \(Ted_{pub}\). Since they both know Ted’s key, he can issue certificates. A certificate is mainly a cryptographic link between the identity of a user and his/her public key. Such a certificate can be computed in different ways. A simple solution is for Ted to generate a file that contains the following information for each certified user :

> *   his/her identity
>     
>     
> *   his/her public key
>     
>     
> *   a hash of the entire file signed with Ted’s private key

Then, knowing Ted’s public key, anyone can verify the validity of a certificate. When a user sends his/her public key, he/she must also attach the certificate to prove the link between his/her identity and the public key. In practice, certificates are more complex than this. Certificates will often be used to authenticate the server and sometimes to authenticate the client.

A possible protocol could then be the following. Alice sends \(Cert(Alice_{pub},Ted)\). Bob replies with a random nonce.

> ![msc {
> a [label="", linecolour=white],
> b [label="Alice", linecolour=black],
> z [label="", linecolour=white],
> c [label="Bob", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "Cert(Alice_{pub},Ted)", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "R", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = ""] ,
> b>>c [ label = "E_p(Alice_{priv},R)", arcskip="1"];
> c=>d [ label = "" ];
> }](../Images/64eccedf6350698b6491b83ea9b24e73.png)<map id="3b6facf99117568ccbf6aa8408c70dddc156d855" name="3b6facf99117568ccbf6aa8408c70dddc156d855"></map>

Until now, we have only discussed the authentication problem. This is an important but not sufficient step to have a secure communication between two users through an insecure network. To securely exchange information, Alice and Bob need to both :

> *   mutually authenticate each other
>     
>     
> *   agree on a way to encrypt the messages that they will exchange

Let us first explore how this could be realized by using public-key cryptography. We assume that Alice and Bob have both a public-private key pair and the corresponding certificates signed by a trusted third party : Ted.

A possible protocol would be the following. Alice sends \(Cert(Alice_{pub},Ted)\). This certificate provides Alice’s identity and her public key. Bob replies with the certificate containing his own public key : \(Cert(Bob_{pub},Ted)\). At this point, they both know the other public key and could use it to send encrypted messages. Alice would send \(E_p(Bob_{pub},M1)\) and Bob would send \(E_p(Alice_{pub},M2)\). In practice, using public key encryption techniques to encrypt a large number of messages is inefficient because these cryptosystems require a large number of computations. It is more efficient to use secret key cryptosystems for most of the data and only use a public key cryptosystem to encrypt the random secret keys that will be used by the secret key encryption scheme.

## Key exchange[#](#key-exchange "Link to this heading")

When users want to communicate securely through a network, they need to exchange information such as the keys that will be used by an encryption algorithm even in the presence of an eavesdropper. The most widely used algorithm that allows two users to safely exchange an integer in the presence of an eavesdropper is the one proposed by Diffie and Hellman [[DH1976]](../bibliography.html#dh1976). It operates with (large) integers. Two of them are public, the modulus, p, which is prime and the base, g, which must be a primitive root of p. The communicating users select a random integer, \(a\) for Alice and \(b\) for Bob. The exchange starts as :

> *   Alice selects a random integer, \(a\) and sends \(A=g^{a} \mod p\) to Bob
>     
>     
> *   Bob selects a random integer, \(b\) and sends \(B=g^{b} \mod p\) to Alice
>     
>     
> *   From her knowledge of \(a\) and \(B\), Alice can compute \(Secret=B^{a} \mod p= (g^{b} \mod p) ^{a} \mod p=g^{a \times b} \mod p\)
>     
>     
> *   From is knowledge of \(b\) and \(A\), Bob can compute \(Secret=A^{b} \mod p=(g^{a} \mod p) ^{b} \mod p=g^{a \times b} \mod p\)

The security of this protocol relies on the difficulty of computing discrete logarithms, i.e. from the knowledge of \(A\) (resp. \(B\)), it is very difficult to extract \(\log(A)=\log(g^{a} \mod p)=a\) (resp. \(\log(B)=\log(g^{b} \mod p)=b\)).

An example of the utilization of the Diffie-Hellman key exchange is shown below. Before starting the exchange, Alice and Bob agree on a modulus (\(p=23\)) and a base (\(g=5\)). These two numbers are public. They are typically part of the standard that defines the protocol that uses the key exchange.

> *   Alice chooses a secret integer : \(a=8\) and sends \(A= g^{a} \mod p= 5^{8} \mod 23=16\) to Bob
>     
>     
> *   Bob chooses a secret integer : \(b=13\) and sends \(B= g^{b} \mod p=5^{13} \mod 23=21\) to Alice
>     
>     
> *   Alice computes \(S_{A}=B^{a} \mod p= 21^{8} \mod 23=3\)
>     
>     
> *   Bob computes \(S_{B}=A^{b} \mod p= 16^{13} \mod 23=3\)

Alice and Bob have agreed on the secret information \(3\) without having sent it explicitly through the network. If the integers used are large enough and have good properties, then even Eve who can capture all the messages sent by Alice and Bob cannot recover the secret key that they have exchanged. There is no formal proof of the security of the algorithm, but mathematicians have tried to solve similar problems with integers during centuries without finding an efficient algorithm. As long as the integers that are used are random and large enough, the only possible attack for Eve is to test all possible integers that could have been chosen by Alice and Bob. This is computationally very expensive. This algorithm is widely used in security protocols to agree on a secret key.

Unfortunately, the Diffie-Hellman key exchange alone cannot cope with man-in-the middle attacks. Consider Mallory who sits in the middle between Alice and Bob and can easily capture and modify their messages. The modulus and the base are public. They are thus known by Mallory as well. He could then operate as follows :

> *   Alice chooses a secret integer and sends \(A= g^{a} \mod p\) to Mallory
>     
>     
> *   Mallory generates a secret integer, \(m\) and sends \(M=g^{m} \mod p\) to Bob
>     
>     
> *   Bob chooses a secret integer and sends \(B=g^{b} \mod p\) to Mallory
>     
>     
> *   Mallory computes \(S_{A}=A^{m} \mod p\) and \(S_{B}=B^{m} \mod p\)
>     
>     
> *   Alice computes \(S_{A}=M^{a} \mod p\) and uses this key to communicate with Mallory (acting as Bob)
>     
>     
> *   Bob computes \(S_{B}=M^{b} \mod p\) and uses this key to communicate with Mallory (acting as Alice)

When Alice sends a message, she encrypts it with \(S_{A}\). Mallory decrypts it with \(S_{A}\) and encrypts the plaintext with \(S_{B}\). When Bob receives the message, he can decrypt it by using \(S_{B}\).

To safely use the Diffie-Hellman key exchange, Alice and Bob must use an authenticated exchange. Some of the information sent by Alice or Bob must be signed with a public key known by the other user. In practice, it is often important for Alice to authenticate Bob. If Bob has a certificate signed by Ted, the authenticated key exchange could be organized as follows.

> *   Alice chooses a secret integer : \(a\) and sends \(A= g^{a} \mod p\) to Bob
>     
>     
> *   Bob chooses a secret integer : \(b\), computes \(B= g^{b} \mod p\) and sends \(Cert(Bob,Bob_{pub},Ted), E_p(Bob_{priv},B)\) to Alice
>     
>     
> *   Alice checks the signature (with \(Bob_{pub}\)) and the certificate and computes \(S_{A}=B^{a} \mod p\)
>     
>     
> *   Bob computes \(S_{B}=A^{b} \mod p\)

This prevents the attack mentioned above since Mallory cannot create a fake certificate and cannot sign a value by using Bob’s private key. Given the risk of man-in-the-middle attacks, the Diffie-Hellman key exchange mechanism should never be used without authentication.

 ## The secure shell (ssh)[#](#the-secure-shell-ssh "Link to this heading")

One of the initial motivations for building computer networks was to allow users to access remote computers over the networks. In the 1960s and 1970s, the mainframes and the emerging minicomputers were composed of a central unit and a set of terminals connected through serial lines or modems. The simplest protocol that was designed to access remote computers over a network is probably [telnet](../glossary.html#term-telnet) [**RFC 854**](https://datatracker.ietf.org/doc/html/rfc854.html). [telnet](../glossary.html#term-telnet) runs over TCP and a telnet server listens on port 23 by default. The TCP connection used by telnet is bidirectional, both the client and the server can send data over it. The data exchanged over such a connection is essentially the characters that are typed by the user on the client machine and the text output of the processes running on the server machine with a few exceptions (e.g. control characters, characters to control the terminal like VT-100, …) . The default character set for telnet is the ASCII character set, but the extensions specified in [**RFC 5198**](https://datatracker.ietf.org/doc/html/rfc5198.html) support the utilization of Unicode characters.

From a security viewpoint, the main drawback of [telnet](../glossary.html#term-telnet) is that all the information, including the usernames, passwords and commands, is sent in cleartext over a TCP connection. This implies that an eavesdropper could easily capture the passwords used by anyone on an unprotected network. Various software tools exist to automate this collection of information. For this reason, [telnet](../glossary.html#term-telnet) is rarely used today to access remote computers. It is usually replaced by [ssh](../glossary.html#term-ssh) or similar protocols.

The secure shell protocol was designed in the mid 1990s by T. Ylonen to counter the eavesdropping attacks against [telnet](../glossary.html#term-telnet) and similar protocols [[Ylonen1996]](../bibliography.html#ylonen1996). [ssh](../glossary.html#term-ssh) became quickly popular and system administrators encouraged its usage. The original version of [ssh](../glossary.html#term-ssh) was freely available. After a few years, his author created a company to distribute it commercially, but other programmers continued to develop an open-source version of [ssh](../glossary.html#term-ssh) called [OpenSSH](http://www.openssh.com). Over the years, [ssh](../glossary.html#term-ssh) evolved and became a flexible applicable whose usage extends beyond remote login to support features such as file transfers, protocol tunneling, … In this section, we only discuss the basic features of [ssh](../glossary.html#term-ssh) and explain how it differs from [telnet](../glossary.html#term-telnet). Entire books have been written to describe [ssh](../glossary.html#term-ssh) in details [[BS2005]](../bibliography.html#bs2005). An overview of the protocol appeared in [[Stallings2009]](../bibliography.html#stallings2009).

The [ssh](../glossary.html#term-ssh) protocol runs directly above the TCP protocol. Once the TCP bytestream has been established, the client and the server exchange messages. The first message exchanged is an ASCII line that announces the version of the protocol and the version of the software implementation used by the client and the server. These two lines are useful when debugging interoperability problems and other issues.

The next message is the `SSH_MSG_KEX_INIT` message that is used to negotiate the cryptographic algorithms that will be used for the `ssh` session. It is very important for security protocols to include mechanisms that enable a negotiation of the cryptographic algorithms that are used. First, these algorithms provide different levels of security. Some algorithms might be considered totally secure and are recommended today while they could become deprecated a few years later after the publication of some attacks. Second, these algorithms provide different levels of performance and have different CPU and memory impacts.

In practice, an `ssh` implementation supports four types of cryptographic algorithms :

> *   key exchange
>     
>     
> *   encryption
>     
>     
> *   Message Authentication Code (MAC)
>     
>     
> *   compression

The [IANA](https://www.iana.org) maintains a [list of the cryptographic algorithms](http://www.iana.org/assignments/ssh-parameters/ssh-parameters.xhtml#ssh-parameters-16) that can be used by `ssh` implementations. For each type of algorithm, the client provides an ordered list of the algorithms that it supports and agrees to use. The server compares the received list with its own list. The outcome of the negotiation is a set of four algorithms [[4]](#fnull) that will be combined for this session.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "SSH-clientP-clientS comments", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "SSH-serverP-serverS comments", arcskip="1"];
> b=>a [ label = "" ];
> 
> a=>b [ label = "" ] ,
> b>>c [ label = "SSH_MSG_KEX_INIT", arcskip="1"];
> c=>d [ label = "" ];
> 
> d=>c [ label = "" ] ,
> c>>b [ label = "SSH_MSG_KEX_INIT", arcskip="1"];
> b=>a [ label = "" ];
> }](../Images/6c5012af9e2d96b7ddb2c31a55acbaec.png)<map id="6ae6d4bce6070be0fd3cb65a88d797c93f775d1d" name="6ae6d4bce6070be0fd3cb65a88d797c93f775d1d"></map>

This negotiation of the cryptographic algorithms allows the implementations to evolve when new algorithms are proposed. If a client is upgraded, it can announce a new algorithm as its preferred one even if the server is not yet upgraded.

Once the cryptographic algorithms have been negotiated, the key exchange algorithm is used to negotiate a secret key that will be shared by the client and the server. These key exchange algorithms include some variations over the basic algorithms. As an example, let us analyze how the Diffie-Hellman key exchange algorithm is used within the `ssh` protocol. In this case, each host has both a private and a public key. (Note that \(g\) is a generator for the subgroup of the Galois field of order \(p\), where \(p\) is a prime number, and || is the concatenation operator. For additional background information, see [[Schneier1996]](../bibliography.html#schneier1996).)

> *   the client generates the random number \(a\) and sends \(A=g^{a} \mod p\) to the server
>     
>     
> *   the server generates the random number \(b\). It then computes \(B=g^{b} \mod p\), \(K=B^{a} \mod p\) and signs with its private key \(hash(V_{Client} || V_{Server} || KEX\_INIT_{Client} || KEX\_INIT_{Server} || Server_{pub} || A || B || K )\) where \(V_{Server}\) (resp. \(V_{Client}\)) is the initial messages sent by the client (resp. server), \(KEX\_INIT_{Client}\) (resp. \(KEX\_INIT_{Server}\)) is the key exchange message sent by the client (resp. server) and \(A\), \(B\) and \(K\) are the messages of the Diffie-Hellman key exchange
>     
>     
> *   the client can recompute \(K=A^{b} \mod p\) and verify the signature provided by the server

This is a slightly modified authenticated Diffie-Hellman key exchange with two interesting points. The first point is that when the server authenticates the key exchange it does not provide a certificate. This is because `ssh` assumes that the client will store inside its cache the public key of the servers that it uses on a regular basis. This assumption is valid for a protocol like `ssh` because users typically use it to interact with a small number of servers, typically a few or a few tens. Storing this information does not require a lot of storage. In practice, most `ssh` clients will accept to connect to remote servers without knowing their public key before the connection. In this case, the client issues a warning to the user who can decide to accept or reject the key. This warning can be associated with a fingerprint of the key, either as a sequence of letters or as an ASCII art which can be posted on the web or elsewhere [[5]](#fdnsssh) by the system administrator of the server. If a client connects to a server whose public key does not match the stored one, a stronger warning is issued because this could indicate a man-in-the-middle attack or that the remote server has been compromised. It can also indicate that the server has been upgraded and that a new key has been generated during this upgrade.

The second point is that the server authenticates not only the result of the Diffie-Hellman exchange but also a hash of all the information sent and received during the exchange. This is important to prevent downgrade attacks. A downgrade attack is an attack where an active attacker modifies the messages sent by the communicating hosts (typically the client) to request the utilization of weaker encryption algorithms. Consider a client that supports two encryption schemes. The preferred one uses 128 bits secret keys and the second one is an old encryption scheme that uses 48 bits keys. This second algorithm is kept for backward compatibility with older implementations. If an attacker can remove the preferred algorithm from the list of encryption algorithms supported by the client, he can force the server to use a weaker encryption scheme that will be easier to break. Thanks to the hash that covers all the messages exchanged by the server, the downgrade attack cannot occur against `ssh`. Algorithm agility is a key requirement for security protocols that need to evolve when encryption algorithms are broken by researchers. This agility cannot be used without care and signing a hash of all the messages exchanged is a technique that is frequently used to prevent downgrade attacks.

Note

Single use keys

Thanks to the Diffie-Hellman key exchange, the client and the servers share key \(K\). A naive implementation would probably directly use this key for all the cryptographic algorithms that have been negotiated for this session. Like most security protocols, `ssh` does not directly use key \(K\). Instead, it uses the negotiated hash function with different parameters [[6]](#fsshkeys) to allow the client and the servers to compute six keys from \(K\) :

> *   a key used by the client (resp. server) to encrypt the data that it sends
>     
>     
> *   a key used by the client (resp. server) to authenticate the data that it sends
>     
>     
> *   a key used by the client (resp. server) to initialize the negotiated encryption scheme (if required by this scheme)

It is common practice among designers of security protocols to never use the same key for different purposes. For example, allowing the client and the server to use the same key to encrypt data could enable an attacker to launch a replay attack by sending to the client data that it has itself encrypted.

At this point, all the messages sent over the TCP connection will be encrypted with the negotiated keys. The `ssh` protocol uses messages that are encoded according to the Binary Packet Protocol defined in [**RFC 4253**](https://datatracker.ietf.org/doc/html/rfc4253.html). Each of these messages contains the following information :

> *   `length` : this is the length of the message in bytes, excluding the MAC and length fields
>     
>     
> *   `padding length` : this is the number of random bytes that have been added at the end of the message.
>     
>     
> *   `payload` : the data (after optional compression) passed by the user
>     
>     
> *   `padding` : random bytes added in each message (at least four) to ensure that the message length is a multiple of the block size used by the negotiated encryption algorithm
>     
>     
> *   `MAC` : this field is present if a Message Authentication Code has been negotiated for the session (in practice, using `ssh` without authentication is risky and this field should always be present). Note that to compute the MAC, an `ssh` implementation must maintain a message counter. This counter is incremented by one every time a message is sent and the MAC is computed with the negotiated authentication algorithm using the MAC key over the concatenation of the message counter and the cleartext message. The message counter is not transmitted, but the recipient can easily recover its value. The `MAC` is computed as \(mac = MAC(key, sequence\_number || unencrypted\_message)\) where the key is the negotiated authentication key.

Note

Authenticating messages with HMAC

ssh is one example of a protocol that uses Message Authentication Codes (MAC) to authenticates the messages that are sent. A naive implementation of such a MAC would be to simply use a hash function like SHA-1\. However, such a construction would not be safe from a security viewpoint. Internet protocols usually rely on the HMAC construction defined in [**RFC 2104**](https://datatracker.ietf.org/doc/html/rfc2104.html). It works with any hash function (H) and a key (K). As an example, let us consider HMAC with the SHA-1 hash function. SHA-1 uses 20 bytes blocks and the block size will play an important role in the operation of HMAC. We first require the key to be as long as the block size. Since this key is the output of the key generation algorithm, this is one parameter of this algorithm.

HMAC uses two padding strings : ipad (resp. opad) which is a string containing 20 times byte `0x36` (resp. byte `0x5C`). The HMAC is then computed as \(H[K \oplus opad, H(K \oplus ipad, data) ]\) where \(\oplus\) denotes the bitwise XOR operation. This computation has been shown to be stronger than the naive \(H(K,data)\) against some types of cryptographic attacks.

Among the various features of the `ssh` protocol, it is interesting to mention how users are authenticated by the server. The `ssh` protocol supports the classical username/password authentication (but both the username and the password are transmitted over the secure encrypted channel). In addition, `ssh` supports two authentication mechanisms that rely on public keys. To use the first one, each user needs to generate his/her own public/private key pair and store the public key on the server. To be authenticated, the user needs to sign a message containing his/her public key by using his/her private key. The server can easily verify the validity of the signature since it already knows the user’s public key. The second authentication scheme is designed for hosts that trust each other. Each host has a public/private key pair and stores the public keys of the other hosts that it trusts. This is typically used in environments such as university labs where each user could access any of the available computers. If Alice has logged on `computer1` and wants to execute a command on `computer2`, she can create an `ssh` session on this computer and type (again) her password. With the host-based authentication scheme, `computer1` signs a message with its private key to confirm that it has already authenticated Alice. `computer2` would then accept Alice’s session without asking for her credentials.

The `ssh` protocol includes other features that are beyond the scope of this book. Additional details may be found in [[BS2005]](../bibliography.html#bs2005).

 ## Transport Layer Security[#](#transport-layer-security "Link to this heading")

The Transport Layer Security family of protocols were initially proposed under the name Secure Socket Layer (SSL). The first deployments used this name and many researchers still refer to this security protocol as SSL [[FKC1996]](../bibliography.html#fkc1996). In this chapter, we use the official name that was standardized by the IETF: TLS for Transport Layer Security.

The TLS protocol was designed to be usable by a wide range of applications that use the transport layer to reliably exchange information. TLS is mainly used over the TCP protocol. There are variants of TLS that operate over SCTP [**RFC 3436**](https://datatracker.ietf.org/doc/html/rfc3436.html) or UDP [**RFC 6347**](https://datatracker.ietf.org/doc/html/rfc6347.html), but these are outside the scope of this chapter.

A TLS session operates over a TCP connection. TLS is responsible for the encryption and the authentication of the SDUs exchanged by the application layer protocol while TCP provides the reliable delivery of this encrypted and authenticated bytestream. TLS is used by many different application layer protocols. The most frequent ones are HTTP (HTTP over TLS is called HTTPS), SMTP [**RFC 3207**](https://datatracker.ietf.org/doc/html/rfc3207.html) or POP and IMAP [**RFC 2595**](https://datatracker.ietf.org/doc/html/rfc2595.html), but proprietary application-layer protocols also use TLS [[AM2019]](../bibliography.html#am2019).

A TLS session can be initiated in two different ways. First, the application can use a dedicated TCP port number for application layer protocol x-over-TLS. This is the solution used by many HTTP servers that reserve port \(443\) for HTTP over TLS. This solution works, but it requires to reserve two ports for each application : one where the application-layer protocol is used directly over TCP and another one where the application-layer protocol is used over TLS. Given the limited number of TCP ports that are available, this is not a scalable solution. The table below provides some of the reserved port numbers for application layer protocols on top of TLS.

| Application | TCP port | TLS port |
| --- | --- | --- |
| POP3 | 110 | 995 |
| IMAP | 143 | 993 |
| NNTP | 119 | 563 |
| HTTP | 80 | 443 |
| FTP | 21 | 990 |

A second approach to initiate a TLS session is to use the standard TCP port number for the application layer protocol and define a special message in this protocol to trigger the start of the TLS session. This is the solution used for SMTP with the `STARTTLS` message. This extension to SMTP [**RFC 3207**](https://datatracker.ietf.org/doc/html/rfc3207.html) defines the new STARTTLS command. The client can issue this command to indicate to the server that it wants to start a TLS session as shown in the example below captured during a session on port 25.

[PRE3]

In the remaining parts of this chapter, we assume that the TLS session starts immediately after the establishment of the TCP connection. This corresponds to the deployments on web servers. We focus our presentation of TLS on this very popular use case. TLS is a complex protocol that supports other features than the one used by web servers. A more detailed presentation of TLS may be found in [[KPS2002]](../bibliography.html#kps2002) and [[Ristic2015]](../bibliography.html#ristic2015).

A TLS session is divided in two phases: the handshake and the data transfer. During the handshake, the client and the server negotiate the security parameters and the keys that will be used to secure the data transfer. During the second phase, all the messages exchanged are encrypted and authenticated with the negotiated algorithms and keys.

### The TLS handshake[#](#the-tls-handshake "Link to this heading")

When used to interact with a regular web server, the TLS handshake has three important objectives:

> 1.  Securely negotiate the cryptographic algorithms that will be used by the client and the server over the TLS session
>     
>     
> 2.  Verify that the client interacts with a valid server
>     
>     
> 3.  Securely agree on the keys that will be used to encrypt and authenticate the messages exchanged over the TLS session

The TLS handshake is a four-way handshake illustrated in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random], Certificate", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "E(K,MasterSecret), Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> b>>c [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> }](../Images/300581e55a5559d21fa138b6759650ca.png)<map id="6851383dfd7cb6f84e42dde9992fdd9303de35d3" name="6851383dfd7cb6f84e42dde9992fdd9303de35d3"></map>

In a nutshell, the client starts the TLS handshake by proposing a random nonce. The server replies with its random nonce and a certificate that binds its name to a public key. The client generates a MasterSecret that will be used later to derive the session keys and encrypts it with the public key of the server. It also generates a Finished message that contains a MAC of all the messages exchanged to allow the server to detect any modification of the messages sent by the client. The server also sends its own Finished message. At that point, the client and the server sent encrypted records thanks to the keys derived from the MasterSecret.

Let us first discuss the negotiation of the cryptographic algorithms and parameters. Like all security protocols, TLS includes some agility in its design since new cryptographic algorithms appear over the years and some older algorithms become deprecated once cryptanalysts find flaws. The TLS handshakes starts with the `ClientHello` message that is sent by the client. This message carries the following information :

> *   Protocol version number: this is the version of the TLS protocol supported by the client. The server should use the same version of the TLS protocol as the client, but may opt for an older version. Both versions 1.2 and 1.3 of TLS are deployed today. Older versions are being deprecated.
>     
>     
> *   Random number: security protocols rely on random numbers. The client sends a 32 bytes long random number where usually four of these bytes correspond to the client’s clock. This random number is used, together with the server’s random number, as a seed to generate the security keys.
>     
>     
> *   Cipher suites : this ordered list contains the set of cryptographic algorithms that are supported by the client, with the most preferred one listed first. In contrast with `ssh` that allows negotiating independent algorithms for encryption, key exchange and authentication, TLS relies on suites that combine these algorithms together. Many cryptographic suites have been defined for TLS. Various recommendations have been published on the security of some of these suites [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Compression algorithm : the client may propose the utilization of a specific compression algorithm (e.g. zlib). In theory, compressing the data before encrypting it is an intelligent way to reduce the amount of data exchanged. Unfortunately, its implementation in TLS has caused several security problems [[PHG2013]](../bibliography.html#phg2013). For this reason, compression is usually disabled in TLS [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Extensions : TLS supports various extensions in the `ClientHello` message. These extensions [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) are important to allow the protocol to evolve, but many of them go beyond the scope of this chapter.

Note

The `Server Name Indication (SNI)`

The `Server Name Indication (SNI)` extension defined in [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) is an important TLS extension for web servers. It is used by the client to indicate the name of the server that it wishes to contact. The IP address associated to this name has been queried from the DNS and used to establish the TCP connection. Why should the client indicate the server name in the TLS `ClientHello` ? The motivation is the same as for the `Host` header line in HTTP/1.0\. With the SNI extension, a single TLS server can support several web sites that use different domain names. Thanks to the SNI extension, the server knows the concerned domain name at the start of the TLS session. Without this extension, hosting providers would have been forced use one IP address per TLS-enabled server.

The server replies to the `ClientHello` with several messages:

> *   the `ServerHello` message that contains the protocol version chosen by the server (assumed to be the same as the client version in this chapter), the 32 random bytes chosen by the server, the Cipher Suite selected by the server from the list advertised by the client and a Session Id. This Session Id is an identifier which is chosen by the server. It identifies the TLS session and the security parameters (algorithms and keys) negotiated for this session. It is used to support session resumption.
>     
>     
> *   the `Certificate` message provides the certificate (or usually a chain of certificates) that binds a domain name to the public key used by the server. TLS uses the server certificates to authenticate the server. It relies on a Public Key Infrastructure that is composed of a set of root certification authorities that issue certificates to certification authorities that in the end issue certificates to servers. TLS clients are usually configured with the public keys of several root certification authorities and use this information to validate the certificates that they receive from servers. For historical reasons, the TLS certificates are encoded in ASN.1 format. The details of the ASN.1 syntax [[Dubuisson2000]](../bibliography.html#dubuisson2000) are outside the scope of this book.
>     
>     
> *   the `ServerKeyExchange` message is used by the server to transmit the information that is required to perform the key exchange. The content of this message is function of the selected key exchange algorithm.
>     
>     
> *   the `ServerHelloDone` indicates that the server has sent all the messages for the first phase of the handshake.

At this point, it is time to describe the TLS key exchange. TLS supports different key exchange mechanisms that can be negotiated as part of the selection of the cipher suite. We focus on two of them to highlight their differences:

> *   `RSA`. This key exchange algorithm uses the encryption capabilities of the RSA public-key algorithm. The client has validated the server’s public key thanks to the `Certificate` message. It then generates a (48 bytes) random number, encrypts it with the server public key and sends the encrypted number to the server in the `ClientKeyExchange` message. The server uses its private key to decrypt the random number. At this point, the client and the server share the same (48 bytes long) secret and use it to derive the secret keys required to encrypt and authenticate data in the second phase. With this key exchange algorithm, the server does not need to send a `ServerKeyExchange` message.
>     
>     
> *   `DHE_RSA`. This key exchange algorithm is the Ephemeral Diffie Hellman key exchange with RSA signatures to authenticate the key exchange. It operates as a classical authenticated Diffie Hellman key exchange. If this key exchange has been selected by the server, it sends its Diffie Hellman parameters in the `ServerKeyExchange` message and signs them with its private key. The client then continues the key exchange and sends the results of its own computation in the `ClientKeyExchange` message. `DHE_RSA` is thus an authenticated Diffie Hellman key exchange where the initial message is sent by the server (instead of the client as in our first example but since the protocol is symmetric, this does not matter).

An important difference between `DHE_RSA` and `RSA` is their reaction against attacks. `DHE_RSA` is considered by many to be stronger than `RSA` because it supports Perfect Forward Secrecy. This property is important against attackers that are able to eavesdrop all the (encrypted) data sent and received by a server. Consider that Terrence is such an attacker that has stored all the packets exchanged by Bob’s server during the last six months. If he manages, by any means, to obtain Bob’s private key, he will be able to decrypt all the keys used to secure the TLS sessions with Bob’s server during this period. With `DHE_RSA`, a similar attack is less devastating. If Terrence knows Bob’s private key, he will be able to launch a man-in-the-middle attack against future TLS sessions with Bob’s server. However, he will not be able to recover the keys used for all the past sessions that he captured.

Note

Perfect Forward Secrecy

Perfect Forward Secrecy (PFS) is an important property for key exchange protocols. A protocol provides PFS if its design guarantees that the keys used for former sessions will not be compromised even if the private key of the server is compromised. This is a very important property. `DHE_RSA` provides Perfect Forward Secrecy, but the `RSA` key exchange does not provide this property. In practice, `DHE_RSA` is costly from a computational viewpoint. Recent implementations of TLS thus prefer `ECDHE_RSA` or `ECDHE_ECDSA` when Perfect Forward Secrecy is required.

All the information required for the key exchange has now been transmitted. There are two important messages that will be sent by the client and the server to conclude the handshake and start the data transfer phase.

The client sends the `ChangeCipherSpec` message followed by the `Finished` message. The `ChangeCipherSpec` message indicates that the client has received all the information required to generate the security keys for this TLS session. This messages can also appear later in the session to indicate a change in the encryption algorithms that are used, but this usage is outside the scope of this book. The `Finished` message is more important. It confirms to the server that the TLS handshake has been performed correctly and that no attacker has been able to modify the data sent by the client or the server. This is the first message that is encrypted with the selected security keys. It contains a hash of all the messages that were exchanged during the handshake.

The server also sends a `ChangeCipherSpec` message followed by a `Finished` message.

Note

TLS Cipher suites

A TLS cipher suite is usually represented as an ASCII string that starts with TLS and contains the acronym of the key exchange algorithm, the encryption scheme with the key size and its mode of operation and the authentication algorithm. For example, `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256` is a TLS cipher suite that uses the `DHE_RSA` key exchange algorithm with 128 bits AES in GCM mode for encryption and SHA-256 for authentication. The official list of TLS cipher suites is maintained by IANA [[7]](#fianatls). The NULL acronym indicates that no algorithm has been specified. For example, `TLS_ECDH_RSA_WITH_NULL_SHA` is a cipher suite that does not use any encryption but still uses the `ECDH_RSA` key exchange and `SHA` for authentication.

### The TLS record protocol[#](#the-tls-record-protocol "Link to this heading")

The handshake is now finished. The client and the server will exchange authenticated and encrypted records. TLS defines different formats for the records depending on the cryptographic algorithms that have been negotiated for the session. A detailed discussion of these different types of records is outside the scope of this introduction. For illustration, we briefly describe one record format.

As other security protocols, TLS uses different keys to encrypt and authenticate records. These keys are derived from the MasterSecret that is either randomly generated by the client after the `RSA` key exchange or derived from the Diffie Hellman parameters after the `DH_RSA` key exchange. The exact algorithm used to derive the keys is defined in [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html).

A TLS record is always composed of four different fields :

> *   a Type that indicates the type of record. The most frequent type is application data which corresponds to a record containing encrypted data. The other types are handshake, change_cipher_spec and alert.
>     
>     
> *   a Protocol Version field that indicates the version of the TLS protocol used. This version is composed of two sub fields : a major and a minor version number.
>     
>     
> *   a Length field. A TLS record cannot be longer than 16,384 bytes.
>     
>     
> *   a TLSPlainText that contains the encrypted data

TLS supports several methods to encrypted records. The selected method depends on the cryptographic algorithms that have been negotiated for the TLS session. A detailed presentation of the different methods that can be used to produce the TLSPlainText from the user data is outside the scope of this book. As an example, we study one method: Stream Encryption. This method is used with cryptographic algorithms which can operate on a stream of bytes. The method starts with a sequence of bytes provided by the user application: the plain text. The first step is to compute the authentication code to verify the integrity of the data. For this, TLS computes \(MAC(SeqNum, Header, PlainText)\) using HMAC where SeqNum is a sequence number which is incremented by one for each new TLS record transmitted. The Header is the header of the TLS record described above and PlainText is the information that needs to be encrypted. Note that the sequence number is maintained at the two endpoints of the TLS session, but it is not transmitted inside the TLS record. This sequence number is used to prevent replay attacks.

Note

MAC-then-encrypt or Encrypt-then-MAC

When secure protocols use Message Authentication and Encryption, they need to specify how these two algorithms are combined. A first solution, which is used by the current version of TLS, is to compute the authentication code and then encrypt both the data and the authentication code. A drawback of this approach is that the receiver of an encrypted TLS record must first attempt to decrypt data that has potentially been modified by an attacker before being able to verify the authenticity of the record. A better approach is for the sender to first encrypt the data and then compute the authentication code over the encrypted data. This is the encrypt-then-MAC approach proposed in [**RFC 7366**](https://datatracker.ietf.org/doc/html/rfc7366.html). With encrypt-then-MAC, the receiver first checks the authentication code before attempting to decrypt the record.

### Improving TLS[#](#improving-tls "Link to this heading")

During the last two decades, the deployment of TLS has continued to grow. The early TLS servers were only used for critical services such as e-commerce websites or online banks. As CPU performance improved, it became much more cost-effective to use TLS to secure non-critical parts of web servers, including the delivery of HTML pages and even video services. There is now a growing number of applications that rely on TLS [[AM2019]](../bibliography.html#am2019).

In 2013, the statistics collected by the Firefox Telemetry project [[9]](#ftelemetry) revealed that 30% of the web pages loaded by Firefox users were done over HTTPS. In October 2019, 80% of the web pages are loaded over HTTPS. In six years, HTTPS became the dominant protocol to access web services. Another look at the deployment of HTTPS on web sites may be found in [[Helme2019]](../bibliography.html#helme2019).

Measurement studies that analyzed the evolution of TLS over the years have identified several important changes in the TLS ecosystem [[KRA2018]](../bibliography.html#kra2018). First, the preferred cryptographic algorithms have changed. While RC4 was used by 60% of the connections in 2012, its usage has dropped since 2015\. AES started to be deployed in 2013 and is now used for more than 90% of the connections. The deployed versions of TLS have also changed. TLS 1.0 and TLS 1.1 are now rarely used. The deployment of TLS 1.2 started in 2013 and reached 70% of the connections in 2015\. Version 1.3 of TLS, that is described below, is also widely deployed.

Another interesting fact is the key exchange schemes. In 2012, RSA was the dominant solution, used by more than 80% of the observed connections [[KRA2018]](../bibliography.html#kra2018). In 2013, Edward Snowden revealed the surveillance activities of several governments. These revelations had a huge impact on the Internet community. The IETF, which standardizes Internet protocols, considered in [**RFC 7258**](https://datatracker.ietf.org/doc/html/rfc7258.html) that such pervasive monitoring was an attack. Since then, several IETF working groups have developed solutions to counter pervasive monitoring. One of these solutions is to encourage Perfect Forward Security. Within TLS, this implies replacing RSA by an authenticated Diffie Hellman key exchange such as ECDHE. Measurements indicate that since summer 2014, ECDHE is more popular than RSA. In 2018, more than 90% of the observed TLS connections used ECDHE.

The last point is the difficulty of deploying TLS servers [[KMS2017]](../bibliography.html#kms2017). When TLS servers are installed, the system administrator needs to obtain certificates and configure a range of servers. Initially, getting certificates was complex and costly, but initiatives such as [https://letsencrypt.org](https://letsencrypt.org) have simplified this workflow.

In 2014, the IETF TLS working started to work on the development of version 1.3 of the TLS protocol. Their main objectives [[Rescorla2015]](../bibliography.html#rescorla2015) for this new version were:

> *   simplify the design by removing unused or unsafe protocol features
>     
>     
> *   improve the security of TLS by leveraging the lessons learned from TLS 1.2 and some documented attacks
>     
>     
> *   improve the privacy of the protocol
>     
>     
> *   reduce the latency of TLS

Since 2014, latency has become an important concern for web services. As access networks bandwidth continue to grow, latency is becoming a key factor that affects the performance of interactive web services. With TLS 1.2, the download of a web page requires a minimum of four round-trip-times, one to create the underlying TCP connection, one to exchange the ClientHello/ServerHello, one to exchange the keys and then one to send the HTTP GET and retrieve the response. This can be very long when the server is not near the client. TLS 1.3 aimed at reducing this handshake to one round-trip-time and even zero by placing some of the cryptographic handshake in the TCP handshake. This part will be discussed in the TCP chapter. We focus here on the reducing the TLS handshake to a single round-trip-time.

To simplify both the design and the implementations, TLS 1.3 uses only a small number of cipher suites. Five of them are specified in [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html) and `TLS_AES_128_GCM_SHA256` must be supported by all implementations. To ensure privacy, all cipher suites that did not provide Perfect Forward Secrecy have been removed. Compression has also been removed from TLS since several attacks on TLS 1.2 exploited its compression capability [**RFC 7457**](https://datatracker.ietf.org/doc/html/rfc7457.html).

Note

Enterprises, privacy and TLS

By supporting only cipher suites that provide Perfect Forward Secrecy in TLS 1.3, the IETF aims at protecting the privacy of users against a wide range of attacks. However, this choice has resulted in intense debates in some enterprises. Some enterprises, notably in financial organizations, have deployed TLS, but wish to be able to decrypt TLS traffic for various security-related activities. These enterprises tried to lobby within the IETF to maintain RSA-based cipher suites that do not provide Perfect Forward Secrecy. Their arguments did not convince the IETF. Eventually, these enterprises moved to ETSI, another standardization body, and convinced them to adopt entreprise TLS, a variant of TLS 1.3 that does not provide Perfect Forward Secrecy [[eTLS2018]](../bibliography.html#etls2018).

The TLS 1.3 handshake differs from the TLS 1.2 handshake in several ways. First, the TLS 1.3 handshake requires a single round-trip-time when the client connects for the first time to a server. To achieve this, the TLS designers look at the TLS 1.2 handshake in details and found that the first round-trip-time is mainly used to select the set of cryptographic algorithms and the cryptographic exchange scheme that will be used over the TLS session. TLS 1.3 drastically simplifies this negotiation by requiring to use the Diffie Hellman exchange with a small set of possible parameters. This means that the client can guess the parameters used by the server (i.e. the modulus, p and the base g) and immediately start the Diffie Hellman exchange. A simplified version of the TLS 1.3 handshake is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/e5ae4c8c184c7b414c018f2972f772de.png)<map id="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0" name="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0"></map>

There are several important differences with the TLS 1.2 handshake. First, the Diffie Hellman key exchange is required in TLS 1.3 and this exchange is initiated by the client (before having validated the server identity). To initiate the Diffie Hellman key exchange, the client needs to guess the modulus and the base that can be accepted by the server. Either the client uses standard parameters that most server supports or the client remembers the last modulus/base that it used with this particular server. If the client guessed incorrectly, the server replies with the parameters that it expects and one round-trip-time is lost. When the server sends its ServerHello, it already knows the session key. This implies that the server can encrypt all subsequent messages. After one round-trip-time, all data exchanged over the TLS 1.3 session is encrypted and authenticated. In TLS 1.3, the server certificate is encrypted with the session key, as well as the Finished message. The server signs the handshake to confirm that it owns the public key of its certificate. If the server wants to send application data, it can already encrypt it and send it to the client. Upon reception of the server Certificate, the client verifies it and checks the signature of the handshake and the Finished message. The client confirms the end of the handshake by sending its own Finished message. At that time, the client can send encrypted data. This means that the client only had to wait one round-trip-time before sending encrypted data. This is much faster than with TLS 1.2.

For some applications, waiting one round-trip-time before being able to send data is too long. TLS 1.3 allows the client to send encrypted data immediately after the ClientHello, without having to wait for the ServerHello message. At this point in the handshake, the client cannot know the key that will be derived by the Diffie Hellman key exchange. The trick is that the server and the client need to have previously agreed on a pre-shared-key. This key could be negotiated out of band, but usually it was exchanged over a previous TLS session between the client and the server. Both the client and the server can store this key in their cache. When the client creates a new TLS session to a server, it checks whether it already knows a pre-shared key for this server. If so, the client announces the identifier of this key in its ClientHello message. Thanks to this identifier, the server can recover the key and use it to decrypt the 0-rtt Encrypted record. A simplified version of the 0-rtt TLS 1.3 handshake [[8]](#fhandshake) is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/37315cb85580d2a0a41ccf2aa6b695ae.png)<map id="04ec071344c2fae0632e2d690c2782c9017b631e" name="04ec071344c2fae0632e2d690c2782c9017b631e"></map>

On the web, TLS clients use certificates to authenticate servers but the clients are not authenticated. However, there are environments such as enterprise networks where servers may need to authenticate clients as well. A popular deployment is to authenticate remote clients who wish to access the enterprise network through a Virtual Private Network service. Some of these services run above TLS (or more precisely a variant of TLS named DTLS that runs above UDP [[MoR2004]](../bibliography.html#mor2004) but is outside the scope of this chapter). In such services, each client is authenticated thanks to a public key and a certificate that is trusted by the servers. To establish a TLS session, such a client needs to prove that it owns the public key associated with the certificate. This is done by the server thanks to the CertificateRequest message. The TLS handshake becomes the following one:

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> 
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "CertificateRequest, Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Certificate, Sign(Kc, Handshake), Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/6b610706117d1538fe4793f16be928b1.png)<map id="36d37bda46d72f2f438e0e029b02b9c54e69fa82" name="36d37bda46d72f2f438e0e029b02b9c54e69fa82"></map>

The server sends a CertificatRequest message. The client returns its certificate and signs the Handshake with is private key. This confirms to the server that the client owns the public key indicated in its certificate.

There are many more differences between TLS 1.2 and TLS 1.3\. Additional details may be found in their respective specifications, [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html) and [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html).

### The TLS handshake[#](#the-tls-handshake "Link to this heading")

When used to interact with a regular web server, the TLS handshake has three important objectives:

> 1.  Securely negotiate the cryptographic algorithms that will be used by the client and the server over the TLS session
>     
>     
> 2.  Verify that the client interacts with a valid server
>     
>     
> 3.  Securely agree on the keys that will be used to encrypt and authenticate the messages exchanged over the TLS session

The TLS handshake is a four-way handshake illustrated in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random], Certificate", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "E(K,MasterSecret), Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Finished=MAC(MasterSecret||Handshake)", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> b>>c [ label = "Encrypted Record", linecolour="red", textcolour="red"];
> }](../Images/300581e55a5559d21fa138b6759650ca.png)<map id="6851383dfd7cb6f84e42dde9992fdd9303de35d3" name="6851383dfd7cb6f84e42dde9992fdd9303de35d3"></map>

In a nutshell, the client starts the TLS handshake by proposing a random nonce. The server replies with its random nonce and a certificate that binds its name to a public key. The client generates a MasterSecret that will be used later to derive the session keys and encrypts it with the public key of the server. It also generates a Finished message that contains a MAC of all the messages exchanged to allow the server to detect any modification of the messages sent by the client. The server also sends its own Finished message. At that point, the client and the server sent encrypted records thanks to the keys derived from the MasterSecret.

Let us first discuss the negotiation of the cryptographic algorithms and parameters. Like all security protocols, TLS includes some agility in its design since new cryptographic algorithms appear over the years and some older algorithms become deprecated once cryptanalysts find flaws. The TLS handshakes starts with the `ClientHello` message that is sent by the client. This message carries the following information :

> *   Protocol version number: this is the version of the TLS protocol supported by the client. The server should use the same version of the TLS protocol as the client, but may opt for an older version. Both versions 1.2 and 1.3 of TLS are deployed today. Older versions are being deprecated.
>     
>     
> *   Random number: security protocols rely on random numbers. The client sends a 32 bytes long random number where usually four of these bytes correspond to the client’s clock. This random number is used, together with the server’s random number, as a seed to generate the security keys.
>     
>     
> *   Cipher suites : this ordered list contains the set of cryptographic algorithms that are supported by the client, with the most preferred one listed first. In contrast with `ssh` that allows negotiating independent algorithms for encryption, key exchange and authentication, TLS relies on suites that combine these algorithms together. Many cryptographic suites have been defined for TLS. Various recommendations have been published on the security of some of these suites [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Compression algorithm : the client may propose the utilization of a specific compression algorithm (e.g. zlib). In theory, compressing the data before encrypting it is an intelligent way to reduce the amount of data exchanged. Unfortunately, its implementation in TLS has caused several security problems [[PHG2013]](../bibliography.html#phg2013). For this reason, compression is usually disabled in TLS [**RFC 7525**](https://datatracker.ietf.org/doc/html/rfc7525.html).
>     
>     
> *   Extensions : TLS supports various extensions in the `ClientHello` message. These extensions [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) are important to allow the protocol to evolve, but many of them go beyond the scope of this chapter.

Note

The `Server Name Indication (SNI)`

The `Server Name Indication (SNI)` extension defined in [**RFC 6066**](https://datatracker.ietf.org/doc/html/rfc6066.html) is an important TLS extension for web servers. It is used by the client to indicate the name of the server that it wishes to contact. The IP address associated to this name has been queried from the DNS and used to establish the TCP connection. Why should the client indicate the server name in the TLS `ClientHello` ? The motivation is the same as for the `Host` header line in HTTP/1.0\. With the SNI extension, a single TLS server can support several web sites that use different domain names. Thanks to the SNI extension, the server knows the concerned domain name at the start of the TLS session. Without this extension, hosting providers would have been forced use one IP address per TLS-enabled server.

The server replies to the `ClientHello` with several messages:

> *   the `ServerHello` message that contains the protocol version chosen by the server (assumed to be the same as the client version in this chapter), the 32 random bytes chosen by the server, the Cipher Suite selected by the server from the list advertised by the client and a Session Id. This Session Id is an identifier which is chosen by the server. It identifies the TLS session and the security parameters (algorithms and keys) negotiated for this session. It is used to support session resumption.
>     
>     
> *   the `Certificate` message provides the certificate (or usually a chain of certificates) that binds a domain name to the public key used by the server. TLS uses the server certificates to authenticate the server. It relies on a Public Key Infrastructure that is composed of a set of root certification authorities that issue certificates to certification authorities that in the end issue certificates to servers. TLS clients are usually configured with the public keys of several root certification authorities and use this information to validate the certificates that they receive from servers. For historical reasons, the TLS certificates are encoded in ASN.1 format. The details of the ASN.1 syntax [[Dubuisson2000]](../bibliography.html#dubuisson2000) are outside the scope of this book.
>     
>     
> *   the `ServerKeyExchange` message is used by the server to transmit the information that is required to perform the key exchange. The content of this message is function of the selected key exchange algorithm.
>     
>     
> *   the `ServerHelloDone` indicates that the server has sent all the messages for the first phase of the handshake.

At this point, it is time to describe the TLS key exchange. TLS supports different key exchange mechanisms that can be negotiated as part of the selection of the cipher suite. We focus on two of them to highlight their differences:

> *   `RSA`. This key exchange algorithm uses the encryption capabilities of the RSA public-key algorithm. The client has validated the server’s public key thanks to the `Certificate` message. It then generates a (48 bytes) random number, encrypts it with the server public key and sends the encrypted number to the server in the `ClientKeyExchange` message. The server uses its private key to decrypt the random number. At this point, the client and the server share the same (48 bytes long) secret and use it to derive the secret keys required to encrypt and authenticate data in the second phase. With this key exchange algorithm, the server does not need to send a `ServerKeyExchange` message.
>     
>     
> *   `DHE_RSA`. This key exchange algorithm is the Ephemeral Diffie Hellman key exchange with RSA signatures to authenticate the key exchange. It operates as a classical authenticated Diffie Hellman key exchange. If this key exchange has been selected by the server, it sends its Diffie Hellman parameters in the `ServerKeyExchange` message and signs them with its private key. The client then continues the key exchange and sends the results of its own computation in the `ClientKeyExchange` message. `DHE_RSA` is thus an authenticated Diffie Hellman key exchange where the initial message is sent by the server (instead of the client as in our first example but since the protocol is symmetric, this does not matter).

An important difference between `DHE_RSA` and `RSA` is their reaction against attacks. `DHE_RSA` is considered by many to be stronger than `RSA` because it supports Perfect Forward Secrecy. This property is important against attackers that are able to eavesdrop all the (encrypted) data sent and received by a server. Consider that Terrence is such an attacker that has stored all the packets exchanged by Bob’s server during the last six months. If he manages, by any means, to obtain Bob’s private key, he will be able to decrypt all the keys used to secure the TLS sessions with Bob’s server during this period. With `DHE_RSA`, a similar attack is less devastating. If Terrence knows Bob’s private key, he will be able to launch a man-in-the-middle attack against future TLS sessions with Bob’s server. However, he will not be able to recover the keys used for all the past sessions that he captured.

Note

Perfect Forward Secrecy

Perfect Forward Secrecy (PFS) is an important property for key exchange protocols. A protocol provides PFS if its design guarantees that the keys used for former sessions will not be compromised even if the private key of the server is compromised. This is a very important property. `DHE_RSA` provides Perfect Forward Secrecy, but the `RSA` key exchange does not provide this property. In practice, `DHE_RSA` is costly from a computational viewpoint. Recent implementations of TLS thus prefer `ECDHE_RSA` or `ECDHE_ECDSA` when Perfect Forward Secrecy is required.

All the information required for the key exchange has now been transmitted. There are two important messages that will be sent by the client and the server to conclude the handshake and start the data transfer phase.

The client sends the `ChangeCipherSpec` message followed by the `Finished` message. The `ChangeCipherSpec` message indicates that the client has received all the information required to generate the security keys for this TLS session. This messages can also appear later in the session to indicate a change in the encryption algorithms that are used, but this usage is outside the scope of this book. The `Finished` message is more important. It confirms to the server that the TLS handshake has been performed correctly and that no attacker has been able to modify the data sent by the client or the server. This is the first message that is encrypted with the selected security keys. It contains a hash of all the messages that were exchanged during the handshake.

The server also sends a `ChangeCipherSpec` message followed by a `Finished` message.

Note

TLS Cipher suites

A TLS cipher suite is usually represented as an ASCII string that starts with TLS and contains the acronym of the key exchange algorithm, the encryption scheme with the key size and its mode of operation and the authentication algorithm. For example, `TLS_DHE_RSA_WITH_AES_128_GCM_SHA256` is a TLS cipher suite that uses the `DHE_RSA` key exchange algorithm with 128 bits AES in GCM mode for encryption and SHA-256 for authentication. The official list of TLS cipher suites is maintained by IANA [[7]](#fianatls). The NULL acronym indicates that no algorithm has been specified. For example, `TLS_ECDH_RSA_WITH_NULL_SHA` is a cipher suite that does not use any encryption but still uses the `ECDH_RSA` key exchange and `SHA` for authentication.

### The TLS record protocol[#](#the-tls-record-protocol "Link to this heading")

The handshake is now finished. The client and the server will exchange authenticated and encrypted records. TLS defines different formats for the records depending on the cryptographic algorithms that have been negotiated for the session. A detailed discussion of these different types of records is outside the scope of this introduction. For illustration, we briefly describe one record format.

As other security protocols, TLS uses different keys to encrypt and authenticate records. These keys are derived from the MasterSecret that is either randomly generated by the client after the `RSA` key exchange or derived from the Diffie Hellman parameters after the `DH_RSA` key exchange. The exact algorithm used to derive the keys is defined in [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html).

A TLS record is always composed of four different fields :

> *   a Type that indicates the type of record. The most frequent type is application data which corresponds to a record containing encrypted data. The other types are handshake, change_cipher_spec and alert.
>     
>     
> *   a Protocol Version field that indicates the version of the TLS protocol used. This version is composed of two sub fields : a major and a minor version number.
>     
>     
> *   a Length field. A TLS record cannot be longer than 16,384 bytes.
>     
>     
> *   a TLSPlainText that contains the encrypted data

TLS supports several methods to encrypted records. The selected method depends on the cryptographic algorithms that have been negotiated for the TLS session. A detailed presentation of the different methods that can be used to produce the TLSPlainText from the user data is outside the scope of this book. As an example, we study one method: Stream Encryption. This method is used with cryptographic algorithms which can operate on a stream of bytes. The method starts with a sequence of bytes provided by the user application: the plain text. The first step is to compute the authentication code to verify the integrity of the data. For this, TLS computes \(MAC(SeqNum, Header, PlainText)\) using HMAC where SeqNum is a sequence number which is incremented by one for each new TLS record transmitted. The Header is the header of the TLS record described above and PlainText is the information that needs to be encrypted. Note that the sequence number is maintained at the two endpoints of the TLS session, but it is not transmitted inside the TLS record. This sequence number is used to prevent replay attacks.

Note

MAC-then-encrypt or Encrypt-then-MAC

When secure protocols use Message Authentication and Encryption, they need to specify how these two algorithms are combined. A first solution, which is used by the current version of TLS, is to compute the authentication code and then encrypt both the data and the authentication code. A drawback of this approach is that the receiver of an encrypted TLS record must first attempt to decrypt data that has potentially been modified by an attacker before being able to verify the authenticity of the record. A better approach is for the sender to first encrypt the data and then compute the authentication code over the encrypted data. This is the encrypt-then-MAC approach proposed in [**RFC 7366**](https://datatracker.ietf.org/doc/html/rfc7366.html). With encrypt-then-MAC, the receiver first checks the authentication code before attempting to decrypt the record.

### Improving TLS[#](#improving-tls "Link to this heading")

During the last two decades, the deployment of TLS has continued to grow. The early TLS servers were only used for critical services such as e-commerce websites or online banks. As CPU performance improved, it became much more cost-effective to use TLS to secure non-critical parts of web servers, including the delivery of HTML pages and even video services. There is now a growing number of applications that rely on TLS [[AM2019]](../bibliography.html#am2019).

In 2013, the statistics collected by the Firefox Telemetry project [[9]](#ftelemetry) revealed that 30% of the web pages loaded by Firefox users were done over HTTPS. In October 2019, 80% of the web pages are loaded over HTTPS. In six years, HTTPS became the dominant protocol to access web services. Another look at the deployment of HTTPS on web sites may be found in [[Helme2019]](../bibliography.html#helme2019).

Measurement studies that analyzed the evolution of TLS over the years have identified several important changes in the TLS ecosystem [[KRA2018]](../bibliography.html#kra2018). First, the preferred cryptographic algorithms have changed. While RC4 was used by 60% of the connections in 2012, its usage has dropped since 2015\. AES started to be deployed in 2013 and is now used for more than 90% of the connections. The deployed versions of TLS have also changed. TLS 1.0 and TLS 1.1 are now rarely used. The deployment of TLS 1.2 started in 2013 and reached 70% of the connections in 2015\. Version 1.3 of TLS, that is described below, is also widely deployed.

Another interesting fact is the key exchange schemes. In 2012, RSA was the dominant solution, used by more than 80% of the observed connections [[KRA2018]](../bibliography.html#kra2018). In 2013, Edward Snowden revealed the surveillance activities of several governments. These revelations had a huge impact on the Internet community. The IETF, which standardizes Internet protocols, considered in [**RFC 7258**](https://datatracker.ietf.org/doc/html/rfc7258.html) that such pervasive monitoring was an attack. Since then, several IETF working groups have developed solutions to counter pervasive monitoring. One of these solutions is to encourage Perfect Forward Security. Within TLS, this implies replacing RSA by an authenticated Diffie Hellman key exchange such as ECDHE. Measurements indicate that since summer 2014, ECDHE is more popular than RSA. In 2018, more than 90% of the observed TLS connections used ECDHE.

The last point is the difficulty of deploying TLS servers [[KMS2017]](../bibliography.html#kms2017). When TLS servers are installed, the system administrator needs to obtain certificates and configure a range of servers. Initially, getting certificates was complex and costly, but initiatives such as [https://letsencrypt.org](https://letsencrypt.org) have simplified this workflow.

In 2014, the IETF TLS working started to work on the development of version 1.3 of the TLS protocol. Their main objectives [[Rescorla2015]](../bibliography.html#rescorla2015) for this new version were:

> *   simplify the design by removing unused or unsafe protocol features
>     
>     
> *   improve the security of TLS by leveraging the lessons learned from TLS 1.2 and some documented attacks
>     
>     
> *   improve the privacy of the protocol
>     
>     
> *   reduce the latency of TLS

Since 2014, latency has become an important concern for web services. As access networks bandwidth continue to grow, latency is becoming a key factor that affects the performance of interactive web services. With TLS 1.2, the download of a web page requires a minimum of four round-trip-times, one to create the underlying TCP connection, one to exchange the ClientHello/ServerHello, one to exchange the keys and then one to send the HTTP GET and retrieve the response. This can be very long when the server is not near the client. TLS 1.3 aimed at reducing this handshake to one round-trip-time and even zero by placing some of the cryptographic handshake in the TCP handshake. This part will be discussed in the TCP chapter. We focus here on the reducing the TLS handshake to a single round-trip-time.

To simplify both the design and the implementations, TLS 1.3 uses only a small number of cipher suites. Five of them are specified in [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html) and `TLS_AES_128_GCM_SHA256` must be supported by all implementations. To ensure privacy, all cipher suites that did not provide Perfect Forward Secrecy have been removed. Compression has also been removed from TLS since several attacks on TLS 1.2 exploited its compression capability [**RFC 7457**](https://datatracker.ietf.org/doc/html/rfc7457.html).

Note

Enterprises, privacy and TLS

By supporting only cipher suites that provide Perfect Forward Secrecy in TLS 1.3, the IETF aims at protecting the privacy of users against a wide range of attacks. However, this choice has resulted in intense debates in some enterprises. Some enterprises, notably in financial organizations, have deployed TLS, but wish to be able to decrypt TLS traffic for various security-related activities. These enterprises tried to lobby within the IETF to maintain RSA-based cipher suites that do not provide Perfect Forward Secrecy. Their arguments did not convince the IETF. Eventually, these enterprises moved to ETSI, another standardization body, and convinced them to adopt entreprise TLS, a variant of TLS 1.3 that does not provide Perfect Forward Secrecy [[eTLS2018]](../bibliography.html#etls2018).

The TLS 1.3 handshake differs from the TLS 1.2 handshake in several ways. First, the TLS 1.3 handshake requires a single round-trip-time when the client connects for the first time to a server. To achieve this, the TLS designers look at the TLS 1.2 handshake in details and found that the first round-trip-time is mainly used to select the set of cryptographic algorithms and the cryptographic exchange scheme that will be used over the TLS session. TLS 1.3 drastically simplifies this negotiation by requiring to use the Diffie Hellman exchange with a small set of possible parameters. This means that the client can guess the parameters used by the server (i.e. the modulus, p and the base g) and immediately start the Diffie Hellman exchange. A simplified version of the TLS 1.3 handshake is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/e5ae4c8c184c7b414c018f2972f772de.png)<map id="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0" name="7c40afa8b5d3bc7ec147430dbd78098f688a7eb0"></map>

There are several important differences with the TLS 1.2 handshake. First, the Diffie Hellman key exchange is required in TLS 1.3 and this exchange is initiated by the client (before having validated the server identity). To initiate the Diffie Hellman key exchange, the client needs to guess the modulus and the base that can be accepted by the server. Either the client uses standard parameters that most server supports or the client remembers the last modulus/base that it used with this particular server. If the client guessed incorrectly, the server replies with the parameters that it expects and one round-trip-time is lost. When the server sends its ServerHello, it already knows the session key. This implies that the server can encrypt all subsequent messages. After one round-trip-time, all data exchanged over the TLS 1.3 session is encrypted and authenticated. In TLS 1.3, the server certificate is encrypted with the session key, as well as the Finished message. The server signs the handshake to confirm that it owns the public key of its certificate. If the server wants to send application data, it can already encrypt it and send it to the client. Upon reception of the server Certificate, the client verifies it and checks the signature of the handshake and the Finished message. The client confirms the end of the handshake by sending its own Finished message. At that time, the client can send encrypted data. This means that the client only had to wait one round-trip-time before sending encrypted data. This is much faster than with TLS 1.2.

For some applications, waiting one round-trip-time before being able to send data is too long. TLS 1.3 allows the client to send encrypted data immediately after the ClientHello, without having to wait for the ServerHello message. At this point in the handshake, the client cannot know the key that will be derived by the Diffie Hellman key exchange. The trick is that the server and the client need to have previously agreed on a pre-shared-key. This key could be negotiated out of band, but usually it was exchanged over a previous TLS session between the client and the server. Both the client and the server can store this key in their cache. When the client creates a new TLS session to a server, it checks whether it already knows a pre-shared key for this server. If so, the client announces the identifier of this key in its ClientHello message. Thanks to this identifier, the server can recover the key and use it to decrypt the 0-rtt Encrypted record. A simplified version of the 0-rtt TLS 1.3 handshake [[8]](#fhandshake) is shown in the figure below.

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> c>>b [ label = "Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> b>>c [ label = "Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/37315cb85580d2a0a41ccf2aa6b695ae.png)<map id="04ec071344c2fae0632e2d690c2782c9017b631e" name="04ec071344c2fae0632e2d690c2782c9017b631e"></map>

On the web, TLS clients use certificates to authenticate servers but the clients are not authenticated. However, there are environments such as enterprise networks where servers may need to authenticate clients as well. A popular deployment is to authenticate remote clients who wish to access the enterprise network through a Virtual Private Network service. Some of these services run above TLS (or more precisely a variant of TLS named DTLS that runs above UDP [[MoR2004]](../bibliography.html#mor2004) but is outside the scope of this chapter). In such services, each client is authenticated thanks to a public key and a certificate that is trusted by the servers. To establish a TLS session, such a client needs to prove that it owns the public key associated with the certificate. This is done by the server thanks to the CertificateRequest message. The TLS handshake becomes the following one:

> ![msc {
> a [label="", linecolour=white],
> b [label="Client",linecolour=black],
> z [label="", linecolour=white],
> c [label="Server", linecolour=black],
> d [label="", linecolour=white];
> 
> b>>c [ label = "ClientHello[Random, g^c,server_conf=abcd]", arcskip="2"];
> |||;
> b>>c [ label = "0-rtt Encrypted record", textcolour="magenta", arcskip="2"];
> |||;
> 
> c>>b [ label = "ServerHello[Random, g^s]", arcskip="2"];
> |||;
> |||;
> c>>b [ label = "CertificateRequest, Certificate, Sign(K,Handshake), Finished, Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Certificate, Sign(Kc, Handshake), Finished", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> c>>b [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> b>>c [ label = "Encrypted Record", textcolour="red", linecolour="red", arcskip="2"];
> |||;
> |||;
> }](../Images/6b610706117d1538fe4793f16be928b1.png)<map id="36d37bda46d72f2f438e0e029b02b9c54e69fa82" name="36d37bda46d72f2f438e0e029b02b9c54e69fa82"></map>

The server sends a CertificatRequest message. The client returns its certificate and signs the Handshake with is private key. This confirms to the server that the client owns the public key indicated in its certificate.

There are many more differences between TLS 1.2 and TLS 1.3\. Additional details may be found in their respective specifications, [**RFC 5246**](https://datatracker.ietf.org/doc/html/rfc5246.html) and [**RFC 8446**](https://datatracker.ietf.org/doc/html/rfc8446.html).

 ## Securing the Domain Name System[#](#securing-the-domain-name-system "Link to this heading")

The Domain Name System provides a critical service in the Internet infrastructure since it maps the domain names that are used by end users onto IP addresses. Since end users rely on names to identify the servers that they connect to, any incorrect information distributed by the DNS would direct end users’ connections to invalid destinations. Unfortunately, several attacks of this kind occurred in the past. A detailed analysis of the security threats against the DNS appeared in [**RFC 3833**](https://datatracker.ietf.org/doc/html/rfc3833.html). We consider three of these threats in this section and leave the others to [**RFC 3833**](https://datatracker.ietf.org/doc/html/rfc3833.html).

The first type of attack is eavesdropping. An attacker who can capture packets sent to a DNS resolver or a DNS server can gain valuable information about the DNS names that are used by a given end user. If the attacker can capture all the packets sent to a DNS resolver, he/she can collect a lot of meta data about the domain names used by the end user. Preventing this type of attack has not been an objective of the initial design of the DNS. There are currently discussions with the IETF to carry DNS messages over TLS sessions to protect against such attacks. However, these solutions are not yet widely deployed.

The second type of attack is the man-in-the-middle attack. Consider that Alice is sending DNS requests to her DNS resolver. Unfortunately, Mallory sits in front of this resolver and can capture and modify all the packets sent by Alice to her resolver. In this case, Mallory can easily modify the DNS responses sent by the resolver to redirect Alice’s packets to a different IP address controlled by Mallory. This enables Mallory to observe (and possibly modify) all the packets sent and received by Alice. In practice, executing this attack is not simple since DNS resolvers are usually installed in protected datacenters. However, if Mallory controls the WiFi access point that Alice uses to access the Internet, he could easily modify the packets on this access point and some software packages automate this type of attacks.

If Mallory cannot control a router on the path between Alice and her resolver, she could still launch a different attack. To understand this attack, it is important to correctly understand how the DNS protocol operates and the roles of the different fields of the DNS header which is reproduced in [Fig. 46](#fig-dns-header2).

[![../_images/dnsheader.svg](../Images/21a7bad41190434264bf8afe39a2de6a.png)](../_images/dnsheader.svg) 

Fig. 46 DNS header[#](#id39 "Link to this image") 

The first field of the header is the Identification field. When Alice sends a DNS request, she places a 16-bits integer in this field and remembers it. When she receives a response, she uses this Identification field to locate the initial DNS request that she sent. The response is only used if its Identification matches a pending DNS request (containing the same question).

Mallory has studied the DNS protocol and understands how it works. If he can predict a popular domain for which Alice will regularly send DNS requests, then he can prepare a set of DNS responses that map the name requested by Alice to an IP address controlled by Mallory instead of the legitimate DNS response. Each DNS response has a different Identification. Since there are only 65,536 values for the Identification field, it is possible for Mallory to send them to Alice hoping that one of them will be received while Alice is waiting for a DNS response with the same identifier. In the past, it was difficult to send 65,536 DNS responses quickly enough. However, with the high speed links that are available today, this is not an issue anymore. A second concern for Mallory is that he must be able to send the DNS responses as if they were coming directly from the DNS resolver. This implies that Mallory must be able to send IP packets that appear to originate from a different address. Although networks should be configured to prevent this type of attack, this is not always the case and there are networks where it is possible for a host to send packets with a different source IP address [[10]](#fspoof). If the attack targets a single end user, e.g. Alice, this is annoying for this user. However, if the attacker can target a DNS resolver that serves an entire company or an entire ISP, the impact of the attack can be much larger in particular if the injected DNS response carries a long TTL and thus resides in the resolver’s cache for a long period of time.

Fortunately, designers of DNS servers and resolvers have found solutions to mitigate this type of attack. The easiest approach would have been to update the format of the DNS requests and responses to include a larger Identifier field. Unfortunately, this elegant solution was not possible with the DNS because the DNS messages do not include any version number that would have enabled such a change. Since the DNS messages are exchanged inside UDP segments, the DNS developers found an alternate solution to counter this attack. There are two ways for the DNS library used by Alice to send her DNS requests. A first solution is to bind one UDP source port and always send the DNS requests from this source port (the destination port is always port `53`). The advantage of this solution is that Alice’s DNS library can easily receive the DNS responses by listening to her chosen port. Unfortunately, once the attacker has found the source port used by Alice, he only needs to send 65,536 DNS responses to inject an invalid response. Fortunately, Alice can send her DNS requests in a different way. Instead of using the same source port for all DNS requests, she can use a different source port for each request. In practice, each DNS request will be sent from a different source port. From an implementation viewpoint, this implies that Alice’s DNS library will need to listen to one different port number for each pending DNS request. This increases the complexity of her implementation. From a security viewpoint there is a clear benefit since the attacker needs to guess both the 16 bits Identifier and the 16 bits UDP source port to inject a fake DNS response. To generate all possible DNS responses, the attacker would need to generate almost \(2^{32}\) different messages, which is excessive in today’s networks. Most DNS implementations use this second approach to prevent these cache poisoning attacks.

These attacks affect the DNS messages that are exchanged between a client and its resolver or between a resolver and name servers. Another type of attack exploits the possibility of providing several resource records inside one DNS response. A frequent optimization used by DNS servers and resolvers is to include several related resource records in each response. For example, if a client sends a DNS query for an NS record, it usually receives in the response both the queried record, i.e. the name of the DNS server that serves the queried domain, and the IP addresses of this server. Some DNS servers return several NS records and the associated IP addresses. The cache poisoning attack exploits this DNS optimization.

Let us illustrate it on an example. Assume that Alice frequently uses the example.net domain and in particular the web server whose name is www.example.net. Mallory would like to redirect the TCP connections established by Alice towards www.example.net to one IP address that he controls. Assume that Mallory controls the mallory.net domain. Mallory can tune the DNS server of his domain and add special DNS records to the responses that it sends. An attack could go roughly as follows. Mallory forces Alice to visit the www.mallory.net web site. He can achieve this by sending a spam message to Alice or buying advertisements on a web site visited by Alice and redirect one of these advertisements to www.mallory.net. When visiting the advertisement, Alice’s DNS resolver will send a DNS request for www.mallory.net. Since Mallory control the DNS server, he can easily add in the response a AAAA record that associates www.example.net to the IP address controlled by Mallory. If Alice’s DNS library does not check the returned response, the cache entry for www.example.net will be replaced by the AAAA record sent by Mallory.

To cope with these security threats and improve the security of the DNS, the IETF has defined several extensions that are known as DNSSEC. DNSSEC exploits public-key cryptography to authenticate the content of the DNS records that are sent by DNS servers and resolvers. DNSEC is defined in three main documents [**RFC 4033**](https://datatracker.ietf.org/doc/html/rfc4033.html), [**RFC 4034**](https://datatracker.ietf.org/doc/html/rfc4034.html), [**RFC 4035**](https://datatracker.ietf.org/doc/html/rfc4035.html). With DNSSEC, each DNS zone uses one public-private key pair. This key pair is only used to sign and authenticate DNS records. The DNS records are not encrypted and DNSSEC does not provide any confidentiality. Other DNS extensions are being developed to ensure the confidentiality of the information exchanged between a client and its resolvers [**RFC 7626**](https://datatracker.ietf.org/doc/html/rfc7626.html). Some of these extensions exchange DNS records over a TLS session which provides the required confidentiality, but they are not yet deployed and outside the scope of this chapter.

DNSSEC defines four new types of DNS records that are used together to authenticate the information distributed by the DNS.

> *   the DNSKEY record allows storing the public key associated with a zone. This record is encoded as a TLV and includes a Base64 representation of the key and the identification of the public key algorithm. This allows the DNSKEY record to support different public key algorithms.
>     
>     
> *   the RRSIG record is used to encode the signature of a DNS record. This record contains several sub-fields. The most important ones are the algorithm used to generate the signature, the identifier of the public key used to sign the record, the original TTL of the signed record and the validity period for the signature.
>     
>     
> *   the DS record contains a hash of a public key. It is used by a parent zone to certify the public key used by one of its child zones.
>     
>     
> *   the NSEC record is used when non-existent domain names are queried. Its usage will be explained later

The simplest way to understand the operation of DNSSEC is to rely on a simple example. Let us consider the example.org domain and assume that Alice wants to retrieve the AAAA record for www.example.org using DNSSEC.

The security of DNSSEC relies on anchored keys. An anchored key is a public key that is considered as trusted by a resolver. In our example, we assume that Alice’s resolver has obtained the public key of the servers that manage the root zone in a secure way. This key has been distributed outside of the DNS, e.g. it has been published in a newspaper or has been received in a sealed letter.

To obtain an authenticated record for www.example.org, Alice’s resolver first needs to retrieve the NS which is responsible for the .org Top-Level Domain (TLD). This record is served by the DNS root server and Alice’s resolver can retrieve the signature (RRSIG record) for this NS record. Since Alice knows the DNSKEY of the root, she can verify the validity of this signature.

The next step is to contact ns.org, the NS responsible for the .org TLD to retrieve the NS record for the example.org domain. This record is accompanied by a RRSIG record that authenticates it. This RRSIG record is signed with the key of the .org domain. Alice’s resolver can retrieve this public key as the DNSKEY record for the .org, but how can it trust this key since it is distributed by using the DNS and could have been modified by attackers ? DNSSEC solves this problem by using the DS record that is stored in the parent zone (in this case, the root zone). This record contains a hash of a public key that is signed with a RRSIG signature. Since Alice’s resolver’s trusts the root key, it can validate the signature of the DS record for the .org domain. It can then retrieve the DNSKEY record for this domain from the DNS and compare the hash of this key with the DS record. If they match, the public key of the .org domain can be trusted. The same technique is used to obtain and validate the key of the example.org domain. Once this key is trusted, Alice’s resolver can request the AAAA record for www.example.org and validate its signature.

Thanks to the DS record, a resolver can validate the public keys of client zones as long as their is a chain of DS -> DNSKEY records from an anchored key. If the resolver trusts the public key of the root zone, it can validate all DNS replies for which this chain exists.

There are several details of the operation of DNSSEC that are worth being discussed. First, a server that supports DNSSEC must have a public-private key pair. The public key is distributed with the DNSKEY record. The private key is never distributed and it does not even need to be stored on the server that uses the public key. DNSSEC does not require the DNSSEC servers to perform any operation that requires a private key in real time. All the RRSIG records can be computed offline, possibly on a different server than the server that returns the DNSSEC replies. The initial motivation for this design choice was the CPU complexity of computing the RRSIG signatures for zones that contain millions of records. In the early days of DNSSEC, this was an operational constraint. Today, this is less an issue, but avoiding costly signature operations in real time has two important benefits. First, this reduces the risk of denial of service attacks since an attacker cannot force a DNSSEC server to perform computationally intensive signing operations. Second, the private key can be stored offline, which means that even if an attacker gains access to the DNSSEC server, it cannot retrieve its private key. Using offline signatures for the RRSIG records has some practical implications that are reflected in the content of this record. First, each RRSIG record contains the original TTL of the signed record. When DNS resolvers cache records, they change the value of the TTL of these cached records and then return the modified records to their clients. When a resolver receives a signed DNS record, it must replace the received TTL of the record with the original TTL (and check that the received TTL is smaller than the original one) before checking the signature. Second, the RRSIG records contain a validity period, i.e. a starting time and an ending time for the validity of the signature. This period is specified as two timestamps. This period is only the validity of the signature. It does not affect the TTL of the signed record and is independent from the TTL. In practice, the validity period is important to allow DNS server operators to update their public/private keys. When such a key is changed, e.g. because the private could have been compromised, there is some period of time during which records signed with the two keys coexist in the network. The validity period allows ensuring that old signatures do not remain in DNS caches for ever.

The last record introduced by DNSSEC is the NSEC record. It is used to authenticate a negative response returned by a DNS server. If a resolver requests a domain name that is not defined in the zone, the server replies with an error message. The designers of the original version of the DNS thought that these errors would not be very frequent and resolvers were not required to cache those negative responses. However, operational experience showed that queries for invalid domain names are more frequent than initially expected and a large fraction of the load on some servers is caused by repeated queries for invalid names. Typical examples include queries for invalid TLDs to the root DNS servers or queries caused by configuration errors [[WF2003]](../bibliography.html#wf2003). Current DNS deployments allow resolvers to cache those negative answers to reduce the load on the entire DNS [**RFC 2308**](https://datatracker.ietf.org/doc/html/rfc2308.html).

The simplest way to allow a DNSSEC server to return signed negative responses would be for the server to return a signed response that contains the received query and some information indicating the error. The client could then easily check the validity of the negative response. Unfortunately, this would force the DNSSEC server to generate signatures in real time. This implies that the private key must be stored in the server memory, which leads to risks if an attacker can take control of the server. Furthermore, those signatures are computationally complex and a simple denial of service attack would be to send invalid queries to a DNSSEC server.

Given the above security risks, DNSSEC opted for a different approach that allows the negative replies to be authenticated by using offline signatures. The NSEC record exploits the lexicographical ordering of all the domain names. To understand its usage, consider a simple domain that contains three names (the associated AAAA and other records that are not shown) :

[PRE4]

In this domain, the DNSSEC server adds three NSEC records. A RRSIG signature is also computed for each of these records.

[PRE5]

If a resolver queries delta.example.org, the server will parse its zone. If this name were present, it would have been placed, in lexicographical order, between the beta.example.org and the gamma.example.org names. To confirm that the delta.example.org name does not exist, the server returns the NSEC record for beta.example.org that indicates that the next valid name after beta.example.org is gamma.example.org. If the server receives a query for pi.example.org, this is the NSEC record for gamma.example.org that will be returned. Since this record contains a name that is before pi.example.org in lexicographical order, this indicates that pi.example.org does not exist.

Footnotes