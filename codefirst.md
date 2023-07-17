## EF Core Code First
<p>
A database schema can be developed gradually using code first migration. Code first Migrations provide a set of tools that allow:
</p>

<p>1. Create an initial database that works with your EF model</p>
<p>2. Generating migrations to keep track of changes you make to your EF model</p>
<p>3. Keep your database up to date with those changes</p>


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
	public DbSet<dept> dept { get; set; }
	public DbSet<emp>  emp  { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
		modelBuilder.ApplyConfiguration(new dept_config());
		modelBuilder.ApplyConfiguration(new emp_config());
        }
}
```

#### Fluent Api - Orgranizing the code
```
	public class dept_config : IEntityTypeConfiguration<dept>
    	{
        	public void Configure(EntityTypeBuilder<dept> modelBuilder)
	        {
            		modelBuilder.ToTable("dept");
		        modelBuilder.HasKey(u => u.dept_id)
		        modelBuilder.Property(u => u.dept_name)
                		.HasMaxLength(60)
		                .IsRequired();
            		modelBuilder.HasIndex(e => new { e.dept_name})
                		.IsUnique();
        	}
	    }
	}

	public class emp_config : IEntityTypeConfiguration<emp>
    	{
        	public void Configure(EntityTypeBuilder<emp> modelBuilder)
	        {
            		modelBuilder.ToTable("emp");
		        modelBuilder.HasKey(u => u.emp_id)
		        modelBuilder.Property(u => u.emp_name)
                		.HasMaxLength(60)
		                .IsRequired();
		        modelBuilder.Property(u => u.emp_address1)
                		.HasMaxLength(100)
		                .IsRequired();
		        modelBuilder.Property(u => u.emp_address2)
                		.HasMaxLength(100)
		                .IsRequired(false);
		        modelBuilder.Property(u => u.emp_address3)
                		.HasMaxLength(100)
		                .IsRequired(false);
		        modelBuilder
                	.HasOne(b => b.dept)
                	.WithMany(c => c.employees)
                	.HasForeignKey(b => b.emp_deptid)
                	.OnDelete(DeleteBehavior.NoAction)
                	.IsRequired();
        	}
	    }
	}

```



