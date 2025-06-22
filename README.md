# PHPMX - Cookie

Controlador estático de cookies com suporte a criptografia, codificação e configurações dinâmicas.

```
composer require phpmx/cookie
```

## Utilização

Utilize a classe estática **PhpMx\Cookie** para criar, ler e gerenciar cookies de forma segura.

```php
use PhpMx\Cookie;

Cookie::set('nome', 'valor');
Cookie::get('nome');
Cookie::remove('nome');
Cookie::check('nome');
```

## Cookies seguros

Para armazenar valores de forma segura, prefixe o nome do cookie com `#`. Isso utiliza **Cif** e **Code** do [PHPMX-CORE](https://github.com/php-mx/phpmx-core).

```php
Cookie::set('#user_token', '12345');
Cookie::get('#user_token'); // retorna '12345'
```

> Cookies seguros são cifrados, validados e possuem prefixo de identidade para evitar colisão ou interceptação.

## Comportamento

- Cookies definidos como `null` serão marcados para remoção imediata
- Valores criptografados recebem o prefixo `Cookie:` internamente
- Cookies invalidos ou com dados quebrados serão automaticamente ignorados

## Configuração via .env

A classe respeita as variáveis do ambiente:

```env
COOKIE_LIFE = "+30 days"     # Tempo padrão de vida dos cookies (strtotime-compatible)
COOKIE_DOMAIN = ""           # Domínio dos cookies (opcional)
```

## Métodos

**get(string \$name): ?string**
Retorna o valor do cookie, ou `null` caso inexistente.

**set(string \$name, ?string \$value): void**
Define um cookie. Usa criptografia se nome for iniciado com `#`. Se valor for `null`, marca para remoção.

**check(string \$name): bool**
Verifica se um cookie existe e é válido.

**remove(string \$name): void**
Remove um cookie explicitamente.

## Considerações

- Cookies são armazenados de forma segura no cliente, mas ainda são passíveis de exclusão local
- Cookies seguros (prefixados com `#`) são recomendados para tokens, flags e sessões
- Cookies simples devem ser usados apenas para dados não sensíveis (como preferências de idioma)

## Exemplo completo

```php
use PhpMx\Cookie;

// Cookie seguro (criptografado)
Cookie::set('#auth', 'xyz123');
if (Cookie::check('#auth')) {
    $token = Cookie::get('#auth');
}

// Cookie público
Cookie::set('theme', 'dark');
echo Cookie::get('theme'); // dark

// Remover
Cookie::remove('theme');
```

---

[phpmx](https://github.com/php-mx) | [phpmx-core](https://github.com/php-mx/phpmx-core) | [phpmx-server](https://github.com/php-mx/phpmx-server) | [phpmx-datalayer](https://github.com/php-mx/phpmx-datalayer) | [phpmx-view](https://github.com/php-mx/phpmx-view)
