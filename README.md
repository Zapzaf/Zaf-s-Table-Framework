# Zaf's Table Framework

A flexible, reusable data table component for Laravel Blade views. Works with any data source and supports server-side pagination, sorting, filtering, and optional selection checkboxes.

## Table of Contents

- [Features](#features)
- [Getting Started](#getting-started)
- [Configuration Options](#configuration-options)
- [Column Definitions](#column-definitions)
- [Actions Configuration](#actions-configuration)
- [Working with Checkboxes](#working-with-checkboxes)
- [Examples](#examples)
- [API Reference](#api-reference)

---

## Features

✅ **Server-Side Processing** - Pagination, sorting, and filtering
✅ **Flexible Column Rendering** - Custom render functions per column
✅ **Action Buttons** - Icon-only buttons with custom handlers
✅ **Optional Checkboxes** - Select-all + individual row selection
✅ **State Persistence** - Remembers page, sort, search (1-hour TTL)
✅ **Multi-Table Support** - Use multiple tables on the same page
✅ **Multiple Response Formats** - Supports simple REST, DataTables, and custom envelopes
✅ **Responsive Design** - Mobile-friendly with sticky columns
✅ **Search & Suggestions** - Built-in search with dropdown suggestions

---

## Getting Started

### 1. Include the Framework

Add these to your Blade view:

```blade
<link rel="stylesheet" href="{{ asset('css/custom-table.css') }}">
<script src="{{ asset('js/custom-table.js') }}"></script>
```

### 2. Create a Table HTML Structure

```blade
<div class="table-wrapper">
    <table id="myTable" class="table table-striped">
        <thead>
            <tr>
                <th class="sticky-col" data-column="ItemID">ID <span class="sort-icon">⇅</span></th>
                <th data-column="ItemName">Name <span class="sort-icon">⇅</span></th>
                <th data-column="ItemQty">Quantity <span class="sort-icon">⇅</span></th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody id="tableBody"></tbody>
    </table>
</div>
```

### 3. Add UI Controls (Optional)

```blade
<!-- Search -->
<input type="text" id="searchInput" data-table-search placeholder="Search...">

<!-- Per Page Selector -->
<select id="perPageSelect" data-table-perpage>
    <option value="10">10</option>
    <option value="25" selected>25</option>
    <option value="50">50</option>
</select>

<!-- Pagination -->
<ul id="pagination" data-table-pagination class="pagination"></ul>

<!-- Table Info -->
<div id="tableInfo" data-table-info class="text-muted">Showing 0 to 0</div>

<!-- Select All Checkbox (if using checkboxes) -->
<input type="checkbox" id="selectAllRows" />
```

### 4. Initialize the Table

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'myTable',
    enableCheckboxes: false,  // Set to true to enable checkboxes
    columns: [
        { key: 'ItemID', label: 'ID' },
        { key: 'ItemName', label: 'Name' },
        { key: 'ItemQty', label: 'Quantity' }
    ],
    actions: [
        { icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});
```

---

## Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `apiUrl` | string | **required** | API endpoint for data requests |
| `tableId` | string | **required** | HTML ID of the `<table>` element |
| `columns` | array | `[]` | Column definitions (see below) |
| `actions` | array | `[]` | Action button definitions (see below) |
| `enableCheckboxes` | boolean | `false` | Enable row selection checkboxes |
| `primaryKey` | string | `'id'` | Name of unique identifier field in data |
| `method` | string | `'GET'` | HTTP method (GET or POST) |
| `requestFormat` | string | `'simple'` | Request format: `'simple'` or `'datatables'` |
| `containerId` | string | `null` | HTML ID of container (auto-detected if omitted) |
| `stateKey` | string | `tableState_{tableId}` | LocalStorage key for state persistence |
| `perPage` | number | `10` | Default rows per page |
| `defaultSort` | string | `null` | Default sort column |
| `defaultOrder` | string | `'asc'` | Default sort direction: `'asc'` or `'desc'` |

---

## Column Definitions

Each column is an object with the following properties:

```javascript
{
    key: 'fieldName',              // Data field name (required)
    label: 'Display Name',         // Column header text
    sticky: true,                  // Make column sticky on left (default: false, except checkbox)
    sortable: true,                // Enable sorting (default: true)
    searchable: true,              // Include in search (default: true)
    render: function(value, row) { // Custom render function (optional)
        return `<strong>${value}</strong>`;
    }
}
```

### Examples

#### Simple Column
```javascript
{ key: 'ItemID', label: 'Item ID' }
```

#### Column with Custom Rendering
```javascript
{
    key: 'ItemQty',
    label: 'Stock Level',
    render: function(value, row) {
        const badgeClass = value > 10 ? 'success' : value > 5 ? 'warning' : 'danger';
        return `<span class="badge bg-${badgeClass}">${value}</span>`;
    }
}
```

#### Currency Column
```javascript
{
    key: 'ItemPrice',
    label: 'Price',
    render: function(value) {
        return '₱' + parseFloat(value).toLocaleString('en-US', {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2
        });
    }
}
```

#### Date Column
```javascript
{
    key: 'CreatedAt',
    label: 'Created',
    render: function(value) {
        if (!value || value === 'N/A') return '<span class="text-muted">Never</span>';
        return new Date(value).toLocaleDateString('en-US', {
            year: 'numeric',
            month: 'short',
            day: 'numeric'
        });
    }
}
```

---

## Actions Configuration

Action buttons appear in the last column. Each action is an object:

```javascript
{
    label: 'Edit',                    // Button text (for tooltip)
    icon: 'edit-2',                   // Feather icon name
    class: 'btn-primary',             // Button color class
    title: 'Edit this item',          // Tooltip text
    onclick: 'editItem',              // Function name or handler reference
}
```

### Button Color Classes

- `btn-primary` - Blue
- `btn-success` - Green
- `btn-warning` - Yellow
- `btn-danger` - Red
- `btn-info` - Cyan
- `btn-secondary` - Gray

### Handling Clicks

#### Global Function (Recommended)
```javascript
// Define function globally
function viewItem(id) {
    window.location.href = `/items/${id}`;
}

// Reference by name in actions
{ onclick: 'viewItem' }
```

#### Inline Function
```javascript
{ 
    onclick: function(id, row) {
        console.log('Row data:', row);
        fetch(`/items/${id}`).then(...)
    }
}
```

---

## Working with Checkboxes

### Enabling Checkboxes

Set `enableCheckboxes: true` when initializing:

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'myTable',
    enableCheckboxes: true,  // ← Enable checkboxes
    columns: [...]
});
```

### Getting Selected Items

```javascript
// Get all checked checkboxes
const checked = Array.from(document.querySelectorAll('#myTable .select-row:checked'));

// Extract IDs
const ids = checked.map(cb => cb.getAttribute('data-id'));

// Send to server
fetch('/api/notify', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ ids: ids })
});
```

### Select-All Checkbox

The framework automatically manages the select-all checkbox:

```blade
<input type="checkbox" id="selectAllRows" />
```

- ✅ Automatically checks when all rows are selected
- ✅ Automatically unchecks when any row is deselected
- ✅ Shows indeterminate state when some rows are selected
- ✅ Clears on page navigation

### Clearing Selections

```javascript
// Clear all checkboxes
document.querySelectorAll('#myTable .select-row').forEach(cb => cb.checked = false);

// Update select-all
const header = document.getElementById('selectAllRows');
if (header) header.checked = false;
```

---

## Examples

### Example 1: Simple Items Table (No Checkboxes)

**Blade View:**
```blade
<div class="table-wrapper">
    <table id="itemsTable" class="table">
        <thead>
            <tr>
                <th data-column="ItemID">ID <span class="sort-icon">⇅</span></th>
                <th data-column="ItemName">Name <span class="sort-icon">⇅</span></th>
                <th data-column="ItemQty">Stock <span class="sort-icon">⇅</span></th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>

<div id="tableInfo" class="text-muted"></div>
<ul id="pagination" class="pagination"></ul>
```

**JavaScript:**
```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    columns: [
        { key: 'ItemID', label: 'ID' },
        { key: 'ItemName', label: 'Name' },
        { key: 'ItemQty', label: 'Stock' }
    ],
    actions: [
        { icon: 'edit-2', class: 'btn-primary', onclick: 'editItem' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});

function editItem(id) {
    window.location.href = `/items/${id}/edit`;
}

function deleteItem(id) {
    if (confirm('Delete this item?')) {
        fetch(`/items/${id}`, { method: 'DELETE' })
            .then(r => r.json())
            .then(d => location.reload());
    }
}
```

---

### Example 2: Deliveries Table with Checkboxes

**Blade View:**
```blade
<input type="checkbox" id="selectAllRows" />

<div class="table-wrapper">
    <table id="deliveriesTable" class="table">
        <thead>
            <tr>
                <th style="width:40px;"><input type="checkbox" id="selectAllRows" /></th>
                <th data-column="DeliveryID">Delivery ID <span class="sort-icon">⇅</span></th>
                <th data-column="SupplierName">Supplier <span class="sort-icon">⇅</span></th>
                <th data-column="DeliveryDate">Date <span class="sort-icon">⇅</span></th>
                <th data-column="TotalItems">Items <span class="sort-icon">⇅</span></th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody></tbody>
    </table>
</div>
```

**JavaScript:**
```javascript
new CustomDataTable({
    apiUrl: '/api/deliveries',
    tableId: 'deliveriesTable',
    enableCheckboxes: true,  // Enable checkboxes
    columns: [
        { key: 'DeliveryID', label: 'Delivery ID' },
        { key: 'SupplierName', label: 'Supplier' },
        {
            key: 'DeliveryDate',
            label: 'Date',
            render: (value) => new Date(value).toLocaleDateString()
        },
        { key: 'TotalItems', label: 'Items' }
    ],
    actions: [
        { icon: 'eye', class: 'btn-primary', onclick: 'viewDelivery' },
        { icon: 'check-circle', class: 'btn-success', onclick: 'confirmDelivery' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'cancelDelivery' }
    ],
    defaultSort: 'DeliveryDate',
    defaultOrder: 'desc'
});

// Bulk approve button
document.getElementById('bulkApproveBtn').addEventListener('click', function() {
    const checked = Array.from(document.querySelectorAll('#deliveriesTable .select-row:checked'));
    const ids = checked.map(cb => cb.getAttribute('data-id'));
    
    if (ids.length === 0) return alert('Select deliveries first');
    
    fetch('/api/deliveries/approve-bulk', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ delivery_ids: ids })
    }).then(r => r.json()).then(d => location.reload());
});
```

---

### Example 3: Users Table with Custom Rendering

```javascript
new CustomDataTable({
    apiUrl: '/api/users',
    tableId: 'usersTable',
    enableCheckboxes: true,
    columns: [
        { key: 'UserID', label: 'User ID' },
        {
            key: 'UserName',
            label: 'Name',
            render: (value, row) => `<strong>${value}</strong> (${row.UserEmail})`
        },
        {
            key: 'UserRole',
            label: 'Role',
            render: (value) => {
                const colors = { 'admin': 'danger', 'manager': 'warning', 'staff': 'info' };
                return `<span class="badge bg-${colors[value] || 'secondary'}">${value}</span>`;
            }
        },
        {
            key: 'CreatedAt',
            label: 'Joined',
            render: (value) => new Date(value).toLocaleDateString()
        }
    ],
    actions: [
        { icon: 'edit-2', class: 'btn-primary', onclick: 'editUser' },
        { icon: 'lock', class: 'btn-warning', onclick: 'resetPassword' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteUser' }
    ]
});
```

---

## API Reference

### Methods

#### `loadData()`
Fetch data from server and render table.
```javascript
dataTable.loadData();
```

#### `saveState()`
Persist current page, sort, search to localStorage.
```javascript
dataTable.saveState();
```

#### `restoreState()`
Restore table state from localStorage.
```javascript
dataTable.restoreState();
```

#### `updateSelectAllCheckbox()`
Update select-all checkbox based on row selections (auto-called).
```javascript
dataTable.updateSelectAllCheckbox();
```

#### `filterData()`
Re-fetch data with current search term, reset to page 1.
```javascript
dataTable.filterData();
```

#### `sortTable(column, headerElement)`
Sort by column, toggle between asc/desc.
```javascript
dataTable.sortTable('ItemName', headerElement);
```

#### `scrollToTop()`
Scroll page to top (smooth animation).
```javascript
dataTable.scrollToTop();
```

---

## Request/Response Formats

### Simple REST Format (Default)

**Request:**
```json
GET /api/items?per_page=10&page=1&search=seafood&sort_by=ItemName&sort_order=asc
```

**Response:**
```json
{
    "success": true,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna", "ItemQty": 5 },
        ...
    ],
    "pagination": {
        "total": 25,
        "per_page": 10,
        "current_page": 1
    }
}
```

### DataTables Format

**Request:**
```json
GET /api/items?draw=1&start=0&length=10&search[value]=seafood&order[0][column]=1&order[0][dir]=asc
```

**Response:**
```json
{
    "draw": 1,
    "recordsTotal": 100,
    "recordsFiltered": 25,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna", "ItemQty": 5 },
        ...
    ]
}
```

---

## Best Practices

1. **Always include `data-column` attributes** on sortable headers
2. **Use `enableCheckboxes: true`** only when needed (adds overhead)
3. **Implement server-side validation** for delete/update actions
4. **Handle errors gracefully** with user feedback
5. **Test with multiple tables** on the same page
6. **Validate custom render functions** to prevent XSS
7. **Use meaningful `stateKey`** names when multiple tables exist

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Table not loading | Check API URL in console, verify CORS headers |
| Checkboxes not showing | Ensure `enableCheckboxes: true` in config |
| Select-all not working | Check that `#selectAllRows` checkbox exists |
| Sort not working | Verify `data-column` attributes on headers |
| Actions not firing | Check function names in `onclick` property |
| State not persisting | Check browser localStorage is enabled |

---

## License

This framework is part of the Dianne's Seafood House Admin Panel project.
