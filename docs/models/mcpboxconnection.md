# McpboxConnection


## Fields

| Field                                                                   | Type                                                                    | Required                                                                | Description                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| `endpoint`                                                              | *str*                                                                   | :heavy_check_mark:                                                      | Base URL of the MCP Box; its MCP endpoint is this URL followed by /mcp. |
| `token`                                                                 | *str*                                                                   | :heavy_check_mark:                                                      | The MCP Box's token. Send it in X-Continuous-Token on each request.     |