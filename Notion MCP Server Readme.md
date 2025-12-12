Notion

Install
Refresh
Official Notion MCP Server that allows interaction with Notion workspaces, pages, databases, and comments via the Notion API.
About
Configure
Tools
Notion MCP Server
[!NOTE]

We’ve introduced Notion MCP, a remote MCP server with the following improvements:

Easy installation via standard OAuth. No need to fiddle with JSON or API token anymore.
Powerful tools tailored to AI agents. These tools are designed with optimized token consumption in mind.
Learn more and try it out here

notion-mcp-sm

This project implements an MCP server for the Notion API.

mcp-demo

Installation
1. Setting up Integration in Notion:
Go to https://www.notion.so/profile/integrations and create a new internal integration or select an existing one.

Creating a Notion Integration token

While we limit the scope of Notion API's exposed (for example, you will not be able to delete databases via MCP), there is a non-zero risk to workspace data by exposing it to LLMs. Security-conscious users may want to further configure the Integration's Capabilities.

For example, you can create a read-only integration token by giving only "Read content" access from the "Configuration" tab:

Notion Integration Token Capabilities showing Read content checked

2. Connecting content to integration:
Ensure relevant pages and databases are connected to your integration.

To do this, visit the Access tab in your internal integration settings. Edit access and select the pages you'd like to use. Integration Access tab

Edit integration access

Alternatively, you can grant page access individually. You'll need to visit the target page, and click on the 3 dots, and select "Connect to integration".

Adding Integration Token to Notion Connections

3. Adding MCP config to your client:
Using npm:
Cursor & Claude:

Add the following to your .cursor/mcp.json or claude_desktop_config.json (MacOS: ~/Library/Application\ Support/Claude/claude_desktop_config.json)

Option 1: Using NOTION_TOKEN (recommended)

{ "mcpServers": { "notionApi": { "command": "npx", "args": ["-y", "@notionhq/notion-mcp-server"], "env": { "NOTION_TOKEN": "ntn_****" } } } }
Option 2: Using OPENAPI_MCP_HEADERS (for advanced use cases)

{ "mcpServers": { "notionApi": { "command": "npx", "args": ["-y", "@notionhq/notion-mcp-server"], "env": { "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ntn_****\", \"Notion-Version\": \"2022-06-28\" }" } } } }
Zed

Add the following to your settings.json

{ "context_servers": { "some-context-server": { "command": { "path": "npx", "args": ["-y", "@notionhq/notion-mcp-server"], "env": { "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ntn_****\", \"Notion-Version\": \"2022-06-28\" }" } }, "settings": {} } } }
Using Docker:
There are two options for running the MCP server with Docker:

Option 1: Using the official Docker Hub image:
Add the following to your .cursor/mcp.json or claude_desktop_config.json:

Using NOTION_TOKEN (recommended):

{ "mcpServers": { "notionApi": { "command": "docker", "args": [ "run", "--rm", "-i", "-e", "NOTION_TOKEN", "mcp/notion" ], "env": { "NOTION_TOKEN": "ntn_****" } } } }
Using OPENAPI_MCP_HEADERS (for advanced use cases):

{ "mcpServers": { "notionApi": { "command": "docker", "args": [ "run", "--rm", "-i", "-e", "OPENAPI_MCP_HEADERS", "mcp/notion" ], "env": { "OPENAPI_MCP_HEADERS": "{\"Authorization\":\"Bearer ntn_****\",\"Notion-Version\":\"2022-06-28\"}" } } } }
This approach:

Uses the official Docker Hub image
Properly handles JSON escaping via environment variables
Provides a more reliable configuration method
Option 2: Building the Docker image locally:
You can also build and run the Docker image locally. First, build the Docker image:

docker compose build
Then, add the following to your .cursor/mcp.json or claude_desktop_config.json:

Using NOTION_TOKEN (recommended):

{ "mcpServers": { "notionApi": { "command": "docker", "args": [ "run", "--rm", "-i", "-e", "NOTION_TOKEN=ntn_****", "notion-mcp-server" ] } } }
Using OPENAPI_MCP_HEADERS (for advanced use cases):

