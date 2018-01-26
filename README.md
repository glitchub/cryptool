Wrapper script for openssl RSA operations, supporting the following functions:

    cryptool [-v] sign from[.p from.s] < unsignedtext > signedtext
        Sign the input on stdin with the sender's key pair, and output as
        obfuscated result.

    cryptool [-v] verify from[.p] [signer[.p]] < signedtext > unsignedtext
        Verify the signature of signed data on stdin with the sender's public
        key and output the original unsigned input. If the sender's key is
        signed, then must also provide the signer's public key.

    cryptool [-v] encrypt to[.p] < plaintext > ciphertext
        Encrypt the input on stdin with recipient's public key, output the
        encrypted result.

    cryptool [-v] decrypt to[.s] < ciphertext > plaintext
        Decrypt the input with the recipient's private key, output the original
        unencrypted input.

    cryptool [-v] generate [-n bits] [-i 'info text'] [-s secret.s] keyname [signer[.p signer.s]]
        
        Create new keyname.s and keyname.p. If a signer keypair is provided
        then the public key will be signed. 
        
            -n bits - specify number of key bits, default is 4096

            -i 'info text' - arbitrary text that will appear in the key files,
            for documentary purposes.

            -s secret.s - Clone the specified secret key rather than generating
            a new one. This allows creation of new public certificates for
            existing secret keys.

            -d days = number of days that the certificate is valid from time of
            creation. Default is that the certificate is valid for the entire
            21st century, to avoid issues on embedded systems with possibly
            faulty clocks.
        
    cryptool [-v] check key[.p] signer[.p]

        Check that specified key is signed by the specified signer key.
        
    cryptool [-v] dump [-m] key.(s|p)
        
        Dump interesting information from provided secret or public key.
            
            -m - only dump the modulus, in hex format.

All operations exit with 0 in the case of success, otherwise non-zero.

The .p extension is used for public key, .s for secret (private) key.

The -v option enables verbosity on stdout, try this when an operation fails.

There's no way to distinguish a decrypt or verify failure from an actual error,
such as providing a corrupted key.

This script requires and checks for the openssl command-line program version
1.0.1e or later.
