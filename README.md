# Backend-template
Backend vizsga setup lépésről lépésre

## Projekt setup

### 1. NuGet csomagok

- Microsoft.EntityFrameworkCore (8.0.26)
- Microsoft.EntityFrameworkCore.Tools (8.0.26)
- Microsoft.EntityFrameworkCore.Design (8.0.26)
- Microsoft.EntityFrameworkCore.SqlServer (8.0.26)
- MySql.EntityFrameworkCore (Legújabb)

### 2. appsettings.json

Ezt az appsetting.json fölé rakni. **Neveket átírni a projekten megfelelően**
```json
  "ConnectionStrings": {
  "{név}Context": "server=localhost;port=3306;user=root;password=;database={db név}"
},
```

### 3. Context fájl a Models mappában (létrehozni azt is)

```csharp
using Microsoft.EntityFrameworkCore;
using KonyvtarWebApi_BG.Models;
using Microsoft.Extensions.DependencyModel;

namespace KonyvtarWebApi_BG.Models; // saját namespace, átírni!!

public class {név}Context : DbContext
{
    public {név}Context(DbContextOptions<{név}Context> options)
        : base(options)
    {
    }

    
```


### 4. Program.cs

Az "Add services to the container" **comment fölé**

```csharp
  builder.Services.AddDbContext<{név}Context>(options =>
    options.UseMySQL(builder.Configuration.GetConnectionString("{név}Context"))
);
```


## Kapcsolatok (példák)

### 1-több

```csharp
// Principal (parent)
public class Blog
{
    public int Id { get; set; }
    public List<Post> Posts { get; } = new List<Post>(); // Collection navigation containing dependents
}

// Dependent (child)
public class Post
{
    public int Id { get; set; }
    public int BlogId { get; set; } // Required foreign key property
    [ForeignKey("BlogId")]
    public Blog Blog { get; set; } = null!; // Required reference navigation to principal
}
```

### több-több

**Kapcsoló tábla!!!**

```csharp
public class Post
{
    public int Id { get; set; }
    public List<PostTag> PostTags { get; } = new List<PostTag>();
}

public class Tag
{
    public int Id { get; set; }
    public List<PostTag> PostTags { get; } = List<PostTag>();
}

public class PostTag
{
    public int PostsId { get; set; }
    public int TagsId { get; set; }
    public Post Post { get; set; } = null!;
    public Tag Tag { get; set; } = null!;
}
```


### 1-1

```csharp
// Principal (parent)
public class Blog
{
    public int Id { get; set; }
    public BlogHeader? Header { get; set; } // Reference navigation to dependent
}

// Dependent (child)
public class BlogHeader
{
    public int Id { get; set; }
    public int BlogId { get; set; } // Required foreign key property
    [ForeignKey("BlogId")]
    public Blog Blog { get; set; } = null!; // Required reference navigation to principal
}
```
