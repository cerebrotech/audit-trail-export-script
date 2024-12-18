# Audit trail - CSV Export Script

This script can be used to export Audit trail events into a CSV file

It takes several parameters to customize the filter of the export process, including a required hostname and optional authentication details like JWT tokens or API keys.

The output is a CSV file containing all the events that matched the query

> Note: You can upload the script into a **Domino Project** and run it as a **Job**

## Usage

The script requires a `Hostname` and either a `JWT` or an `API-Key` for authentication. They can be provided as arguments or can be defined in a `.env` file to be loaded

Example of the .env file
```
DOMINO_HOSTNAME=<A-DOMINO-INSTANCE>
JWT=<A-JWT>
API_KEY=<AN-API-KEY>
```

It also accepts optional parameters such as an event name and user information.

## Arguments

| Argument         | Required | Description                                                            |
|------------------|----------|------------------------------------------------------------------------|
| `--hostname`     | Yes*     | The hostname to connect to.                                            |
| `--jwt`          | Yes*     | The JWT token for authentication.                                      |
| `--api-key`      | Yes*     | The Domino API key for authentication.                                 |
| `--event`        | No       | The event name.                                                        |
| `--user_name`    | No       | The name of the user performing the export action.                     |
| `--target_name`  | No       | Object that received the action                                        |
| `--project_name` | No       | Name of the project                                                    |
| `--start_date`   | No       | Timestamp from when to start looking data. Format: YYYY-MM-DD HH:MM:SS |
| `--end_date`     | No       | Timestamp up to when to look for data. Format: YYYY-MM-DD HH:MM:SS     |

***Note:** Either `--jwt` or `--api-key` must be provided. If neither is supplied, the script will fail.

## Examples

### Example 1: Using JWT for authentication

```bash
python3 export_audit_trail.py --hostname https://domino_instance.com --jwt my-jwt-token
```

### Example 2: Providing the hostname and authentication through the .env file and exporting all available data

`.env` definition
```
DOMINO_HOSTNAME=<A-DOMINO-INSTANCE>
JWT=<A-JWT>
```

Calling the script
```bash
python3 export_audit_trail.py
```

### Example 3: Using API key for authentication and specifying an event
```bash
python3 export_audit_trail.py --hostname https://domino_instance.com --api-key my-api-key --event "Create Dataset"
```

### Example 4: Providing parameters to filter the export data
```bash
python3 export_audit_trail.py --hostname https://domino_instance.com --jwt my-jwt-token --user_name "Alice" --event "Create Dataset"
```

### Example 5: Providing start and end timestamps
```bash
python3 export_audit_trail.py --hostname https://domino_instance.com --jwt my-jwt-token --start_date "2024-10-18 14:10:04" --end_date "2024-10-22 09:04:33"
```

### Notes
* The script runs on Python 3 (Python 2 is not supported)
* The hostname and authentication (jwt or api-key) can be defined in an .env file instead of being sent as arguments
* If no hostname is defined, the script will display an error and exit.
* If no authentication mechanism (jwt or api-key) is provided, the script will display an error and exit.
* You can combine optional parameters to customize the behavior and context of the export action.
* Events are exported in time descending order
* The Date & Time of the events is displayed in UTC
