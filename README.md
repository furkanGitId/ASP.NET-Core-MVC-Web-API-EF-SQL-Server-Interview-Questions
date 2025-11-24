
# 🚀 ASP.NET Core + MVC + Web API + EF + SQL Server  
**Top Interview Q&A (Hinglish) – 5 to 10 Years Experience**

Real interviews mein pooche jaane wale important questions with short and practical answers.

---

## ⭐ 1. Dependency Injection (DI)
DI ek design pattern hai jisme class apni dependencies khud create nahi karti. Container inject karta hai.

ASP.NET Core mein built-in DI lifetimes:
- **Transient** — Har request par new object
- **Scoped** — HTTP request ke liye single object
- **Singleton** — Application start to stop same object

```csharp
builder.Services.AddScoped<IEmployeeService, EmployeeService>();
builder.Services.AddDbContext<AppDbContext>(x =>
    x.UseSqlServer(connectionString));
```

---

## ⭐ 2. ViewState vs Session vs Cookies

| Feature | ViewState | Session | Cookies |
|--------|-----------|---------|---------|
| Store | Page hidden field | Server memory | Browser |
| Size | 100-200 KB | 25-50 KB default | 4 KB |
| Security | Base64 encoded | Safe | Easily editable |
| Best for | Page specific data | Login, cart | Remember me |

---

## ⭐ 3. Partial View vs ViewBag vs TempData
| Feature | Usage |
|--------|-------|
| **Partial View** | Reusable UI piece, example: `_Header.cshtml` |
| **ViewBag** | Pass data Controller → View (one request) |
| **TempData** | Redirect scenarios |

---

## ⭐ 4. JWT Authentication Flow
1. Login request
2. Server validates user
3. JWT token generate
4. Client stores (localStorage)
5. Every API call: `Authorization: Bearer <token>`

```csharp
var claims = new[] { new Claim(ClaimTypes.Name, user.UserName) };
var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("32-char-secret"));
var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);
var token = new JwtSecurityToken(
    issuer: "myapp.com",
    audience: "myapp.com",
    claims: claims,
    expires: DateTime.Now.AddHours(1),
    signingCredentials: creds);

return Ok(new { token = new JwtSecurityTokenHandler().WriteToken(token) });
```

---

## ⭐ 5. Authentication vs Authorization
| Topic | Meaning |
|------|---------|
| Authentication | Kaun ho tum? |
| Authorization | Tum kya kar sakte ho? |

Example:
```csharp
[Authorize(Roles = "Admin")]
```

---

## ⭐ 6. Code First vs Database First vs Model First

| Approach | Flow | Best For |
|---------|------|----------|
| Code First | C# → Migration → DB | New projects |
| DB First | Reverse engineer from DB | Existing DB |
| Model First | Designer → DB | Rarely used |

---

## ⭐ 7. DbContext, DbSet & EF Migrations
- **DbContext** → DB ka gateway  
- **DbSet** → Table represent karta hai  

Commands:
```bash
Add-Migration AddAgeColumn
Update-Database
Script-Migration
```

---

## ⭐ 8. Tracking vs No-Tracking
- **Tracking** → Update/Delete friendly
- **AsNoTracking** → Fast, Read-only

```csharp
context.Employees.AsNoTracking().ToList();
```

---

## ⭐ 9. Popular LINQ Methods

```csharp
Where   → employees.Where(e => e.Age > 18)
Select  → employees.Select(e => e.Name)
GroupBy → employees.GroupBy(e => e.DeptId)
Join    → employees.Join(departments, e => e.DeptId, d => d.Id, ...)
Any     → employees.Any(e => e.Salary > 100000)
```

---

## ⭐ 10. Clustered vs Non-Clustered Index

| Feature | Clustered | Non-Clustered |
|--------|-----------|---------------|
| Max | 1 | 999 |
| Stores | Actual data | Pointer to data |
| Speed | Fast | Thoda slow |
| Default | PK | Optional |

---

## ⭐ 11. Stored Procedure vs Function vs View

| Feature | Stored Procedure | Function | View |
|--------|------------------|---------|------|
| Return | Any | Scalar/Table | Table |
| Output Param | Yes | No | No |
| Transaction | Yes | Limited | No |

---

## ⭐ 12. async-await Benefit
Thread pool block nahi hota → Requests handle capacity badh jati hai.

```csharp
public async Task<IActionResult> Get() =>
    Ok(await context.Employees.ToListAsync());
```

---

## ⭐ 13. Middleware Order Matters

```csharp
app.UseAuthentication(); 
app.UseAuthorization();
```

---

## ⭐ 14. Attribute vs Conventional Routing

```csharp
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult Get(int id) { ... }
}
```

---

## ⭐ 15. Must-Know HTTP Status Codes
200 OK  
201 Created  
400 Bad Request  
401 Unauthorized  
403 Forbidden  
404 Not Found  
500 Internal Server Error  

---

## 🎯 Quick Tips for Interview Prep
- Har answer loud revise karo
- Code run karke dekh lo
- Mini CRUD project bana lo

---

## 📌 Download this as README.md
Aap is file ko directly download kar sakte ho.  

---

Happy Learning and Good Luck for Interviews.  
🔥 Offer letters aa hi jayenge!
