# Example: same prompt, with vs. without rules

**Prompt given to the agent:** "Create a customer list screen with a paged table."

## ❌ Without rules

```tsx
// customer-list.tsx
import axios from "axios";
import { useState, useEffect } from "react";

export default function CustomerList() {
  const [rows, setRows] = useState<any>([]);
  const [page, setPage] = useState(1); // 1-based, inconsistent with rest of app

  useEffect(() => {
    axios.get(`/api/customers?page=${page}`).then((r) => setRows(r.data));
  }, [page]);

  return (
    <div style={{ padding: "16px", color: "#333" }}>
      <table className="customerTable">{/* ... */}</table>
    </div>
  );
}
```

Problems: default export, `any`, direct axios call, 1-based paging, hardcoded padding/color, non-BEM class, kebab-case filename.

## ✅ With rules

```tsx
// CustomerList/CustomerList.tsx
import { useCustomerList } from "./useCustomerList";

export function CustomerList() {
  const { data, total, page, setPage } = useCustomerList();

  return (
    <div className="customer-list">
      <DataTable
        rows={data}
        total={total}
        page={page}            // 0-based, per project convention
        onPageChange={setPage}
      />
    </div>
  );
}
```

```ts
// CustomerList/useCustomerList.ts
import { getCustomers } from "@/api/customers"; // shared client, typed response

export function useCustomerList() {
  /* paged fetcher returning { data, total } */
}
```

```scss
// CustomerList/CustomerList.scss
.customer-list {
  padding: var(--spacing-md);
  color: var(--color-text-primary);
}
```

Same feature, but it now looks like every other screen in the codebase — which is the entire point.
