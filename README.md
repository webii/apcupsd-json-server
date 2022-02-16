# apcupsd-json-server

A simple JSON-spewing HTTP endpoint for apcupsd. Supports python3.

## Setup

Run `python apcupsd-json`, then open <http://localhost:8008>.

### Options

- `-l` (listen): Address to listen to HTTP requests on. Default is `0.0.0.0`.
- `-p` (port): Port to listen to HTTP requests on. Default is 8008.
- `--cors [ip]`: Add a `Access-Control-Allow-Origin`) header to every response. Default is off.
  - Specify an IP after the flag to set an allowed origin, or `*` to allow any origin.

You can specify the ports to watch for `apcupsd` instances (e.g., if you have multiple UPSes) after the command, e.g. `apcupsd-json 3551 3552`.

Ports given without a hostname are assumed to be local. You can also specify ports on another host, e.g. `apcupsd-json 192.168.0.1:3551 192.168.0.2:3551`.

### Instalation as a service

There is an exemplary service configuration, so apcupsd-json will start with the system.

Copying service definition:

```
cp apcupsd-json.service /lib/systemd/system/apcupsd-json.service
```

Enabling and starting service:
```
sudo systemctl --system daemon-reload
sudo systemctl enable apcupsd-json.service
sudo systemctl start apcupsd-json.service
```

## Usage

The only endpoint is `/`, and it returns data that looks like this:

```json
{
	"status": "👍",
	"upses": [
		{
			(ups data)
		},
		{
			(another, if watching more than one ups)
		}
	]
}
```

UPS data is an object of keys and strings that can be anything that APCUPSD provides, most commonly:

- upsname
- status
- bcharge
- timeleft
- loadpct
- battv
- linev

... and plenty of other fields.

Any other endpoints return a 404 with an error message:

```json
{
	"error": "not a valid endpoint"
}
```

# Author

- [Alex Lindeman](aelindeman)

[aelindeman]: https://github.com/aelindeman
