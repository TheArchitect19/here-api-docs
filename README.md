# HERE Batch Geocoding API Integration

This repository provides a guide and examples for integrating the HERE Batch Geocoding API, allowing bulk processing of address data efficiently. The API returns latitude, longitude, and relevance scores to help assess the quality of each geocoded address match.

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Batch Request Format](#batch-request-format)
- [Request Parameters](#request-parameters)
- [Response Parameters and Scoring](#response-parameters-and-scoring)
- [Limitations](#limitations)
- [Example Code](#example-code)
- [Troubleshooting](#troubleshooting)

---

## Overview

The HERE Batch Geocoding API allows you to submit a file containing multiple addresses for bulk geocoding, returning location coordinates and relevance scores.

## Getting Started

To use this API, you’ll need to:
1. **Create a HERE Developer Account**: [HERE Developer Portal](https://developer.here.com/).
2. **Obtain an API Key** for authentication.

## Batch Request Format

**Endpoint**:
```plaintext
https://batch.geocoder.ls.hereapi.com/6.2/jobs
```

### Example Request

```http
POST https://batch.geocoder.ls.hereapi.com/6.2/jobs
```

#### Request Body
- **action**: Set to `run` to initiate a batch job.
- **indelim** and **outdelim**: Specify delimiters used in input/output files.
- **header**: Set to `true` if your input file contains headers.
- **outcols**: Define the output fields (e.g., `latitude`, `longitude`, `matchLevel`, `relevance`).

An example of a CSV file format:
```csv
customerId,address
123,1600 Amphitheatre Parkway, Mountain View, CA
124,1 Infinite Loop, Cupertino, CA
```

## Request Parameters

- **API Key**: Required for authentication.
- **Action**: Defines the operation, with `run` starting a batch job.
- **Outcols**: Specifies output columns, such as `latitude`, `longitude`, and `matchLevel`.
- **Customer ID Reference**: (Optional) Allows adding a custom identifier to map results back to the input data.

## Response Parameters and Scoring

- **Latitude/Longitude**: Geocoded coordinates.
- **Relevance Score**: Confidence level (0-1) for the returned match, where higher values indicate a stronger match.
- **Match Level**: Specifies the granularity of the match, such as:
  - `houseNumber`: Precise address match.
  - `street`: Street-level match only.
  - `city`, `county`, `state`: Broader region matches.

- **Location ID**: A unique identifier for each matched address.

## Limitations

- **Quota**: Request limits vary based on HERE account plan.
- **Batch Size**: Typically up to 100,000 addresses per request (depends on plan).
- **Rate Limiting**: Limited requests per day; consult HERE account for specifics.
- **File Size**: Input file size usually capped at 10 MB (may vary by plan).
- **Processing Time**: Large files may experience longer processing times.

---

## Example Code

### Python Request Example

```python
import requests

api_key = 'YOUR_API_KEY'
file_path = './addresses.csv'

def create_batch_job():
    with open(file_path, 'rb') as file:
        files = {
            'file': file,
            'apiKey': (None, api_key),
            'action': (None, 'run'),
            'header': (None, 'true'),
            'outcols': (None, 'latitude,longitude,matchLevel,relevance')
        }
        
        response = requests.post('https://batch.geocoder.ls.hereapi.com/6.2/jobs', files=files)
        print('Batch Job Created:', response.json())

create_batch_job()

```

Replace `YOUR_API_KEY` with your actual API key.

---

## Troubleshooting

1. **Ensure API Key is valid**: Invalid keys will result in authentication errors.
2. **Check file format**: Ensure the input file meets format requirements.
3. **Monitor request limits**: Exceeding limits may result in throttling.

For additional information, visit the [HERE Developer Documentation](https://developer.here.com/documentation).

---

## License
MIT
