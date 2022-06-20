# LAPS Abuse

Local Administrator Password Solution (LAPS) is a Microsoft product that manages the local administrator password and stores it in Active Directory (AD). This solution automatically updates the password on a routine basis.&#x20;

## Abusing LAPS

Its possible that LAPS or LDAP has been misconfigured enough to potentially contains the computer passwords for computer object in AD.

The `ms-Mcs-AdmPwd` attribute that stores password in AD is marked as _Confidential_ in AD – this means that users need to have extra permission (CONTROL\_ACCESS permission) to read the value – Read permission is not enough. AD honors the read request for confidential attribute value when at least one of the following is true:

* Caller is granted ‘Full Control’ permission
* Caller is granted ‘All Extended Rights’ permission
* Caller is granted ‘Control Access’ on the attribute permission (this is what LAPS PowerShell uses to grant the permission)

**LAPS and password storage in clear text in AD**

{% embed url="https://blogs.msdn.microsoft.com/laps/2015/06/01/laps-and-password-storage-in-clear-text-in-ad" %}
