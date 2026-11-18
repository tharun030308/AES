# EX-8-ADVANCED-ENCRYPTION-STANDARD ALGORITHM
# Aim:
To use Advanced Encryption Standard (AES) Algorithm for a practical application like URL Encryption.

# ALGORITHM:
AES is based on a design principle known as a substitution–permutation.


AES does not use a Feistel network like DES, it uses variant of Rijndael.

It has a fixed block size of 128 bits, and a key size of 128, 192, or 256 bits.

AES operates on a 4 × 4 column-major order array of bytes, termed the state
# PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <openssl/evp.h>
#include <openssl/rand.h>

int main()
{
    unsigned char key[32] = "0123456789abcdef0123456789abcdef";
    unsigned char iv[12];
    unsigned char tag[16];
    unsigned char ciphertext[1024];
    unsigned char decrypted[1024];

    char url[500];
    int len, ciphertext_len, decrypted_len;

    printf("Enter URL: ");
    fgets(url, sizeof(url), stdin);
    url[strcspn(url, "\n")] = '\0';

    RAND_bytes(iv, sizeof(iv));

    EVP_CIPHER_CTX *ctx = EVP_CIPHER_CTX_new();

    EVP_EncryptInit_ex(ctx, EVP_aes_256_gcm(), NULL, NULL, NULL);
    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, 12, NULL);
    EVP_EncryptInit_ex(ctx, NULL, NULL, key, iv);

    EVP_EncryptUpdate(ctx, ciphertext, &len,
                      (unsigned char *)url, strlen(url));
    ciphertext_len = len;

    EVP_EncryptFinal_ex(ctx, ciphertext + len, &len);
    ciphertext_len += len;

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_GET_TAG, 16, tag);
    EVP_CIPHER_CTX_free(ctx);

    printf("\nEncrypted URL: ");

    for (int i = 0; i < 12; i++)
        printf("%02x", iv[i]);

    for (int i = 0; i < ciphertext_len; i++)
        printf("%02x", ciphertext[i]);

    for (int i = 0; i < 16; i++)
        printf("%02x", tag[i]);

    ctx = EVP_CIPHER_CTX_new();

    EVP_DecryptInit_ex(ctx, EVP_aes_256_gcm(), NULL, NULL, NULL);
    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, 12, NULL);
    EVP_DecryptInit_ex(ctx, NULL, NULL, key, iv);

    EVP_DecryptUpdate(ctx, decrypted, &len,
                      ciphertext, ciphertext_len);
    decrypted_len = len;

    EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_TAG, 16, tag);

    int result = EVP_DecryptFinal_ex(ctx, decrypted + len, &len);
    EVP_CIPHER_CTX_free(ctx);

    if (result > 0)
    {
        decrypted_len += len;
        decrypted[decrypted_len] = '\0';
        printf("\n\nDecrypted URL: %s\n", decrypted);
    }
    else
    {
        printf("\nDecryption failed!\n");
    }

    return 0;
}
```

# OUTPUT:
<img width="841" height="87" alt="image" src="https://github.com/user-attachments/assets/6a61283b-4d1f-4606-8af5-dab587d356f0" />



# RESULT:
Thus the given program has been executed successfully



