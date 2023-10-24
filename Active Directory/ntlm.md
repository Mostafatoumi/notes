# NTLM Authentication in Active Directory

## Introduction:

In Active Directory (AD), apart from Kerberos and LDAP, various other authentication methods are used by applications and services. These include LM, NTLM, NTLMv1, and NTLMv2. Understanding the differences between these hashes and protocols is crucial for securing AD environments.

Hash Protocol Comparison:

| Hash/Protocol       | Cryptographic technique               | Mutual Authentication | Message Type          | Trusted Third Party       |
|---------------------|-------------------------------------|-----------------------|-----------------------|--------------------------|
| NTLM                | Symmetric key cryptography           | No                    | Random number         | Domain Controller         |
| NTLMv1              | Symmetric key cryptography           | No                    | MD4 hash, random number| Domain Controller         |
| NTLMv2              | Symmetric key cryptography           | No                    | MD4 hash, random number| Domain Controller         |
| Kerberos            | Symmetric key cryptography & asymmetric cryptography | Yes | Encrypted ticket using DES, MD5 | Domain Controller/Key Distribution Center (KDC) |

	
	
| Hash Type                                 | Example                                           |
|------------------------------------------|---------------------------------------------------|
| LM Hash                                  | 299bd128c1101fd6                                  |
| NTLM Hash                                | Rachel:500:aad3c435b514a4eeaad3b935b51304fe:e46b9e548fa0d122de7f59fb6d48eaa2::: |
| NTLMv1 Hash                             | u4-netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c |
| NTLMv2 Hash                             | admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030 |
| Domain Cached Credentials (MSCache2) Hash | $DCC2$10240#bjones#e4e938d12fe5974dc42a90120bd9… |

	
	
Note: Neither LANMAN nor NTLM uses a salt.


	
	
## NTLM Authentication Request:

			
![Ntml Auth](https://github.com/Mostafatoumi/notes/blob/main/img%20notes/ntlm_01.png)

	
LM Hash :

	• LM hashes are the oldest password storage mechanism in Windows, used until Windows Vista/Server 2008.
	• Limited to 14 characters, case-insensitive, and vulnerable due to the hashing algorithm weaknesses.
	• Encrypted using DES keys created from 7-character chunks and concatenated to form the LM hash.
	• Disallowing LM hashes using Group Policy is recommended.


NTHash (NTLM):

	• NTLM hashes are used on modern Windows systems and are an improvement over LM hashes.
	• Derived from the MD4 hash of the little-endian UTF-16 value of the password.
	• Vulnerable to offline brute-force attacks but more secure than LM hashes.
	• Used in a challenge-response authentication protocol for network authentication.

NTLMv1 (Net-NTLMv1):

	• An improvement over NTLM, but still uses the NT hash along with the LM hash.
	• Vulnerable to certain attacks and can be cracked offline if captured.
	• Used in challenge-response authentication with a 24-byte response to an 8-byte challenge.

NTLMv2 (Net-NTLMv2):

	• Introduced in Windows NT 4.0 SP4 and became the default since Server 2000.
	• More secure than NTLMv1, doesn't use the LM hash, and uses HMAC-MD5 for challenge-response.
	• Sends two responses to an 8-byte challenge, including HMAC-MD5 hash of the user's credentials.

Domain Cached Credentials (MSCache2):

	• MSCache2 is used when a domain-joined host can't communicate with a domain controller.
	• Hosts save the last ten hashes of domain users in the registry (HKEY_LOCAL_MACHINE\SECURITY\Cache).
	• Hashes cannot be used in pass-the-hash attacks and are slow to crack.

## Conclusion

Understanding the various hash types and authentication protocols in Active Directory is essential for maintaining a secure environment. While Kerberos is preferred, it's crucial to implement proper security measures for LM and NTLM to prevent potential attacks. Being aware of the strengths and weaknesses of each hash type helps in designing effective defense strategies for AD environments.
