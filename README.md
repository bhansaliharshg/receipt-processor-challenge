# Receipt Processing API - Fetch

This is a Python-based API for processing receipts and calculating points based on specific rules. It provides several endpoints for interacting with the API.

## API - Hosted

This api is hosted on Render. The endpoints can be accessed through the link: https://receipt-processor-challenge.onrender.com. As the API uses FastAPI library the enpoints can be tested using the link: https://receipt-processor-challenge.onrender.com/docs. No local setup is required.

## API - Local Setup - Prerequisites

- Python 3.9 or above
- Docker (optional)

## Installation

To run the API locally, follow these steps:

1. Clone the repository.
2. Navigate to the project directory.
3. Install the required dependencies by running the following command:
   ```
   pip install -r requirements.txt
   ```
4. Start the API server using the following command:
   ```
   python main.py
   ```
   The API will be accessible at http://localhost:8000.

Alternatively, you can use Docker to build and run the API:

1. Build the Docker image using the provided Dockerfile:
   ```
   docker build -t receipt-api .
   ```
2. Run a Docker container using the built image:
   ```
   docker run -d --name receipt-api-container -p 8000:80 receipt-api
   ```
   The API will be accessible at http://localhost:8000.

## API Endpoints

### Get Points by ID

- **URL**: `/receipts/{id}/points`
- **Method**: GET
- **Description**: Retrieves the points associated with a specific receipt ID.
- **Query Parameters**:
  - `id` (string): The unique identifier of the receipt.
- **Response**:
  - `points` (integer): The calculated points for the receipt ID.
- **Example**:
  ```
  GET /receipts/7565dd88-4ce3-3d0c-a489-fd8331c3877b/points
  Response: {"points": 28}
  ```

### Get All Receipt IDs

- **URL**: `/receipts/ids`
- **Method**: GET
- **Description**: Retrieves all receipt IDs along with their associated information.
- **Response**:
  - List of objects containing the following fields:
    - `id` (string): The unique identifier of the receipt.
    - `receipt` (object): The receipt details.
    - `points` (integer): The calculated points for the receipt.
- **Example**:
  ```
  GET /receipts/ids
  Response: [{"id": "7565dd88-4ce3-3d0c-a489-fd8331c3877b", "receipt": {...}, "points": 28}, {"id": "987dd88-3ce3-3d0f-a480-fd8331c3877a", "receipt": {...}, "points": 75}]
  ```

### Process Receipt

- **URL**: `/receipts/process`
- **Method**: POST
- **Description**: Processes a receipt by calculating the associated points.
- **Request Body**:
  - `receipt` (object): The receipt details including retailer, purchase date, purchase time, total, and items.
    - `retailer` (string): The name of the retailer.
    - `purchaseDate` (string): The date of purchase (format: "YYYY-MM-DD").
    - `purchaseTime` (string): The time of purchase (format: "HH:MM").
    - `total` (string): The total amount of the receipt.
    - `items` (array): An array of objects representing the purchased items.
      - `shortDescription` (string): The short description of the item.
      - `price` (string): The price of the item.
- **Response**:
  - `id` (string): The unique identifier assigned to the processed receipt.
- **Example**:
  ```
  POST /receipts/process
  Request Body: {
  "retailer": "Target",
  "purchaseDate": "2022-01-01",
  "purchaseTime": "13:01",
  "items": [
    {
      "shortDescription": "Mountain Dew 12PK",
      "price": "6.49"
    },{
      "shortDescription": "Emils Cheese Pizza",
      "price": "12.25"
    },{
      "shortDescription": "Knorr Creamy Chicken",
      "price": "1.26"
    },{
      "shortDescription": "Doritos Nacho Cheese",
      "price": "3.35"
    },{
      "shortDescription": "   Klarbrunn 12-PK 12 FL OZ  ",
      "price": "12.00"
    }
  ],"total": "35.35"}
  Response: {"id": "7565dd88-4ce3-3d0c-a489-fd8331c3877b"}
  ```