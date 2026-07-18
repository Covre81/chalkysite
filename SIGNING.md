# SIGNING.md — Assinatura e custódia de chaves (ChalkySite)

> Este arquivo documenta O PROCEDIMENTO. Nenhum segredo (senha, keystore)
> vive no Git. `keystore.properties` e `keystore/*.jks` estão no .gitignore
> — mantenha assim.

## Onde as coisas estão

| Item | Local |
|---|---|
| Upload keystore | `keystore/chalkysite-release.jks` (local, fora do Git) |
| Senhas do keystore | `keystore.properties` na raiz (local, fora do Git) |
| Alias da chave | `chalkysite` |
| Package name | `com.chalkysite.app` |
| Conta Play Console | itarana31@gmail.com |
| Backup atual | OneDrive → `ChalkySite-keystore-backup/` |

## As 3 camadas de proteção

1. **Play App Signing (Google)** — a chave de assinatura real fica com a
   Google; o `.jks` local é só a *upload key*. Se ela se perder:
   Play Console → Setup → App signing → "Request upload key reset"
   (~2 dias úteis). Confirmar que está ativo no primeiro upload do AAB.
2. **Gerenciador de senhas** — uma entrada "ChalkySite — Android Signing"
   com: o arquivo `.jks` anexado, store password, key password, alias,
   SHA-256 do certificado e o package name.
3. **Cópia de desastre criptografada** — `.jks` cifrado (age ou gpg -c)
   em dois lugares independentes (nuvem + pendrive/HD externo).
   A frase-secreta fica no gerenciador de senhas.

## Comandos

Criptografar a cópia de desastre:

    age -p keystore/chalkysite-release.jks > chalkysite-release.jks.age
    # ou: gpg -c keystore/chalkysite-release.jks

Teste de restauração (fazer 1x/ano e ao trocar de máquina):

    keytool -list -v -keystore keystore/chalkysite-release.jks
    # a senha deve funcionar e o SHA-256 deve bater com a página
    # App signing do Play Console

## Nunca

- Commitar `.jks` ou senhas no Git (mesmo repo privado).
- Manter a única cópia na máquina de desenvolvimento.
- Guardar arquivo e senha juntos sem criptografia.

## Pendências

- [ ] Confirmar Play App Signing ativo após o primeiro upload do AAB
- [ ] Anotar SHA-1/SHA-256 da página App signing aqui (não é segredo)
- [ ] Mover o backup do OneDrive para dentro do gerenciador de senhas
      (hoje o `.jks` e o `keystore.properties` estão lá em claro —
      funciona, mas o ideal é cofre criptografado + cópia .age offline)
