---
tags:
  - labnote/herrerasoft/support
---
- [!] I need the app to let the user access certain pages until the user logs in
- I am going to start with `@attribute [Authorize]` to the pages I want to block (all but log in)
- It does work, but I want to access when signing in
- I need to create a service to verify the authorization using `api/users/me` 
```csharp
using Microsoft.AspNetCore.Components.Authorization;
using System.Net.Http.Json;
using System.Security.Claims;

public class ApiAuthenticationStateProvider : AuthenticationStateProvider
{
    private readonly HttpClient _http;

    public ApiAuthenticationStateProvider(HttpClient http)
    {
        _http = http;
    }

    public override async Task<AuthenticationState> GetAuthenticationStateAsync()
    {
        try
        {
            var userInfo = await _http.GetFromJsonAsync<UserInfo>("/me");

            if (userInfo is not null && userInfo.IsAuthenticated)
            {
                var claims = new List<Claim>
                {
                    new Claim(ClaimTypes.Name, userInfo.Username)
                };

                // Si tienes roles, agrégalos:
                if (userInfo.Roles != null)
                {
                    claims.AddRange(userInfo.Roles.Select(r => new Claim(ClaimTypes.Role, r)));
                }

                var identity = new ClaimsIdentity(claims, "apiauth");
                var user = new ClaimsPrincipal(identity);

                return new AuthenticationState(user);
            }
        }
        catch
        {
            // si el endpoint regresa 401 o falla, se sigue como no autenticado
        }

        return new AuthenticationState(new ClaimsPrincipal(new ClaimsIdentity()));
    }

    public void NotifyUserAuthentication(string username)
    {
        var identity = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, username)
        }, "apiauth");

        var user = new ClaimsPrincipal(identity);
        NotifyAuthenticationStateChanged(Task.FromResult(new AuthenticationState(user)));
    }

    public void NotifyUserLogout()
    {
        var anonymous = new ClaimsPrincipal(new ClaimsIdentity());
        NotifyAuthenticationStateChanged(Task.FromResult(new AuthenticationState(anonymous)));
    }
}

public class UserInfo
{
    public bool IsAuthenticated { get; set; }
    public string Username { get; set; }
    public string[] Roles { get; set; }
}

```
- I also declare it in `Program.cs`
```csharp
builder.Services.AddScoped<AuthenticationStateProvider, ApiAuthenticationStateProvider>();
builder.Services.AddAuthorizationCore();
```

- Add a `AuthorizeView`
```csharp
<AuthorizeView>
    <Authorized>
        <p>Bienvenido @context.User.Identity.Name</p>
    </Authorized>
    <NotAuthorized>
        <p>No estás logueado</p>
    </NotAuthorized>
</AuthorizeView>
```

- Login flow
```csharp
((ApiAuthenticationStateProvider)authStateProvider)
    .NotifyUserAuthentication(username);
```
- Update `api/users/me` to work with the authentication
```csharp
[Authorize]
[HttpGet("me")]
public IActionResult GetCurrentUser()
{
    var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
    var email = User.FindFirst(ClaimTypes.Email)?.Value;
    var roles = User.FindAll(ClaimTypes.Role).Select(r => r.Value).ToArray();

    if (userId == null)
    {
        return Unauthorized(new { message = "User is not authenticated" });
    }

    return Ok(new
    {
        isAuthenticated = true,
        userId,
        email,
        roles
    });
}
```

- I finally solved it, the main problem is that the server "reloads" when I navigate, I need to navigate in a certain way that the server does not refresh or save the Token on the backend
- I am going to skip the api completely and make the calls to the backend, there is no need to use the api if I use Blazor server
