# coffee-shop-api
coffee-shop-api
# Coffee Shop API

A serverless REST API for managing coffee shop products built with AWS Lambda, API Gateway, and DynamoDB.

[![Deploy to AWS](https://github.com/actions/workflow-badge.svg)](https://github.com/yourusername/coffee-shop-api/actions)

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Requirements](#requirements)
- [Project Structure](#project-structure)
- [Setup Instructions](#setup-instructions)
- [API Documentation](#api-documentation)
- [CI/CD Pipeline](#cicd-pipeline)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Overview

This project implements a serverless REST API for a coffee shop product catalog. The API allows for managing coffee products with full CRUD operations (Create, Read, Update, Delete) and is built using the AWS Serverless Framework with Python Lambda functions, API Gateway, and DynamoDB for data storage.

### Business Case

This API serves as the backend for a coffee shop's digital product catalog, enabling:
- Baristas to add new products to the menu
- Customers to browse available coffee products
- Management to update product details (price, description, availability)
- Staff to remove discontinued items

## Architecture

![Architecture Diagram](https://via.placeholder.com/800x400?text=Coffee+Shop+API+Architecture)

- **AWS Lambda**: Python functions handle business logic for each API endpoint
- **API Gateway**: Manages REST API endpoints and routes requests to Lambda functions
- **DynamoDB**: NoSQL database for storing product data
- **Serverless Framework**: Infrastructure as Code (IaC) tool for AWS resource provisioning
- **GitHub Actions**: CI/CD pipeline for automated testing and multi-stage deployments

## Requirements

- AWS CLI configured with appropriate permissions
- Node.js 14+ and npm
- Python 3.8+
- Serverless Framework CLI
- Git

## Project Structure

```
coffee-shop-api/
├── .github/
│   └── workflows/
│       └── deploy.yml         # GitHub Actions workflow
├── functions/                 # Lambda functions
│   ├── create_product.py
│   ├── get_product.py
│   ├── list_products.py
│   ├── update_product.py
│   └── delete_product.py
├── scripts/                   # Helper scripts
│   └── deploy.sh              # Deployment script
├── tests/                     # Unit tests
│   └── test_functions.py
├── package.json               # Node.js dependencies for Serverless Framework
├── requirements.txt           # Python dependencies
├── serverless.yml             # Main serverless configuration
└── README.md                  # Project documentation
```

## Setup Instructions

### Prerequisites

1. Install the required tools:
   ```bash
   # Install Node.js and npm (if not already installed)
   # Then install Serverless Framework
   npm install -g serverless

   # Install project dependencies
   npm install
   
   # Install Python dependencies
   pip install -r requirements.txt
   ```

2. Configure AWS credentials:
   ```bash
   aws configure
   ```

### Local Development

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/coffee-shop-api.git
   cd coffee-shop-api
   ```

2. Install dependencies:
   ```bash
   npm install
   pip install -r requirements.txt
   ```

3. Run tests:
   ```bash
   python -m pytest tests/
   ```

4. Local invocation of Lambda functions:
   ```bash
   serverless invoke local -f listProducts
   ```

## API Documentation

### Endpoints

| Method | Endpoint | Description | Function |
|--------|----------|-------------|----------|
| GET | /products | List all products | list_products.py |
| GET | /products/{id} | Get a specific product | get_product.py |
| POST | /products | Create a new product | create_product.py |
| PUT | /products/{id} | Update a product | update_product.py |
| DELETE | /products/{id} | Delete a product | delete_product.py |

### Request/Response Examples

#### Create Product
```
POST /products

Request Body:
{
  "name": "Espresso",
  "description": "Strong Italian coffee",
  "price": 3.50,
  "category": "Hot Coffee",
  "available": true
}

Response:
{
  "id": "12345",
  "name": "Espresso",
  "description": "Strong Italian coffee",
  "price": 3.50,
  "category": "Hot Coffee",
  "available": true,
  "createdAt": "2023-04-14T10:00:00Z"
}
```

#### Get Product
```
GET /products/12345

Response:
{
  "id": "12345",
  "name": "Espresso",
  "description": "Strong Italian coffee",
  "price": 3.50,
  "category": "Hot Coffee",
  "available": true,
  "createdAt": "2023-04-14T10:00:00Z"
}
```

#### Update Product
```
PUT /products/12345

Request Body:
{
  "price": 3.75,
  "available": false
}

Response:
{
  "id": "12345",
  "name": "Espresso",
  "description": "Strong Italian coffee",
  "price": 3.75,
  "category": "Hot Coffee",
  "available": false,
  "createdAt": "2023-04-14T10:00:00Z",
  "updatedAt": "2023-04-14T11:30:00Z"
}
```

#### Delete Product
```
DELETE /products/12345

Response:
{
  "message": "Product deleted successfully"
}
```

#### List Products
```
GET /products

Response:
{
  "items": [
    {
      "id": "12345",
      "name": "Espresso",
      "description": "Strong Italian coffee",
      "price": 3.75,
      "category": "Hot Coffee",
      "available": false
    },
    {
      "id": "67890",
      "name": "Cappuccino",
      "description": "Espresso with steamed milk foam",
      "price": 4.50,
      "category": "Hot Coffee",
      "available": true
    }
  ],
  "count": 2
}
```

## CI/CD Pipeline

This project uses GitHub Actions for continuous integration and deployment to multiple environments (dev, prod).

### Pipeline Workflow

![CI/CD Pipeline](https://via.placeholder.com/800x300?text=CI/CD+Pipeline+Workflow)

The pipeline performs the following steps:
1. Checkout code
2. Set up Python and Node.js
3. Install dependencies
4. Run linting and unit tests
5. Deploy to the appropriate environment based on branch:
   - `develop` branch → `dev` environment
   - `main` branch → `prod` environment

### Pipeline Configuration

The GitHub Actions workflow is defined in `.github/workflows/deploy.yml`. The workflow is triggered on push to the `develop` and `main` branches.

To set up the pipeline:

1. Configure the following secrets in your GitHub repository:
   - `AWS_ACCESS_KEY_ID`
   - `AWS_SECRET_ACCESS_KEY`
   - `AWS_REGION`

2. The pipeline will automatically deploy to the appropriate stage when commits are pushed.

## Testing

### Unit Tests

Unit tests are located in the `tests/` directory and can be run with:

```bash
python -m pytest tests/
```

### Integration Tests

You can test the deployed API using curl or Postman. Example requests:

```bash
# List all products
curl -X GET https://your-api-gateway-url.amazonaws.com/dev/products

# Create a new product
curl -X POST https://your-api-gateway-url.amazonaws.com/dev/products \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Latte",
    "description": "Espresso with steamed milk",
    "price": 4.00,
    "category": "Hot Coffee",
    "available": true
  }'
```

## Deployment

### Manual Deployment

You can manually deploy the service using the Serverless Framework:

```bash
# Deploy to dev stage
serverless deploy --stage dev

# Deploy to production stage
serverless deploy --stage prod
```

### Using the Helper Script

A helper script is provided for convenient deployment:

```bash
# Deploy to dev stage
./scripts/deploy.sh dev

# Deploy to production stage
./scripts/deploy.sh prod
```

### Deployment Configuration

The `serverless.yml` file contains the main configuration for the service. Key aspects include:

- Stage-specific DynamoDB table names
- Stage-specific API Gateway endpoints
- IAM role configurations
- Lambda function definitions

## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

