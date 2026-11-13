\# Campus Equipment API



\## Project Purpose



The Campus Equipment API is a .NET 10 ASP.NET Core Minimal API that allows a campus technology desk to manage equipment inventory. The application stores equipment names, categories, and availability information using Entity Framework Core and a SQLite database. The API supports Create, Read, Update, and Delete (CRUD) operations and demonstrates data persistence through migrations, seeding, and application restarts.



\## Prerequisites



The following software is required:



\- .NET 10 SDK

\- Visual Studio Code

\- Git

\- REST Client extension (Huachao Mao)

\- Entity Framework Core CLI Tool (dotnet-ef 10.0.14)



\### Tested Operating System



Windows 11



\---



\# Project Structure



```text

CampusEquipmentApi/

│

├── .config/

│   └── dotnet-tools.json

│

├── Contracts/

│   └── EquipmentItemRequest.cs

│

├── Data/

│   └── EquipmentDbContext.cs

│

├── Models/

│   └── EquipmentItem.cs

│

├── Migrations/

│   ├── 20261113184444\_InitialCreate.cs

│   ├── 20261113184444\_InitialCreate.Designer.cs

│   └── EquipmentDbContextModelSnapshot.cs

│

├── evidence/

│   └── screenshots

│

├── CampusEquipmentApi.http

├── Program.cs

├── appsettings.json

├── global.json

├── CampusEquipmentApi.csproj

└── README.md

```



\---



\# Reproducing the Project



Run all commands from the project root folder.



Verify SDK:



```powershell

dotnet --version

```



Restore the EF tool:



```powershell

dotnet tool restore

```



Restore packages:



```powershell

dotnet restore

```



Build the project:



```powershell

dotnet build

```



Apply migrations and create the database:



```powershell

dotnet ef database update

```



Run the API:



```powershell

dotnet run --no-launch-profile --urls http://localhost:5050

```



HTTP testing is performed using the REST Client extension and the file:



```text

CampusEquipmentApi.http

```



\---



\# Request Results



| Request | Expected Status | Actual Status | Observation |

|----------|----------|----------|----------|

| 01 | 200 | 200 | Returned 3 seeded equipment records |

| 02 | 200 | 200 | Returned the selected seed item |

| 03 | 201 | 201 | Created Student Test Microphone and returned Location header |

| 04 | 200 | 200 | Retrieved created equipment record |

| 05 | 204 | 204 | Updated item successfully with empty body |

| 06 | 200 | 200 | Verified updated values were saved |

| 07 | 400 | 400 | Whitespace name rejected |

| 08 | 400 | 400 | Missing category rejected |

| 09 | 400 | 400 | Name exceeding 100 characters rejected |

| 10 | 400 | 400 | Whitespace category rejected |

| 11 | 400 | 400 | Category exceeding 50 characters rejected |

| 12 | 400 | 400 | Null name rejected |

| 13 | 200 | 200 | Invalid requests did not modify stored values |

| 14 | 200 | 200 | Collection contained 4 records before restart |

| 15 | 200 | 200 | Updated item persisted after application restart |

| 16 | 204 | 204 | Item deleted successfully |

| 17 | 404 | 404 | Deleted item not found |

| 18 | 404 | 404 | Update against missing item returned 404 |

| 19 | 404 | 404 | Delete against missing item returned 404 |

| 20 | 200 | 200 | Final collection contained only the 3 seed records |



\---



\# Evidence Index



| Evidence ID | Description |

|------------|-------------|

| E1 | SDK verification, EF version, package list, successful build |

| E2 | Migration creation, migration application, migration list |

| E3 | Initial seeding and second database update showing no duplicate records |

| E4 | CRUD operations including GET, POST, PUT, DELETE |

| E5 | Persistence proof before and after application restart |

| E6 | Validation tests and missing-resource error tests |

| E7 | Database file, migration files, migration history, final persisted state |

| E8 | Git repository, README, .http file, evidence folder |



\---



\# Conceptual Questions



\## 1. How do a request DTO and entity differ, and why is EquipmentItemId absent from the DTO?



The EquipmentItem entity represents data stored in the database. The EquipmentItemRequest DTO represents the data accepted from API requests. EquipmentItemId is not included in the DTO because the database automatically generates the primary key for newly created records, and existing records are identified using the route parameter.



\---



\## 2. How do dependency injection, DbContext, and SaveChangesAsync work together for a PUT?



Dependency injection provides an EquipmentDbContext instance to the endpoint handler. The handler uses the DbContext to locate the existing EquipmentItem, updates its properties, and SaveChangesAsync writes those changes to the SQLite database.



\---



\## 3. How does generating a migration differ from applying it?



Generating a migration creates C# migration files describing schema changes. Applying a migration executes those schema changes against the database and records the migration in the EF migration history table.



\---



\## 4. Why does a second database update not duplicate seed rows?



The UseSeeding callback checks whether any EquipmentItem records already exist using Any(). If records are found, no additional seed data is inserted. Running database update again therefore does not create duplicates.



\---



\## 5. What does the restart check prove that one successful GET does not?



The restart check proves that data is persisted in the SQLite database file. The updated record remained available even after the API application was shut down and restarted, demonstrating that the data was not stored only in memory.



\---



\## 6. Why must this API validate string lengths even though the EF model declares maximum lengths?



The EF model documents the intended schema and constraints, but SQLite does not automatically enforce the specified maximum string lengths. Explicit validation ensures invalid request data is rejected before being stored.



\---



\# Repository



Repository URL:



```text

Eleazar-Gonzalez/CampusEquipmentAPI

```



\---



\# Author



Eleazar Gonzalez-Martinez

