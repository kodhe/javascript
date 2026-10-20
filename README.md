# Kodhe Javascript

A PHP-side JavaScript/jQuery code generator package — a refactored version of the CodeIgniter 3 `Javascript` + `Jquery` libraries (namespace `Kodhe\Framework\Javascript`). The `Javascript` class is a *driver-agnostic wrapper* that forwards calls to the active JS driver (default: jQuery), while `Jquery` contains the concrete implementation for events, effects, and `<script>` tag output.

> ⚠️ **Legacy**: since CI 3.0 this library is deprecated in CodeIgniter. For new projects it is better to write modern JS yourself; use this package only to maintain legacy applications.

## Installation

```bash
composer require kodhe/javascript
```

Requirements: PHP >= 8.1. Works fully inside a CodeIgniter application (requires a `kodhe()` instance and the CI loader for `$this->CI->load->library(...)`).

## Quick Start

```php
<?php

declare(strict_types=1);

// Within a CI/Kodhe framework application context:
$js = new Kodhe\Framework\Javascript\Javascript();

echo $js->ready(
    $js->click('#tombol', "alert('Halo!');")
);
// <script type="text/javascript">
// $(document).ready(function(){ ... });
// </script>
```

## Directory Structure

```
src/
├── Javascript.php   # Wrapper/facade; forwards methods to the driver ($this->js)
└── Jquery.php       # jQuery driver: events, effects, class operations, script(), etc.
```

## Usage

### 1. Choosing the driver & autoloading the core script

```php
$js = new Javascript([
    'js_library_driver' => 'jquery', // driver loaded via the CI loader
    'autoload'          => true,     // immediately prints <script src="...jquery...">
]);
```

### 2. Event handlers

All events accept an element selector (default `'this'`) and a code string:

```php
echo $js->change('#kota', 'hitungOngkir();');
echo $js->click('#delete', 'return confirm("Are you sure?");', false);
echo $js->hover('#menu', "$('#submenu').show()", "$('#submenu').hide()");
echo $js->keyup('#cari', 'saran()');
```

Available: `blur, change, click, dblclick, error, focus, hover, keydown, keyup, load, mousedown, mouseout, mouseover, mouseup, resize, scroll, unload`.

### 3. Effects & class manipulation

```php
echo $js->addClass('.card', 'active');
echo $js->removeClass('.card', 'active');
echo $js->animate('#ball', ['left' => '200px'], 'slow');
echo $js->fadeIn('#panel', 'fast');
echo $js->fadeOut('#panel', 'fast', 'cleanup();');
echo $js->slideUp('#drawer');
echo $js->slideDown('#drawer', 'medium');
echo $js->slideToggle('#drawer');
echo $js->hide('#elemen');
echo $js->toggle('#elemen');
```

### 4. Wrapping with document-ready

```php
echo $js->ready($js->click('#simpan', 'submitForm();'));
```

### 5. Writing script tags / jQuery assets

```php
echo $js->script();                              // <script src=".../js/jquery.js">
echo $js->script('assets/plugins/mask.js', true);// path relative to the js base
echo $js->plugin('nifty-corner');                // load a jQuery plugin
echo $js->ui('draggable');                       // load a jQuery UI component
echo $js->effect('blind');                       // load a jQuery UI effect
echo $js->modal('#dialog');                      // setup modal window
echo $js->sortable('#list', ['constraint' => 'vertical']);
echo $js->tablesorter('#table');                 // make the table sortable
echo $js->corner('#card', 'rounded-small');
```

### 6. Raw output

```php
echo $js->output("console.log('running');"); // wraps in <script> automatically
```

## API Reference (summary)

| Group | Method |
|---|---|
| Event | `blur change click dblclick error focus hover keydown keyup load mousedown mouseout mouseover mouseup resize scroll unload` |
| Efek | `animate fadeIn fadeOut fadeTo slideUp slideDown slideToggle hide show toggle switchClass` |
| Class | `addClass removeClass toggleClass prependContent appendContent addDomEvent` |
| Aset | `script plugin ui effect modal corner sortable tablesorter zebraTables` |
| Util | `ready output alert confirm prompt window_open` |

All methods on `Javascript` are delegated to the driver (`Jquery`), so the APIs are identical.

## CodeIgniter 3 Compatibility

It is a functional copy of `CI_Javascript`/`CI_Jquery`; only the class name & namespace changed (`Kodhe\Framework\Javascript\Javascript`). The constructor still calls `$this->CI->load->library('Javascript/'.$driver)`, so it needs the CI loader. Outside CI (without `kodhe()`), instantiating `Jquery` directly is not recommended because it depends on framework helpers.

## Notes

- Historically deprecated; no new features are planned.
- The `'this'` element is only valid within the DOM event context of the old CI library.

## Testing

```bash
vendor/bin/phpunit --filter Javascript
```
