# Applications

## MSI Installers

We can use an MSI package builder to create our own malicious MSI files.

As an example, we can use [EMCO MSI Package Builder](https://emcosoftware.com/msi-package-builder/download) to create an MSI file.

We can sign an MSI file with the [Microsoft SDKs signtool](https://docs.microsoft.com/en-us/windows/win32/seccrypto/signtool):

```cmd
signtool.exe sign /v /f \Users\username\Desktop\MySPC.pfx /tr "http://timestamp.digicert.com" /td sha256 /fd sha256 \Users\username\Desktop\Project\Filename.msi
```

We could verify the signature with the following:

```cmd
signtool.exe verify /pa \Users\username\Desktop\Project\Filename.msi
```

> Note: You might need a certificate authority (CA) certificate.

