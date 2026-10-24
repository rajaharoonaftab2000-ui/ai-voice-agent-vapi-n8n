# Flower Bouquet AI Voice Agent — Vapi, n8n & MCP

An AI voice ordering assistant for a flower business, connected to n8n workflows through MCP. The system provides tools to search bouquet products in Supabase, create orders in Google Sheets, and update existing orders.

## Overview

This project demonstrates how a voice assistant can connect customer conversations with product lookup and order management.

Vapi handles the voice conversation. The n8n MCP server exposes three workflow tools that the assistant can use to retrieve product information, create an order, or update an existing order.

Supabase stores the product catalog, while Google Sheets stores the order records.

## What It Does

- Provides a voice interface for bouquet inquiries and order requests.
- Searches product information stored in Supabase.
- Creates order records in Google Sheets.
- Generates the next order ID using existing sheet records.
- Finds an existing order by its order ID.
- Applies provided changes to an existing order.
- Preserves existing order values when no replacement is provided.
- Returns structured results for the assistant to use in its response.

The voice assistant’s wording and conversation behavior depend on its Vapi configuration.

## Architecture

**Customer → Vapi Voice Assistant → n8n MCP Server → Product Search / Create Order / Update Order → Tool Response**

The MCP server connects the assistant to three separate tool workflows.

## Included Workflows

| File | Purpose |
| --- | --- |
| Voice Calling Agent (MCP).json | Exposes the three workflow tools through an MCP Server Trigger |
| Supabase Data Fetching Tool.json | Searches the product catalog in Supabase and formats the response |
| Create Order Tool.json | Generates an order ID and appends an order record to Google Sheets |
| Update Order Tool.json | Finds an order by ID and updates the supplied fields in Google Sheets |

There are four workflow files in total: one MCP server workflow and three connected tool workflows.

## Connected Tools

### 1. Product Search

**Tool name:** `supabase_data_fetching`

This tool calls the Supabase Data Fetching workflow to search the configured product table.

The workflow retrieves matching product data and formats the result for the voice assistant.

Product information depends on the fields and records available in the connected Supabase table.

### 2. Create Order

**Tool name:** `create_order`

This tool calls the Create Order workflow.

The workflow:

1. Receives the order inputs.
2. Reads existing order IDs from Google Sheets.
3. Computes the next order ID.
4. Maps the order information to the configured sheet columns.
5. Appends a new order record.
6. Returns a confirmation response.

### 3. Update Order

**Tool name:** `update_order`

This tool calls the Update Order workflow.

The workflow:

1. Receives an order ID and the requested updates.
2. Looks up the matching order in Google Sheets.
3. Combines the existing record with the update inputs.
4. Applies only the provided changes.
5. Updates the order row.
6. Returns an update confirmation response.

## Example Customer Journey

### Product Inquiry

A customer asks about a bouquet. The assistant uses the product search tool to retrieve information from the connected catalog.

### New Order

The assistant collects the information required by the configured order workflow and confirms the details with the customer.

It then calls the create-order tool. The workflow generates an order ID, saves the record, and returns the result.

### Order Changes

A customer provides an existing order ID and requests a change.

The assistant calls the update-order tool with the order ID and the fields to change. The workflow retrieves the existing record and applies the supplied updates.

## Technologies Used

| Technology | Purpose |
| --- | --- |
| Vapi | Voice conversations and assistant tool calls |
| n8n | MCP server and order automation workflows |
| MCP | Access to the three connected workflow tools |
| Supabase | Bouquet product catalog lookup |
| Google Sheets | Order creation, order IDs, and record updates |
| JavaScript | Order data processing and update logic |

## Setup Requirements

- A running n8n instance with the required nodes.
- A configured Vapi voice assistant.
- An MCP connection supported by the assistant setup.
- A Supabase project containing the product table.
- A Google spreadsheet containing the order columns.
- Supabase and Google Sheets credentials configured in n8n.
- An accessible MCP endpoint.
- Business information and order-handling instructions for the assistant.

## Configuration

### 1. Import the Tool Workflows

Import these three files into n8n:

- `Supabase Data Fetching Tool.json`
- `Create Order Tool.json`
- `Update Order Tool.json`

### 2. Configure Supabase

Connect your own Supabase credentials.

Select the product table and check that its fields match the search filters and response mapping used by the workflow.

### 3. Configure Google Sheets

Connect your own Google Sheets credentials.

Replace `YOUR_GOOGLE_SHEET_ID` with your spreadsheet ID and select the correct sheet.

Check that the spreadsheet headers match the mappings used by both order workflows.

### 4. Import the MCP Workflow

Import `Voice Calling Agent (MCP).json`.

In each workflow tool node, select the corresponding imported workflow:

- `supabase_data_fetching` → Supabase Data Fetching Tool
- `create_order` → Create Order Tool
- `update_order` → Update Order Tool

The public exports contain placeholders for the original workflow references.

### 5. Configure Vapi

Configure the assistant’s greeting, business information, and conversation instructions.

Connect the assistant to the MCP endpoint and explain when it should use each tool.

The assistant should collect the required inputs and confirm order details before submitting a create or update request.

### 6. Test the Full Process

Enable the required workflows, test the tool calls, and verify the resulting product responses and order records.

## Suggested Test Cases

- Search for a product that exists.
- Search for a product with no matching record.
- Create a new order with all required information.
- Verify that the order receives an ID.
- Verify that the order appears in Google Sheets.
- Update an existing order using its ID.
- Update one field and check that other values remain unchanged.
- Request an update for an unknown order ID.
- Provide incomplete order information.
- Test the assistant’s response when a connected service fails.

## Business Value

The project demonstrates how a flower business can connect voice inquiries with product information and order records.

Separating product search, order creation, and order updates into individual workflows makes each function easier to configure and maintain.

## Customization

The solution can be adapted to the business’s:

- Bouquet catalog and product fields.
- Order information requirements.
- Spreadsheet structure.
- Voice and supported language.
- Greeting and conversation instructions.
- Delivery information and business rules.
- Additional integrations.

Changes require corresponding updates to the assistant instructions, workflow inputs, and data mappings.

## Important Notes

- The uploaded JSON files contain the n8n workflows. The Vapi assistant must be configured separately.
- Product search depends on the connected Supabase catalog.
- Orders are stored in Google Sheets.
- Order ID generation uses existing sheet records; simultaneous orders require additional duplicate-ID protection before production use.
- The assistant should confirm success only after the relevant tool reports a successful result.
- Missing products, unknown order IDs, and service failures should be tested before deployment.
- n8n and the MCP endpoint must remain accessible while the assistant is in use.
- Calling, hosting, and AI services may have separate usage charges.
- Configure your own credentials and resource IDs before running the workflows.

## Project Status

Portfolio demonstration of a flower bouquet voice ordering system with one MCP server workflow and three connected tools for product search, order creation, and order updates.


## Author

**Haroon AI Automation**

Practical AI assistants and n8n workflows for business automation.

## Demo Video

[Watch the AI Voice Receptionist Demo](./Flower-Bouquet-Voice-Agent-Clean-Call-Captions.mp4)
