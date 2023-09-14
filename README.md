# Digiflazz PHP Library
[![Latest Stable Version](http://poser.pugx.org/nurfaizfy/digiflazz-php-library/v)](https://packagist.org/packages/nurfaizfy/digiflazz-php-library)
[![Total Downloads](http://poser.pugx.org/nurfaizfy/digiflazz-php-library/downloads)](https://packagist.org/packages/nurfaizfy/digiflazz-php-library)
[![License](http://poser.pugx.org/nurfaizfy/digiflazz-php-library/license)](https://packagist.org/packages/nurfaizfy/digiflazz-php-library)

This library is unofficial Digiflazz API written with PHP.

-   [Documentation](#documentation)
-   [Installation](#installation)
-   [Usage](#usage)
-   [Methods' Signature and Examples](#methods-signature-and-examples)
    -   [Balance](#balance)
        -   [Get Balance](#get-balance)
    -   [Price List](#price-list)
        -   [Get Price List](#get-price-list)
    -   [Deposit](#deposit)
        -   [Create Deposit Ticket](#create-deposit-ticket)
    -   [Transaction](#transaction)
        -   [Create Transaction](#create-transaction)
        -   [Inquiry Postpaid](#inquiry-postpaid)
        -   [Pay Postpaid](#pay-postpaid)
        -   [Inquiry PLN](#inquiry-pln)
    -   [Callback](#callback)
        -   [Get Callback](#get-callback)
-   [Exceptions](#exceptions)
    -   [InvalidArgumentException](#invalidargumentexception)
    -   [ApiException](#apiexception)
-   [Contributing](#contributing)

---

## Documentation

For the API documentation, check [Digiflazz API Reference](https://developer.digiflazz.com/api/).

## Installation

Install digiflazz-php-library with composer by following command:

```bash
composer require nurfaizfy/digiflazz-php-library
```

or add it manually in your `composer.json` file.

## Usage

Configure package with your account's secret key obtained from [Digiflazz Dashboard](https://member.digiflazz.com/buyer-area/connection/api).

```php
use Mdigi\Digiflazz;
Digiflazz::initDigiflazz('username', 'apikey');
```

## Methods' Signature and Examples

### Balance

#### Get Balance

```php
\Mdigi\Balance::getBalance();
```

Usage example:

```php
$getBalance = \Mdigi\Balance::getBalance();
var_dump($getBalance);
```

### Price List

#### Get Price List

```php
\Mdigi\PriceList::getPrePaid(); // Prepaid product
\Mdigi\PriceList::getPostPaid(); // Postpaid product
```

Usage example:

```php
$priceList = \Mdigi\PriceList::getPrePaid();
var_dump($priceList);
```

### Deposit

#### Create Deposit Ticket

```php
\Mdigi\Deposit::createDeposit(array $params);
```

Parameters for this method
| Name | Required | Description |
| --- | --- | --- |
| `amount_bank` | `yes` | The deposit amount |
| `bank` | `yes` | The name of the destination bank to which your transfer will be made (BRI, BCA, MANDIRI) |
| `owner_name` | `yes` | The name of the account holder who made the deposit transfer to Digiflazz |

Usage example:

```php
$params = [
    'amount' => '200000',
    'bank' => 'BCA',
    'owner_name' => 'Digiflazz',
];
$createDeposit = \Mdigi\Deposit::createDeposit($params);
var_dump($createDeposit);
```

### Transaction

#### Create Transaction

```php
\Mdigi\Transaction::createTransaction(array $params);
```

Parameters for this method
| Name | Required | Description |
| --- | --- | --- |
| `buyer_sku_code` | `yes` | Product SKU |
| `customer_no` | `yes` | Customer number |
| `ref_id` | `yes` | Your unique reference ID |

Usage example:

```php
$params = [
    'buyer_sku_code' => 'xl10',
    'customer_no' => '08123456789',
    'ref_id' => 'some1d',
];
$createTrasaction = \Mdigi\Transaction::createTransaction($params);
var_dump($createTrasaction);
```

#### Inquiry Postpaid

```php
\Mdigi\Transaction::inquiryPostpaid(array $params);
```

Parameters for this method
| Name | Required | Description |
| --- | --- | --- |
| `buyer_sku_code` | `yes` | Product SKU |
| `customer_no` | `yes` | Customer number |
| `ref_id` | `yes` | Your unique reference ID |

Usage example:

```php
$params = [
    'buyer_sku_code' => 'xl10',
    'customer_no' => '08123456789',
    'ref_id' => 'some1d',
];
$pascaInquiry = \Mdigi\Transaction::inquiryPostpaid($params);
var_dump($pascaInquiry);
```

#### Pay Postpaid

```php
\Mdigi\Transaction::payPostpaid(array $params);
```

Parameters for this method
| Name | Required | Description |
| --- | --- | --- |
| `buyer_sku_code` | `yes` | Product SKU |
| `customer_no` | `yes` | Customer number |
| `ref_id` | `yes` | Your unique reference ID |

Usage example:

```php
$params = [
    'buyer_sku_code' => 'xl10',
    'customer_no' => '08123456789',
    'ref_id' => 'some1d',
];
$payPasca = \Mdigi\Transaction::payPostpaid($params);
var_dump($payPasca);
```

#### Inquiry PLN

```php
\Mdigi\Transaction::inquiryPLN(array $params);
```

Parameters for this method
| Name | Required | Description |
| --- | --- | --- |
| `customer_no` | `yes` | Customer number |

Usage example:

```php
$params = [
    'customer_no' => '123456789',
];
$iquiryPLN = \Mdigi\Transaction::inquiryPLN($params);
var_dump($iquiryPLN);
```

### Callback

#### Get Callback

Use this method to get Callback
```php
\Mdigi\Callback::getCallback();
```

Use this method to get JSON Callback
```php
\Mdigi\Callback::getJsonCallback();
```

## Exceptions

### InvalidArgumentException

`InvalidArgumentException` will be thrown if the argument provided by user is not sufficient to create the request.

For example, there are required arguments such as `ref_id`, `customer_no`, and `buyer_sku_code` to create an transaction. If user lacks one or more arguments when attempting to create one, `InvalidArgumentException` will be thrown.

`InvalidArgumentException` is derived from PHP's `InvalidArgumentException`. For more information about this Exception methods and properties, please check [PHP Documentation](https://www.php.net/manual/en/class.invalidargumentexception.php).

### ApiException

`ApiException` wraps up Digiflazz API error. This exception will be thrown if there are errors from Digiflazz API side.

To get exception message:

```php
try {
    $transaction = \Mdigi\Transaction::createTransaction(array $params);
} catch (\Mdigi\Exceptions\ApiException $e) {
    var_dump($e->getMessage());
}
```

To get exception HTTP error code:

```php
try {
    $transaction = \Mdigi\Transaction::createTransaction(array $params);
} catch (\Mdigi\Exceptions\ApiException $e) {
    var_dump($e->getCode());
}
```

To get exception Digiflazz API error code:

```php
try {
    $transaction = \Mdigi\Transaction::createTransaction(array $params);
} catch (\Mdigi\Exceptions\ApiException $e) {
    var_dump($e->getErrorCode());
}
```

## Contributing

For any requests, bugs, or comments, please open an [issue](https://github.com/nurfaizfy/digiflazz-php-library/issues)

### Installing Packages

Before you start to code, run this command to install all of the required packages. Make sure you have `composer` installed in your computer

```bash
composer install
```

There is a pre-commit hook to run phpcs and phpcbf. Please make sure they passed before making commits/pushes.
