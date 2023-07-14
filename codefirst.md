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
            modelBuilder.HasData(new userm
            {
                user_id = 1,
                user_code = "admin",
                user_name = "admin",
                user_email = "admin@gmail.com",
                user_is_admin = "Y",
                user_is_locked = "N",
                user_password = "admin"
            });
        }
    }
```


