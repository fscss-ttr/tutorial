# 17 — Data Table

A data table succeeds on rhythm: measured rows, aligned numerics, a zebra stripe, and a status pill. Generate the column widths from arrays, let one block define own the shell, and use an event to decide which columns lean right.

## The Brief

- A table shell with a sticky header and zebra striping.
- Column widths generated from an array so a new column is one entry.
- Numeric columns right-aligned automatically.
- Row status reuses the badge pattern from the last project.

## The FSCSS

```fscss
@fun(color){
  ink: #0f172a;
  muted: #64748b;
  line: #e2e8f0;
  stripe: #f8fafc;
  head: #f1f5f9;
  white: #ffffff;
}

@define table-shell() {
  `
  .table {
    width: 100%;
    border-collapse: collapse;
    background: @fun.color.white.value;
    border: 1px solid @fun.color.line.value;
    border-radius: 12px;
    overflow: hidden;
    font-size: 0.9rem;
  }

  .table thead th {
    position: sticky;
    top: 0;
    background: @fun.color.head.value;
    color: @fun.color.muted.value;
    font-weight: 600;
    padding: num(0.75 * 16)px num(16)px;
    border-bottom: 1px solid @fun.color.line.value;
  }

  .table tbody td {
    padding: num(0.7 * 16)px num(16)px;
    border-bottom: 1px solid @fun.color.line.value;
    color: @fun.color.ink.value;
  }

  .table tbody tr:nth-child(even) { background: @fun.color.stripe.value; }
  .table tbody tr:last-child td { border-bottom: none; }
  .table tbody tr:hover { background: #eef2ff; }

  .table .num { font-variant-numeric: tabular-nums; }
  `
}

@table-shell()
```

## The Data

```fscss
@arr(cols[name, plan, seats, mrr, status])
@arr(widths[32%, 16%, 14%, 18%, 20%])
@arr(c[count(@arr.cols!.length, 1)])

.table col.col-@arr.cols[@arr.c[]] {
  width: @arr.widths[@arr.c[]];
}

@event align(col) {
  if col: seats { return: right; }
  el-if col: mrr { return: right; }
  el { return: left; }
}

.table .cell-@arr.cols[@arr.c[]] {
  text-align: @event.align(@arr.cols[@arr.c[]]);
}
```

## The HTML

```html
<table class="table">
  <colgroup>
    <col class="col-name" />
    <col class="col-plan" />
    <col class="col-seats" />
    <col class="col-mrr" />
    <col class="col-status" />
  </colgroup>
  <thead>
    <tr>
      <th>Customer</th>
      <th>Plan</th>
      <th>Seats</th>
      <th>MRR</th>
      <th>Status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td class="cell-name">Acme Corp</td>
      <td class="cell-plan">Scale</td>
      <td class="cell-seats num">42</td>
      <td class="cell-mrr num">$1,260</td>
      <td class="cell-status"><span class="badge badge-success">Paid</span></td>
    </tr>
    <tr>
      <td class="cell-name">Globex</td>
      <td class="cell-plan">Starter</td>
      <td class="cell-seats num">8</td>
      <td class="cell-mrr num">$160</td>
      <td class="cell-status"><span class="badge badge-warning">Pending</span></td>
    </tr>
    <tr>
      <td class="cell-name">Initech</td>
      <td class="cell-plan">Scale</td>
      <td class="cell-seats num">27</td>
      <td class="cell-mrr num">$810</td>
      <td class="cell-status"><span class="badge badge-danger">Overdue</span></td>
    </tr>
  </tbody>
</table>
```

## What Happens

- `table-shell` writes the whole structure — sticky header included — as one block define.
- The column array emits one `<col>` width rule per entry, with `widths` supplying the percentages positionally.
- `@event.align` reads a column name and returns an alignment, so `seats` and `mrr` lean right without a hand-written rule each.
- Zebra striping and row hover stay plain `:nth-child`/`:hover` — the arrays only carry what genuinely repeats.

## Compile

```bash
fscss style.fscss style.css
```

## Stretch Goals

1. Add a sortable header caret that flips on a `.sorted-desc` class.
2. Generate the status pills from the same array walk used in project 16.
3. Pin the first column so it stays visible when the table scrolls sideways on mobile.

## Summary

One block define for the shell, two arrays for the columns, one event for alignment. The table grows a column by adding a single entry — the rhythm stays intact.

---

**Next: [18 — Avatar Group](../18-avatar-group.md)**
