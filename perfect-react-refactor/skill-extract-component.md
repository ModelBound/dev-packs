# Skill: Extract Component

Use this skill when a JSX block is large enough to obscure intent or when state should be co-located with its usage.

## When to extract

Extract a sub-component when **any** of these are true:

- The JSX block is ≥ 30 lines
- It has ≥ 3 conditional branches
- It has its own local state that nothing else uses
- It is rendered in 2+ places (extract to share)

## When NOT to extract

- The block is < 15 lines and has no conditionals → leave inline
- Extracting would require passing > 5 props → reconsider the boundary
- The "component" would only be a styled `div` → it is probably just markup

## Procedure

1. **Name by domain noun**, not by visual.
   - Good: `InvoiceLineItem`, `PricingTier`, `MemberAvatar`
   - Bad: `BlueBox`, `LeftPanel`, `Wrapper`

2. **Decide where state lives.**
   - If state is only used inside the new component → move it down
   - If state is shared with siblings → keep it in parent, pass via props
   - If state is shared across the tree → context or store

3. **Define explicit prop types** before writing JSX:
   ```ts
   interface InvoiceLineItemProps {
     item: InvoiceItem;
     onRemove: (id: string) => void;
     editable?: boolean;
   }
   ```

4. **Move imports** the new component needs to its file. Remove unused imports from the parent.

5. **Verify behavior unchanged**: run tests, click through the affected screen, check the network tab if it makes requests.

## Worked example

Before:
```tsx
function Invoice({ invoice }: { invoice: Invoice }) {
  return (
    <div>
      <h1>{invoice.number}</h1>
      {invoice.items.map((item) => (
        <div key={item.id} className="flex justify-between p-3 border rounded">
          <div>
            <div className="font-medium">{item.description}</div>
            <div className="text-sm text-muted-foreground">{item.quantity} × {item.unitPrice}</div>
          </div>
          <div className="font-mono">{item.total}</div>
        </div>
      ))}
    </div>
  );
}
```

After:
```tsx
function Invoice({ invoice }: { invoice: Invoice }) {
  return (
    <div>
      <h1>{invoice.number}</h1>
      {invoice.items.map((item) => (
        <InvoiceLineItem key={item.id} item={item} />
      ))}
    </div>
  );
}

interface InvoiceLineItemProps { item: InvoiceItem }

function InvoiceLineItem({ item }: InvoiceLineItemProps) {
  return (
    <div className="flex justify-between p-3 border rounded">
      <div>
        <div className="font-medium">{item.description}</div>
        <div className="text-sm text-muted-foreground">
          {item.quantity} × {item.unitPrice}
        </div>
      </div>
      <div className="font-mono">{item.total}</div>
    </div>
  );
}
```
