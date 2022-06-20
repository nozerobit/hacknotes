# Bypassing Smart Card

## Bypassing Smart Card

Smart cards are a smart way to add security. However, they can still be disabled in many ways.

### Windows Disable Smart Card

The Smart Card Credential Provider from Windows Server 2012 can be disabled using registry keys:

A. Method 1 (This does not require the RSA Agent to be installed on the machine):

```
1. Open the following registry key: HKEY_LOCAL_MACHINE\SOFTWARE \Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{8fd7e19c-3bf7-489b-a72c-846ab3678c96}
2. Verify that the default value is @="Smartcard Credential Provider"
3. Add the following DWORD value: "Disabled"=dword:00000001
```

B. Method 2 ( This requires the RSA Agent to be installed on the machine):

Create the following REG\_STRING value (everything in red will need to be added by hand): \[HKEY\_LOCAL\_MACHINE\SOFTWARE\Policies\RSA\RSA Desktop\Credential Provider Filtering\Smartcard Credential Provider] "ExcludeProvider"="1"

## References

{% embed url="https://community.securid.com/t5/securid-knowledge-base/how-to-disable-smart-card-credential-provider-on-windows-2012/ta-p/11965" %}

{% embed url="https://www.techwalla.com/articles/how-to-disable-a-smart-card-login" %}
