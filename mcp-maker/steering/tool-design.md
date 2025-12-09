# Designing Effective Tools for Agents

Best practices for building MCP tools that agents can use effectively. Tools for agents require different design principles than traditional APIs—they must be optimized for non-deterministic systems with limited context windows.

## Core Philosophy

Tools should match how agents naturally think and work, not how traditional software systems operate. Design for the agent's workflow, not just API coverage.

## Tool Selection Strategy

### Build Fewer, High-Impact Tools

Instead of wrapping every API endpoint, build consolidated tools targeting specific workflows.

```python
# ❌ Bad: Too many granular tools
@mcp.tool()
def list_users() -> list[dict]: ...

@mcp.tool()
def list_events() -> list[dict]: ...

@mcp.tool()
def create_event(title: str, user_id: str) -> dict: ...

# ✅ Good: Consolidated workflow tool
@mcp.tool()
def schedule_event(
    title: str,
    attendee_names: list[str],
    date: str,
    duration_minutes: int = 60
) -> str:
    """Schedule an event with attendees.

    Finds users by name, checks availability, and creates the event.

    Args:
        title: Event title
        attendee_names: Names of people to invite (searches by name)
        date: Date in YYYY-MM-DD format
        duration_minutes: Event duration (default: 60)
    """
    # Implementation handles user lookup internally
```

### Avoid Brute-Force Tools

Don't force agents to search through everything—provide targeted search.

```python
# ❌ Bad: Returns all contacts, wastes context
@mcp.tool()
def list_contacts() -> list[dict]:
    """List all contacts."""
    return db.get_all_contacts()  # Could be thousands

# ✅ Good: Returns only relevant matches
@mcp.tool()
def search_contacts(
    query: str,
    limit: int = 10
) -> list[dict]:
    """Search contacts by name, email, or company.

    Args:
        query: Search term (matches name, email, company)
        limit: Maximum results to return (default: 10)
    """
    return db.search_contacts(query, limit=limit)
```

## Namespacing and Naming

Agents access hundreds of tools from multiple servers. Clear naming prevents confusion.

### Naming Patterns

```python
# By service prefix (recommended)
@mcp.tool()
def github_search_repos(query: str) -> list[dict]: ...

@mcp.tool()
def github_create_issue(repo: str, title: str, body: str) -> dict: ...

# By resource
@mcp.tool()
def asana_projects_search(query: str) -> list[dict]: ...

@mcp.tool()
def asana_tasks_create(project_id: str, title: str) -> dict: ...
```

### Parameter Naming

Use unambiguous, descriptive parameter names.

```python
# ❌ Bad: Ambiguous parameters
@mcp.tool()
def search(user: str, symbol: str, period: int): ...

# ✅ Good: Clear, specific parameters
@mcp.tool()
def search_trades(
    user_id: str,
    ticker_symbol: str,
    days_back: int
) -> list[dict]:
    """Search trading history.

    Args:
        user_id: The user's unique identifier
        ticker_symbol: Stock ticker (e.g., 'AAPL', 'MSFT')
        days_back: Number of days to search back
    """
```

## Response Design

### Return High-Signal Information

Prioritize semantically meaningful data over technical identifiers.

```python
# ❌ Bad: Low-signal response
@mcp.tool()
def get_document(doc_id: str) -> dict:
    return {
        "uuid": "550e8400-e29b-41d4-a716-446655440000",
        "mime_type": "application/pdf",
        "256px_image_url": "https://...",
        "created_ts": 1699574400
    }

# ✅ Good: High-signal response
@mcp.tool()
def get_document(doc_id: str) -> dict:
    return {
        "title": "Q3 Financial Report",
        "description": "Quarterly earnings summary for 2024",
        "author": "Finance Team",
        "created": "2024-11-10",
        "status": "published",
        "doc_id": doc_id  # Include ID only if needed for downstream calls
    }
```

### Response Format Control

Add flexibility for token efficiency.

```python
from typing import Literal

@mcp.tool()
def get_customer_info(
    customer_id: str,
    format: Literal["concise", "detailed"] = "concise"
) -> str:
    """Get customer information.

    Args:
        customer_id: Customer identifier
        format: Response detail level
            - "concise": Essential info only (~70 tokens)
            - "detailed": Full information (~200 tokens)
    """
    customer = db.get_customer(customer_id)

    if format == "concise":
        return f"Customer: {customer.name}, Status: {customer.status}, Balance: ${customer.balance}"
    else:
        return f"""Customer: {customer.name}
Email: {customer.email}
Status: {customer.status}
Balance: ${customer.balance}
Account opened: {customer.created_date}
Recent orders: {len(customer.recent_orders)}
Notes: {customer.notes}"""
```

## Token Efficiency

Context is precious. Optimize for token efficiency without sacrificing utility.

