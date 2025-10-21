# Hybrid-Encryption-Project-RSA-AES-
This project demonstrates the theory and practical application of public-key cryptography using RSA. It includes two distinct implementations:
 A "From-Scratch" Educational Version: This code is built using only basic math principles to show how the RSA algorithm works internally.
 A "Practical" Hybrid-Encryption Version: This code uses the industry-standard pycryptodome library to securely encrypt and decrypt a message. It demonstrates the hybrid encryption model (AES + RSA), which is the standard method used in the real world.

 Why Hybrid Encryption?
This project highlights a crucial concept in modern cryptography:
Asymmetric encryption (like RSA) is fantastic for key exchange because you can share your public key with anyone. However, it is mathematically intensive and very slow, making it unsuitable for encrypting large files or long messages.
Symmetric encryption (like AES) is extremely fast and secure, but it has one major problem: how do you securely share the single secret key with the recipient?
Hybrid Encryption is the solution:
Generate a random, one-time-use session key (e.g., a 256-bit key for AES).
Encrypt your large message using the fast AES cipher with the session key.
Encrypt the small AES session key using the recipient's slow RSA public key.
Send the (AES-encrypted) message and the (RSA-encrypted) session key to the recipient.
The recipient then uses their RSA private key to decrypt the session key, and then uses that session key to quickly decrypt the main message. This gives you the best of both worlds: the security of public-key exchange and the speed of symmetric encryption.


Code Explanation
1. Educational "From-Scratch" RSA (Cell 1)
This section is purely for academic purposes to understand the math behind RSA.
is_prime(x): A simple (but inefficient) function to check for primality.
generate_prime(): A function that finds a prime number for p and q.
gcd(a, b): Calculates the greatest common divisor.
generate_private_key(e, phi): A simple loop to find the modular inverse d.
Main logic: It correctly calculates n, phi, e, and d and performs encryption/decryption using the pow(base, exp, mod) function.
⚠️ SECURITY WARNING: This "from-scratch" implementation is NOT SECURE and must NEVER be used for real data.
The prime numbers are too small and generated insecurely.
The is_prime function is too basic for large crypto-primes.
It does not use secure padding schemes (like OAEP), making it vulnerable to attacks.
2. Practical Hybrid Encryption (Cell 3)
This section demonstrates the correct, secure, and practical way to use RSA and AES together.
Crypto.PublicKey.RSA: Used to generate a secure 2048-bit RSA key pair and save it to .pem files.
Crypto.Cipher.AES: Used to create a fast AES cipher (in GCM mode for authenticity) with a random session key.
Crypto.Cipher.PKCS1_OAEP: This is the secure padding scheme used to encrypt the AES session key with the RSA public key.
Workflow: The code follows the exact hybrid encryption model described above, encrypting the message with AES and the AES key with RSA.


Future Improvements
This project is a great foundation. Here are some ways you could expand it:
Build a Command-Line Tool (CLI): Convert the hybrid encryption logic into a .py script that can be run from the terminal (e.g., python encrypt.py --file "data.txt" --key "public.pem").
Create a Secure Chat App: Use this logic in a simple client-server socket application where the server shares its public key with clients to establish secure communication.
Improve the "From-Scratch" Version: For academic challenge, replace the simple is_prime function with a probabilistic test like Miller-Rabin and replace the generate_private_key loop with the Extended Euclidean Algorithm, which is the correct way to find the modular inverse.
