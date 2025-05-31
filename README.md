# Google Flights MCP

This MCP server provides tools to interact with Google Flights data using the bundled `fast_flights` library.

## Features

Provides the following MCP tools:

*   **`get_flights_on_date`**: Fetches available one-way flights for a specific date between two airports.
    
    | Argument       | Type     | Description                                                  | Default |
    | -------------- | -------- | ------------------------------------------------------------ | ------- |
    | origin         | str      | Origin airport code (e.g., "DEN")                            | N/A     |
    | destination    | str      | Destination airport code (e.g., "LAX")                       | N/A     |
    | date           | str      | Date to search in YYYY-MM-DD format                          | N/A     |
    | adults         | int      | Number of adult passengers                                   | 1       |
    | seat_type      | str      | Fare class (e.g., "economy" or "business")                   | "economy" |
    | sort_cheapest  | bool     | Sort results by cheapest price if True                       | False   |
    | stops          | int      | Number of stops to filter (0 for non-stop)                    | None    |
    | limit          | int      | Maximum number of flights to return                           | 10      |
*   **`get_round_trip_flights`**: Fetches available round-trip flights for specific departure and return dates.
    
    | Argument       | Type     | Description                                                  | Default |
    | -------------- | -------- | ------------------------------------------------------------ | ------- |
    | origin         | str      | Origin airport code (e.g., "DEN")                            | N/A     |
    | destination    | str      | Destination airport code (e.g., "LAX")                       | N/A     |
    | departure_date | str      | Departure date in YYYY-MM-DD format                           | N/A     |
    | return_date    | str      | Return date in YYYY-MM-DD format                              | N/A     |
    | adults         | int      | Number of adult passengers                                   | 1       |
    | seat_type      | str      | Fare class (e.g., "economy" or "business")                   | "economy" |
    | sort_cheapest  | bool     | Sort results by cheapest price if True                       | False   |
    | stops          | int      | Number of stops to filter (0 for non-stop)                    | None    |
    | limit          | int      | Maximum number of flights to return                           | 10      |
*   **`find_all_flights_in_range`**: Finds available round-trip flights within a specified date range. Can optionally return only the cheapest flight found for each date pair.
    
    | Argument       | Type     | Description                                                  | Default |
    | -------------- | -------- | ------------------------------------------------------------ | ------- |
    | origin         | str      | Origin airport code (e.g., "DEN")                            | N/A     |
    | destination    | str      | Destination airport code (e.g., "LAX")                       | N/A     |
    | start_date_str | str      | Start date of range in YYYY-MM-DD format                      | N/A     |
    | end_date_str   | str      | End date of range in YYYY-MM-DD format                        | N/A     |
    | min_stay_days  | int      | Minimum number of days for the stay                            | None    |
    | max_stay_days  | int      | Maximum number of days for the stay                            | None    |
    | adults         | int      | Number of adult passengers                                    | 1       |
    | seat_type      | str      | Fare class (e.g., "economy" or "business")                    | "economy" |
    | sort_cheapest  | bool     | Sort results by cheapest price for each pair if True          | False   |
    | stops          | int      | Number of stops to filter (0 for non-stop)                     | None    |
    | limit          | int      | Maximum number of flights to return per date pair             | 10      |

## Setup

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/msr2903/google-flights-mcp.git
    cd Google-Flights-MCP-Server
    ```
2.  **Create a virtual environment and install dependencies:**
    ```bash
    uv venv
    source .venv/bin/activate  # On Windows: .venv\Scripts\activate
    uv pip install -e .
    ```
3.  **Install Playwright browsers (needed by `fast_flights`):**
    ```bash
    playwright install
    ```

## Running the Server

You can test the server with MCP Inspector by running:

```bash
uv run server.py
```

The server uses STDIO transport by default.

## Integrating with MCP Clients (e.g., Cline, Claude Desktop)

Add the server to your MCP client's configuration file. Example for `cline_mcp_settings.json` or `claude_desktop_config.json`:

-macOS
```json
{
  "mcpServers": {
    "google-flights": {
      "command": "uv",
      "args": [
        "--directory",
        "/ABSOLUTE/PATH/TO/PARENT/FOLDER/google-flights-mcp",
        "run",
        "server.py"
      ],
      "env": {},
      "disabled": false,
      "autoApprove": []
    }
    // ... other servers
  }
}
```
-Windows
```json
{
  "mcpServers": {
    "google-flights": {
      "command": "uv",
      "args": [
        "--directory",
        "C:\\ABSOLUTE\\PATH\\TO\\PARENT\\FOLDER\\google-flights-mcp",
        "run",
        "server.py"
      ],
      "env": {},
      "disabled": false,
      "autoApprove": []
    }
    // ... other servers
  }
}
```
 - **Note**: You may need to put the full path to the uv executable in the command field. You can get this by running `which uv` on MacOS/Linux or `where uv` on Windows.


## Notes

*   This MCP is the fork that originally from [https://github.com/opspawn/Google-Flights-MCP-Server](https://github.com/opspawn/Google-Flights-MCP-Server)) which is modified to add new features and simplify the installation.
*   This server bundles the `fast_flights` library (originally from [https://github.com/AWeirdDev/flights](https://github.com/AWeirdDev/flights)) for its core flight scraping functionality. Please refer to the included `LICENSE` file for its terms.
*   Flight scraping can sometimes be unreliable or slow depending on Google Flights changes and network conditions. The tools include basic error handling.
*   The `find_all_flights_in_range` tool can be resource-intensive as it checks many date combinations.
