# Presentation: ASP.NET Core Interview Questions (Beginner-Friendly Hinglish)

## Slide 1: Title Slide

**Heading:** ASP.NET Core Interview Questions: Beginner's Guide (Hinglish)
**Sub-heading:** MVC, Web API, Entity Framework, aur SQL Server ke Fundamentals
**Content:**
*   Aapka pehla step .NET development ki duniya mein!
*   Hum dekhenge sabse important concepts jo interviews mein puche jaate hain.
*   Focus: Simple explanation aur practical examples.

## Slide 2: Dependency Injection (DI) - Why is it important?

**Heading:** Dependency Injection (DI): Code ko Clean aur Flexible Banata Hai
**Sub-heading:** *Jab ek class doosri class par depend karti hai, toh DI use hota hai.*
**Content:**
*   **DI Kya Hai?** DI ek design pattern hai jisme ek class apni *dependencies* (jin cheezon ki use zaroorat hai) khud nahi banati.
*   **Kaun Banata Hai?** Ek special *Container* (jise IoC Container kehte hain) un dependencies ko bana kar class ko *inject* karta hai.
*   **Fayda (Benefit):** Code **loosely coupled** ho jaata hai. Agar dependency change karni ho, toh sirf ek jagah change karna padta hai.
*   **Example:**
    *   `builder.Services.AddScoped<IEmployeeService, EmployeeService>();`
    *   *Meaning:* Jab bhi koi `IEmployeeService` maangega, toh `EmployeeService` ka object milega.

## Slide 3: DI Lifetimes - Object Kab Tak Zinda Rehta Hai?

**Heading:** DI Lifetimes: Object ki Zindagi (Life) Kitni Hoti Hai?
****Sub-heading:** *Teen main tarah ki lifetimes hoti hain: Transient, Scoped, aur Singleton.*
**Content:**
| Lifetime | Meaning (Hinglish) | Example (Analogy) |
| :--- | :--- | :--- |
| **Transient** | Har baar naya object milega. | Har request ke liye naya **Chai ka Cup**. |
| **Scoped** | Ek HTTP Request ke liye same object. | Ek **Lunch ki Thali** jo poore lunch tak chalegi. |
| **Singleton** | Application start se end tak same object. | **Ghar ka Main Gate** jo hamesha wahi rehta hai. |
*   **Use Case:** Database connection (Scoped) ya utility service (Singleton) ke liye.

## Slide 4: MVC vs Web API - Difference Kya Hai?

**Heading:** MVC aur Web API: Dono Ka Kaam Alag Hai
**Sub-heading:** *Dono hi ASP.NET Core mein bante hain, par unka output different hota hai.*
**Content:**
| Feature | ASP.NET Core MVC | ASP.NET Core Web API |
| :--- | :--- | :--- |
| **Output** | **Views** (HTML, CSS, JavaScript) | **Data** (JSON, XML) |
| **Kaam** | User Interface (UI) banana. | Mobile Apps ya Single Page Apps (SPA) ko data dena. |
| **Example** | `return View(model);` | `return Ok(data);` |
*   **Routing:** Web API mein `[ApiController]` aur `[Route]` attributes use hote hain.

## Slide 5: Data Passing in MVC - Controller se View Tak

**Heading:** Controller se View Tak Data Kaise Bhejte Hain?
**Sub-heading:** *Teen main tareeke hain data ko Controller se View tak le jaane ke.*
**Content:**
| Method | Kaam (Hinglish) | Life Cycle |
| :--- | :--- | :--- |
| **ViewBag** | Simple data pass karna. | Sirf **ek** request ke liye. |
| **TempData** | Data ko **redirect** ke baad bhi use karna. | Next request tak survive karta hai. |
| **Partial View** | Reusable UI ka tukda (e.g., Header, Footer). | Main View ke andar load hota hai. |
*   **Tip:** `ViewBag` se zyada better `ViewModel` use karna hota hai.

## Slide 6: Entity Framework (EF) - Code se Database

**Heading:** Entity Framework (EF): Code First Approach
**Sub-heading:** *EF humare C# classes ko database tables mein convert karta hai.*
**Content:**
*   **Code First:** Sabse popular approach. Hum C# mein classes (Models) likhte hain, aur EF unse database bana deta hai.
*   **DbContext:** Yeh class database ka **gateway** hai. Saare operations iske through hote hain.
*   **DbSet:** Yeh C# class database ke **table** ko represent karti hai.
*   **Example:**
    *   `public DbSet<Employee> Employees { get; set; }`
    *   *Meaning:* `Employee` class ke liye database mein ek `Employees` table banega.

## Slide 7: EF Tracking - Performance aur Updates

