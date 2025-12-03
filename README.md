# 📊 Custom Data Table Framework

A modern, flexible, and feature-rich data table component built for Laravel Blade views. Provides server-side pagination, sorting, filtering, and optional row selection with a beautiful, responsive design.

> Built with performance and developer experience in mind for Dianne's Seafood House Admin Panel

---

## ✨ Features

- ⚡ **Server-Side Processing** - Efficient pagination, sorting, and filtering
- 🎨 **Modern UI Design** - Beautiful gradient headers, smooth animations, and responsive layout
- 🔄 **Flexible Column Rendering** - Custom render functions with full data access
- 🎯 **Smart Action Buttons** - Icon-only buttons with conditional rendering and gradient backgrounds
- ✅ **Optional Checkboxes** - Select-all functionality with indeterminate state support
- 💾 **State Persistence** - Remembers user preferences (1-hour TTL via localStorage)
- 📱 **Fully Responsive** - Mobile-optimized with sticky columns and smooth scrollbars
- 🔍 **Advanced Search** - Real-time search with dropdown suggestions
- 🎭 **Multi-Table Support** - Use multiple tables on the same page independently
- 📊 **Multiple Response Formats** - Supports simple REST, DataTables, and custom envelopes
- 🌈 **Gradient Buttons** - Modern gradient action buttons with hover effects
- ♿ **Accessible** - Semantic HTML and keyboard navigation support
- 🔔 **Callback Functions** - Hooks for custom logic (onDataLoaded, onError, onRowClick, etc.)
- 🔄 **Auto-Refresh** - Optional auto-refresh with configurable intervals
- 📥 **Export Data** - Export as JSON or CSV
- 🖨️ **Print Table** - Built-in print functionality
- 🎛️ **Extensive API** - Rich set of methods for programmatic control

---

## 🚀 Quick Start

### 1️⃣ Include Assets

Add these to your Blade view's `@push('scripts')` section:

```blade
@push('scripts')
    <link rel="stylesheet" href="{{ asset('css/custom-table.css') }}">
    <script src="{{ asset('js/custom-table.js') }}"></script>
@endpush
```

### 2️⃣ Create Table HTML

```blade
<div class="table-wrapper">
    <table id="itemsTable" class="table customTable">
        <thead>
            <tr>
                <th data-column="ItemID">Item ID <span class="sort-icon">⇅</span></th>
                <th data-column="ItemName">Product Name <span class="sort-icon">⇅</span></th>
                <th data-column="ItemQty">Stock <span class="sort-icon">⇅</span></th>
                <th data-column="ItemPrice">Price <span class="sort-icon">⇅</span></th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody id="tableBody">
            <tr><td colspan="5" class="text-center py-4"><div class="spinner-border text-primary"></div></td></tr>
        </tbody>
    </table>
</div>
```

