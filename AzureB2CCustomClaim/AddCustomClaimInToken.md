
Recently I'm trying to add custom attributes to claim in JWT token from Azure Directory B2C. It seems easy but it takes me hours to figure out how to do it.
Adding custom attribute and assign it to cliam does not seem to do the trick since I used ```Sign in using resource owner password credentials (ROPC) and not default user flow.
(ROPC)``` Allow you to sign in directly in native application. So, no Addditional redirect page to microsoft to login. Also, in my backend I use Microsoft graph to handle management tasks such as creating and updating user detail or even login and sign up.


Here is what I've done. When I create a user I want to attach custom claims to token when user login successfully.
In my Login method

```
public async Task<User> CreateUser(string aUsername, string aPassword, string aFullNmae)
    {

      Logger.LogInformation("Creating user {username}", aUsername);
      string principalName = RemoveSpecials(aUsername);
      var user = new User
      {
        AccountEnabled = true,
        DisplayName = aFullNmae.Trim(),
        MailNickname = "Personal",
        UserType = "Member",
        UserPrincipalName = $"{principalName}@{MicrosoftGraphClientOptions.Issuer}",
        PasswordProfile = new PasswordProfile
        {
          Password = aPassword,
          ForceChangePasswordNextSignIn = false,
          ForceChangePasswordNextSignInWithMfa = false
        },
        
        Identities = new[]
        {
          new ObjectIdentity
          {
            SignInType = "emailAddress",
              Issuer = MicrosoftGraphClientOptions.Issuer,
              IssuerAssignedId = aUsername
          }
        }
      };

      //User custom attributes
      IDictionary<string, object> extensionInstance = new Dictionary<string, object>();
      extensionInstance.Add("extension_57286e58017f45a7b574dcaa6008f9b6_Bob", "Member");
      user.AdditionalData = extensionInstance;

      User newUser = await GraphServiceClient.Users
        .Request()
        .AddAsync(user);
      Logger.LogInformation("Successfully created user {username}", aUsername);
      return newUser;
    }
 ```


###It simple where I use Microsoft Graph API to create a user
- Create user object and assign values to user properties included username and password along with other detail.
- Use Graph API to send request to create user.

In User object, it has property called Additional data where it take dictionary key-pair value where key represent key for the claim with the value.

``` IDictionary<string, object> extensionInstance = new Dictionary<string, object>();
extensionInstance.Add("extension_57286e58017f45a7b574dcaa6008f9b6_Bob", "Member");
user.AdditionalData = extensionInstance; 
```

###For some reson, you need to use format 
```extention_{B2CClientID without hyphen}_{CustomAttributeName}``` like ```extension_57286e58017f45a7b574dcaa6008f9b6_Bob```

You will need to create user attribute in Azure B2C Portal and the name of attribute must match with the one we use in key dictionary. In above example, Bob is the name of attribute.

When you decode the token you will see custom claim I have added to the claim.
