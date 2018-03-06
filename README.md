Wrapper script for various OpenSSL RSA operations.

Usage:

    cryptool [opts] sign [-y] from[.p [from.s|label]] < plaintext > signedtext
    cryptool [opts] sign -d [-y] from[.s]|label < plaintext > signature.dat

        In the first form, sign the input with the sender's key pair, and
        output PKCS7 signedtext.

        In the second form, sign the input with sender's secret key, and output
        the detached signature file.

            -y - use the labeled secret key in yubihsm2

    cryptool [opts] verify from[.p] [signer[.p]] < signedtext > plaintext
    cryptool [opts] verify -d signature.dat from[.p] [signer[.p]] < plaintext

        In the first form, verify PKCS7 signedtext with the sender's public
        key and output the original plaintext.

        In the second form, decrypt the signature file with sender's public key
        and verify it is correct for the input plaintext.

        If the sender's public key is signed, then must also provide the
        signer's public key.

    cryptool [opts] encrypt to[.p] < plaintext > ciphertext

        Encrypt the plaintext input with recipient's public key, output the
        PKCS7 ciphertext.

    cryptool [opts] decrypt [-y] to[.s]|label < ciphertext > plaintext

        Decrypt the PKCS7 ciphertest with the recipient's secret key, output
        the original plaintext.

            -y - use the labeled secret key in yubihsm2

    cryptool [opts] generate [-b bits] [-i 'info text'] [-s secret.s] [-d days] [-l] [-n name] keyname [signer[.p signer.s]]

        Create new keyname.s and keyname.p. If a signer keypair is provided
        then the public key will be signed.

            -b bits - specify number of key bits, default is 4096

            -i 'info text' - arbitrary text that will appear in the key files,
            for documentary purposes.

            -s secret.s - Clone the specified secret key rather than generating
            a new one. This allows public certs to be regenerated, for example.

            -d days - number of days that the certificate is valid from time of
            creation. Default is that the certificate is valid for the entire
            21st century, to avoid issues on embedded systems with possibly
            faulty clocks.

            -l - lock the new secret key with passphrase entered on console.

            -c name - use specified common name instead of 'keyname timestamp'

    cryptool [opts] check key[.p] signer[.p]

        Check that the key is signed by the signer key.

    cryptool [opts] lock unlocked[.s] > locked.s

        Lock secret key with a passphrase entered on the console.

    cryptool [opts] unlock locked[.s] > unlocked.s

        Remove the passphrase from secret key.

    cryptool [opts] dump [-m] key.(s|p)

        Dump interesting information about the specified secret or public key.

            -b - show number of RSA key bits

            -c - show the key common name

            -m - show the key modulus in hex

    cryptool [opts] info [-r] < ciphertext|signedtext

        Report whether input is encrypted or signed, and the common name of the
        key that encrypted/signed it.

            -r - show raw CMS info.

All operations exit with 0 in the case of success, otherwise non-zero.

The .p extension is used for public key, .s for secret (private) key.

'opts' can include:

    -v - enable excessive verbosity on stderr

    -s - invoke a shell after creating a configuration environment, to allow
    manual testing within that environment (implies -v)

Operations that support yubihsm2 require that yubihsm-connector is listening on
localhost port 12345, $YUBIHSM contains the full path to yubihsm_pkcs11.so,
and $LIBP11 contains the full path to libp11 pkcs11.so.

Requires openssl CLI version 1.0.1e or later.
