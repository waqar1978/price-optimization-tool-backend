# Price Optimization Tool - Backend

A Django-based REST API backend service that helps businesses optimize product pricing through demand forecasting and profit maximization algorithms. The system analyzes historical sales data, generates demand forecasts using exponential smoothing, and calculates optimal pricing strategies based on price elasticity.

---

## Overview

The Price Optimization Tool backend provides a comprehensive API for managing products, tracking sales history, and generating data-driven pricing recommendations. It uses time series analysis to forecast future demand and applies economic models to determine optimal price points that maximize profitability while considering market elasticity.

The system is designed for e-commerce platforms, retail businesses, and inventory management systems that need intelligent pricing strategies based on historical performance and market dynamics.

---

## Features

### Product Management
- Complete CRUD operations for product catalog
- Product attributes: name, description, category, cost price, selling price, stock levels, units sold, customer ratings
- Advanced filtering and search capabilities using django-filters
- Automatic sales tracking on product updates
- CSV data import via custom management command

### Demand Forecasting
- Time series analysis using Exponential Smoothing (ETS) models
- Configurable forecast intervals (default: 30 days)
- Generates forecasts for:
  - Units demand
  - Revenue projections
  - Profit estimates
- Handles seasonal trends and historical patterns

### Price Optimization
- Calculates optimal pricing based on:
  - Current market price
  - Forecasted demand
  - Cost structure
  - Price elasticity (configurable, default: -1.2)
- Profit maximization algorithm
- Batch optimization support for multiple products
- Returns optimal price and maximum profit projections

### Sales Analytics
- Historical sales tracking with daily granularity
- Sales data linked to products with date-based uniqueness
- Complete sales history retrieval per product
- Automatic sales record creation on product updates

### Authentication & Security
- JWT-based authentication using djangorestframework-simplejwt
- Custom user model with email-based authentication
- Token refresh mechanism
- CORS configuration for frontend integration
- Environment-based configuration management

### API Design
- RESTful architecture with camelCase JSON responses
- Comprehensive viewsets with custom actions
- Query parameter support for dynamic data inclusion
- Batch operations support
- Proper HTTP status codes and error handling

---

## Technology Stack

### Backend Framework
- **Django 5.2+** - High-level Python web framework
- **Django REST Framework** - Powerful toolkit for building Web APIs
- **djangorestframework-simplejwt** - JWT authentication for DRF

### Data & Analytics
- **NumPy** - Numerical computing for optimization algorithms
- **Pandas** - Data manipulation and analysis
- **statsmodels** - Statistical modeling and time series analysis (Exponential Smoothing)

### Database
- **PostgreSQL** - Production-grade relational database
- **psycopg2** - PostgreSQL adapter for Python

### API Utilities
- **django-filter** - Dynamic queryset filtering
- **djangorestframework-camel-case** - Automatic camelCase/snake_case conversion
- **django-cors-headers** - Cross-Origin Resource Sharing (CORS) handling
- **django-environ** - Environment variable management

---

## Project Structure

```
PriceOptimizationTool/
├── PriceOptimizationTool/          # Main project configuration
│   ├── settings.py                 # Django settings with environment config
│   ├── urls.py                     # Root URL configuration
│   ├── wsgi.py                     # WSGI application entry point
│   └── asgi.py                     # ASGI application entry point
│
├── Products/                       # Products app - core business logic
│   ├── models.py                   # Products and ProductSales models
│   ├── serializers.py              # DRF serializers with forecast integration
│   ├── apis.py                     # ViewSets and API endpoints
│   ├── urls.py                     # Products URL routing
│   ├── filters.py                  # Custom filter sets
│   ├── utils.py                    # Demand forecasting and optimization algorithms
│   ├── admin.py                    # Django admin configuration
│   ├── management/
│   │   └── commands/
│   │       └── load_csv.py         # CSV import management command
│   └── migrations/                 # Database migrations
│
├── User/                           # User management app
│   ├── models.py                   # Custom user model
│   ├── managers.py                 # User manager for authentication
│   ├── admin.py                    # User admin configuration
│   └── migrations/                 # Database migrations
│
├── manage.py                       # Django management script
├── product data.csv                # Sample product data
└── README.md                       # Project documentation
```

### Key Components

**Models:**
- `Products` - Product catalog with pricing, inventory, and ratings
- `ProductSales` - Daily sales records with date-based tracking
- `User` - Custom user model with email authentication

**API Endpoints:**
- `/api/v1/products/` - Product CRUD operations
- `/api/v1/products/sales/` - Batch sales data retrieval
- `/api/v1/products/{id}/sales/` - Individual product sales history
- `/api/v1/products/price-optimize/` - Batch price optimization
- `/api/v1/token/` - JWT token generation
- `/api/v1/token/refresh/` - JWT token refresh

**Utilities:**
- `generate_demand_forecast()` - Time series forecasting using ETS
- `calculate_optimal_price()` - Price optimization algorithm

---

## Installation & Setup

### Prerequisites
- Python 3.10 or higher
- PostgreSQL 12 or higher
- pip (Python package manager)

### 1. Clone Repository
```bash
git clone <repository-url>
cd PriceOptimizationTool
```

### 2. Create Virtual Environment
```bash
python -m venv venv

# On Windows
venv\Scripts\activate

# On macOS/Linux
source venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Create a `.env` file in the project root:

```env
DEBUG=True
SECRET_KEY=your-secret-key-here
POSTGRES_DATABASE=price_optimization_db
POSTGRES_USER=your_db_user
POSTGRES_PASSWORD=your_db_password
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
CORS_ALLOWED_ORIGINS=http://localhost:3000,http://localhost:5173
```

### 5. Setup Database
```bash
# Create PostgreSQL database
createdb price_optimization_db

# Run migrations
python manage.py makemigrations
python manage.py migrate
```

### 6. Load Sample Data (Optional)
```bash
python manage.py load_csv "product data.csv"
```

### 7. Create Superuser
```bash
python manage.py createsuperuser
```

### 8. Run Development Server
```bash
python manage.py runserver
```

The API will be available at `http://127.0.0.1:8000/`

---

## API Usage Examples

### Get Products with Demand Forecast
```bash
GET /api/v1/products/?with_demand_forecast=true&demand_forecast_interval=30
```

### Get Product Sales History
```bash
GET /api/v1/products/1/sales/
```

### Optimize Prices (Batch)
```bash
POST /api/v1/products/price-optimize/
Content-Type: application/json

[
  {
    "id": 1,
    "currentPrice": 99.99,
    "totalForecastedDemand": 500,
    "costPrice": 50.00
  },
  {
    "id": 2,
    "currentPrice": 149.99,
    "totalForecastedDemand": 300,
    "costPrice": 80.00
  }
]
```

### Obtain JWT Token
```bash
POST /api/v1/token/
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "your_password"
}
```

---

## Configuration

### Database Configuration
The application uses PostgreSQL by default. Configure connection settings in `.env` file.

### CORS Settings
Add allowed frontend origins to `CORS_ALLOWED_ORIGINS` in `.env` as a comma-separated list.

### REST Framework Settings
- Automatic camelCase conversion for JSON responses
- JWT authentication enabled by default
- Django filters integration for querysets

---

## Development

### Running Tests
```bash
python manage.py test
```

### Creating Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### Accessing Admin Panel
Navigate to `http://127.0.0.1:8000/admin/` and login with superuser credentials.

---

## License

This project is available for educational and commercial use.