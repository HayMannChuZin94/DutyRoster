# DutyRoster

A lightweight, client-side duty roster generator for managing fair weekly duty rotations between two teams.

## About This Project

DutyRoster is a single-page web application that creates fair weekly duty rotation schedules for 2026. The application assigns one person from Team 1 and one from Team 2 each week using a round-robin rotation algorithm to ensure fairness.

### Features

- **Fair Rotation Algorithm**: Round-robin scheduling ensures equal distribution of duties
- **Key Performance Indicators (KPI) Dashboard**: Displays total weeks, team member counts, and fairness metrics
- **Customizable Settings**: Choose year (2026-2028) and week start day (Monday/Sunday/Saturday)
- **Real-time Search**: Filter by names, dates, or week numbers
- **CSV Export**: Export filtered roster to spreadsheet format
- **Print Support**: Print-friendly styling for physical copies
- **Assignment Counters**: Shows duty count per team member

## Technologies Used

This project is built using pure HTML, CSS, and JavaScript - no backend or frameworks required.

---

## ASP.NET vs ADO.NET: Understanding the Difference

While this project doesn't use .NET technologies, here's a comprehensive guide to understanding the differences between ASP.NET and ADO.NET:

### Overview

**ASP.NET** and **ADO.NET** are both part of the Microsoft .NET ecosystem, but they serve completely different purposes in application development.

### ASP.NET

**ASP.NET** is a web application framework for building dynamic web applications and services.

#### Key Characteristics:
- **Purpose**: Web application development framework
- **Layer**: Presentation layer (frontend/UI)
- **What it does**: Handles HTTP requests, renders web pages, manages user sessions, routing, and web-based user interactions
- **Type**: Framework for building web applications

#### Features:
- **MVC Pattern**: Model-View-Controller architecture
- **Razor Pages**: Page-based programming model
- **Web API**: Building RESTful services
- **Authentication & Authorization**: Built-in security features
- **Session Management**: Handling user state
- **Routing**: URL pattern matching
- **Server Controls**: Reusable UI components

#### Example Use Cases:
- E-commerce websites
- Content management systems
- Web portals
- RESTful APIs
- Single-page applications (SPAs)

#### Code Example:
```csharp
// ASP.NET MVC Controller Example
public class HomeController : Controller
{
    public IActionResult Index()
    {
        ViewData["Message"] = "Welcome to ASP.NET";
        return View();
    }
    
    [HttpPost]
    public IActionResult SubmitForm(FormModel model)
    {
        // Handle form submission
        return RedirectToAction("Success");
    }
}
```

### ADO.NET

**ADO.NET** is a data access technology for interacting with databases and other data sources.

#### Key Characteristics:
- **Purpose**: Data access and database connectivity
- **Layer**: Data access layer (backend/database)
- **What it does**: Connects to databases, executes queries, retrieves and manipulates data
- **Type**: Library for database operations

#### Features:
- **Connection Management**: Database connectivity (`SqlConnection`)
- **Command Execution**: SQL queries and stored procedures (`SqlCommand`)
- **Data Retrieval**: Reading data (`SqlDataReader`, `DataSet`, `DataTable`)
- **Transaction Support**: Managing database transactions
- **Disconnected Architecture**: Working with data offline
- **Data Adapters**: Bridge between data source and DataSet

#### Example Use Cases:
- Database CRUD operations (Create, Read, Update, Delete)
- Executing stored procedures
- Reading large datasets
- Batch data processing
- Data migration and ETL operations

#### Code Example:
```csharp
// ADO.NET Database Access Example
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    
    string query = "SELECT Id, Name, Email, Age FROM Users WHERE Age > @age";
    SqlCommand command = new SqlCommand(query, connection);
    command.Parameters.AddWithValue("@age", 18);
    
    using (SqlDataReader reader = command.ExecuteReader())
    {
        while (reader.Read())
        {
            int id = reader.GetInt32(0);
            string name = reader.IsDBNull(1) ? "" : reader.GetString(1);
            string email = reader.IsDBNull(2) ? "" : reader.GetString(2);
            int age = reader.GetInt32(3);
            
            Console.WriteLine($"{name} - {email} (Age: {age})");
        }
    }
}
```

