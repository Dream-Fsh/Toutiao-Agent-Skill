---
name: toutiao-account-import
description: Import and verify Toutiao advertiser accounts in Chuangliang's Programmatic Batch / Juyu Advertising workflow. Use when asked to search, batch-search, select, validate, or diagnose one or more Toutiao media account IDs at the `媒体账户` module before configuring campaign details.
---

# Toutiao-账户导入

Search and select only the requested Toutiao media accounts in the visible `选择媒体账户` dialog. Stop after the dialog successfully confirms; do not configure ads, preview, or submit.

## Inputs and Boundary

- Require one or more numeric account IDs. Normalize whitespace-separated input to unique IDs; reject blank, duplicate, or non-numeric values.
- If an expected account count is supplied, require it to equal the parsed unique-ID count before interacting with the page.
- Do not infer accounts from a previous run, account remarks, search suggestions, or a similar ID.
- The page permits at most 50 selected accounts (`已选(n/50)`). Stop before selection if the request exceeds this limit.

## Workflow

### Mandatory Search Routing

- For exactly one requested account ID, use only the ordinary `搜索媒体账户` control, verify the exact `ID：<account-id>` row, then check that row.
- For two or more requested account IDs, use only `批量搜索`, enter one ID per line, verify the complete exact result set, then use the visible table-header checkbox to select all results. Do not search or check multi-account requests one at a time.

1. On `程序化批量/巨量广告`, open the first `更改` control in the `媒体账户` module. Scope all following locators to the visible `选择媒体账户` dialog.
2. Confirm that `搜索媒体账户`, the account-ID selector, the `批量搜索` control, the results table, `已选(n/50)`, and the dialog `确定` button are visible.
3. For a single account, enter its ID in `搜索媒体账户`, run the page search, and verify that the returned row displays the exact `ID：<account-id>` value.
4. For two or more accounts, click `.cl-search-input__suffix-icon[title="批量搜索"]` once. It is a toggle and is not retry-safe.
5. Continue only after the batch tooltip exposes `请输入账户ID（n/1000）`, the multiline input `请粘贴或输入账户ID，回车可换行`, the `精确匹配` statement, and its `搜索` button. If any is absent, stop and report the selector contract mismatch.
6. Enter one requested ID per line and click the tooltip's `搜索`. Wait for the result table and `共 n 条` count to settle.
7. Extract the displayed `ID：` values and compare them with requested IDs as exact sets. Stop and report missing, duplicate, unexpected, unauthorized, or ambiguous IDs; never select a partial result.
8. With an exact result set, inspect the table-header checkbox. If it is not checked, click it once to select all returned rows; do not select rows individually.
9. Prove that every requested ID is selected and `已选(n/50)` equals the requested count. Only then click the dialog's `确定`.
10. Verify the dialog closes and the main-page `媒体账户` state changes from `请选择媒体账户` to the confirmed account set/count (for example, `已选2个`).

## State Gates and Retry Safety

| Step | Required post-action proof | Retry rule |
| --- | --- | --- |
| Open selector | Visible dialog titled `选择媒体账户` | Do not use a same-text control outside the dialog. |
| Batch mode | Tooltip has multiline input, `精确匹配`, and `搜索` | Do not click the batch toggle again until its closed state is proven. |
| Search | `共 n 条` and visible result rows have settled | Re-read rows and count after every search; never use a stale table. |
| Select all | Header checkbox is checked; `已选(n/50)` equals request count | Inspect state before any retry; select-all is non-idempotent. |
| Confirm | Dialog closes; main account state is populated | A click or toast is not proof. Do not continue if the dialog remains open. |

Use visible semantic DOM controls only. Screenshots may diagnose a selector mismatch but must not be used for coordinate clicking, force-clicking hidden nodes, or selecting a similar account.

## Completion Report

Report requested IDs, result IDs, selected IDs, selected count, and the final dialog-close state. Explicitly distinguish an empty result (`共 0 条`) from a successful import; an empty search is an account-access or account-data exception, not a completed import.
