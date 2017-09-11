# Okta JWT Verifier for PHP

## Installation
The Okta JWT Verifier can be installed through composer.

```bash
composer require okta/jwt-verifier
```

The library is also designed to use your favorite JWT library. We currently support 
[spomky-labs/jose](https://packagist.org/packages/spomky-labs/jose) and 
[firebase/php-jwt](https://packagist.org/packages/firebase/php-jwt) You will have to install one of these or create 
your own adaptor.

```bash
composer require spomky-labs/jose
```

To create your own adaptor, just implement the `Okta/JwtVerifier/Adaptors/Adaptor` in your own class.

You will also need to install a PSR-7 compliant library. We suggest that you use `guzzlehttp/psr7` in your project.

```bash
composer require guzzlehttp/psr-7
```

## Usage

```php
<?php
$jwt = 'eyJhbGciOiJSUzI1Nqd0FfRzh6X0ZsOGlJRnNoUlRuQUkweVUifQ.eyJ2ZXIiOjEsiOiJwaHBAb2t0YS5jb20ifQ.ZGrn4fvIoCq0QdSyA';

$jwtVerifier = (new \Okta\JwtVerifier\JwtVerifierBuilder())
    ->setDiscovery(new \Okta\JwtVerifier\Discovery\Oauth) // This is not needed if using oauth.  The other option is OIDC
    ->setAdaptor(new \Okta\JwtVerifier\Adaptors\SpomkyLabsJose)
    ->setAudience('api://default')
    ->setClientId('{clientId}')
    ->setIssuer('https://{yourOktaDomain}.com/oauth2/default')
    ->build();

$jwt = $jwtVerifier->verify($jwt);

dump($jwt); //Returns instance of \Okta\JwtVerifier\JWT

dump($jwt->toJson()); // Returns Claims as JSON Object

dump($jwt->getClaims()); // Returns Claims as they come from the JWT Package used

dump($jwt->getIssuedAt()); // returns Carbon instance of issued at time
dump($jwt->getIssuedAt(false)); // returns timestamp of issued at time

dump($jwt->getExpirationTime()); //returns Carbon instance of Expiration Time
dump($jwt->getExpirationTime(false)); //returns timestamp of Expiration Time

```