## EF Core Code First
<p>
A database schema can be developed gradually using code first migration. Code first Migrations provide a set of tools that allow:
1 Create an initial database that works with your EF model
2 Generating migrations to keep track of changes you make to your EF model
3 Keep your database up to date with those changes
</p>

####
````
Create below tables

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

#### Creating Classes
```
Create class for tables
 public class dept{
 public int    dept_id {get;set;}
 public string dept_name {get;set;}
}

public class emp{
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
	public DbSet<dept> dept { get; set; }
	public DbSet<emp>  emp  { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
		modelBuilder.ApplyConfiguration(new company_config());
        }
}
```

#### Fluent Api - Orgranizing the code
```
	public class userm_config : IEntityTypeConfiguration<userm>
    	{
        	public void Configure(EntityTypeBuilder<userm> modelBuilder)
	        {
            		modelBuilder.ToTable("mast_userm");
        	}
	    }
	}

	public class AppDbContext : DbContext
	{
        	protected override void OnModelCreating(ModelBuilder modelBuilder)
        	{
			modelBuilder.ApplyConfiguration(new company_config());
        	}
	}

```



