# Employer–Worker Registration System

An **accounting and workforce management program** that maintains information about employers, employees, and the relationships between them.  
It provides features to register new workers and employers, record payments, track work history, and generate reports — making it easier to manage small to medium-sized organizations.

---

## Key Features
- Employer and worker registration  
- Recording of wages, workgroups, and payments  
- Search and display of worker/employer records  
- User authentication with admin login  
- PostgreSQL-backed persistent storage  
- Simple and intuitive desktop UI built in Java Swing  

---

## Why This Project Is Useful
- Simplifies HR and payroll processes for small businesses  
- Provides a digital alternative to manual record-keeping  
- Can be extended for larger ERP-style systems  
- Educational: demonstrates **Java + PostgreSQL + Swing GUI** integration  

---

## Setup Instructions

### 1. Prerequisites
- **Java JDK 11+**  
- **PostgreSQL** installed locally (or any JDBC-compatible database)  
- A Java IDE (Visual Studio, Eclipse, or IntelliJ) or command-line build tools  

### 2. Clone the Repository
```bash
git clone https://github.com/maanvikgupta/employer-worker-registration-system.git
cd employer-worker-registration-system
```

### 3. Configure the Database
1. Create a new PostgreSQL database (e.g., `erpdb`).  
2. Run the following SQL scripts to create required tables:

```sql
CREATE TABLE admin(
    id smallserial primary key not null,
    username varchar,
    password varchar
);

CREATE TABLE employer(
    employer_id serial primary key not null,
    name varchar not null,
    surname varchar not null,
    business varchar,
    phonenumber varchar
);

CREATE TABLE worker(
    worker_id serial primary key not null,
    name varchar not null,
    surname varchar not null,
    phone_number varchar
);

CREATE TABLE worker_record(
    worker_record_id serial primary key not null,
    worker_id integer references worker(worker_id),
    employer_id integer references employer(employer_id),
    date varchar(10) not null,
    wage smallint not null
);

CREATE TABLE employer_record(
    employer_record_id serial primary key not null,
    employer_id integer references employer(employer_id),
    date varchar(10) not null,
    note varchar(255),
    number_worker smallint not null,
    wage smallint not null
);

CREATE TABLE worker_payment(
    worker_payment_id serial primary key not null,
    worker_id integer references worker(worker_id),
    employer_id integer references employer(employer_id),
    date varchar(10) not null,
    paid integer not null
);

CREATE TABLE employer_payment(
    employer_payment_id serial primary key not null,
    employer_id integer references employer(employer_id),
    date varchar(10) not null,
    paid integer not null
);
```

### 4. Configure JDBC Connection
Update `DBConst.java` with your database credentials:
```java
DriverManager.getConnection(
  "jdbc:postgresql://localhost:5432/erpdb", 
  "postgres", 
  "password"
);
```

### 5. Build and Run
- Import the project in your IDE → Build → Run `Main.java`
- Or via terminal:
```bash
javac -d bin src/com/mgupta/**/*.java
java -cp bin com.mgupta.main.Main
```

