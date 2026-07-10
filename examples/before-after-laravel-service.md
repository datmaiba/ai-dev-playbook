# Example: same prompt, with vs. without rules (PHP/Laravel)

**Prompt given to the agent:** "Add an endpoint to update a customer's credit limit."

## ❌ Without rules

```php
// app/Http/Controllers/CustomerController.php
class CustomerController extends Controller
{
    public function index() { /* ... */ }
    public function show($id) { /* ... */ }

    // added at the bottom of an already-fat controller
    public function updateCreditLimit(Request $request, $id)
    {
        $customer = Customer::find($id);              // Eloquent straight in the controller
        if ($request->user()->hasPermission('customer.write')) {  // raw permission string
            $customer->credit_limit = $request->input('limit');   // no validation, no type
            $customer->save();
            DB::table('audit_log')->insert([...]);    // inline query, second responsibility
        }
        return response()->json($customer);           // leaks the whole model
    }
}
```

Problems: business logic in the controller · Eloquent + inline SQL at the top layer · raw permission string (a typo becomes a runtime security hole) · no validation · fat controller grows forever · response leaks internal columns.

**The scary part:** this works. It updates the limit, returns 200, passes a happy-path test. Every defect is of the pay-later kind.

## ✅ With rules

```php
// routes/customer.php — one route file per entity, scoped, Permission constants
Route::middleware(scope(Permission::CUSTOMER_WRITE))->group(function () {
    Route::patch('/customers/{id}/credit-limit', UpdateCustomerCreditLimitController::class);
});
```

```php
// app/Http/Controllers/Api/UpdateCustomerCreditLimitController.php
// one API = one controller = one __invoke
final class UpdateCustomerCreditLimitController
{
    public function __construct(
        private UpdateCustomerCreditLimitService $service,
    ) {}

    public function __invoke(UpdateCreditLimitRequest $request, int $id): JsonResponse
    {
        $result = $this->service->execute($id, $request->validatedLimit());
        return response()->json(CustomerCreditResource::make($result));
    }
}
```

```php
// app/Services/Customer/Command/UpdateCustomerCreditLimitService.php
// command service: one write operation, single execute(), constructor-injected deps
final class UpdateCustomerCreditLimitService
{
    public function __construct(
        private CustomerRepository $customers,
        private AuditLogger $audit,
    ) {}

    public function execute(int $customerId, Money $limit): CustomerCredit
    {
        $customer = $this->customers->getById($customerId);   // plain object, not a live model
        $updated  = $this->customers->updateCreditLimit($customerId, $limit);
        $this->audit->creditLimitChanged($customer, $limit);
        return $updated;
    }
}
```

Same feature — but the route declares its permission at parse time, the controller is one screen tall forever, the write path is a named service you can test without HTTP, and the repository is the only thing that knows Eloquent exists.

The reviewer's job collapses to one question per file: **does anything here reach backward through the layers?**
