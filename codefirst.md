## EF Core Code First

#### Creating DbContext

```
public class AppDbContext : DbContext
{
	public AppDbContext(DbContextOptions<AppDbContext> options) : base(options)
        {
            //Database.EnsureCreated();
        }
	public DbSet<userm> Userm { get; set; }
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



