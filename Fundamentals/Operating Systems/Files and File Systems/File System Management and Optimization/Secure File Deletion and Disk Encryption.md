> Securely deleting data on disk is not easy

Even if original content overwritten by zeroes, it may not be enough
- Some hard disks leave magnetic traces in areas close to actual tracks
- Copes of file may still be in unexpected places on disk
- SSDs even worse, because file system has no control over what flash blocks are overwritten and when
- Secure erasure can be achieved by overwriting disk with 3-7 passes, alternating zeroes and random numbers, 

Full disk encryption makes it impossible to recover data also and is available on modern OSes
- Sometimes provided by storage devices in the form of Self-Encrypting Drives (SEDs). However, many SEDs have critical security weaknesses
- In the case of Windows, if no SED is present, it takes care of encryption using a secret key (volume master key) using AES encryption algorithm
	- Volume master key can be obtained by decrypting itself with user password/ recovery key (automatically generated first time file system was encrypted), or
	- By extracting key from special-purpose cryptoprocessor known as Trusted Platform Module.