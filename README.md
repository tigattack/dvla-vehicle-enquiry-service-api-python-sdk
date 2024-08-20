# DVLA Vehicle Enquiry Service Python SDK

`dvla_vehicle_enquiry_service` is a Python SDK that provides a simple interface for interacting with the DVLA (Driver and Vehicle Licensing Agency) Vehicle Enquiry Service API. It allows retrieval of detailed vehicle information based on the registration number, including tax status, MOT status, and more.

## Installation

Install the package using pip:

```
pip install dvla_vehicle_enquiry_service
```

## Usage

### Initialisation

To use the `VehicleEnquiryAPI` class, you'll need an API key provided by the DVLA service. You can also specify the environment as either `production` or `test` (default is `production`).

```python
from dvla_vehicle_enquiry_service import VehicleEnquiryAPI

api_key = "your_api_key"

# Initialise client using the default ('production') API
client = VehicleEnquiryAPI(api_key)

# Or initialise client using the test API
client = VehicleEnquiryAPI(api_key, environment="test")
```

### Fetch Vehicle Details by Registration

To fetch vehicle details using its registration number:

```python
import asyncio
from dvla_vehicle_enquiry_service import ErrorResponse, Vehicle

async def get_vehicle_details():
    response = await client.get_vehicle("ABC123")
    if isinstance(response, ErrorResponse):
        print(f"Error: {response.errors[0].title}")
    elif isinstance(response, Vehicle):
        print(f"Vehicle Make: {response.make}, MOT Status: {response.motStatus.value}")

asyncio.run(get_vehicle_details())
```

If the request is successful, this example will print something such as: `Vehicle Make: AUDI, MOT Status: Valid`.

### Error Handling

The SDK returns an `ErrorResponse` object when an error occurs, containing a list of `ErrorDetail` objects with specific error information.

```python
if isinstance(response, ErrorResponse):
    for error in response.errors:
        print(f"Error {error.status}: {error.title} - {error.detail}")
```

## Classes and Data Structures

### Vehicle Class

- `Vehicle`: Represents detailed information about a vehicle, including tax status, MOT status, make, model, and various other attributes.

### Error Handling Classes

- `ErrorResponse`: Encapsulates the error response returned by the API.
- `ErrorDetail`: Provides detailed information about individual errors.

### Enum Classes

- `TaxStatus`: Enumerates possible tax statuses of a vehicle (`TAXED`, `UNTAXED`, `SORN`, etc.).
- `MotStatus`: Enumerates possible MOT statuses of a vehicle (`VALID`, `NOT_VALID`, `NO_DETAILS_HELD`, etc.).

## License

This project is licensed under the MIT License.

## Contributing

Contributions are welcome! Please feel free to submit a pull request.
