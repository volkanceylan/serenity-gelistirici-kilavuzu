# Adding Tenants Table and TenantId Field

We need to add a TenantId field to all tables, to isolate tenants from each other.

So, we first need a Tenants table. 

As Northwind tables already have records, we'll define a primary tenant with ID 1, and set all existing records TenantId to it.

It's time to write a migration, actually two migrations, one for Northwind and one for Default database.

**DefaultDB_20160110_092200_MultiTenant.cs:**

```cs
using FluentMigrator;

namespace MultiTenancy.Migrations.DefaultDB
{
    [Migration(20160110092200)]
    public class DefaultDB_20160110_092200_MultiTenant 
        : AutoReversingMigration
    {
        public override void Up()
        {
            Create.Table("Tenants")
                .WithColumn("TenantId").AsInt32()
                    .Identity().PrimaryKey().NotNullable()
                .WithColumn("TenantName").AsString(100)
                    .NotNullable();

            Insert.IntoTable("Tenants")
                .Row(new
                {
                    TenantName = "Primary Tenant"
                });

            Insert.IntoTable("Tenants")
                .Row(new
                {
                    TenantName = "Second Tenant"
                });

            Insert.IntoTable("Tenants")
                .Row(new
                {
                    TenantName = "Third Tenant"
                });

            Alter.Table("Users")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Roles")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Languages")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);
        }
    }
}
```

I have created Tenants table in Default database where user tables are. Here we add 3 predefined tenants. We actually only need first one with ID *1*.

We didn't add TenantId column to tables like UserPermissions, UserRoles, RolePermissions etc, as they instrinsicly have TenantId information through their UserId or RoleId.

**NorthwindDB_20160110_093500_MultiTenant.cs:**

```cs
using FluentMigrator;

namespace MultiTenancy.Migrations.NorthwindDB
{
    [Migration(20160110093500)]
    public class NorthwindDB_20160110_093500_MultiTenant 
        : AutoReversingMigration
    {
        public override void Up()
        {
            Alter.Table("Employees")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Categories")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Customers")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Shippers")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Suppliers")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Orders")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Products")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Region")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);

            Alter.Table("Territories")
                .AddColumn("TenantId").AsInt32()
                    .NotNullable().WithDefaultValue(1);
        }
    }
}
```