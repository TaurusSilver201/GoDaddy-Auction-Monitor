# Domain Auction Scraper

A Python-based web scraper that monitors domain auctions on GoDaddy, providing real-time auction data including bid counts, current prices, and auction status.

## Features

- **Multi-threaded Scraping**: Concurrent processing of multiple domains for faster data collection
- **Proxy Support**: Rotates through multiple proxies to avoid rate limiting
- **Smart Retry Logic**: Automatic retry mechanism with configurable limits
- **Active Auction Detection**: Identifies domains with active auctions using Selenium
- **Flexible Output**: Generates multiple report types (full, filtered, removal lists)
- **Rate Limiting**: Built-in delays and request throttling
- **Reserve Price Detection**: Identifies whether reserve prices have been met

## Installation

### Prerequisites

- Python 3.7+
- Chrome browser (for Selenium automation)
- ChromeDriver

### Dependencies

```bash
pip install requests beautifulsoup4 pandas selenium fake-useragent tenacity
```

### Setup

1. Clone the repository:
```bash
git clone https://github.com/yourusername/domain-auction-scraper.git
cd domain-auction-scraper
```

2. Install required packages:
```bash
pip install -r requirements.txt
```

3. Download ChromeDriver and ensure it's in your PATH

4. Create configuration files:
   - `config.py` - Main configuration settings
   - `utils.py` - Utility functions for domains and proxies

## Configuration

Create a `config.py` file with the following settings:

```python
# Delay between requests (min, max) in seconds
delay_range = (2, 5)

# Number of concurrent threads
threads = 5

# Maximum retry attempts per domain
max_retries = 3

# Generate full detailed reports
full_report = True

# Skip file generation settings
skip_file = True
skip_file_bid_threshold = 100  # Minimum bid amount to skip

# Proxy settings (configure in utils.py)
```

Create a `utils.py` file with:

```python
def get_terms():
    """Return list of domains to check"""
    # Read from file or return list
    return ["example.com", "test.com"]

def get_proxies():
    """Return list of proxy servers"""
    return ["proxy1:port", "proxy2:port"]
```

## Usage

### Basic Usage

```bash
python scraper.py
```

### Output Files

The scraper generates several output files with timestamps:

- **`full_report_YYYYMMDD_HHMMSS.csv`**: Complete auction data for all domains
- **`skip_report_YYYYMMDD_HHMMSS.txt`**: Domains meeting skip criteria
- **`remove_YYYYMMDD_HHMMSS.txt`**: Domains with no auctions or unmet reserves

### Data Fields

| Field | Description |
|-------|-------------|
| Domain | Domain name being checked |
| Source | 'G' for GoDaddy auction, '0' for no auction |
| Bidders | Number of bidders/offers |
| Current_Bid | Current highest bid amount |
| Notes | Additional information |
| active_auction | Boolean indicating active auction status |
| inventoryType | Type of domain listing |
| isReserveMet | Whether reserve price has been met |

## Features in Detail

### Smart Filtering
- Automatically identifies domains with active auctions
- Filters based on bid thresholds
- Detects member listings vs regular auctions

### Proxy Rotation
- Randomly selects from configured proxy list
- Helps avoid IP-based rate limiting
- Supports HTTP proxies

### User Agent Rotation
- Multiple browser user agents
- Reduces detection as automated traffic
- Improves scraping success rate

### Error Handling
- Retry logic for failed requests
- Graceful handling of network timeouts
- Detailed error reporting

## Advanced Configuration

### Custom Headers
Modify the `headers_list` in the script to add custom HTTP headers:

```python
headers_list = [
    {
        'User-Agent': 'Your Custom User Agent',
        'Accept': 'text/html,application/xhtml+xml',
        # ... other headers
    }
]
```

### Selenium Options
Customize Chrome options for different environments:

```python
chrome_options = Options()
chrome_options.add_argument('--headless')  # Run in background
chrome_options.add_argument('--no-sandbox')  # For server environments
chrome_options.add_argument('--disable-dev-shm-usage')  # For Docker
```

## API Endpoints

The scraper uses the following GoDaddy endpoints:

- Domain listing: `https://auctions.godaddy.com/trpItemListing.aspx?domain={domain}`
- Auction API: `https://www.godaddy.com/domain-auctions/api/listing/{id}/`

## Troubleshooting

### Common Issues

1. **ChromeDriver not found**
   - Ensure ChromeDriver is installed and in PATH
   - Update ChromeDriver to match your Chrome version

2. **Proxy connection errors**
   - Verify proxy addresses and ports
   - Check proxy authentication if required

3. **Rate limiting**
   - Increase delay ranges in config
   - Reduce number of threads
   - Use more proxy servers

4. **Selenium timeouts**
   - Increase page load timeout
   - Check network connectivity
   - Verify domain accessibility

### Debug Mode

Enable detailed logging by modifying the print statements or adding:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Legal Notice

This tool is for educational and research purposes only. Users are responsible for:
- Complying with GoDaddy's Terms of Service
- Respecting rate limits and robot.txt files
- Using proxies and automation ethically
- Following applicable laws and regulations

## Disclaimer

This scraper is not affiliated with or endorsed by GoDaddy. Use at your own risk and ensure compliance with all applicable terms of service and legal requirements.

## Support

For issues, feature requests, or questions:
- Open an issue on GitHub
- Check existing issues for solutions
- Review the troubleshooting section

---

**Note**: Always test thoroughly in a development environment before running on production data or at scale.
