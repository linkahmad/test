
|Product|Code|Price|
|-------|----|-----|
|Red Widget|R01|$32.95|
|Green Widget|G01|$24.95|
|Blue Widget|B01|$7.95|

To incentive customers to spend more, delivery costs are reduced based on the amount spent. Orders under $50 cost $4.95. For orders under $90, delivery costs $2.95. Orders of $90 or more have free delivery.

They are also experimenting with special offers. The initial offer will be “buy one red widget, get the second half price”.

## Requirements

* PHP 8.0
* Composer
* Codeception for testing



## Usage

```php
use Acme\Sales\Entity\Cart;
use Acme\Sales\Entity\Product;
use Acme\Sales\Entity\Sales\Rule\NthPercentOffRule;
use Acme\Sales\Entity\Shipping\Method\OrderTotalBased;

// Create a new cart
$cart = new Cart();

// Instantiate catalog for the cart
$cart->getCatalog()->add(new Product('R01', 'Red Widget', 32.95));
$cart->getCatalog()->add(new Product('G01', 'Green Widget', 24.95));
$cart->getCatalog()->add(new Product('B01', 'Blue Widget', 7.95));

// Add a sales rule, every second product with code will be half price.
$cart->addSalesRule(
    new NthPercentOffRule('R01', 2, 0.5)
);

// Add available shipping methods to the cart.
$cart->addShippingMethod(
    new OrderTotalBased(0, 50, 4.95)
);
$cart->addShippingMethod(
    new OrderTotalBased(50, 90, 2.95)
);
$cart->addShippingMethod(
    new OrderTotalBased(90, null, 0)
);

// Add a product to the cart by product code
$cart->addProduct('R01');

// Get the calculated grand total
$grandTotal = $cart->getGrandTotal();

// Adding the same product to the cart twice increases quantity
$cart->addProduct('R01');

// Get the calculated grand total
$grandTotal = $cart->getGrandTotal();

// Totals available:
$subTotal = $cart->getSubTotal();
$discountTotal = $cart->getDiscountTotal();
$shippingTotal = $cart->getShippingTotal();

$cart->addProduct('G01');

// Return list of available items
$items = $cart->getItems();

foreach ($items as $item) {
    // Get the total of the row
    $rowTotal = $item->getTotal()
}

// Remove an item from the cart
$cart->removeProduct('R01');

```

## Tests

Unit tests can be with docker using `docker-compose run --rm codecept run unit`, acceptance tests are run on docker start after the web container is online.

If docker is not available codeception can be used, first install dependencies by running `composer install` in the
project folder then `vendor/bin/codecept run unit` would go through all unit tests.



```