**Heading:** Tracking vs No-Tracking: Performance aur Updates
**Sub-heading:** *EF ko batana ki object ko track karna hai ya nahi.*
**Content:**
*   **Tracking:**
    *   EF object ko monitor karta hai.
    *   Agar object mein change hua, toh `SaveChanges()` par DB mein update ho jayega.
    *   **Use:** Jab data ko update ya delete karna ho.
*   **No-Tracking:**
    *   EF object ko monitor nahi karta.
    *   **Bahut Fast** hota hai, kyunki monitoring ka overhead nahi hota.
    *   **Use:** Sirf data ko **read** karna ho (Read-only operations).
*   **Example:**
    *   `context.Employees.AsNoTracking().ToList();` (*Fast Read*)

## Slide 8: LINQ - Data Querying ka Easy Tareeka

**Heading:** LINQ: C# mein Data se Baat Karne ka Tareeka
**Sub-heading:** *Language Integrated Query (LINQ) se hum C# code mein hi data filter kar sakte hain.*
**Content:**
*   **Where:** Data ko filter karna.
    *   `employees.Where(e => e.Age > 18)` (*18 se upar age wale employees*)
*   **Select:** Sirf zaroori columns nikalna.
    *   `employees.Select(e => e.Name)` (*Sirf employees ke naam*)
*   **Any:** Check karna ki koi condition match ho rahi hai ya nahi.
    *   `employees.Any(e => e.Salary > 100000)` (*Check if koi employee 1 Lakh se zyada kamata hai*)
*   **Benefit:** Code readable aur type-safe ho jaata hai.

## Slide 9: Authentication vs Authorization - Kaun aur Kya?

**Heading:** Authentication vs Authorization: Kaun Ho Tum aur Kya Kar Sakte Ho?
**Sub-heading:** *Yeh do alag concepts hain jo security ke liye zaroori hain.*
**Content:**
| Topic | Meaning (Hinglish) | Example |
| :--- | :--- | :--- |
| **Authentication** | **Kaun ho tum?** (Identity check) | Username aur Password se **Login** karna. |
| **Authorization** | **Tum kya kar sakte ho?** (Permission check) | Login ke baad **Admin** page access karna. |
*   **JWT (JSON Web Token):** Authentication ke liye use hota hai. Server ek token deta hai, aur client har request mein woh token bhejta hai.
*   **Example (Authorization):**
    *   `[Authorize(Roles = "Admin")]` (*Sirf Admin role wale hi is function ko access kar sakte hain*)

## Slide 10: SQL Server Indexes - Data Ko Jaldi Dhundhna

**Heading:** Clustered vs Non-Clustered Index: Data Search Speed
**Sub-heading:** *Index database mein data ko jaldi dhundhne mein help karte hain.*
**Content:**
| Feature | Clustered Index | Non-Clustered Index |
| :--- | :--- | :--- |
| **Max Count** | Sirf **1** per table | **999** tak ho sakte hain |
| **Data Storage** | Actual data ko **sort** karta hai. | Data ki location ka **pointer** store karta hai. |
| **Speed** | Bahut Fast (Primary Key par banta hai) | Thoda slow (Pointer follow karna padta hai) |
*   **Analogy:** Clustered Index ek **kitaab** hai jo alphabetical order mein hai. Non-Clustered Index us kitaab ka **Index Page** hai.

## Slide 11: async-await - Performance Badhana

**Heading:** async-await: Application ki Capacity Badhana
**Sub-heading:** *Jab koi operation time leta hai (e.g., DB call), toh hum `async-await` use karte hain.*
**Content:**
*   **Problem:** Jab DB call hoti hai, toh woh thread (worker) **block** ho jaata hai aur doosri requests handle nahi kar pata.
*   **Solution:** `async-await` use karne se, jab DB call ho rahi hoti hai, toh woh thread **free** ho jaata hai aur doosri requests handle karta hai.
*   **Fayda:** Server ki **capacity** badh jaati hai. Zyada requests handle ho sakti hain.
*   **Example:**
    *   `public async Task<IActionResult> Get() => Ok(await context.Employees.ToListAsync());`

## Slide 12: Quick Tips aur Next Steps

**Heading:** Interview Tips aur Next Steps
**Sub-heading:** *Thodi si mehnat aur offer letter aapka!*
**Content:**
*   **Tip 1: Practice Code:** Sirf padhne se nahi hoga. Ek **Mini CRUD Project** banao.
*   **Tip 2: Revise Loud:** Har answer ko **zor se** bol kar practice karo.
*   **Tip 3: HTTP Status Codes:** Important codes yaad rakho (200, 201, 400, 401, 404, 500).
*   **Next Step:** Aur advanced topics (like Microservices, Docker) explore karo.
*   **Good Luck!** 🔥 Offer letters aa hi jayenge!
