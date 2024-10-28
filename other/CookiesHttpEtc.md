# HTTP-куки

**HTTP cookie** (web cookie, куки браузера) — это небольшой фрагмент данных, который сервер отправляет браузеру пользователя. Браузер может сохранить этот фрагмент у себя и отправлять на сервер с каждым последующим запросом. Это, в частности, позволяет узнать, с одного ли браузера пришли несколько запросов (например, для аутентификации пользователя). С помощью кук можно сохранить любую информацию о состоянии, HTTP-протокол сам по себе этого делать не умеет.

Куки часто используются для:

- Управления сеансом (логины, корзины для виртуальных покупок)
- Персонализации (пользовательские предпочтения)
- Трекинга (отслеживания поведения пользователей)

До недавнего времени куки использовались в качестве хранилища информации на стороне пользователя. Это могло иметь смысл в отсутствии вариантов, но теперь, когда в распоряжении браузеров появились различные API для хранения данных, это уже не так. Из-за того, что куки пересылаются с каждым запросом, они могут ухудшать производительность (особенно при использовании мобильных сетей). В качестве хранилищ данных на стороне пользователя вместо них можно использовать **Web Storage API** (localStorage и sessionStorage) и **IndexedDB**.

> **Примечание:** Чтобы посмотреть сохранённые куки и другие хранилища данных, которые использует веб-страница, можно использовать **Storage Inspector** (Инспектор хранилища) в инструментах разработчика.

---

## Создание куки

Получив HTTP-запрос, вместе с ответом сервер может отправить заголовок `Set-Cookie`. Куки обычно запоминаются браузером и посылаются в HTTP-заголовке `Cookie` с каждым новым запросом к одному и тому же серверу. Можно задать срок действия кук, а также срок их жизни, после которого куки не будут отправляться. Также можно указать ограничения на путь и домен, то есть указать, в течение какого времени и к какому сайту они будут отсылаться.

### Заголовки Set-Cookie и Cookie

Заголовок `Set-Cookie` HTTP-ответа используется для отправки куки с сервера в клиентское приложение (браузер). Простой куки может задаваться так:
Set-Cookie: <имя-куки>=<заголовок-куки>



Этот заголовок с сервера даёт клиенту указание сохранить куки (это делают, например, PHP, Node.js, Python и Ruby on Rails). Ответ, отправляемый браузеру, содержит заголовок `Set-Cookie`, и куки запоминаются браузером.

Пример:

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry
```

Теперь с каждым новым запросом к серверу при помощи заголовка Cookie браузер будет возвращать серверу все сохранённые ранее куки:

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```
### Сессионные cookie
Простой cookie, пример которого приведён выше, представляет собой сессионный cookie (session cookie) — такие cookie удаляются при закрытии клиента, то есть существуют только на протяжении текущего сеанса, поскольку атрибуты Expires или Max-Age для него не задаются. Однако, если в браузере включено автоматическое восстановление сеанса, что случается очень часто, куки сеанса могут храниться постоянно, как если бы браузер никогда не закрывался.

### Постоянные cookies
Постоянные cookie (permanent cookies) удаляются не с закрытием клиента, а при наступлении определённой даты (атрибут Expires) или после определённого интервала времени (атрибут Max-Age).

Пример:
`Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;`

## Secure ("безопасные") и HttpOnly куки
"Безопасные" (secure) куки отсылаются на сервер только тогда, когда запрос отправляется по протоколу SSL и HTTPS.
Однако важные данные никогда не следует передавать или хранить в куках, поскольку сам их механизм весьма уязвим в отношении безопасности,
а флаг secure никакого дополнительного шифрования или средств защиты не обеспечивает. Начиная с Chrome 52 и Firefox 52, незащищённые сайты (http:) не могут создавать куки с флагом Secure. 

Куки HTTPonly не доступны из JavaScript через свойства Document.cookie API, что помогает избежать межсайтового скриптинга (XSS). Устанавливайте этот флаг для тех кук, к которым не требуется обращаться через JavaScript. В частности, если куки используются только для поддержки сеанса, то в JavaScript они не нужны, так что в этом случае следует устанавливать флаг HttpOnly.

`Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly`