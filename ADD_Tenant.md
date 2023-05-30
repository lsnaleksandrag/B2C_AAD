# Tworzenie aplikacji w tenanacie z AAD
## 1. Tworzenie aplikacji 
W głownym menu azure klikamy App Registration i New Registration
Rejestrujemy aplikacje z opcjami<br />
  a) Supported account types: `Accounts in this organizational directory only (Single tenant)`<br />
  b) Redirect URI (optional): wybierz WEB and wpisz url tenanta B2C wedlug poniższego szablonu <br />
`https://${your-tenant-name}.b2clogin.com/${your-tenant-id}/oauth2/authresp`
Kliknij Register na dole strony 
## 2. Dodawanie sekretu do aplikacji
W uprzednio stworzonej aplikacji klikamy w Cerfitifcate & Secrets 
(po lewo w menu azure po wejsciu w aplikacje) klikamy w Secret i add a secret. Kopiujemy wartośc(jest ona widoczna tylko   przy tworzeniu)
<img width="800" alt="222176373-4b726ec3-18bf-4c0a-a830-2548f9a0a73d" src="https://user-images.githubusercontent.com/125549354/241729124-90b9044a-95e3-4acc-be77-18074376ddf5.png">
 ## 3. Api Permission 
W api permission ( bedzie w tym samym menu po lewej co Certificates & Secrets) musimy dodać uprawnienia "profile"    klikamy w  Add Permission > Microsoft Graph > Delegated Permissions > i szukamy profile
![image](https://user-images.githubusercontent.com/125549354/241729629-e8005e7b-dd9d-4663-800d-5dd6b0d81801.png)
## 4. Dodawanie manifestu
W Manifest ( bedzie w tym samym menu po lewej co dwa poprzednie kroki) potrzebujemy zmienic kilka rzeczy

### AcceptMappedClaims
z<br />
```
"acceptMappedClaims": null,
```
na<br />
```
"acceptMappedClaims": true,
```
### GroupMembershipClaims
z<br />
```
"groupMembershipClaims": null,
```
na<br />
```
"groupMembershipClaims": "All",
```

### OptionalClaims
z<br />
```
"optionalClaims": null,
```
na<br />
```
"optionalClaims": {
  "idToken": [
    {
      "name": "groups",
      "source": null,
      "essential": false,
      "additionalProperties": [
        "sam_account_name"
      ]
    }
  ],
  "accessToken": [
    {
      "name": "groups",
      "source": null,
      "essential": false,
      "additionalProperties": []
    }
  ],
  "saml2Token": [
    {
      "name": "groups",
      "source": null,
      "essential": false,
      "additionalProperties": []
    }
  ]
}
```
Trzeba skopiowac, cały powyższy kod dokładnie tak jak jest napisany inaczej Manifest nie pozwoli na zapis zmian.

## 5. Enterprise Application
W menu Azure znajdź Enterprise Applications i wyszukaj aplikacje po nazwie którą stworzyliśmy w pierwszym kroku i w menu po lewej wybierz Single-SignOn. Będziemy musieli zmienić Edit Attributes & Claims tak jak ponizej.

|name|source attribute|
|-----|----------------|
|SamAccName|user.onpremisessamaccountname|
|firstName|user.givenname|
|lastName|user.surname|
|displayName|user.displayname|
![image](https://user-images.githubusercontent.com/125549354/241732254-203a0764-2bae-4fb9-89ad-f8df9ffc06aa.png)
![image](https://user-images.githubusercontent.com/125549354/241732290-71dd6b9f-b64c-4770-beee-f555677d7b49.png)
## Co bedzie potrzebne do integracji z aplikacją na tenancie B2C
 applicationId - stworzonej w pierwszym kroku<br />
clientSecret - stworzonego w drugim kroku <br />
tenantID - AAD Tenanta na którym stworzona została aplikacja <br />

