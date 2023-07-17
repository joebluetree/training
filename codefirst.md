## EF Core Code First
<p>
A database schema can be developed gradually using code first migration. Code first Migrations provide a set of tools that allow:
</p>

<p>1. Create an initial database that works with your EF model</p>
<p>2. Generating migrations to keep track of changes you make to your EF model</p>
<p>3. Keep your database up to date with those changes</p>


#### Create below tables
````
dept		//table name
 dept_id  	//primary key
 dept_name 	//unique key, length 60

emp		//table name
 emp_id		//primary key
 emp_name 	//unique key, length 60
 emp_dept_id	//foreign key
 emp_address1	//not null length 100
 emp_address2	//Nullable
 emp_address3	//Nullable

````

#### Creating classes for tables
```
public class dept {
 public int    dept_id {get;set;}
 public string dept_name {get;set;}
}

public class emp {
 public int    emp_id {get;set;}
 public string emp_name {get;set;}
 public string emp_dept_id {get;set;}
 public string emp_address1 {get;set;}
 public string? emp_address2 {get;set;}
 public string? emp_address3 {get;set;}
}
	
```

#### Creating DbContext

```
public class AppDbContext : DbContext
{
	public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
            //Database.EnsureCreated();
        }
	public DbSet<class name> tablename  { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
        }
}
```

#### Orgranizing the migration code using fluent api
````
 public class dept_config : IEntityTypeConfiguration<table name>
 {
	public void Configure(EntityTypeBuilder<table name> modelBuilder)
	{
	}
 }
````

#### Orgranizing the code Class Wise
````
// add below code in OnModelCreating Method
modelBuilder.ApplyConfiguration(new class_name());
````


#### Table Name
````
modelBuilder.ToTable("dept");
````
#### Primary Key
````
modelBuilder.HasKey(u => u.dept_id)
````
#### Unique Key
````
modelBuilder.HasIndex(e => new { e.dept_name})
.IsUnique();

````

#### Setting Column Properties
````
modelBuilder.Property(u => u.dept_name)
.HasMaxLength(60) // Length
.IsRequired() // Not Null
.IsRequired(false) // AllowNull

````
#### Foreign Key
````
modelBuilder
.HasOne(b => b.dept)
.WithMany(c => c.employees)
.HasForeignKey(b => b.emp_dept_id)
.OnDelete(DeleteBehavior.NoAction)
.IsRequired();

modelBuilder
.HasOne(e => e.branch)
.WithMany()
.HasForeignKey(e => e.rec_branch_id)
.HasPrincipalKey(e => e.branch_id)
.IsRequired(false);

modelBuilder
.HasOne(typeof(dept))
.WithMany()
.HasForeignKey(e => e.emp_dept_id)
.HasPrincipalKey(e => e.dept_id)
.IsRequired(false);


````
                		

#### Insert Data
````
modelBuilder.HasData(
  new dept{dept_id = 1,branch_name = "HR"}
  new dept{dept_id = 2,branch_name = "Accounts"}
);

````



#### Migration Commands

````
Commands for migration

add-migration "migration name" //Add/Track Migration
update-database // Apply Changes to database

update-database 0 // Remove database migration
remove-migration // Remove migration from code

drop-database // Remove database

```

