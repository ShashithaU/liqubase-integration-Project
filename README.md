# 🗄️ Liquibase Database Version Control Tutorial

[![Java](https://img.shields.io/badge/Java-22-orange.svg)](https://openjdk.java.net/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.3.4-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Liquibase](https://img.shields.io/badge/Liquibase-4.31.0-blue.svg)](https://www.liquibase.org/)
[![MySQL](https://img.shields.io/badge/MySQL-8.0+-blue.svg)](https://www.mysql.com/)

> **Learn database version control and change management with Liquibase in Spring Boot applications**



## 🚀 Quick Start

### Prerequisites
- Java 22+
- MySQL 8.0+
- Maven 3.6+

### Setup
```bash
# Clone the repository
git clone <repository-url>
cd liqubase-integration-Project

# Configure database connection
# Edit src/main/resources/application.yml

# Run the application
mvn spring-boot:run
```

---

## 🏗️ Project Structure

```
src/main/resources/
├── application.yml          # Database configuration
└── db.changelog/
    ├── db.changelog-master.xml    # Main changelog
    └── scripts/
        ├── customer-changlog.sql
        └── customer-add-address_coloum.sql
```

---

## 🔧 Configuration

### Database Connection
```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/testNew
    username: root
    password: 12345
  
  liquibase:
    change-log: classpath:/db.changelog/db.changelog-master.xml
```

### Maven Dependencies
```xml
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
    <version>4.31.0</version>
</dependency>
```

---

## 📖 Liquibase Fundamentals

### ChangeLog Structure
The master changelog (`db.changelog-master.xml`) orchestrates all database changes:

```xml
<databaseChangeLog>
    <changeSet id="1" author="developer">
        <createTable tableName="customer">
            <column name="id" type="int" autoIncrement="true">
                <constraints primaryKey="true"/>
            </column>
            <column name="name" type="varchar(255)"/>
        </createTable>
    </changeSet>
</databaseChangeLog>
```

### Key Concepts

| Concept | Description |
|---------|-------------|
| **ChangeSet** | Atomic database change unit |
| **ChangeLog** | Collection of changeSets |
| **Rollback** | Reverse database changes |
| **Preconditions** | Validate before execution |

---

## 🛠️ Change Management Workflow

### 1. Create ChangeSets
```xml
<changeSet id="add-address-column" author="developer">
    <addColumn tableName="customer">
        <column name="address" type="varchar(500)"/>
    </addColumn>
</changeSet>
```

### 2. Version Control
- ✅ One changeSet per logical change
- ✅ Never modify executed changeSets
- ✅ Use meaningful IDs and authors
- ✅ Include rollback statements

### 3. Environment Promotion
```
Development → Testing → Staging → Production
```

---

## 📋 Best Practices

### ✅ Do's
- Use descriptive changeSet IDs
- Test rollbacks before deployment
- Keep changeSets small and focused
- Use preconditions for safety
- Version control all changes

### ❌ Don'ts
- Modify executed changeSets
- Skip rollback testing
- Use auto-generated IDs
- Mix DDL and DML in one changeSet

---

## 🔄 Common Operations

### Add New Table
```xml
<changeSet id="create-product-table" author="dev">
    <createTable tableName="product">
        <column name="id" type="bigint" autoIncrement="true">
            <constraints primaryKey="true"/>
        </column>
        <column name="name" type="varchar(255)">
            <constraints nullable="false"/>
        </column>
    </createTable>
</changeSet>
```

### Modify Existing Column
```xml
<changeSet id="modify-customer-name" author="dev">
    <modifyDataType tableName="customer" 
                    columnName="name" 
                    newDataType="varchar(500)"/>
</changeSet>
```

### Add Index
```xml
<changeSet id="add-customer-email-index" author="dev">
    <createIndex tableName="customer" indexName="idx_customer_email">
        <column name="email"/>
    </createIndex>
</changeSet>
```

---

## 🚀 Running Migrations

### Automatic (Spring Boot)
Migrations run automatically on application startup.

### Manual Commands
```bash
# Update database
mvn liquibase:update

# Rollback last change
mvn liquibase:rollback -Dliquibase.rollbackCount=1

# Generate SQL preview
mvn liquibase:updateSQL
```

---

## 📊 Monitoring & Troubleshooting

### Check Migration Status
```sql
SELECT * FROM DATABASECHANGELOG;
SELECT * FROM DATABASECHANGELOGLOCK;
```

### Common Issues
- **Lock Issues**: Clear locks with `mvn liquibase:releaseLocks`
- **Failed Changes**: Fix and use `mvn liquibase:changelogSync`
- **Rollback Errors**: Verify rollback SQL statements

---

## 🎯 Advanced Features

- **Contexts**: Environment-specific changes
- **Labels**: Tag and filter changeSets
- **Preconditions**: Validate before execution
- **Custom Changes**: Extend Liquibase functionality

---

## 📚 Resources

- [Liquibase Documentation](https://docs.liquibase.com/)
- [Spring Boot Integration](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-database-initialization)
- [Best Practices Guide](https://www.liquibase.org/get-started/best-practices)

---




