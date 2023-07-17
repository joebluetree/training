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
			//Table Name
            		modelBuilder.ToTable("dept");
			//Primary Key
		        modelBuilder.HasKey(u => u.dept_id)
			//Column Max Length, Not Null
		        modelBuilder.Property(u => u.dept_name)
                		.HasMaxLength(60)
		                .IsRequired();
			//Unique
            		modelBuilder.HasIndex(e => new { e.dept_name})
                		.IsUnique();
        	}
	    }
	}

	public class emp_config : IEntityTypeConfiguration<emp>
    	{
        	public void Configure(EntityTypeBuilder<emp> modelBuilder)
	        {
			//Table Name
            		modelBuilder.ToTable("emp");
			//Primary Key
		        modelBuilder.HasKey(u => u.emp_id)
			//Set Length and Not Null
		        modelBuilder.Property(u => u.emp_name)
                		.HasMaxLength(60)
		                .IsRequired();
			//Set Length and Not Null
		        modelBuilder.Property(u => u.emp_address1)
                		.HasMaxLength(100)
		                .IsRequired();
			//Set Length and Allow Null
		        modelBuilder.Property(u => u.emp_address2)
                		.HasMaxLength(100)
		                .IsRequired(false);
			//Set Length and Allow Null
		        modelBuilder.Property(u => u.emp_address3)
                		.HasMaxLength(100)
		                .IsRequired(false);
			//Foreign Key - one to many relationship
		        modelBuilder
                	.HasOne(b => b.dept)
                	.WithMany(c => c.employees)
                	.HasForeignKey(b => b.emp_dept_id)
                	.OnDelete(DeleteBehavior.NoAction)
                	.IsRequired();

        	}
	    }
	}

```

#### Insert Data
````
modelBuilder.HasData(
  new dept{dept_id = 1,branch_name = "HR"}
  new dept{dept_id = 2,branch_name = "Accounts"}
);

````

#### Migration

````
Commands for migration

add-migration "migration name" //Add/Track Migration
update-database // Apply Changes to database

update-database 0 // Remove database migration
remove-migration // Remove migration from code

drop-database // Remove database

```

