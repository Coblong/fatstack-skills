# Mocking

Mock only at the edges of the system, where the code meets something it does not control:

- external services such as payments, email, or third-party APIs;
- time and randomness;
- the database or file system, only when a real test database or temporary directory is impractical.

Use the real thing for code the project owns: its own modules, classes, and internal collaborators.

## Make the edges easy to replace

**Pass dependencies in.** A function that receives its client can be given a fake in tests; one that builds its own client cannot.

```python
# Easy to replace
def send_receipt(order, mailer):
    mailer.send(to=order.email, subject="Your receipt")

# Hard to replace
def send_receipt(order):
    mailer = ResendMailer(api_key=os.environ["RESEND_API_KEY"])
    mailer.send(to=order.email, subject="Your receipt")
```

**Give each external operation its own function.** A client with one method per operation lets each fake return one fixed shape. A single generic `request(method, path, body)` forces branching logic into every fake.

```python
# Each operation can be faked on its own
class TrackerClient:
    def create_ticket(self, title, body): ...
    def get_ticket(self, ticket_id): ...
    def set_status(self, ticket_id, status): ...
```

This keeps test setup free of conditionals, shows at a glance which operations a test uses, and gives each operation its own types.
