## MCP configuration
```json
{
	"servers": {
		"playwright": {
			"command": "npx",
			"args": [
				"@playwright/mcp@latest",
				"--caps=vision",
				"--browser=msedge"
			],
			"type": "stdio"
		},
		"context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    },
		"mslearn": {
			"type": "http",
			"url": "https://learn.microsoft.com/api/mcp"
		},
		"exa": {
      "type":"http",
			"url": "https://mcp.exa.ai/mcp"
    }
	},
	"inputs": []
}
```