### Comparison Table

| Aspect | ASP.NET | ADO.NET |
|--------|---------|---------|
| **Primary Purpose** | Web application framework | Data access technology |
| **Application Layer** | Presentation/UI Layer | Data Access Layer |
| **Main Function** | Handle web requests, render pages | Connect to databases, manage data |
| **Key Components** | Controllers, Views, Routing, Middleware | Connection, Command, DataReader, DataAdapter |
| **Works With** | HTTP, HTML, CSS, JavaScript | SQL Server, Oracle, MySQL, other databases |
| **Output** | Web pages, HTML, JSON, XML | Data from databases (rows, tables) |
| **User Interaction** | Direct (users interact via browser) | Indirect (behind the scenes) |
| **Dependencies** | Can use ADO.NET for data access | Standalone, doesn't require ASP.NET |
| **Examples** | MVC apps, Web APIs, Razor Pages | Database queries, stored procedures |

### How They Work Together

In a typical .NET web application, **ASP.NET** and **ADO.NET** work together:

1. **ASP.NET** handles the web interface:
   - User visits a web page
   - ASP.NET Controller receives the HTTP request
   - Routes request to appropriate action method

2. **ADO.NET** handles data operations:
   - Controller calls a data access layer that uses ADO.NET
   - ADO.NET connects to database and retrieves data
   - Data is returned to the controller

3. **ASP.NET** renders the response:
   - Controller passes data to a View
   - View renders HTML with the data
   - HTML is sent back to user's browser

#### Complete Example:
```csharp
// ASP.NET MVC Controller using ADO.NET
public class ProductController : Controller
{
    private readonly string _connectionString;
    
    // Connection string injected via dependency injection (best practice)
    public ProductController(IConfiguration configuration)
    {
        _connectionString = configuration.GetConnectionString("DefaultConnection");
    }
    
    // ASP.NET handles the web request
    public IActionResult List()
    {
        List<Product> products = new List<Product>();
        
        // ADO.NET handles database access
        using (SqlConnection conn = new SqlConnection(_connectionString))
        {
            conn.Open();
            // Specify explicit columns instead of SELECT *
            SqlCommand cmd = new SqlCommand(
                "SELECT Id, Name, Price FROM Products", conn);
            
            using (SqlDataReader reader = cmd.ExecuteReader())
            {
                while (reader.Read())
                {
                    products.Add(new Product
                    {
                        // Use safe type conversion methods
                        Id = reader.GetInt32(0),
                        Name = reader.IsDBNull(1) ? "" : reader.GetString(1),
                        Price = reader.GetDecimal(2)
                    });
                }
            }
        }
        
        // ASP.NET renders the view with data
        return View(products);
    }
}
```

### Modern Alternatives

#### For ASP.NET:
- **ASP.NET Core**: Modern, cross-platform version
- **Blazor**: Build interactive web UIs using C# instead of JavaScript
- **Minimal APIs**: Lightweight approach for building APIs

#### For ADO.NET:
- **Entity Framework (EF)**: Object-Relational Mapper (ORM) that simplifies data access
- **Entity Framework Core**: Modern, cross-platform version
- **Dapper**: Lightweight ORM that extends ADO.NET with additional functionality for simplified data access

### Summary

- **ASP.NET** = Web framework for building web applications (the frontend/presentation)
- **ADO.NET** = Data access library for working with databases (the backend/data)
- They serve different purposes but often work together in .NET applications
- ASP.NET handles "what users see and interact with"
- ADO.NET handles "how the application gets and saves data"

Think of it this way:
- **ASP.NET** is like the restaurant's dining area and waitstaff (customer-facing)
- **ADO.NET** is like the kitchen's ingredient storage and retrieval system (behind-the-scenes)

---

## Getting Started with DutyRoster

Simply open `index.html` in any modern web browser. No installation or server setup required!

### Usage

1. Open the application
2. View the automatically generated roster for 2026
3. Use search functionality to filter by name or week
4. Export to CSV for spreadsheet use
5. Print for physical copies

## Project Structure

```
DutyRoster/
├── index.html    # Single-page application with embedded CSS and JavaScript
└── README.md     # This file
```

## License

This project is open source and available for educational purposes.
