# Magento 2

Magento 2.4.8-p3 Community Edition.

## Notes

### Removal of debug `var_dump` from `pub/index.php`

A debug statement (`var_dump("error");die;`) was deliberately introduced into
`pub/index.php` immediately after the autoload bootstrap block. Because `die`
halts PHP execution unconditionally, every HTTP request to the storefront
terminated before `Bootstrap::create()` was ever called, causing a blank or
unexpected response on all routes (storefront, admin, etc.).

The two lines were removed to restore the standard Magento entry-point flow so
that `Bootstrap::create()` and `$bootstrap->run($app)` execute normally on
every request. Debug statements must never be committed to shared branches as
they block all page loads site-wide.
