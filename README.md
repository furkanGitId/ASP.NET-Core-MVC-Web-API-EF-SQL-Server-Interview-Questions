
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
**Easy language**
Index matlab **book ka index page.**

**Clustered Index**
-   Data **physically sorted** hota hai
-   Table me sirf **1 clustered index**
-   Usually **Primary Key**

```csharp
CREATE CLUSTERED INDEX IX_Emp_Id ON Employee(EmpId);
```

**Non-Clustered Index**
-   Alag structure hota hai
-   Data ka **pointer** rakhta hai
-   Max **999**

```csharp
CREATE NONCLUSTERED INDEX IX_Emp_Name ON Employee(Name);
```

| Feature | Clustered | Non-Clustered |
|--------|-----------|---------------|
| Max | 1 | 999 |
| Stores | Actual data | Pointer to data |
| Speed | Fast | Thoda slow |
| Default | PK | Optional |

---

## ⭐ 11. Stored Procedure vs Function vs View
**Stored Procedure**
-   Insert, Update, Delete sab
-   Transaction support
-   Output parameter allowed

```csharp
CREATE PROCEDURE GetEmployees
AS
BEGIN
    SELECT * FROM Employee;
END
```

**Function**
-   Value return karta hai
-   SELECT ke andar use hota hai

```csharp
CREATE FUNCTION GetEmpCount()
RETURNS INT
AS
BEGIN
    RETURN (SELECT COUNT(*) FROM Employee)
END
```

**View**
-   Saved SELECT query
-   Logic hide karne ke kaam aata hai

```csharp
CREATE VIEW vw_Employees
AS
SELECT Name, Salary FROM Employee;
```

| Feature | Stored Procedure | Function | View |
|--------|------------------|---------|------|
| Return | Any | Scalar/Table | Table |
| Output Param | Yes | No | No |
| Transaction | Yes | Limited | No |

---

## ⭐ 12. async-await Benefit
**Simple idea**

Normal (sync) code me jab DB call hota hai, tab **thread wait karta rehta hai.**

Async code me thread free ho jaata hai jab tak DB response nahi aata.

**Real life example**

Socho restaurant me waiter:

-   **Sync:** waiter khana aane tak table ke paas khada rahe

-   **Async:** waiter dusre tables serve kare jab tak khana ban raha ho

```csharp
public async Task<IActionResult> Get() =>
    Ok(await context.Employees.ToListAsync());
```

**Benefit**

-   Thread pool block nahi hota

-   Zyada users ko handle kar sakte ho

-   Web app fast feel hoti hai

---

## ⭐ 13. Middleware Order Matters

```csharp
app.UseAuthentication(); 
app.UseAuthorization();
```

---

## ⭐ 14. Attribute vs Conventional Routing
**Attribute Routing (Recommended)**
Controller ke upar route define

```csharp
[Route("api/[controller]")]
[ApiController]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult Get(int id) { ... }
}
```

```bash
/api/products/5
```

**Conventional Routing**

```Program.cs``` me route defined hota hai (old style)

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

## ⭐ 16. SELECT 1 vs SELECT \* vs SELECT column

-   SELECT 1: Sabse fast. Sirf existence check hota hai.
-   SELECT \*: Saare columns fetch karta hai, IO heavy.
-   SELECT column: Covering index ke saath fast, sirf required data.

---

## ⭐ 17. UNION vs UNION ALL
**UNION**
-   Duplicate remove karta hai
-   Internally sorting karta hai
-   Slow

```sql
SELECT Name FROM Emp1
UNION
SELECT Name FROM Emp2;
```

**UNION ALL**
-   Duplicate allow
-   Fast

```sql
SELECT Name FROM Emp1
UNION ALL
SELECT Name FROM Emp2;
```

-   UNION: Duplicates remove karta hai, slow.
-   UNION ALL: Duplicates ko rehne deta hai, fast.

---

## ⭐ 18. EXISTS vs IN vs JOIN

-   EXISTS: Pehla match milte hi ruk jata hai, large outer table ke liye
    best.
-   IN: List-based check karta hai, NULL ka dhyaan.
-   JOIN: One-to-many ho to duplicates aa sakte hain.

---

## ⭐ 19. Window Functions
**Problem**

Har department ka highest salary wala employee chahiye

``` sql
SELECT ROW_NUMBER() OVER (PARTITION BY DeptId ORDER BY Salary DESC) AS rn
FROM Employee;
```

-   ```PARTITION BY``` → group jaisa
-   ```ORDER BY``` → ranking

---

## ⭐ 20. CTE vs Temp Table vs Table Variable
**CTE**
-   Temporary result set
-   Same query ke andar

```sql
WITH EmpCTE AS (
    SELECT * FROM Employee
)
SELECT * FROM EmpCTE;
```

**Temp Table**
-   Disk pe banti hai
-   Large data ke liye best

