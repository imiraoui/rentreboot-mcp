# RentReboot MCP: apartment alerts, renter guides and NYC rent data

[RentReboot](https://rentreboot.com) connects AI assistants to public NYC renter guides and neighborhood asking-rent data. With the renter's scoped OAuth consent, assistants can also read their apartment alerts, manage their saved search, save or track alerted apartments, and view Housing Lottery application statuses.

**Hosted MCP endpoint:** `https://rentreboot.com/api/mcp`  
**Transport:** Streamable HTTP  
**Authentication:** No account for public tools. OAuth 2.1 with PKCE for personal tools.

This is the public setup and discovery repository for RentReboot's hosted MCP service. It contains installation metadata and documentation, not the production server implementation. Nothing needs to be deployed or installed from this repository.

## Connect

Add the remote server URL in an MCP client that supports Streamable HTTP:

```json
{
  "mcpServers": {
    "rentreboot": {
      "url": "https://rentreboot.com/api/mcp"
    }
  }
}
```

Clients use different configuration formats; use your client's remote-server setup if it does not accept this JSON. For personal features, choose its OAuth connection flow and sign in on `rentreboot.com`. New renters can create a free account through the same verified-email flow. Never paste an email code, private account link or token into a chat or configuration example.

## Public tools

| Tool | Purpose |
| --- | --- |
| `search_guides` | Find published renter guides by keyword, with canonical URLs. |
| `get_guide` | Read a published guide as Markdown. |
| `list_neighborhoods` | Discover NYC neighborhoods with published rent data. |
| `get_rent_data` | Read aggregate asking-rent percentiles and sample sizes by neighborhood and bedroom count. |

Public tools are read-only and do not require an account or API key. Asking-rent percentiles describe aggregate data; they are not individual apartment availability or a guaranteed future rent.

## Personal tools with OAuth

| Tool | Purpose | Scope |
| --- | --- | --- |
| `list_alerted_listings` | Read apartments already sent to the renter, including saved and tracking state. | `alerts:read` |
| `update_alerted_listing` | Save, hide, or mark an alerted apartment contacted, toured or applied. | `alerts:write` |
| `get_account` | Read plan and alert settings. | `searches:read` |
| `get_search_preferences` | Read the renter's saved apartment search. | `searches:read` |
| `find_search_neighborhoods` | Find valid neighborhood names for the renter's market. | `searches:read` |
| `update_search_preferences` | Change the renter's budget, neighborhoods and other search preferences. | `preferences:write` |
| `set_apartment_alerts` | Pause or resume apartment alerts. | `preferences:write` |
| `list_upcoming_matches` | Read historical apartment records matching the renter's saved search. | `searches:read` |
| `list_housing_lottery_applications` | Read provider-confirmed NYC Housing Connect application statuses. | `lottery:read` |

Ask the renter to confirm a change before calling a write tool. Personal tools act only on the connected renter's account. Available tools depend on the client and granted scopes; `tools/list` is authoritative. Some clients may also expose a scoped checkout-link tool, described in [the authentication documentation](https://rentreboot.com/auth.md).

Upcoming matches are historical records useful for planning, not promises of future availability. The service does not contact brokers or submit Housing Connect applications through MCP tools. Account deletion, unsubscribing and paid-plan cancellation remain website workflows. Public MCP tools are available without a paid plan; paid RentReboot alert features follow the account's website plan.

## Example requests

- "Find renter guides about rent stabilization in NYC."
- "Compare published one-bedroom asking-rent percentiles in Chelsea."
- "Show the apartments RentReboot sent me most recently." (OAuth)
- "Save this alerted apartment and mark it toured." (OAuth, confirm first)
- "Change my maximum rent to $3,000." (OAuth, confirm first)
- "Show my Housing Lottery application statuses." (OAuth)

## Test public access

```bash
curl https://rentreboot.com/api/mcp \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  --data '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"example","version":"1.0.0"}}}'
```

## Documentation and useful links

- [RentReboot apartment alerts](https://rentreboot.com)
- [Authentication, scopes and limits](https://rentreboot.com/auth.md)
- [Live MCP server card](https://rentreboot.com/.well-known/mcp/server-card.json)
- [AI discovery index](https://rentreboot.com/llms.txt)
- [NYC neighborhood rent data](https://rentreboot.com/average)
- [Renter guides](https://rentreboot.com/guide)
- [Privacy](https://rentreboot.com/privacy) and [terms](https://rentreboot.com/terms)
- Support: support@rentreboot.com

Tool descriptions above were checked against the public service on October 21, 2026. The live service and its authentication documentation are authoritative for current availability.
