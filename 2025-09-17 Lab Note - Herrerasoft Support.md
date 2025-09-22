---
tags:
  - labnote/herrerasoft/support
---

 > [!info]
 > Today I learned how to access the server address by the `appsettings` I also was finally able to send the authentication cookies to make calls to the API directly from Blazor.
  
 - I am using Blazor server for this project, I can choose between sending api calls from the server or skipping the api completely since the solution is bundled up together. I want to keep using the api since I feel is better to keep it in the long run
 - I am going to follow the [Documentation](https://learn.microsoft.com/en-us/aspnet/core/blazor/security/additional-scenarios?view=aspnetcore-8.0) to add tokens when sending an api call from Blazor Server.
 - First I must add a `DelegationHandler`
 ```csharp
using System.Net.Http.Headers;
using Microsoft.AspNetCore.Authentication;

public class TokenHandler(IHttpContextAccessor httpContextAccessor) : 
    DelegatingHandler
{
    protected override async Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken cancellationToken)
    {
        if (httpContextAccessor.HttpContext is null)
        {
            throw new Exception("HttpContext not available");
        }

        var accessToken = await httpContextAccessor.HttpContext.GetTokenAsync("access_token");

        if (accessToken is null)
        {
            throw new Exception("No access token");
        }

        request.Headers.Authorization =
            new AuthenticationHeaderValue("Bearer", accessToken);

        return await base.SendAsync(request, cancellationToken);
    }
}
 ```

- Then, the required services are added to `Program.cs`

 ```csharp
builder.Services.AddHttpContextAccessor();

builder.Services.AddScoped<TokenHandler>();

builder.Services.AddHttpClient("{HTTP CLIENT NAME}",
      client => client.BaseAddress = new Uri("{BASE ADDRESS}"))
      .AddHttpMessageHandler<TokenHandler>();
 ```

- The documentation also recommends to supply the HTTP client base address from configuration

```csharp
new Uri(builder.Configuration["ExternalApiUri"] ?? throw new IOException("No URI!"))
```

- In `appsettings.json` add `"ExternalApiUri": "http://localhost:5174/",`
- Full example 
```csharp
builder.Services.AddHttpClient("Api",
      client => client.BaseAddress = new Uri(builder.Configuration["ExternalApiUri"] ?? throw new IOException("No URI")))
      .AddHttpMessageHandler<TokenHandler>();
```

- `HttpClient` can be created by a component to make secure web API requests

```csharp
@inject IHttpClientFactory ClientFactory

using var request = new HttpRequestMessage(HttpMethod.Get, "{REQUEST URI}");
var client = ClientFactory.CreateClient("{HTTP CLIENT NAME}");
using var response = await client.SendAsync(request);
```

- [!] This exception stops the app, when I try to call the login api
```exception
Unable to find the required 'IAuthenticationService' service. Please add all the required services by calling 'IServiceCollection.AddAuthentication' in the application startup code.
```
- Solved, but can be improved
```csharp
 builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
 .AddCookie(options =>
 {
     options.LoginPath = "/login";
 });
 ...
app.UseAuthentication();
app.UseAuthorization();
```

- Made a separate `HttpClient` so that login works, since it does not send token.
- [!] Trying to use it on my situation, I don't know how to send the email and password
- [!] Apparently can't send them, but the api responds like the Uri does not exist.
- My mistake, `api/user/login` instead of `api/users/login`
- Final code
```csharp
var client = ClientFactory.CreateClient("PublicApi");
var response = await client.PostAsJsonAsync("api/users/login", new
{
    email = model.Email,
    password = model.Password
});
```

- Apparently the method explained in the docs only work if using OIDC. I am going to rework the http management.
- The solution I came up with is just returning the token in the response instead of by cookies
- Blazor then stores it for future requests with `ProtectedSessionStorage` and returns it with `AuthenticationHeaderValue`