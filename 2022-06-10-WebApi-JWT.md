# JWT

## token
+ Token的引入：Token是在客户端频繁向服务端请求数据，服务端频繁的去数据库查询用户名和密码并进行对比，判断用户名和密码正确与否，并作出相应提示，在这样的背景下，Token便应运而生

+ Token的定义：Token是服务端生成的一串字符串，以作客户端进行请求的一个令牌，当第一次登录后，服务器生成一个Token便将此Token返回给客户端，以后客户端只需带上这个Token前来请求数据即可，无需再次带上用户名和密码

+ 使用Token的目的：Token的目的是为了减轻服务器的压力，减少频繁的查询数据库，使服务器更加健壮

颁发Token
```cs
FormsAuthenticationTicket token = new FormsAuthenticationTicket(0,uName,DateTime.Now,DateTime.Now.AddHours(12),true,$"{UserName}&{PassWord}",FormsAuthentication.FormsCookiePath);
var _token = FormsAuthentication.Encrypt(token);
TokenValue = _token;
```

获取Token 
```cs
var token = HttpContext.Current.Request.Headers["TokenValue"];
```

Filter 常件的有：
+ Authorization Filter（认证过滤器）
+ Resource Filter（资源过滤器）
+ Exception Filter（异常过滤器）
+ Action Filter（方法过滤器）
+ Result Filter（结果过滤器）

需要引入包
+ System.IdentityModel.Tokens.Jwt 
+ Microsoft.IdentityModel.Tokens 

创建实体类用于数据库建表
```cs
public class TokenParameter
{
    public string Secret { get; set; }
    public string Issuer { get; set; }
    public double AccessExpiration { get; set; }
    public double RefreshExpiration { get; set; }
}
```

TokenHelper 文件
```cs
public static string GenerateToken(TokenParameter tokenParameter, string username, string rolename)
{
    var claims = new[]
    {
        new Claim(ClaimTypes.Name,username),
        new Claim(ClaimTypes.Role,rolename)
    };
    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(tokenParameter.Secret));
    var credentials = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
    var securityToken = new JwtSecurityToken(tokenParameter.Issuer, null, claims, expires: DateTime.UtcNow.AddMinutes(tokenParameter.AccessExpiration), signingCredentials: credentials);
    //生成token
    var token = new JwtSecurityTokenHandler().WriteToken(securityToken);
    return token;
}
```

token 颁发方
```cs
"TokenParameter": {
    "Secret": "112358112358112358",
    "Issuer": "中华人民共和国",
    "AccessExpiration": 120,
    "RefreshExpiration": 1440
  }
```
