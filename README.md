# Currency-Converter-API
A project to demonstrate skills
# Currency Converter API

A RESTful API service for currency conversion with real-time exchange rates. Built with Flask and powered by open exchange rate APIs.

## Features

### Currency Conversion
- Support for 160+ world currencies
- Real-time exchange rates
- GET and POST request methods
- Precision up to 6 decimal places

### Performance
- Exchange rate caching (1 hour)
- Automatic updates
- Fallback API support
- Fast response times

### API Endpoints
- Currency conversion endpoint
- All exchange rates retrieval
- Currency list endpoint
- Health check endpoint
- Web interface

### User Interface
- Clean web interface
- API documentation page
- Real-time statistics display
- Request examples

## Technologies

- Flask 3.0 - web framework
- requests - HTTP client
- ExchangeRate-API - exchange rate data source
- JSON - data format

## Installation

```bash
git clone https://github.com/yourusername/currency-api.git
cd currency-api
pip install -r requirements.txt
```

## Quick Start

### Run the server

```bash
python app.py
```

Server will be available at `http://localhost:5000`

### Make a request

```bash
curl "http://localhost:5000/api/convert?amount=100&from=USD&to=EUR"
```

### Open web interface

Navigate to `http://localhost:5000` in your browser to see:
- API documentation
- Live statistics
- Usage examples

## API Documentation

### 1. Convert Currency

**GET** `/api/convert`

Convert amount from one currency to another.

**Parameters:**
- `amount` (float) - amount to convert (required)
- `from` (string) - source currency code (required)
- `to` (string) - target currency code (required)

**Example:**
```bash
curl "http://localhost:5000/api/convert?amount=100&from=USD&to=EUR"
```

**Response:**
```json
{
  "success": true,
  "query": {
    "from": "USD",
    "to": "EUR",
    "amount": 100
  },
  "result": 92.15,
  "rate": 0.9215,
  "timestamp": "2024-11-27T10:30:00"
}
```

### 2. Convert Currency (POST)

**POST** `/api/convert`

Convert currency using JSON request body.

**Request Body:**
```json
{
  "amount": 100,
  "from": "USD",
  "to": "EUR"
}
```

**Example:**
```bash
curl -X POST http://localhost:5000/api/convert \
  -H "Content-Type: application/json" \
  -d '{"amount":100,"from":"USD","to":"EUR"}'
```

**Response:**
Same as GET request

### 3. Get All Exchange Rates

**GET** `/api/rates`

Retrieve all exchange rates for base currency.

**Parameters:**
- `base` (string, optional) - base currency (default: USD)

**Example:**
```bash
curl "http://localhost:5000/api/rates"
```

**Response:**
```json
{
  "success": true,
  "base": "USD",
  "rates": {
    "EUR": 0.9215,
    "GBP": 0.7923,
    "JPY": 149.82,
    "UAH": 41.25,
    ...
  },
  "timestamp": "2024-11-27T10:30:00"
}
```

### 4. Get Currency List

**GET** `/api/currencies`

Get list of all available currencies.

**Example:**
```bash
curl "http://localhost:5000/api/currencies"
```

**Response:**
```json
{
  "success": true,
  "count": 162,
  "currencies": ["AED", "AFN", "ALL", "AMD", ...]
}
```

### 5. Health Check

**GET** `/api/health`

Check service status and availability.

**Example:**
```bash
curl "http://localhost:5000/api/health"
```

**Response:**
```json
{
  "status": "healthy",
  "currencies_available": 162,
  "last_update": "2024-11-27T10:30:00",
  "timestamp": "2024-11-27T10:31:00"
}
```

## Usage Examples

### Python

```python
import requests

# Convert currency
response = requests.get(
    "http://localhost:5000/api/convert",
    params={
        "amount": 100,
        "from": "USD",
        "to": "EUR"
    }
)

data = response.json()
print(f"Result: {data['result']} EUR")
print(f"Rate: {data['rate']}")
```

### JavaScript

```javascript
fetch('http://localhost:5000/api/convert?amount=100&from=USD&to=EUR')
  .then(response => response.json())
  .then(data => {
    console.log(`Result: ${data.result} EUR`);
    console.log(`Rate: ${data.rate}`);
  });
```

### cURL

```bash
# GET request
curl "http://localhost:5000/api/convert?amount=100&from=USD&to=EUR"

# POST request
curl -X POST http://localhost:5000/api/convert \
  -H "Content-Type: application/json" \
  -d '{"amount":100,"from":"USD","to":"EUR"}'

# Get all rates
curl "http://localhost:5000/api/rates"

# Get currency list
curl "http://localhost:5000/api/currencies"
```

## Supported Currencies

Over 160 currencies including:

- USD - United States Dollar
- EUR - Euro
- GBP - British Pound Sterling
- JPY - Japanese Yen
- UAH - Ukrainian Hryvnia
- PLN - Polish Zloty
- CHF - Swiss Franc
- CAD - Canadian Dollar
- AUD - Australian Dollar
- CNY - Chinese Yuan

And many more.

## Code Examples

### Basic Conversion

```python
from app import CurrencyConverter

converter = CurrencyConverter()

result = converter.convert(
    amount=100,
    from_currency="USD",
    to_currency="EUR"
)

print(result)
```

### Get Available Currencies

```python
currencies = converter.get_available_currencies()
print(f"Available: {len(currencies)} currencies")
```