```sql
CREATE TABLE #TempEmp (Id INT);
```

**Table Variable**
-   Memory based
-   Small data

```sql
DECLARE @Emp TABLE (Id INT);
```

  Feature    CTE                 Temp Table (#)   Table Variable (@)
  ---------- ------------------- ---------------- --------------------
  Scope      Single statement    Session          Batch
  Index      No                  Yes              Sirf PK
  Stats      No                  Yes              No
  Best Use   Clean & recursive   Large data       Small data / TVP

---

## ⭐ 21. Deadlock -- Kya hai & Fix kaise karein

-   Do queries ek dusre ka lock wait karte rehte hain → deadlock.
-   Debug: trace flag 1222, system_health.
-   Fix: Same order mein locks lo, transactions short rakho, indexes
    tune karo.

---

## ⭐ 22. Covering Index
**Problem**

Query ko data + extra columns chahiye

**Solution**

Include columns index me hi add kar do

``` sql
CREATE INDEX IX_Emp_Name_Cover ON Employee(Name) INCLUDE (Salary, DeptId);
```

Result: Query ko table hit karna nahi padta.

---

## ⭐ 23. Parameter Sniffing

-   SQL pehla parameter dekhkar plan cache karta hai.
-   Fix: RECOMPILE, OPTIMIZE FOR, local variables, proc split.

---

## ⭐ 24. Pagination
**Use case**

Page-wise data load karna

``` sql
SELECT * FROM Employee
ORDER BY EmpId
OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
```

-   OFFSET → skip
-   FETCH → kitne chahiye

---

## ⭐ 25. TVP -- Bulk Insert from .NET
**Problem**

Loop me insert slow hota hai

**SQL Type**
``` sql
CREATE TYPE dbo.EmpTableType AS TABLE
(
    Id INT,
    Name NVARCHAR(50)
);
```

**.NET side**
``` csharp
var table = new DataTable();
table.Columns.Add("Id", typeof(int));
table.Columns.Add("Name", typeof(string));
table.Rows.Add(1, "Raj");
```

**Benefit**

Bulk insert
Performance best

---

# ⭐ 26. CHAR vs VARCHAR vs NVARCHAR

## 1. CHAR

Fixed-length data type.\
Agar `CHAR(10)` mein sirf 3 letters store kiye, toh 10 characters ka
space reserve karega (padding ke saath).

``` sql
CREATE TABLE SampleChar (
    Code CHAR(10)
);

INSERT INTO SampleChar VALUES ('Sam');
-- Stored: 'Sam       '
```

**Kab Use Karein:**
- Jab length fix ho
- Examples: `Gender ('M', 'F')`, `Country Code ('IN', 'USA')`

------------------------------------------------------------------------

## 2. VARCHAR

Variable-length data type.\
Sirf actual data jitna space leta hai.

``` sql
CREATE TABLE SampleVarchar (
    Name VARCHAR(10)
);

INSERT INTO SampleVarchar VALUES ('Sam');
-- Stored: 'Sam'
```

**Kab Use Karein:**
- Jab data ki length change hoti rahe
- Examples: Names, Emails, Address

------------------------------------------------------------------------

## 3. NVARCHAR

VARCHAR jaisa hi lekin Unicode support karta hai.\
Hindi, Tamil, Telugu, Chinese, Emojis sab store ho jate hain.

``` sql
CREATE TABLE SampleNvarchar (
    Description NVARCHAR(50)
);

INSERT INTO SampleNvarchar VALUES (N'नमस्ते दुनिया');
-- N prefix = Unicode literal
```

**Kab Use Karein:**
- Jab multi-language text store karna ho
- Examples: Hindi names, Unicode comments, Emojis

---

# ⭐ 27. Static ke Questions

**Q1. static keyword kya karta hai?**\
Ans: Ye value ko class level par store karta hai. Object banane ki
zaroorat nahi hoti.

**Q2. static constructor kya hota hai?**\
Ans: Ye class load hote hi ek baar run hota hai. Parameters nahi leta.

**Q3. static aur instance members me difference?**\
Ans: static class ka hota hai, instance object ka.

---

# ⭐ 28. Readonly ke Questions

**Q1. readonly field kab set ho sakti hai?**\
Ans: Declaration par ya constructor ke andar.

**Q2. readonly aur const me kya difference hai?**\
Ans: const compile time fixed hota hai. readonly runtime me bhi set ho
sakta hai (constructor ke through).

---

# ⭐ 29. Interface ke Questions

**Q1. Interface kya hota hai?**\
Ans: Ek rulebook jisme sirf method names hote hain.

**Q2. Kya interface me constructor ho sakta hai?**\
Ans: Nahi.

**Q3. Kya class multiple interfaces implement kar sakti hai?**\
Ans: Haan.

**Q4. Interface ka real use kya hai?**\
Ans: Loose coupling aur common rules set karna.

---

# ⭐ 29. Abstract Class ke Questions

**Q1. Abstract class kya hoti hai?**\
Ans: Ek base class jisme complete + incomplete methods dono ho sakte
hain.

**Q2. Kya abstract class ka object ban sakta hai?**\
Ans: Nahi.

**Q3. Abstract aur non-abstract methods me difference?**\
Ans: Abstract me body nahi hoti. Non-abstract me hoti hai.

---

# ⭐ 30. Abstract vs Interface ke Top Questions

**Q1. Abstract class aur interface me main difference?**\
Ans: Abstract code de sakti hai. Interface sirf rules deta hai.

**Q2. Kab abstract class use karte hain?**\
Ans: Jab kuch common logic share karna ho.

**Q3. Kab interface use karte hain?**\
Ans: Jab sirf rule dena ho aur multiple inheritance chahiye ho.

---

# ⭐ 31. const vs readonly ke Questions

**Q1. const kya hota hai?**  
Ans: `const` ek aisa variable hota hai jiska value compile time pe fix hota hai aur kabhi change nahi hota.

**Q2. readonly kya hota hai?**  
Ans: `readonly` ka value runtime pe set ho sakta hai, mostly constructor ke andar.

**Q3. const aur readonly me main difference kya hai?**  
Ans: `const` ka value compile time pe decide hota hai, jabki `readonly` ka value runtime pe.

**Q4. Kya const ka value baad me change ho sakta hai?**  
Ans: Nahi, bilkul nahi.

**Q5. Kya readonly ka value change ho sakta hai?**  
Ans: Sirf constructor me set ho sakta hai, uske baad nahi.

---

# ⭐ 32. Inheritance ke Questions

**Q1. Inheritance kya hota hai?**  
Ans: Jab ek class dusri class ki properties aur methods use karti hai, use inheritance kehte hain.

**Q2. Inheritance kyun use karte hain?**  
Ans: Code reuse ke liye aur duplication kam karne ke liye.

**Q3. Parent class ko kya kehte hain?**  
Ans: Base class ya Super class.

**Q4. Child class ko kya kehte hain?**  
Ans: Derived class ya Sub class.

**Q5. Inheritance ka real example kya hai?**  
Ans: `Animal` parent class hai aur `Dog` child class, jo animal ke features use karta hai.

---

# ⭐ 33. Polymorphism ke Questions

**Q1. Polymorphism kya hota hai?**  
Ans: Same method ka different behavior hona hi polymorphism hai.

**Q2. Polymorphism ke kitne types hote hain?**  
Ans: Do types hote hain: Compile-time aur Runtime.

**Q3. Compile-time polymorphism kya hota hai?**  
Ans: Jab method overloading use hoti hai.

**Q4. Runtime polymorphism kya hota hai?**  
Ans: Jab method overriding hoti hai aur object runtime pe decide hota hai.

**Q5. Polymorphism ka real life example kya hai?**  
Ans: Shape class ka `Draw()` method circle ke liye alag aur rectangle ke liye alag kaam karta hai.

---

# ⭐ 34. Normalization ke Questions

**Q1. Normalization kya hota hai?**  
Ans: Database ke data ko properly organize karna hi normalization hai.

**Q2. Normalization kyun use karte hain?**  
Ans: Duplicate data kam karne aur data consistency maintain karne ke liye.

**Q3. Normalization ka main benefit kya hai?**  
Ans: Data redundancy kam hoti hai aur update easy ho jata hai.

**Q4. Normal forms kaun kaun si hoti hain?**  
Ans: 1NF, 2NF, 3NF, BCNF.

**Q5. 1NF ka simple rule kya hai?**  
Ans: Table me duplicate ya multi-value data nahi hona chahiye.

---

# ⭐ 35. ACID ke Questions

**Q1. ACID kya hota hai?**  
Ans: ACID database transaction ke rules hote hain.

**Q2. ACID ka full form kya hai?**  
Ans: Atomicity, Consistency, Isolation, Durability.

**Q3. Atomicity kya hoti hai?**  
Ans: Transaction ya to poori complete hogi ya bilkul nahi hogi.

**Q4. Consistency kya hoti hai?**  
Ans: Transaction ke baad data hamesha valid state me rahe.

**Q5. Durability kya hoti hai?**  
Ans: Transaction commit hone ke baad data permanently save ho jata hai, crash me bhi safe rehta hai.

---

## Pro Tip

Agar interview mein koi issue-based question aaye, jaise “site slow ho gayi,” to sabse pehle:

Execution Plan

Missing Indexes

Parameter Sniffing

In teenon se zyada tar real-world issues solve ho jaate hain.

## Pro Tip: In interview, agar koi scenario-based sawaal aaye (e.g. “site slow ho gaya”) to pehle Execution Plan dekho, missing indexes & parameter-sniffing batana – 80% chances interviewer impress.

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
