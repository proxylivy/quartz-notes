Lecturas Recomendadas
- [Cisco Community - Tim Glen - Understand Cisco Password Types](https://community.cisco.com/t5/networking-knowledge-base/understanding-the-differences-between-the-cisco-password-secret/ta-p/3163238)
- [InfosecMatter - Cisco Password Cracking and Decrypt Guide](https://www.infosecmatter.com/cisco-password-cracking-and-decrypting-guide/)
- [NSA - Best Practices for Selecting Cisco Password Types](https://www.nsa.gov/Press-Room/News-Highlights/Article/Article/2938313/nsa-publishes-best-practices-for-selecting-cisco-password-types/)
- [Cisco Security - Next Generation Cryptography](https://sec.cloudapps.cisco.com/security/center/resources/next_generation_cryptography)

> [!NOTE] Ataques Conocidos
> Las contraseñas utilizadas se guardan en diccionarios, el mas conocido es [Github - danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) (Rockyou).

TL;DR
- Tipo 0, 4, 5, 7: NO
- Tipo 6: Transicional (Solo usar si Tipo 9/8 no estan soportados)
- Tipo 9 y luego 8: Hash Seguros

Explicado de mejor forma
- Tipo 0: Texto Plano
- Tipo 4: Fallo peor que MD5
- Tipo 5: MD5 Salteado, comienzan con `$5$`
- Tipo 6: Encriptado AES 128, visible en run-config. Mas mañoso
- Tipo 7: Vigenere, Olvidalo
- Tipo 8: Hash PBKDF2, SHA-256, 80-bit salteado, 20.000 iteraciones, comienza en `$8$`. Esta aprovada por el NIST y la NSA
- Tipo 9: Algoritmo [SCRYPT](https://www.tarsnap.com/scrypt.html), Documentado en el [RFC7914](https://www.rfc-editor.org/rfc/rfc7914), se implemento en la version de IOS 15.3(3) alrededor de 2012. No esta aprovada por el NIST