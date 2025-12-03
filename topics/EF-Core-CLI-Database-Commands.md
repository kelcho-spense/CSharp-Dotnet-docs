# EF Core CLI Database Commands

### **View Database Information**
```bash
dotnet ef database list
```
Lists all databases for the configured context.

### **Create/Update Database**
```bash
dotnet ef database update
```
Applies all pending migrations to the database.

```bash
dotnet ef database update <MigrationName>
```
Updates database to a specific migration (can rollback or forward).

```bash
dotnet ef database update 0
```
Reverts all migrations (removes all tables).

### **Drop Database**
```bash
dotnet ef database drop
```
Deletes the database (prompts for confirmation).

```bash
dotnet ef database drop --force
```
Deletes the database without confirmation.

## Migration Commands

### **Create Migration**
```bash
dotnet ef migrations add <MigrationName>
```
Creates a new migration based on model changes.

### **Remove Migration**
```bash
dotnet ef migrations remove
```
Removes the last migration (only if not applied).

### **List Migrations**
```bash
dotnet ef migrations list
```
shows all migrations and their status.

### **Generate SQL Script**
```bash
dotnet ef migrations script
```
Generates SQL script for all migrations.

```bash
dotnet ef migrations script <FromMigration> <ToMigration>
```
Generates SQL for specific migration range.

## DbContext Commands

```bash
dotnet ef dbcontext info
```
bashows information about the DbContext.

```bash
dotnet ef dbcontext list
```
Lists all available DbContext types.

```bash
dotnet ef dbcontext scaffold <ConnectionString> <Provider>
```
Scaffolds a DbContext from an existing database (Database-First).

## Common Workflow

**Development Cycle:**
```bash
# 1. Make model changes
# 2. Create migration
dotnet ef migrations add AddNewFeature

# 3. Review migration files
# 4. Apply to database
dotnet ef database update

# 5. If needed, rollback
dotnet ef database update PreviousMigration
```

**Fresh Start:**
```bash
dotnet ef database drop --force
dotnet ef migrations remove
dotnet ef migrations add InitialCreate
dotnet ef database update
```
