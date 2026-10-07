# Good and bad tests

Examples are in Python with pytest; the ideas apply in any language.

## Good: behaviour through the public interface

```python
def test_shared_basket_shows_the_same_items():
    basket = Basket.create(owner=alice)
    basket.add(apples, quantity=2)

    link = basket.share()
    shared = Basket.open(link, viewer=bob)

    assert shared.items() == [("apples", 2)]
```

This test:

- checks something a caller cares about;
- uses only the public interface;
- keeps passing if the internals change;
- names what happens, not how;
- makes one logical assertion.

## Bad: coupled to the implementation

```python
def test_share_calls_token_generator(mocker):
    generate = mocker.patch("baskets.tokens.generate")
    Basket.create(owner=alice).share()
    generate.assert_called_once()
```

Warning signs:

- mocking the code's own modules or collaborators;
- calling private functions or methods;
- asserting how many times, or in what order, something was called;
- breaking on a refactor that does not change behaviour;
- a name that describes the mechanism rather than the outcome;
- checking the result some other way than through the interface.

## Bad: checking through a back door

```python
# Bad: reads the database directly
def test_create_basket_inserts_row(db):
    Basket.create(owner=alice)
    assert db.execute("SELECT count(*) FROM basket").scalar() == 1

# Good: checks through the interface
def test_created_basket_can_be_found_again():
    basket = Basket.create(owner=alice)
    assert Basket.get(basket.id).owner == alice
```

## Bad: tautological

```python
# Bad: works out the answer the same way the code does
def test_basket_total():
    lines = [Line(price=3), Line(price=4)]
    assert basket_total(lines) == sum(line.price for line in lines)

# Good: the expected value is a known literal
def test_basket_total():
    assert basket_total([Line(price=3), Line(price=4)]) == 7
```