### 3️⃣ Initialize with JavaScript

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    primaryKey: 'ItemID',
    columns: [
        { key: 'ItemID', label: 'Item ID' },
        { key: 'ItemName', label: 'Product Name' },
        { key: 'ItemQty', label: 'Stock' },
        { key: 'ItemPrice', label: 'Price' }
    ],
    actions: [
        { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
        { label: 'Edit', icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
        { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});

function viewItem(id) { window.location.href = `/items/${id}`; }
function editItem(id) { window.location.href = `/items/${id}/edit`; }
function deleteItem(id) { if(confirm('Delete?')) fetch(`/items/${id}`, {method: 'DELETE'}); }
```

---

## ⚙️ Configuration Options

### Required Options

| Option | Type | Description |
|--------|------|-------------|
| `apiUrl` | string | **required** - API endpoint that returns table data |
| `tableId` | string | **required** - HTML ID of the `<table>` element |

### Column & Display Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `columns` | array | `[]` | Array of column definitions |
| `actions` | array | `[]` | Array of action button definitions |
| `primaryKey` | string | `'id'` | Unique identifier field name |

### Request & Response Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `method` | string | `'GET'` | HTTP method: `'GET'` or `'POST'` |
| `requestFormat` | string | `'simple'` | Format: `'simple'` or `'datatables'` |
| `additionalParams` | object | `{}` | Extra parameters to send with each request |
| `headers` | object | `{}` | Custom HTTP headers (merged with defaults) |

### Pagination & Sorting Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `perPage` | number | `10` | Default rows per page |
| `defaultSort` | string | `null` | Default sort column name |
| `defaultOrder` | string | `'asc'` | Default sort direction: `'asc'` or `'desc'` |
| `maxPaginationLinks` | number | `5` | Number of page links to show in pagination |

### Search & Filter Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `searchDebounceDelay` | number | `300` | Debounce delay for search input (ms) |
| `enableColumnFilters` | boolean | `false` | Enable per-column filters |
| `columnFilters` | object | `{}` | Define column-specific filter options |

### Row Interaction Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `enableCheckboxes` | boolean | `false` | Enable selection checkboxes |
| `onRowClick` | function | `null` | Callback when row is clicked |
| `rowHighlight` | boolean | `false` | Highlight rows on hover/click |
| `enableRowSelection` | boolean | `enableCheckboxes` | Enable row selection |

### Messaging & UI Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `emptyStateMessage` | string | `'No data found'` | Message when no data |
| `emptyStateIcon` | string | `'🔍'` | Icon for empty state |
| `loadingMessage` | string | `'Loading...'` | Loading message |
| `errorMessage` | string | `'Failed to load data. Please try again.'` | Error message |
| `noResultsMessage` | string | `'No results match your search'` | No results message |
| `showTableInfo` | boolean | `true` | Show "Showing X to Y of Z" text |

### Callback Functions

| Option | Type | Description |
|--------|------|-------------|
| `onDataLoaded` | function | Called after data loads successfully - `(data, totalRecords)` |
| `onError` | function | Called if request fails - `(error)` |
| `onRowsSelected` | function | Called when rows selected/deselected - `(selectedIds, selectedData)` |
| `beforeRequest` | function | Called before each request |
| `afterRequest` | function | Called after each request - `(data)` |
| `onSort` | function | Called when sorting changes - `(column, direction)` |
| `onPageChange` | function | Called when page changes |
| `onRowClick` | function | Called when row is clicked - `(id, rowData, rowElement)` |

### Auto-Refresh Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `autoRefresh` | number/boolean | `false` | Enable auto-refresh (ms interval, false = disabled) |

### State & DOM Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `stateKey` | string | `tableState_{tableId}` | LocalStorage key for state persistence |
| `containerId` | string | `null` | ID of container element for scoped DOM lookups |

---

## 📋 Column Configuration

Define columns with these properties:

```javascript
{
    key: 'fieldName',              // ✓ Required: Data field name
    label: 'Display Name',         // Column header text
    sticky: false,                 // Make column stick to left (default: false)
    sortable: true,                // Enable column sorting (default: true)
    searchable: true,              // Include in search (default: true)
    render: function(value, row) { // Custom rendering function
        return `<strong>${value}</strong>`;
    }
}
```

### 📌 Column Examples

**Simple Column:**
```javascript
{ key: 'ItemID', label: 'Item ID' }
```

**Custom Render - Status Badge:**
```javascript
{
    key: 'Status',
    label: 'Status',
    render: (value) => {
        const badgeClass = value === 'pending' ? 'pending-soft' : 
                          value === 'approved' ? 'approved-soft' : 'disapproved-soft';
        return `<span class="status-badge bg-${badgeClass}">${value.toUpperCase()}</span>`;
    }
}
```

**Custom Render - Currency:**
```javascript
{
    key: 'ItemPrice',
    label: 'Price',
    render: (value) => {
        return '₱' + parseFloat(value).toLocaleString('en-PH', {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2
        });
    }
}
```

**Custom Render - Date:**
```javascript
{
    key: 'CreatedAt',
    label: 'Created',
    render: (value) => {
        if (!value) return '<span class="text-muted">Never</span>';
        return new Date(value).toLocaleDateString('en-US', {
            year: 'numeric', month: 'short', day: 'numeric'
        });
    }
}
```

**Sticky Column (First):**
```javascript
{
    key: 'ItemID',
    label: 'Item ID',
    sticky: true  // Remains visible during horizontal scroll
}
```

---

## 🎯 Actions Configuration

Action buttons with modern gradient backgrounds:

```javascript
{
    label: 'Edit',                  // Button label (used for tooltip)
    icon: 'edit-2',                 // Feather icon name
    class: 'btn-primary',           // Button color class
    title: 'Edit this item',        // Tooltip text
    onclick: 'editItem',            // Handler function name or reference
    condition: function(row) {      // Optional: Show button only if true
        return row.Status === 'pending';
    }
}
```

### 🎨 Available Button Colors

| Class | Color | Use Case |
|-------|-------|----------|
| `btn-primary` | Blue Gradient | View / Open |
| `btn-warning` | Pink-Red Gradient | Edit / Modify |
| `btn-success` | Green Gradient | Approve / Confirm |
| `btn-danger` | Pink-Yellow Gradient | Delete / Remove |
| `btn-info` | Cyan Gradient | Info / Details |
| `btn-secondary` | Pastel Gradient | Secondary Actions |

### 📌 Action Examples

**Basic Actions:**
```javascript
actions: [
    { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
    { label: 'Edit', icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
    { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
]
```

**Conditional Edit Button (only for pending items):**
```javascript
{
    label: 'Edit',
    icon: 'edit-2',
    class: 'btn-warning',
    onclick: 'editDelivery',
    condition: function(row) {
        return row.delivery_status && String(row.delivery_status).toLowerCase() === 'pending';
    }
}
```

**Custom Handler:**
```javascript
function editItem(id, status) {
    if (status !== 'pending') {
        alert('Can only edit pending items');
        return;
    }
    window.location.href = `/items/${id}/edit`;
}
```

---

## ✅ Row Selection (Checkboxes)

### Enable Checkboxes

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    enableCheckboxes: true,  // ← Enable checkboxes
    columns: [...]
});
```

### Select All Checkbox

Add to your HTML:

```blade
<div class="mb-3">
    <input type="checkbox" id="selectAllRows" /> Select All
</div>
```

The framework automatically manages:
- ✅ Checks when all visible rows are selected
- ✅ Unchecks when any row is deselected
- ✅ Shows indeterminate state when partially selected
- ✅ Persists across pagination

### Get Selected Rows

```javascript
const table = new CustomDataTable({...});

// Get IDs of selected rows
const selectedIds = table.getSelectedRows();

// Get full data objects of selected rows
const selectedData = table.getSelectedRowData();
```

### Bulk Actions Example

```javascript
document.getElementById('bulkApproveBtn').addEventListener('click', function() {
    const selectedIds = window.menuTable.getSelectedRows();
    
    if (selectedIds.length === 0) {
        alert('Please select items first');
        return;
    }
    
    fetch('/api/items/approve-bulk', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ ids: selectedIds })
    })
    .then(r => r.json())
    .then(data => {
        if (data.success) {
            window.menuTable.refresh();
        }
    });
});
```

---

## 🖱️ Row Click Handling

### Enable Row Click Callback

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    onRowClick: function(id, rowData, rowElement) {
        console.log('Row clicked:', id, rowData);
        // Navigate to detail page
        // window.location.href = `/items/${id}`;
    },
    rowHighlight: true,  // Highlight on hover/click
    columns: [...]
});
```

The `onRowClick` callback receives:
- `id`: Primary key value
- `rowData`: Full row data object
- `rowElement`: DOM row element

---

## 🔄 Callback Functions

### Data Loading Callbacks

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    
    // Before request is sent
    beforeRequest: function() {
        console.log('About to fetch data...');
    },
    
    // After request completes (regardless of success)
    afterRequest: function(data) {
        console.log('Request completed with data:', data);
    },
    
    // After successful data load
    onDataLoaded: function(data, totalRecords) {
        console.log('Data loaded successfully!');
        console.log(`${data.length} items loaded of ${totalRecords} total`);
    },
    
    // When error occurs
    onError: function(error) {
        console.error('Failed to load data:', error);
        // Send to error tracking service
    },
    
    columns: [...]
});
```

### Interaction Callbacks

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    enableCheckboxes: true,
    
    // When rows are selected/deselected
    onRowsSelected: function(selectedIds, selectedData) {
        console.log('Selected IDs:', selectedIds);
        const count = selectedIds.length;
        document.getElementById('selectionCount').textContent = `${count} selected`;
    },
    
    // When sorting changes
    onSort: function(column, direction) {
        console.log(`Sorted by ${column} (${direction})`);
    },
    
    // When page changes
    onPageChange: function(page) {
        console.log(`Navigated to page ${page}`);
    },
    
    columns: [...]
});
```

---

## 🔄 Auto-Refresh

### Enable Auto-Refresh

```javascript
const table = new CustomDataTable({
    apiUrl: '/api/deliveries',
    tableId: 'deliveriesTable',
    autoRefresh: 5000,  // Refresh every 5 seconds
    columns: [...]
});

// Or start it programmatically
table.startAutoRefresh(5000);  // 5000ms = 5 seconds

// Stop auto-refresh
table.stopAutoRefresh();
```

---

## 📥 Export & Print Functionality

### Export as JSON

```javascript
const table = new CustomDataTable({...});

const jsonData = table.exportAsJSON();
console.log(jsonData);

// Download JSON file
const blob = new Blob([JSON.stringify(jsonData, null, 2)], { type: 'application/json' });
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'table-data.json';
a.click();
```

### Export as CSV

```javascript
const csvData = table.exportAsCSV();
console.log(csvData);

// Download CSV file
const blob = new Blob([csvData], { type: 'text/csv' });
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'table-data.csv';
a.click();
```

### Print Table

```javascript
const table = new CustomDataTable({...});
table.print();  // Opens print dialog
```

---

## 🔌 API Methods

### Data Loading

```javascript
const table = new CustomDataTable({...});

// Reload data with current filters
table.refresh();

// Load data (automatically called on init)
table.loadData();
```

### Selection Management

```javascript
// Get selected IDs
table.getSelectedRows();  // Returns array of IDs

// Get selected row data
table.getSelectedRowData();  // Returns array of data objects

// Select all rows
table.selectAllRows();

// Clear all selections
table.clearSelection();
```

### Filter Management

```javascript
// Get current filter state
const filters = table.getFilters();
// Returns: { search, filters, sort, pagination }

// Apply multiple filters at once
table.setFilters({
    search: 'seafood',
    filterParams: { category: 'fish' },
    page: 1
});

// Clear all filters
table.clearAllFilters();
```

### State Management

```javascript
// Save state to localStorage
table.saveState();

// Restore state from localStorage
table.restoreState();
```

### Auto-Refresh

```javascript
// Start auto-refresh (5 seconds)
table.startAutoRefresh(5000);

// Stop auto-refresh
table.stopAutoRefresh();
```

### Export & Print

```javascript
// Export as JSON
const json = table.exportAsJSON();

// Export as CSV
const csv = table.exportAsCSV();

// Print table
table.print();
```

### Cleanup

```javascript
// Destroy instance and cleanup resources
table.destroy();
```

---

## 🔄 API Response Format

### Simple REST Format (Recommended)

**Request:**
```
GET /api/items?per_page=10&page=1&search_term=seafood&sort_column=ItemName&sort_direction=asc&category=fish
```

**Response:**
```json
{
    "success": true,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna Fillet", "ItemQty": 45 },
        { "ItemID": 2, "ItemName": "Salmon", "ItemQty": 32 }
    ],
    "pagination": {
        "total": 125,
        "per_page": 10,
        "current_page": 1
    }
}
```

### DataTables Format

**Request:**
```
GET /api/items?draw=1&start=0&length=10&search=tuna&order[0][column]=1&order[0][dir]=asc
```

**Response:**
```json
{
    "draw": 1,
    "recordsTotal": 125,
    "recordsFiltered": 5,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna Fillet", "ItemQty": 45 },
        { "ItemID": 2, "ItemName": "Tuna Sashimi", "ItemQty": 28 }
    ]
}
```

---

## 📚 Complete Examples

### Example 1: Delivery Management Table with Callbacks

```javascript
new CustomDataTable({
    apiUrl: '/api/deliveries',
    tableId: 'deliveriesTable',
    method: 'POST',
    primaryKey: 'delivery_id',
    enableCheckboxes: true,
    rowHighlight: true,
    
    // Additional parameters sent with each request
    additionalParams: {
        status: 'pending'
    },
    
    // Callbacks
    beforeRequest: () => console.log('Fetching deliveries...'),
    onDataLoaded: (data, total) => console.log(`Loaded ${data.length} of ${total}`),
    onError: (error) => console.error('Load failed:', error),
    onRowClick: (id, data) => window.location.href = `/deliveries/${id}`,
    onRowsSelected: (ids, data) => console.log('Selected:', ids),
    
    columns: [
        { key: 'delivery_id', label: 'Delivery ID' },
        { 
            key: 'delivery_date',
            label: 'Date',
            render: (value) => new Date(value).toLocaleDateString('en-US', {
                year: 'numeric', month: 'short', day: 'numeric'
            })
        },
        { key: 'supplier_name', label: 'Supplier' },
        {
            key: 'material_cost',
            label: 'Cost',
            render: (value) => '₱' + parseFloat(value).toLocaleString('en-PH', {
                minimumFractionDigits: 2, maximumFractionDigits: 2
            })
        },
        {
            key: 'delivery_status',
            label: 'Status',
            render: (value) => {
                const badgeClass = value === 'APPROVED' ? 'approved-soft' :
                                  value === 'DISAPPROVED' ? 'disapproved-soft' : 'pending-soft';
                return `<span class="status-badge bg-${badgeClass}">${value}</span>`;
            }
        }
    ],
    actions: [
        { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewDelivery' },
        { 
            label: 'Edit', 
            icon: 'edit-2', 
            class: 'btn-warning', 
            onclick: 'editDelivery',
            condition: (row) => String(row.delivery_status).toLowerCase() === 'pending'
        },
        { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteDelivery' }
    ],
    defaultSort: 'delivery_id',
    defaultOrder: 'desc',
    autoRefresh: 10000  // Refresh every 10 seconds
});
```

### Example 2: Inventory with Stock Levels & Auto-Refresh

```javascript
new CustomDataTable({
    apiUrl: '/api/inventory',
    tableId: 'inventoryTable',
    autoRefresh: 5000,  // Auto-refresh every 5 seconds
    
    onDataLoaded: (data) => {
        const lowStockCount = data.filter(item => item.ItemQty <= 5).length;
        if (lowStockCount > 0) {
            showToast(`${lowStockCount} items low in stock!`, 'warning');
        }
    },
    
    columns: [
        { key: 'ItemID', label: 'Item ID', sticky: true },
        { key: 'ItemName', label: 'Product Name' },
        {
            key: 'ItemQty',
            label: 'Stock Level',
            render: (value, row) => {
                if (value <= 5) {
                    return `<span class="badge bg-danger-soft text-danger">${value}</span>`;
                } else if (value <= 15) {
                    return `<span class="badge bg-warning-soft text-warning">${value}</span>`;
                }
                return `<span class="badge bg-success-soft text-success">${value}</span>`;
            }
        },
        { key: 'ItemUnit', label: 'Unit' },
        {
            key: 'ItemPrice',
            label: 'Unit Price',
            render: (value) => '₱' + parseFloat(value).toLocaleString('en-PH', {
                minimumFractionDigits: 2, maximumFractionDigits: 2
            })
        }
    ],
    actions: [
        { icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
        { icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});
```

### Example 3: Blog Post Table with Export

```javascript
const blogTable = new CustomDataTable({
    apiUrl: '/api/blogs',
    tableId: 'blogTable',
    
    onRowClick: (id) => window.location.href = `/blogs/${id}`,
    rowHighlight: true,
    
    columns: [
        { key: 'blog_id', label: 'Blog ID', sticky: true },
        { 
            key: 'blog_title', 
            label: 'Title',
            render: (value, row) => `<strong>${value}</strong><br><small>${row.blog_description?.substring(0, 50)}...</small>`
        },
        {
            key: 'created_at',
            label: 'Published',
            render: (value) => new Date(value).toLocaleDateString()
        },
        {
            key: 'blog_category_name',
            label: 'Category',
            render: (value) => `<span class="badge bg-primary">${value}</span>`
        }
    ],
    actions: [
        { icon: 'eye', class: 'btn-primary', onclick: 'viewBlog' },
        { icon: 'edit-2', class: 'btn-warning', onclick: 'editBlog' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteBlog' }
    ]
});

// Export functionality
document.getElementById('exportBtn').addEventListener('click', () => {
    const csv = blogTable.exportAsCSV();
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `blogs_${new Date().toISOString().split('T')[0]}.csv`;
    a.click();
});

document.getElementById('printBtn').addEventListener('click', () => {
    blogTable.print();
});
```

---

## 🎨 Styling Classes

### Status Badges

```html
<span class="status-badge bg-pending-soft">Pending</span>
<span class="status-badge bg-approved-soft">Approved</span>
<span class="status-badge bg-disapproved-soft">Disapproved</span>
```

### Row Highlighting

```html
<tr class="low-stock"><!-- Yellow highlight --></tr>
<tr class="critical-stock"><!-- Orange highlight --></tr>
<tr class="out-of-stock"><!-- Red highlight --></tr>
<tr class="row-interactive"><!-- Clickable row styling --></tr>
```

### Table Size Variants

```html
<!-- Compact table -->
<table class="table customTable table-sm">

<!-- Large table -->
<table class="table customTable table-lg">
```

---

## 📱 Responsive Design

The table automatically adapts to mobile screens:

- ✅ Sticky column (first column) remains visible
- ✅ Horizontal scroll for overflow content
- ✅ Action buttons remain accessible
- ✅ Compact padding on small screens
- ✅ Touch-friendly button sizes
- ✅ Checkbox column adapts to mobile view

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Table not loading | Check API URL, verify CORS and authentication |
| Checkboxes not showing | Set `enableCheckboxes: true` in config |
| Sort not working | Add `data-column` attribute to header `<th>` |
| Actions not firing | Verify function names in `onclick` property exist globally |
| State not persisting | Check browser localStorage is enabled |
| Duplicate tables | Each table needs unique `tableId` |
| Styling issues | Ensure CSS file is loaded before JS |
| Row click not firing | Check if clicking on checkbox/action button (excluded) |
| Auto-refresh not working | Verify interval is > 100ms |
| Export not downloading | Check browser download settings |

---

## 🏆 Best Practices

1. ✅ Always include `data-column` attributes on sortable headers
2. ✅ Use `enableCheckboxes: true` only when needed
3. ✅ Implement server-side validation for delete/update actions
4. ✅ Handle errors gracefully with user feedback via `onError` callback
5. ✅ Test pagination with different page sizes
6. ✅ Use meaningful `stateKey` names for multiple tables
7. ✅ Sanitize data in custom render functions to prevent XSS
8. ✅ Test on mobile devices for responsive behavior
9. ✅ Use `beforeRequest` callback to show loading states
10. ✅ Cache table instance if accessing multiple methods
11. ✅ Call `destroy()` when removing table from DOM
12. ✅ Use `additionalParams` for consistent filter context
13. ✅ Implement proper CSRF token handling in `headers` option
14. ✅ Test all callbacks are properly defined before enabling

---

## 📦 Browser Support

- ✅ Chrome/Edge (Latest)
- ✅ Firefox (Latest)
- ✅ Safari (Latest)
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

---

## 📄 License

Part of the **Dianne's Seafood House Admin Panel** project.

---

## 💡 Tips & Tricks

**Freeze First Column:**
```javascript
{ key: 'ItemID', label: 'Item ID', sticky: true }
```

**Custom Row Classes:**
```javascript
{
    key: 'ItemQty',
    render: (value, row) => value <= 5 ? 
        '<span class="low-stock">' + value + '</span>' : value
}
```

**Disable Sorting for a Column:**
```javascript
{ key: 'Actions', label: 'Actions', sortable: false }
```

**Format Large Numbers:**
```javascript
{
    key: 'Amount',
    render: (value) => (value / 1000).toFixed(1) + 'K'
}
```

**Custom Icons:**
```javascript
{
    key: 'Status',
    render: (value) => `<i data-feather="${value === 'active' ? 'check' : 'x'}"></i> ${value}`
}
```

**Link Rendering:**
```javascript
{
    key: 'ItemName',
    render: (value, row) => `<a href="/items/${row.ItemID}">${value}</a>`
}
```

**Show Limit:**
```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    showTableInfo: false,  // Hide "Showing X to Y of Z"
    columns: [...]
});
```

---

**Happy Coding! 🚀 For questions or issues, refer to the framework source code comments.**

---

## 🚀 Quick Start

### 1️⃣ Include Assets

Add these to your Blade view's `@push('scripts')` section:

```blade
@push('scripts')
    <link rel="stylesheet" href="{{ asset('css/custom-table.css') }}">
    <script src="{{ asset('js/custom-table.js') }}"></script>
@endpush
```

### 2️⃣ Create Table HTML

```blade
<div class="table-wrapper">
    <table id="itemsTable" class="table customTable">
        <thead>
            <tr>
                <th data-column="ItemID">Item ID <span class="sort-icon">⇅</span></th>
                <th data-column="ItemName">Product Name <span class="sort-icon">⇅</span></th>
                <th data-column="ItemQty">Stock <span class="sort-icon">⇅</span></th>
                <th data-column="ItemPrice">Price <span class="sort-icon">⇅</span></th>
                <th>Actions</th>
            </tr>
        </thead>
        <tbody id="tableBody">
            <tr><td colspan="5" class="text-center py-4"><div class="spinner-border text-primary"></div></td></tr>
        </tbody>
    </table>
</div>
```

### 3️⃣ Initialize with JavaScript

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    primaryKey: 'ItemID',
    columns: [
        { key: 'ItemID', label: 'Item ID' },
        { key: 'ItemName', label: 'Product Name' },
        { key: 'ItemQty', label: 'Stock' },
        { key: 'ItemPrice', label: 'Price' }
    ],
    actions: [
        { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
        { label: 'Edit', icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
        { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});

function viewItem(id) { window.location.href = `/items/${id}`; }
function editItem(id) { window.location.href = `/items/${id}/edit`; }
function deleteItem(id) { if(confirm('Delete?')) fetch(`/items/${id}`, {method: 'DELETE'}); }
```

---

## ⚙️ Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `apiUrl` | string | **required** | API endpoint that returns table data |
| `tableId` | string | **required** | HTML ID of the `<table>` element |
| `columns` | array | `[]` | Array of column definitions |
| `actions` | array | `[]` | Array of action button definitions |
| `primaryKey` | string | `'id'` | Unique identifier field name |
| `enableCheckboxes` | boolean | `false` | Enable row selection checkboxes |
| `method` | string | `'GET'` | HTTP method: `'GET'` or `'POST'` |
| `requestFormat` | string | `'simple'` | Format: `'simple'` or `'datatables'` |
| `perPage` | number | `10` | Default rows per page |
| `defaultSort` | string | `null` | Default sort column name |
| `defaultOrder` | string | `'asc'` | Default sort direction: `'asc'` or `'desc'` |
| `stateKey` | string | `tableState_{id}` | LocalStorage key for state persistence |

---

## 📋 Column Configuration

Define columns with these properties:

```javascript
{
    key: 'fieldName',              // ✓ Required: Data field name
    label: 'Display Name',         // Column header text
    sticky: false,                 // Make column stick to left (default: false)
    sortable: true,                // Enable column sorting (default: true)
    searchable: true,              // Include in search (default: true)
    render: function(value, row) { // Custom rendering function
        return `<strong>${value}</strong>`;
    }
}
```

### 📌 Column Examples

**Simple Column:**
```javascript
{ key: 'ItemID', label: 'Item ID' }
```

**Custom Render - Status Badge:**
```javascript
{
    key: 'Status',
    label: 'Status',
    render: (value) => {
        const badgeClass = value === 'pending' ? 'pending-soft' : 
                          value === 'approved' ? 'approved-soft' : 'disapproved-soft';
        return `<span class="status-badge bg-${badgeClass}">${value.toUpperCase()}</span>`;
    }
}
```

**Custom Render - Currency:**
```javascript
{
    key: 'ItemPrice',
    label: 'Price',
    render: (value) => {
        return '₱' + parseFloat(value).toLocaleString('en-PH', {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2
        });
    }
}
```

**Custom Render - Date:**
```javascript
{
    key: 'CreatedAt',
    label: 'Created',
    render: (value) => {
        if (!value) return '<span class="text-muted">Never</span>';
        return new Date(value).toLocaleDateString('en-US', {
            year: 'numeric', month: 'short', day: 'numeric'
        });
    }
}
```

---

## 🎯 Actions Configuration

Action buttons with modern gradient backgrounds:

```javascript
{
    label: 'Edit',                  // Button label (used for tooltip)
    icon: 'edit-2',                 // Feather icon name
    class: 'btn-primary',           // Button color class
    title: 'Edit this item',        // Tooltip text
    onclick: 'editItem',            // Handler function name or reference
    condition: function(row) {      // Optional: Show button only if true
        return row.Status === 'pending';
    }
}
```

### 🎨 Available Button Colors

| Class | Color | Use Case |
|-------|-------|----------|
| `btn-primary` | Blue Gradient | View / Open |
| `btn-warning` | Pink-Red Gradient | Edit / Modify |
| `btn-success` | Green Gradient | Approve / Confirm |
| `btn-danger` | Pink-Yellow Gradient | Delete / Remove |
| `btn-info` | Cyan Gradient | Info / Details |
| `btn-secondary` | Pastel Gradient | Secondary Actions |

### 📌 Action Examples

**Basic Actions:**
```javascript
actions: [
    { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
    { label: 'Edit', icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
    { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
]
```

**Conditional Edit Button (only for pending items):**
```javascript
{
    label: 'Edit',
    icon: 'edit-2',
    class: 'btn-warning',
    onclick: 'editDelivery',
    condition: function(row) {
        return row.delivery_status && String(row.delivery_status).toLowerCase() === 'pending';
    }
}
```

**Custom Handler:**
```javascript
function editItem(id, status) {
    if (status !== 'pending') {
        alert('Can only edit pending items');
        return;
    }
    window.location.href = `/items/${id}/edit`;
}
```

---

## ✅ Row Selection (Checkboxes)

### Enable Checkboxes

```javascript
new CustomDataTable({
    apiUrl: '/api/items',
    tableId: 'itemsTable',
    enableCheckboxes: true,  // ← Enable checkboxes
    columns: [...]
});
```

### Select All Checkbox

Add to your HTML:

```blade
<div class="mb-3">
    <input type="checkbox" id="selectAllRows" /> Select All
</div>
```

The framework automatically manages:
- ✅ Checks when all visible rows are selected
- ✅ Unchecks when any row is deselected
- ✅ Shows indeterminate state when partially selected
- ✅ Persists across pagination

### Get Selected Rows

```javascript
const checked = Array.from(document.querySelectorAll('#itemsTable .select-row:checked'));
const ids = checked.map(cb => cb.getAttribute('data-id'));

console.log('Selected IDs:', ids);
```

### Bulk Actions Example

```javascript
document.getElementById('bulkApproveBtn').addEventListener('click', function() {
    const checked = Array.from(document.querySelectorAll('#deliveriesTable .select-row:checked'));
    const ids = checked.map(cb => cb.getAttribute('data-id'));
    
    if (ids.length === 0) {
        alert('Please select items first');
        return;
    }
    
    fetch('/api/deliveries/approve-bulk', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ delivery_ids: ids })
    })
    .then(r => r.json())
    .then(data => {
        if (data.success) {
            location.reload();
        }
    });
});
```

---

## 🔄 API Response Format

### Simple REST Format (Recommended)

**Request:**
```
GET /api/items?per_page=10&page=1&search=seafood&sort_by=ItemName&sort_order=asc
```

**Response:**
```json
{
    "success": true,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna Fillet", "ItemQty": 45 },
        { "ItemID": 2, "ItemName": "Salmon", "ItemQty": 32 }
    ],
    "pagination": {
        "total": 125,
        "per_page": 10,
        "current_page": 1
    }
}
```

### DataTables Format

**Request:**
```
GET /api/items?draw=1&start=0&length=10&search[value]=tuna&order[0][column]=1&order[0][dir]=asc
```

**Response:**
```json
{
    "draw": 1,
    "recordsTotal": 125,
    "recordsFiltered": 5,
    "data": [
        { "ItemID": 1, "ItemName": "Tuna Fillet", "ItemQty": 45 },
        { "ItemID": 2, "ItemName": "Tuna Sashimi", "ItemQty": 28 }
    ]
}
```

---

## 📚 Complete Examples

### Example 1: Delivery Management Table

```javascript
new CustomDataTable({
    apiUrl: '/api/deliveries',
    tableId: 'deliveriesTable',
    method: 'POST',
    primaryKey: 'delivery_id',
    enableCheckboxes: true,
    columns: [
        { key: 'delivery_id', label: 'Delivery ID' },
        { 
            key: 'delivery_date',
            label: 'Date',
            render: (value) => new Date(value).toLocaleDateString('en-US', {
                year: 'numeric', month: 'short', day: 'numeric'
            })
        },
        { key: 'supplier_name', label: 'Supplier' },
        {
            key: 'material_cost',
            label: 'Cost',
            render: (value) => '₱' + parseFloat(value).toLocaleString('en-PH', {
                minimumFractionDigits: 2, maximumFractionDigits: 2
            })
        },
        {
            key: 'delivery_status',
            label: 'Status',
            render: (value) => {
                const badgeClass = value === 'APPROVED' ? 'approved-soft' :
                                  value === 'DISAPPROVED' ? 'disapproved-soft' : 'pending-soft';
                return `<span class="status-badge bg-${badgeClass}">${value}</span>`;
            }
        }
    ],
    actions: [
        { label: 'View', icon: 'eye', class: 'btn-primary', onclick: 'viewDelivery' },
        { 
            label: 'Edit', 
            icon: 'edit-2', 
            class: 'btn-warning', 
            onclick: 'editDelivery',
            condition: (row) => String(row.delivery_status).toLowerCase() === 'pending'
        },
        { label: 'Delete', icon: 'trash-2', class: 'btn-danger', onclick: 'deleteDelivery' }
    ],
    defaultSort: 'delivery_id',
    defaultOrder: 'desc'
});
```

### Example 2: Inventory with Stock Levels

```javascript
new CustomDataTable({
    apiUrl: '/api/inventory',
    tableId: 'inventoryTable',
    columns: [
        { key: 'ItemID', label: 'Item ID', sticky: true },
        { key: 'ItemName', label: 'Product Name' },
        {
            key: 'ItemQty',
            label: 'Stock Level',
            render: (value, row) => {
                if (value <= 5) {
                    return `<span class="badge bg-danger-soft text-danger">${value}</span>`;
                } else if (value <= 15) {
                    return `<span class="badge bg-warning-soft text-warning">${value}</span>`;
                }
                return `<span class="badge bg-success-soft text-success">${value}</span>`;
            }
        },
        { key: 'ItemUnit', label: 'Unit' },
        {
            key: 'ItemPrice',
            label: 'Unit Price',
            render: (value) => '₱' + parseFloat(value).toLocaleString('en-PH', {
                minimumFractionDigits: 2, maximumFractionDigits: 2
            })
        }
    ],
    actions: [
        { icon: 'eye', class: 'btn-primary', onclick: 'viewItem' },
        { icon: 'edit-2', class: 'btn-warning', onclick: 'editItem' },
        { icon: 'trash-2', class: 'btn-danger', onclick: 'deleteItem' }
    ]
});
```

---

## 🎨 Styling Classes

### Status Badges

```html
<span class="status-badge bg-pending-soft">Pending</span>
<span class="status-badge bg-approved-soft">Approved</span>
<span class="status-badge bg-disapproved-soft">Disapproved</span>
```

### Row Highlighting

```html
<tr class="low-stock"><!-- Yellow highlight --></tr>
<tr class="critical-stock"><!-- Orange highlight --></tr>
<tr class="out-of-stock"><!-- Red highlight --></tr>
```

### Table Size Variants

```html
<!-- Compact table -->
<table class="table customTable table-sm">

<!-- Large table -->
<table class="table customTable table-lg">
```

---

## 🔌 API Methods

```javascript
// Get table instance
const table = window.dataTable;  // if stored in global scope

// Reload data
table.loadData();

// Save state to localStorage
table.saveState();

// Restore state from localStorage
table.restoreState();

// Sort by column
table.sortTable('ItemName', headerElement);

// Filter with search term
table.filterData();

// Update checkbox states
table.updateSelectAllCheckbox();

// Scroll to top
table.scrollToTop();
```

---

## 📱 Responsive Design

The table automatically adapts to mobile screens:

- ✅ Sticky column (first column) remains visible
- ✅ Horizontal scroll for overflow content
- ✅ Action buttons remain accessible
- ✅ Compact padding on small screens
- ✅ Touch-friendly button sizes

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Table not loading | Check API URL, verify CORS and authentication |
| Checkboxes not showing | Set `enableCheckboxes: true` in config |
| Sort not working | Add `data-column` attribute to header `<th>` |
| Actions not firing | Verify function names in `onclick` property |
| State not persisting | Check browser localStorage is enabled |
| Duplicate tables | Each table needs unique `tableId` |
| Styling issues | Ensure CSS file is loaded before JS |

---

## 🏆 Best Practices

1. ✅ Always include `data-column` attributes on sortable headers
2. ✅ Use `enableCheckboxes: true` only when needed
3. ✅ Implement server-side validation for delete/update actions
4. ✅ Handle errors gracefully with user feedback
5. ✅ Test pagination with different page sizes
6. ✅ Use meaningful stateKey names for multiple tables
7. ✅ Sanitize data in custom render functions to prevent XSS
8. ✅ Test on mobile devices for responsive behavior

---

## 📦 Browser Support

- ✅ Chrome/Edge (Latest)
- ✅ Firefox (Latest)
- ✅ Safari (Latest)
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

---

## 📄 License

Part of the **Dianne's Seafood House Admin Panel** project.

---

## 💡 Tips & Tricks

**Freeze First Column:**
```javascript
{ key: 'ItemID', label: 'Item ID', sticky: true }
```

**Custom Row Classes:**
```javascript
{
    key: 'ItemQty',
    render: (value, row) => value <= 5 ? 
        '<span class="low-stock">' + value + '</span>' : value
}
```

**Disable Sorting for a Column:**
```javascript
{ key: 'Actions', label: 'Actions', sortable: false }
```

**Format Large Numbers:**
```javascript
{
    key: 'Amount',
    render: (value) => (value / 1000).toFixed(1) + 'K'
}
```

---

**Happy Coding! 🚀 For questions or issues, refer to the framework source code comments.**