{ "mcpServers": { "notionApi": { "command": "docker", "args": [ "run", "--rm", "-i", "-e", "OPENAPI_MCP_HEADERS={\"Authorization\": \"Bearer ntn_****\", \"Notion-Version\": \"2022-06-28\"}", "notion-mcp-server" ] } } }
Don't forget to replace ntn_**** with your integration secret. Find it from your integration configuration tab:

Copying your Integration token from the Configuration tab in the developer portal

Transport Options
The Notion MCP Server supports two transport modes:

STDIO Transport (Default)
The default transport mode uses standard input/output for communication. This is the standard MCP transport used by most clients like Claude Desktop.

# Run with default stdio transport npx @notionhq/notion-mcp-server # Or explicitly specify stdio npx @notionhq/notion-mcp-server --transport stdio
Streamable HTTP Transport
For web-based applications or clients that prefer HTTP communication, you can use the Streamable HTTP transport:

# Run with Streamable HTTP transport on port 3000 (default) npx @notionhq/notion-mcp-server --transport http # Run on a custom port npx @notionhq/notion-mcp-server --transport http --port 8080 # Run with a custom authentication token npx @notionhq/notion-mcp-server --transport http --auth-token "your-secret-token"
When using Streamable HTTP transport, the server will be available at http://0.0.0.0:<port>/mcp.

Authentication
The Streamable HTTP transport requires bearer token authentication for security. You have three options:

Option 1: Auto-generated token (recommended for development)

npx @notionhq/notion-mcp-server --transport http
The server will generate a secure random token and display it in the console:

Generated auth token: a1b2c3d4e5f6789abcdef0123456789abcdef0123456789abcdef0123456789ab Use this token in the Authorization header: Bearer a1b2c3d4e5f6789abcdef0123456789abcdef0123456789abcdef0123456789ab
Option 2: Custom token via command line (recommended for production)

npx @notionhq/notion-mcp-server --transport http --auth-token "your-secret-token"
Option 3: Custom token via environment variable (recommended for production)

AUTH_TOKEN="your-secret-token" npx @notionhq/notion-mcp-server --transport http
The command line argument --auth-token takes precedence over the AUTH_TOKEN environment variable if both are provided.

Making HTTP Requests
All requests to the Streamable HTTP transport must include the bearer token in the Authorization header:

# Example request curl -H "Authorization: Bearer your-token-here" \ -H "Content-Type: application/json" \ -H "mcp-session-id: your-session-id" \ -d '{"jsonrpc": "2.0", "method": "initialize", "params": {}, "id": 1}' \ http://localhost:3000/mcp
Note: Make sure to set either the NOTION_TOKEN environment variable (recommended) or the OPENAPI_MCP_HEADERS environment variable with your Notion integration token when using either transport mode.

Examples
Using the following instruction
Comment "Hello MCP" on page "Getting started"
AI will correctly plan two API calls, v1/search and v1/comments, to achieve the task

Similarly, the following instruction will result in a new page named "Notion MCP" added to parent page "Development"
Add a page titled "Notion MCP" to page "Development"
You may also reference content ID directly
Get the content of page 1a6b35e6e67f802fa7e1d27686f017f2
Development
Build

npm run build
Execute

npx -y --prefix /path/to/local/notion-mcp-server @notionhq/notion-mcp-server
Publish

npm publish --access public

The following image can be moved on the page using keyboard controls (left, right, up, down)
Jump to Content
Notion API
View my integrations
Home
Guides
API Reference
Changelog
Examples

Search
CTRL-K
JUMP TO
CTRL-/
Notion API
Introduction
Integration capabilities
Webhooks

Request limits
Status codes
Versioning

Objects
Block

Page

Database
Data source

Comment

File

User
Parent
Emoji
Unfurl attribute (Link Previews)
Endpoints
Authentication

Blocks

Pages

Databases

Data sources

Databases (deprecated)


Comments

File Uploads

Search

Users

Powered by 

Introduction
The reference is your key to a comprehensive understanding of the Notion API.

Integrations use the API to access Notion's pages, databases, and users. Integrations can connect services to Notion and build interactive experiences for users within Notion. Using the navigation on the left, you'll find details for objects and endpoints used in the API.

📘
You need an integration token to interact with the Notion API. You can find an integration token after you create an integration on the integration settings page. If this is your first look at the Notion API, we recommend beginning with the Getting started guide to learn how to create an integration.