### Implement Pagination and Filtering

```python
@mcp.tool()
def search_logs(
    query: str,
    start_date: str | None = None,
    end_date: str | None = None,
    severity: Literal["info", "warn", "error"] | None = None,
    limit: int = 25,
    offset: int = 0
) -> dict:
    """Search application logs.

    Args:
        query: Search term
        start_date: Filter from date (YYYY-MM-DD)
        end_date: Filter to date (YYYY-MM-DD)
        severity: Filter by log level
        limit: Max results per page (default: 25, max: 100)
        offset: Pagination offset

    Returns:
        Dict with 'results', 'total_count', 'has_more'
    """
    results = db.search_logs(query, start_date, end_date, severity, limit, offset)
    return {
        "results": results[:limit],
        "total_count": len(results),
        "has_more": len(results) > limit,
        "next_offset": offset + limit if len(results) > limit else None
    }
```

### Truncation with Clear Indicators

```python
MAX_RESPONSE_TOKENS = 25000  # Claude Code default

@mcp.tool()
def get_file_content(path: str, max_lines: int = 500) -> str:
    """Read file content.

    Args:
        path: File path
        max_lines: Maximum lines to return (default: 500)
    """
    lines = read_file(path)

    if len(lines) > max_lines:
        truncated = lines[:max_lines]
        return "\n".join(truncated) + f"\n\n[TRUNCATED: Showing {max_lines} of {len(lines)} lines. Use max_lines parameter for more.]"

    return "\n".join(lines)
```

## Error Messages

Provide actionable guidance, not opaque errors.

```python
@mcp.tool()
def search_database(query: str, table: str) -> list[dict]:
    """Search database table.

    Args:
        query: Search query
        table: Table name (users, orders, products)
    """
    valid_tables = ["users", "orders", "products"]

    # ❌ Bad error
    # if table not in valid_tables:
    #     raise ValueError("Invalid table")

    # ✅ Good error
    if table not in valid_tables:
        raise ValueError(
            f"Invalid table '{table}'. "
            f"Valid tables are: {', '.join(valid_tables)}. "
            f"Example: search_database(query='john', table='users')"
        )

    results = db.search(table, query)

    if len(results) > 1000:
        raise ValueError(
            f"Query returned too many results ({len(results)}). "
            "Please narrow your search by adding more specific terms or filters."
        )

    return results
```

## Tool Descriptions

Write descriptions as if onboarding a new team member. Make implicit knowledge explicit.

```python
@mcp.tool()
def query_metrics(
    metric_name: str,
    aggregation: Literal["sum", "avg", "min", "max", "count"] = "avg",
    interval: Literal["1m", "5m", "1h", "1d"] = "1h",
    start_time: str | None = None,
    end_time: str | None = None
) -> dict:
    """Query time-series metrics from the monitoring system.

    Retrieves aggregated metric data for analysis. Metrics are stored with
    1-minute granularity and can be aggregated over larger intervals.

    Args:
        metric_name: Metric identifier. Common metrics:
            - "cpu_usage": CPU utilization percentage (0-100)
            - "memory_mb": Memory usage in megabytes
            - "request_count": HTTP requests per interval
            - "error_rate": Percentage of failed requests
            - "latency_p99": 99th percentile response time in ms
        aggregation: How to combine data points within each interval
        interval: Time bucket size for aggregation
        start_time: ISO 8601 timestamp (default: 24 hours ago)
        end_time: ISO 8601 timestamp (default: now)

    Returns:
        Dict with 'metric', 'interval', 'data_points' (list of {timestamp, value})

    Example:
        query_metrics("cpu_usage", aggregation="avg", interval="1h")
        # Returns hourly average CPU usage for the last 24 hours
    """
```

## Quick Reference

| Do | Don't |
|----|-------|
| `search_contacts(query)` | `list_contacts()` (brute-force) |
| `schedule_event(...)` consolidated | Separate `list_users`, `list_events`, `create_event` |
| Return `name`, `title`, `status` | Return `uuid`, `mime_type`, `256px_url` |
| Use `user_id` parameter name | Use ambiguous `user` parameter |
| Provide actionable error messages | Return opaque error codes |
| Add `format` parameter for response control | Always return full details |
| Implement pagination with `limit`/`offset` | Return unbounded result sets |
| Namespace tools: `github_create_issue` | Generic names: `create_issue` |

## Development Process

1. **Prototype**: Build basic functionality, test manually with MCP Inspector
2. **Evaluate**: Create realistic tasks requiring multiple tool calls
3. **Measure**: Track accuracy, token usage, error rates
4. **Iterate**: Refine descriptions and responses based on evaluation
5. **Test**: Use held-out test sets to validate improvements

Small refinements to tool descriptions can yield dramatic performance improvements. Evaluation-driven optimization is highly effective.
