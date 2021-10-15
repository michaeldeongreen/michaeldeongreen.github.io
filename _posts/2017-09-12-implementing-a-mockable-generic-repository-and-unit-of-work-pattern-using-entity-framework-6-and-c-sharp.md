---
layout: post
title:  "Implementing a Mockable Generic Repository and Unit of Work Pattern Using Entity Framework 6 and C#"
date:  2017-09-12
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Unit Testing, Visual Studio, Entity Framework, Design Patterns]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

In this blog, I wanted to take some time to walkthrough how to implement a generic [Repository Pattern](https://msdn.microsoft.com/en-us/library/ff649690.aspx) and the [Unit of Work Pattern](http://wiki.c2.com/?UnitOfWork) using Entity Framework 6.

### Assumptions

In this blog entry, I make the following assumptions:

* You have a solid understanding of Entity Framework
* You have a solid understanding of C#
* You have a solid understanding of the Repository and Unit of Work Pattern

### Background

My main motivation for writing this blog post is that in one of my projects, which I took over, the Unit of Work and Repository Pattern have been implemented. I came across a few issues with how the developer implemented these patterns and so I thought it would be a good idea to create a project and demonstrate an alternative way on how to implement these two very common Software Design Patterns.  
  
The previous developer implemented what I call a "reverse" Unit of Work Pattern. What I mean by this is that usually when the Unit of Work and Repository Pattern are implemented, the development team will work with a Unit of Work object, which isolates and centralizes the Database Context. In this particular project, the developer [injects](https://en.wikipedia.org/wiki/Dependency_injection) the Unit of Work object into each domain Repository and the developers interact with domain repositories. Here is some sample pseudo code:

```csharp
     //unit of work
     public class UnitOfWork : IUnitOfWork
     {
      //implementation
     }
     
     //
     pubic class Repository : IRepository
     {
      public Repository(IUnitOfWork uow)
      {
       //implementation
      }
     }
    
     //domain repository
     public class LoginRepository : Repository, ILoginRepository
     {
      public LoginRepository(IUnitOfWork uow) : base(uow) {
       //implementation
      }
     }
     
     //some api controller
     public class LoginController
     {
      public LoginController(ILoginRepository loginRepository)
      {
       _loginRepository = loginRepository;
       //implementation
      }
      
      public IEnumerable Get(int Id)
      {
       return _loginRepository.Get(Id);
      }
     }
    
```
  
**The implementation is fairly well done, I have just ran into a few issues, namely:**

* You have to inject each domain repository that you want to use into each class
* If you have domains that are not related ie child, grandchild etc etc, you cannot save them in the same transaction because each domain has its own repository.
* When testing, you could end up mocking several domain repositories, if the class you are testing uses several domain repositories

**With my implementation, I seek to provide the following:**

* 1 object, ie a Unit of Work object that developers use to interact with the Database and inject into classes
* A Unit of Work object, that can be mocked for testing
* Domain repositories share common interfaces but can also implement different interfaces as some domain objects may need extra functionality

### The Domains

Below is a basic Entity Relationship Diagram of the entities that I will work with in this blog post:  
  
[![Grenitaus Consulting](assets/images/implementing-a-mockable-generic-repository-and-unit-of-work pattern-using-entity-framework-6-and-c-sharp-001.png)](assets/images/implementing-a-mockable-generic-repository-and-unit-of-work pattern-using-entity-framework-6-and-c-sharp-001.png)  
  
First, I will make a Base Entity interface for all domains to implement:

```csharp
    
    public interface IEntity
    {
     int Id { get; set; }
    }
```

Below, I have created each domain object:

```csharp
    public class Employee : IEntity
    {
     [Key]
     [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
     [Column("EmployeeId")]
     public int Id { get; set; }
     
     [Required]
     [StringLength(100)]
     public virtual string FirstName { get; set; }
     
     [Required]
     [StringLength(100)]
     public virtual string LastName { get; set; }
     
     public virtual Company Company { get; set; }
     
     [ForeignKey("Company")]
     public virtual int CompanyId { get; set; }
    }
    
    public class Company : IEntity
    {
     [Key]
     [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
     [Column("CompanyId")]
     public int Id { get; set; }
     
     [StringLength(300)]
     public virtual string Name { get; set; }
     
     public virtual ICollection Employees { get; set; }
    }
    
    public class Log : IEntity
    {
     [Key]
     [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
     [Column("LogId")]
     public int Id { get; set; }
     
     [StringLength(4000)]
     public virtual string Description { get; set; }
    }
```

### DbContext Implementation

Next, I will create an interface that the Entity Framework Database Context will implement and create the DbContext. Creating this interface will allow us to mock the DbContext when performing Unit Tests:

```csharp
    public interface IDbContext : IDisposable
    {
     DbSet Set() where TEntity : class;
     
     DbEntityEntry Entry(object entity);
     
     int SaveChanges();
    }
    
```
  
I have created a external mapping class to map Entity Framework entities. This class will be called by the DbContext OnModelCreating method:  
  
```csharp
    
    internal class DomainMap
    {
     private readonly DbModelBuilder _modelBuilder;
     
     public DomainMap(DbModelBuilder modelBuilder)
     {
      _modelBuilder = modelBuilder;
     }
     
     public void MapEntities()
     {
      MapCompany();
      MapEmployee();
      MapLog();
     }
    
     private void MapCompany()
     {
      _modelBuilder.Entity().ToTable("Company");
     }
     private void MapEmployee()
     {
      
      _modelBuilder.Entity().ToTable("Employee")
       .HasRequired(e => e.Company)
       .WithMany(e => e.Employees)
       .HasForeignKey(e => e.CompanyId);
     }
     private void MapLog()
     {
      _modelBuilder.Entity().ToTable("Log");
     }
    }
```

Please not that I am using "new" on the DbEntityEntry method because the DbEntityEntry method on the parent class cannot be overridden and therefore, we have to hide the parent implementation.  
  
```csharp
    public class DomainDbContext : DbContext, IDbContext
    {
     public DomainDbContext(string connectionString) : base(connectionString) {
     }
    
     public override int SaveChanges() 
     {
      return base.SaveChanges();
     }
     
     public override DbSet Set() 
     {
      return base.Set();
     }
    
     public new DbEntityEntry Entry(object entity)
     {
      return base.Entry(entity);
     }
     
     protected override void OnModelCreating(DbModelBuilder modelBuilder)
     {
      Database.SetInitializer(new CreateDatabaseIfNotExists());
      DomainMap map = new DomainMap(modelBuilder);
      map.MapEntities();
     }
    }
    
```
  
### Generic Repository Implementation

Next, I will create the generic repository interface using generics. I am specifiying that any class that implements this interface must be a reference type and of type IEntity, the interface we created earlier:  
  
```csharp
    
    public interface IRepository where TEntity : class, IEntity
    {
     IQueryable GetAll();
     
     TEntity Get(object id);
     
     IEnumerable Find(Expression> filter);
     
     TEntity Add(TEntity entity);
     
     void Delete(TEntity entity);
     
     void Update(TEntity entity);
    }
```

Next, I will implement the IRepository interface. Notice that I am injecting the IDbContext into the contructor. I will use [StructureMap](http://structuremap.github.io/) to perform [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection). I have made the IDbContext protected so all classes who implement the Repository class will have access to the IDbContext implementation. Also, in this particular version, I didn't implement any async methods but those are fairly straight forwoard to add:

### The Repositories

```csharp
    public class Repository : IRepository where TEntity : class, IEntity
    {
     protected readonly IDbContext _dbContext;
     
     private readonly DbSet _dbSet;
     
     public Repository(IDbContext dbContext)
     {
      _dbContext = dbContext;
      _dbSet = _dbContext.Set();
     }
     
     public TEntity Add(TEntity entity)
     {
      return _dbSet.Add(entity);
     }
    
     public void Delete(TEntity entity)
     {
      if (_dbContext.Entry(entity).State == EntityState.Detached)
      {
       _dbSet.Attach(entity);
      }
      _dbSet.Remove(entity);
     }
    
     public IEnumerable Find(Expression> filter)
     {
      IQueryable query = _dbSet;
    
      if (filter != null)
      {
       query = _dbSet.Where(filter);
      }
    
      return query.ToList();
     }
    
     public TEntity Get(object id)
     {
      return _dbSet.Find(id);
     }
    
     public IQueryable GetAll()
     {
      return _dbSet;
     }
    
     public void Update(TEntity entity)
     {
      _dbSet.Attach(entity);
      _dbContext.Entry(entity).State = EntityState.Modified;
     }
    }
```

Now let's create each entity's repository interface and implement the repository class. The reason why I have designed the repository in this manner ie each entity implements its own repository is because inevitably, some entities will need custom queries that do not match the IRepository interface.  
  
So what I have done is use IRepository to share common functionality but I have also given each entity their own repository interface so they may implement custom functionality, without having to touch IRepository. I have demonstrated this with the ICompanyRepository.  
  
Finally, notice that I am injecting the IDbContext into each repository implementation so that any custom implementation will have access to the Database Context wrapper class.  
  
```csharp
    
    public class LogRepository : Repository, ILogRepository
    {
     public LogRepository(IDbContext dbContext) : base(dbContext)
     {
    
     }
    }
    
    public class LogRepository : Repository, ILogRepository
    {
     public LogRepository(IDbContext dbContext) : base(dbContext)
     {
    
     }
    }
    
    
    
    public interface IEmployeeRepository : IRepository
    {
    }
    
    
    
    public class EmployeeRepository : Repository, IEmployeeRepository
    {
     public EmployeeRepository(IDbContext dbContext) : base(dbContext)
     {
     }
    }
    
    
    
    
    public interface ICompanyRepository : IRepository
    {
     IEnumerable GetByName(string name);
    }
    
    
    public class CompanyRepository : Repository, ICompanyRepository
    {
     public CompanyRepository(IDbContext dbContext) : base(dbContext)
     {
     }
    
     public IEnumerable GetByName(string name)
     {
      var companies = _dbContext.Set()
       .Where(c => c.Name.Contains(name));
      return companies;
     }
    }
```

### Unit of Work Implementation

Now, let's create the Unit of Work interface and the class that implements this interface. As you will see below, I am injecting the IDbContext into the UnitOfWork class. Also, each domain repository will have an exposed public property in the UnitOfWork class. In this version, I have only created a Save method, which calls the DbContext to save changes to the Database.  
  
```csharp
    
    public interface IUnitOfWork
    {
     ICompanyRepository CompanyRepository{ get; }
     
     IEmployeeRepository EmployeeRepository { get; }
     
     ILogRepository LogRepository { get; }
     
     int Save();
    }
    
    
    public class UnitOfWork : IUnitOfWork
    {
     private readonly IDbContext _dbContext;
     
     private ICompanyRepository _companyRepository;
     
     private IEmployeeRepository _employeeRepository;
     
     private ILogRepository _logRepository;
     
     public ICompanyRepository CompanyRepository { get { return _companyRepository = _companyRepository ?? new CompanyRepository(_dbContext); } }
     
     public IEmployeeRepository EmployeeRepository { get { return _employeeRepository = _employeeRepository ?? new EmployeeRepository(_dbContext); } }
     
     public ILogRepository LogRepository { get { return _logRepository = _logRepository ?? new LogRepository(_dbContext); } }
     
     public UnitOfWork(IDbContext dbContext)
     {
      _dbContext = dbContext;
     }
    
     public int Save()
     {
      return _dbContext.SaveChanges();
     }
    }
    
```
  
### How to use the Unit of Work object?

Below are some examples of how you would use the Unit of Work object in a Web Api controller:  
  
```csharp
    
    public class EmployeeController : ApiController
    {
     private readonly IUnitOfWork _uow;
     
     public EmployeeController(IUnitOfWork uow)
     {
      _uow = uow;
     }
    
     [HttpGet]
     public IEnumerable Get()
     {
      return _uow.EmployeeRepository.GetAll();
     }
     
     [HttpPost]
     public Employee Post([FromBody]Employee employee)
     {
      return _uow.EmployeeRepository.Add(employee);
     }   
    }
    

  ```
  
### Unit Test Example

Given some log service that uses the Unit of Work object to save a log entry, below is how you could use [moq](https://github.com/Moq/moq4/wiki/Quickstart) to mock the Unit of Work object:  
  
```csharp
    
    [TestFixture]
    public class LogServiceTests
    {
     [Test]
     public void Can_Add_A_New_Log_Entry_Test()
     {
      //given
      string logEntry = "some log entry";
      var uowMock = new Mock();
      uowMock.Setup(u => u.LogRepository.Add(It.IsAny()));
      ILogService service = new LogService(uowMock.Object);
      
      //when
      bool result = service.Log(logEntry);
      
      //then
      value.Should().BeTrue();
     }
    }
    

  ```
  
### Design Weaknesses

Every design has pros and cons and what I have implemented in this blog is no exception. I think the main weakness of this design is that there are 4 steps one has to take to add a new repository:  
  
1. Create a new IDomainRepository
2. Implement create a new DomainRepository and implement the new IDomainRepository
3. Add the new domain repository property to the IUnitOfWork interace
4. Implement the new domain propery in the UnitOfWork class

### Conclusion

This concludes another blog entry for me and I hope that this post was beneficial to you in some way. You can find the complete code on Github [here](https://github.com/michaeldeongreen/FromScratch).
