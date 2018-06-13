# reCAPTCHA PHP client library

[![Build Status](https://travis-ci.org/google/recaptcha.svg)](https://travis-ci.org/google/recaptcha)
[![Latest Stable Version](https://poser.pugx.org/google/recaptcha/v/stable.svg)](https://packagist.org/packages/google/recaptcha)
[![Total Downloads](https://poser.pugx.org/google/recaptcha/downloads.svg)](https://packagist.org/packages/google/recaptcha)

* Project page: http://www.google.com/recaptcha/
* Repository: https://github.com/google/recaptcha
* Version: 1.1.3
* License: BSD, see [LICENSE](LICENSE)

## Description

reCAPTCHA is a free CAPTCHA service that protect websites from spam and abuse.
This is Google authored code that provides plugins for third-party integration
with reCAPTCHA.

## Installation

### Composer (Recommended)

[Composer](https://getcomposer.org/) is a widely used dependency manager for PHP
packages. This reCAPTCHA client is available on Packagist as
[`google/recaptcha`](https://packagist.org/packages/google/recaptcha) and can be
installed either by running the `composer require` command or adding the library
to your `composer.json`. To enable Composer for you project, refer to the
project's [Getting Started](https://getcomposer.org/doc/00-intro.md)
documentation.

To add this dependency using the command, run the following from within your
project directory:
```
composer require google/recaptcha "~1.1"
```

Alternatively, add the dependency directly to your `composer.json` file:
```json
"require": {
    "google/recaptcha": "~1.1"
}
```

### Direct download (no Composer)

If you wish to install the library manually (i.e. without Composer), then you
can use the links on the main project page to either clone the repo or download
the [ZIP file](https://github.com/google/recaptcha/archive/master.zip). For
convenience, an autoloader script is provided in `src/autoload.php` which you
can require into your script instead of Composer's `vendor/autoload.php`. For
example:

```php
require('/path/to/recaptcha/src/autoload.php');
$recaptcha = new \ReCaptcha\ReCaptcha($secret);
```

The classes in the project are structured according to the
[PSR-4](http://www.php-fig.org/psr/psr-4/) standard, so you may of course also
use your own autoloader or require the needed files directly in your code.

### Development install

If you would like to contribute to this project or run the unit tests on within
your own environment you will need to install the development dependencies, in
this case that means [PHPUnit](https://phpunit.de/). If you clone the repo and
run `composer install` from within the repo, this will also grab PHPUnit and all
its dependencies for you. If you only need the autoloader installed, then you
can always specify to Composer not to run in development mode, e.g. `composer
install --no-dev`.

*Note:* These dependencies are only required for development, there's no
requirement for them to be included in your production code.

## Usage

First, register keys for your site at https://www.google.com/recaptcha/admin

When your app receives a form submission containing the `g-recaptcha-response`
field, you can verify it using:
```php
<?php
$recaptcha = new \ReCaptcha\ReCaptcha($secret);
$resp = $recaptcha->verify($gRecaptchaResponse, $remoteIp);
if ($resp->isSuccess()) {
    // verified!
    // if Domain Name Validation turned off don't forget to check hostname field
    // if($resp->getHostName() === $_SERVER['SERVER_NAME']) {  }
} else {
    $errors = $resp->getErrorCodes();
}
```

You can see an end-to-end working example in
[examples/example-captcha.php](examples/example-captcha.php)

## Upgrading

### From 1.0.0

The previous version of this client is still available on the `1.0.0` tag [in
this repo](https://github.com/google/recaptcha/tree/1.0.0) but it is purely for
reference and will not receive any updates.

The major changes in 1.1.0 are:
* installation now via Composer;
* class loading also via Composer;
* classes now namespaced;
* old method call was `$rc->verifyResponse($remoteIp, $response)`, new call is
  `$rc->verify($response, $remoteIp)`

## Contributing

We accept contributions via GitHub Pull Requests, but all contributors need to
be covered by the standard Google Contributor License Agreement. You can find
instructions for this in [CONTRIBUTING](CONTRIBUTING.md)
<div class="news-detail">
				<span class="news-date-time">15.05.2016</span>
				<h3>Интеграция ReCaptcha в 1С-Битрикс. Несколько ReCaptcha на одной странице.</h3>
					Многие сталкивались с тем, что стандартная Captcha от Битрикс не справляется со спам ботами, как не настраивай ее. Даже, сделав ее практически не читаемой людьми, боты все-равно ее обходят.
<br><br>
Выход только в том, чтобы использовать капчи более продвинутые, логические и т.п., которых нет в Битриксе. 
<br><br>
Очень удобный и довольно простой вариант - это ReCaptcha от Google. Помимо всего, у нее выходят новые версии, если вдруг ее начнут обходить какие-либо боты, но пока она неуязвима.
<br>
<center><img src="/upload/images/recap.gif" alt="Интеграция ReCaptcha в 1С-Битрикс. Несколько ReCaptcha на одной странице" style="max-width:300px;"></center>
<br>
Установить ее "простым смертным" будет не просто, но если вы умеете создавать инфоблоки новостей в 1С-Битрикс, то осилите и интеграцию ReCaptcha в Битрикс.
<br><br>
Делаем все по шагам:
<br><br>
1. Заходим в личный кабинет ReCaptcha в Google <a rel="nofollow" target="_blank" href="https://www.google.com/recaptcha/admin#list">https://www.google.com/recaptcha/admin#list</a>, для этого у вас должен быть аккаунт в гугл, т.е. заведенная почта. И в этом личном кабинете добавляем сайт, на который встраиваем ReCaptcha. После добавления сайта вы получите 2-а ключа, которые нам нужны дальше.
<br><br>
2. Скачиваем библиотеку ReCaptcha тут <a rel="nofollow" target="_blank" href="https://github.com/google/recaptcha">https://github.com/google/recaptcha</a>, далее из этой библиотеки берем только файлы из директории /SRC/ autoload.php и директорию /ReCaptcha/ и кjпируем их в /bitrix/php_interface/include/ (директорию /include/ нужно создать, ее там изначально нет).
<br><br>
3. Далее в init,php подключаем капчу с ключами, вставляя следующий код в самом начале файла, ну после &lt;?  :
<code>
@require_once 'include/autoload.php';
define("RE_SITE_KEY","6Lfi5R8TAAAAACpUg7U2IJjuTVMGglUsz-fgTgdu");
define("RE_SEC_KEY","6Lfi5R8TAAAAADVYQPeYcECtyFAQATCoIkruBdhrbn"); 
</code>
Ключи заменяем соответственно на свои, которые получили в личном кабинете.
<br><br>
4. Далее в шаблоне сайта или компонента подключаем js с Google:
<code>
&lt;script src="https://www.google.com/recaptcha/api.js" async defer&gt;&lt;/script&gt;
</code>
<br>
5. В шаблоне компонента (template.php), в месте отображения капчи вставляем код:
<code>
&lt;div class="g-recaptcha" data-sitekey="&lt;?=RE_SITE_KEY?&gt;"&gt;&lt;/div&gt;
</code>
<br>
6. В компоненте добавляем проверку, т.е. в файл /bitrix/components/bitrix/компонент/component.php (но только, лучше создать свое пространство имен, чтобы обновлениями не затереть все)
<code>
$recaptcha = new \ReCaptcha\ReCaptcha(RE_SEC_KEY);
$resp = $recaptcha-&gt;verify($_REQUEST['g-recaptcha-response'], $_SERVER['REMOTE_ADDR']);

if (!$resp-&gt;isSuccess()){
foreach ($resp-&gt;getErrorCodes() as $code) {
echo "Ошибка! Проверка не пройдена.";
echo $code;
return;
}
}
</code>

Теперь капча должна работать.


<h2>Как подключить несколько ReCaptcha на одной странице.</h2>

Кто пробовал уже поняли, что на странице будет отображаться только одна капча, а если у нас, например, 2-е и более форм на одной странице?! Решение тоже есть.
<br><br>
1, 2, 3 и 6-й шаги делаем, как описано выше.
<br><br>
4. Далее в КОНЦЕ шаблона сайта или компонента подключаем js с Google:
<code>
&lt;script src="https://www.google.com/recaptcha/api.js?onload=onloadCallback&amp;render=explicit" async defer&gt;&lt;/script&gt;
</code>
<br>
5. В шаблоне компонента (template.php), в месте отображения капчи вставляем код:
<code>
&lt;!--Первая форма...
  &lt;div id="example1"&gt;&lt;/div&gt;

&lt;!--Вторая форма...
  &lt;div id="example2"&gt;&lt;/div&gt;

&lt;!--Третья форма...
  &lt;div id="example3"&gt;&lt;/div&gt;
</code>
<br>
6. Все как описано выше.
<br><br>
7. Вставляем javascript инициализации, у Google в примере он находится в head, но мы размещали и после всех форм, т.к. нужно было генерировать после цикла foreach и все тоже работало. Код следующий:

<code>
&lt;script type="text/javascript"&gt;
var verifyCallback = function(response) {
alert(response);
};
var widgetId1;
var widgetId2;
var onloadCallback = function() {
    widgetId1 = grecaptcha.render('example1', {
    'sitekey' : '&lt;?=RE_SITE_KEY?&gt;',
    });
    widgetId2 = grecaptcha.render(document.getElementById('example2'), {
    'sitekey' : '&lt;?=RE_SITE_KEY?&gt;'
    });
    grecaptcha.render('example3', {
    'sitekey' : '&lt;?=RE_SITE_KEY?&gt;',
    });
};
&lt;/script&gt;
</code>
