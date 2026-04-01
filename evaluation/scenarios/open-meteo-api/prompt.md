# Scenario: Open-Meteo API

Apply the Adaptive Tool Discovery (ATD) skill to map the capabilities
of the Open-Meteo weather API.

The API documentation and OpenAPI spec are available at:
<https://open-meteo.com/en/docs>

The API requires no authentication.

Produce a tool registry at `tools/open-meteo.toml` covering:

- the forecast endpoint
- key input parameters (latitude, longitude, hourly, daily)
- response structure
- known constraints

Do not map every parameter exhaustively.
Map what is sufficient for an agent to make a correct forecast request.

After completing the registry:

1. Create or update `tools/manifest.toml` to include `tools/open-meteo.toml`
2. Copy `tools/open-meteo.toml` to `evaluation/scenarios/open-meteo-api/tools/`
3. Write a summary of the ATD run to `evaluation/scenarios/open-meteo-api/response.txt`
4. Score the output against `evaluation/rubric.md` and include the score table
   in `response.txt`