### Force Rate Update

```python
rates = converter.get_rates(force_refresh=True)
```

## Configuration

### Change Port

In `app.py`:
```python
app.run(host='0.0.0.0', port=8080)
```

### Modify Cache Duration

```python
CACHE_DURATION = timedelta(hours=2)  # 2 hours
```

### Use Different API

Replace `EXCHANGE_API_URL` in `app.py`:
```python
EXCHANGE_API_URL = "https://your-api-url.com/latest/USD"
```

## Response Format

### Successful Conversion

```json
{
  "success": true,
  "query": {
    "from": "USD",
    "to": "EUR",
    "amount": 100
  },
  "result": 92.15,
  "rate": 0.9215,
  "timestamp": "2024-11-27T10:30:00"
}
```

### Error Response

```json
{
  "success": false,
  "error": "Unknown currency: XXX"
}
```

## HTTP Status Codes

- `200` - Success
- `400` - Bad request (invalid parameters)
- `404` - Endpoint not found
- `500` - Internal server error

## Project Structure

```
currency-api/
├── app.py              # Main Flask application (500+ lines)
├── examples.py         # Usage examples
├── requirements.txt    # Python dependencies
├── README.md          # Documentation
└── .gitignore
```

## Architecture

### CurrencyConverter Class

**Methods:**
- `fetch_rates()` - Fetch exchange rates from API
- `get_rates(force_refresh)` - Get rates with caching
- `convert(amount, from_currency, to_currency)` - Convert currency
- `get_available_currencies()` - List available currencies

### Flask Routes

- `/` - Web interface
- `/api/convert` - Currency conversion (GET/POST)
- `/api/rates` - All exchange rates (GET)
- `/api/currencies` - Currency list (GET)
- `/api/health` - Health check (GET)

### Error Handling

- Request timeout handling
- Invalid currency detection
- API failure fallback
- Invalid amount validation

## Production Deployment

### Using Gunicorn

```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

### Using Docker

Create `Dockerfile`:
```dockerfile
FROM python:3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 5000
CMD ["python", "app.py"]
```

Build and run:
```bash
docker build -t currency-api .
docker run -p 5000:5000 currency-api
```

### Using systemd

Create `/etc/systemd/system/currency-api.service`:
```ini
[Unit]
Description=Currency Converter API
After=network.target

[Service]
Type=simple
User=youruser
WorkingDirectory=/path/to/api
ExecStart=/usr/bin/python3 app.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start:
```bash
sudo systemctl start currency-api
sudo systemctl enable currency-api
```

## Performance Optimization

### Caching Strategy

The API caches exchange rates for 1 hour to:
- Reduce API calls
- Improve response time
- Avoid rate limiting

### Rate Limiting

For production, implement rate limiting:
```python
from flask_limiter import Limiter

limiter = Limiter(
    app,
    default_limits=["100 per hour"]
)
```

### CORS Support

Add CORS for web applications:
```python
from flask_cors import CORS

CORS(app)
```

## Security Considerations

- No API keys required for basic usage
- Input validation on all parameters
- Error messages don't expose internals
- HTTPS recommended for production

## Troubleshooting

### API not starting
- Check Python version (3.8+)
- Verify dependencies installed
- Check port 5000 availability

### Conversion errors
- Verify currency codes are valid
- Check amount is positive number
- Ensure exchange rates loaded

### Rate update failures
- Check internet connection
- Verify API endpoints accessible
- Check logs for error details

## Testing

Run example scripts:
```bash
python examples.py
```

Test endpoints:
```bash
# Test conversion
curl "http://localhost:5000/api/convert?amount=100&from=USD&to=EUR"

# Test health
curl "http://localhost:5000/api/health"

# Test rates
curl "http://localhost:5000/api/rates"
```

## Monitoring

### Log Format

The API logs all requests:
```
2024-11-27 10:30:00 - INFO - Fetched rates: 162 currencies
2024-11-27 10:30:15 - INFO - Using cached rates
```

### Health Endpoint

Monitor service health:
```bash
curl http://localhost:5000/api/health
```

## API Limitations

- Rate updates: once per hour (cached)
- Precision: 6 decimal places
- Supported currencies: 160+
- Free tier limits apply to data source

## Future Enhancements

Potential improvements:
- Historical exchange rates
- Currency trend charts
- Multiple base currencies
- Cryptocurrency support
- Rate alerts
- API authentication
- Database storage
- Admin dashboard

## Contributing

1. Fork the repository
2. Create feature branch: `git checkout -b feature/NewFeature`
3. Commit changes: `git commit -m 'Add NewFeature'`
4. Push to branch: `git push origin feature/NewFeature`
5. Open Pull Request

## License

MIT License - see LICENSE file for details

## Author

Your Name
GitHub: github.com/your_username
Email: your@email.com

## Acknowledgments

- ExchangeRate-API - free exchange rate data
- Flask - Python web framework
- requests - HTTP library

## Support

For issues or questions:
- Create an issue on GitHub
- Email: nazarijsevcuk69@gmail.com

## Related Projects

- Telegram ToDo Bot - Task management bot
- Excel Automation - Automated Excel processing
- News Parser - Web scraping tool
- Text Analyzer - Text analysis with visualization

---

Last updated: November 2025
Version: 1.0
Status: Production Ready