If you want to work on a specific integration, but can't access the token, confirm that you are an admin in the associated workspace. You can check inside the Notion UI via Settings & Members in the left sidebar. If you're not an admin in any of your workspaces, you can create a personal workspace for free.

Conventions
The base URL to send all API requests is https://api.notion.com. HTTPS is required for all API requests.

The Notion API follows RESTful conventions when possible, with most operations performed via GET, POST, PATCH, and DELETE requests on page and database resources. Request and response bodies are encoded as JSON.

JSON conventions
Top-level resources have an "object" property. This property can be used to determine the type of the resource (e.g. "database", "user", etc.)
Top-level resources are addressable by a UUIDv4 "id" property. You may omit dashes from the ID when making requests to the API, e.g. when copying the ID from a Notion URL.
Property names are in snake_case (not camelCase or kebab-case).
Temporal values (dates and datetimes) are encoded in ISO 8601 strings. Datetimes will include the time value (2020-08-12T02:12:33.231Z) while dates will include only the date (2020-08-12)
The Notion API does not support empty strings. To unset a string value for properties like a url Property value object, for example, use an explicit null instead of "".
Code samples & SDKs
Samples requests and responses are shown for each endpoint. Requests are shown using the Notion JavaScript SDK, and cURL. These samples make it easy to copy, paste, and modify as you build your integration.

Notion SDKs are open source projects that you can install to easily start building. You may also choose any other language or library that allows you to make HTTP requests.

Pagination
Endpoints that return lists of objects support cursor-based pagination requests. By default, Notion returns ten items per API call. If the number of items in a response from a support endpoint exceeds the default, then an integration can use pagination to request a specific set of the results and/or to limit the number of returned items.

Supported endpoints
HTTP method	Endpoint
GET	List all users
GET	Retrieve block children
GET	Retrieve a comment
GET	Retrieve a page property item
POST	Query a database
POST	Search
Responses
If an endpoint supports pagination, then the response object contains the below fields.

Field	Type	Description
has_more	boolean	Whether the response includes the end of the list. false if there are no more results. Otherwise, true.
next_cursor	string	A string that can be used to retrieve the next page of results by passing the value as the start_cursor parameter to the same endpoint.

Only available when has_more is true.
object	"list"	The constant string "list".
results	array of objects	The list, or partial list, of endpoint-specific results. Refer to a supported endpoint's individual documentation for details.
type	"block"

"comment"

"database"

"page"

"page_or_database"

"property_item"

"user"	A constant string that represents the type of the objects in results.
{type}	paginated list object	An object containing type-specific pagination information. For property_items, the value corresponds to the paginated page property type. For all other types, the value is an empty object.
Parameters for paginated requests
🚧
Parameter location varies by endpoint

GET requests accept parameters in the query string.

POST requests receive parameters in the request body.

Parameter	Type	Description
page_size	number	The number of items from the full list to include in the response.

Default: 100
Maximum: 100

The response may contain fewer than the default number of results.
start_cursor	string	A next_cursor value returned in a previous response. Treat this as an opaque value.

Defaults to undefined, which returns results from the beginning of the list.
How to send a paginated request
Send an initial request to the supported endpoint.
Retrieve the next_cursor value from the response (only available when has_more is true).
Send a follow up request to the endpoint that includes the next_cursor param in either the query string (for GET requests) or in the body params (POST requests).
Example: request the next set of query results from a database
cURL

curl --location --request POST 'https://api.notion.com/v1/databases/<database_id>/query' \
--header 'Authorization: Bearer <secret_bot>' \
--header 'Content-Type: application/json' \
--data '{
    "start_cursor": "33e19cb9-751f-4993-b74d-234d67d0d534"
}'
Updated over 2 years ago

Integration capabilities
Did this page help you?
Table of Contents
Conventions
JSON conventions
Code samples & SDKs
Pagination
Supported endpoints
Responses
Parameters for paginated requests
How to send a paginated request
The all-in-one workspace for your notes, tasks, wikis, and databases.
@NotionAPI
©2025 Notion Labs, Inc.
Terms & Privacy
Developers
Guides
API reference
My integrations
Developer terms
Notion
Product
Teams
Enterprise
Blog
Careers
Community
Slack
Stack Overflow
Twitter

Ask