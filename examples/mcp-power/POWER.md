---
name: "weather-lookup"
displayName: "Weather Lookup"
description: "Get current weather and forecasts for any location using the Open-Meteo API"
keywords: ["weather", "forecast", "temperature", "climate", "meteorology"]
author: "Example Author"
mcpServers: ["weather-api"]
---

# Weather Lookup

## Overview

Weather Lookup provides real-time weather data and forecasts through the Open-Meteo API. Use this power to check current conditions, get forecasts, and analyze weather patterns for any location worldwide.

## Available MCP Servers

### weather-api

The weather-api server provides tools for fetching weather data.

#### Tools

| Tool | Description |
|------|-------------|
| `get_current_weather` | Get current weather conditions for a location |
| `get_forecast` | Get weather forecast for upcoming days |
| `get_historical` | Get historical weather data for a date range |

#### get_current_weather

Fetches current weather conditions including temperature, humidity, wind speed, and conditions.

**Parameters:**
- `latitude` (number, required): Latitude of the location
- `longitude` (number, required): Longitude of the location
- `units` (string, optional): "metric" or "imperial" (default: "metric")

**Example:**
```
Get the current weather for Seattle (47.6062, -122.3321)
```

#### get_forecast

Fetches weather forecast for the specified number of days.

**Parameters:**
- `latitude` (number, required): Latitude of the location
- `longitude` (number, required): Longitude of the location
- `days` (number, optional): Number of forecast days, 1-16 (default: 7)
- `units` (string, optional): "metric" or "imperial" (default: "metric")

**Example:**
```
What's the 5-day forecast for New York City?
```

#### get_historical

Fetches historical weather data for analysis.

**Parameters:**
- `latitude` (number, required): Latitude of the location
- `longitude` (number, required): Longitude of the location
- `start_date` (string, required): Start date in YYYY-MM-DD format
- `end_date` (string, required): End date in YYYY-MM-DD format

**Example:**
```
What was the weather like in London last week?
```

## Setup

This power uses the Open-Meteo API which is free and requires no API key. The MCP server is installed automatically via uvx.

## Common Workflows

### Check Current Weather
Ask: "What's the weather like in [city]?"

### Get a Forecast
Ask: "What's the forecast for [city] this week?"

### Compare Weather
Ask: "Compare the weather in [city1] and [city2]"

### Plan for Weather
Ask: "Will it rain in [city] tomorrow?"

## Best Practices

- Provide specific locations for accurate results
- Specify units preference if not using metric
- For historical data, keep date ranges reasonable (under 30 days)
- Use city names or coordinates - the server handles geocoding
