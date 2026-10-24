# mojkomsiluk-privacy

Sajt aplikacije **Moj Komšiluk** — https://mojkomsiluk.com (GitHub Pages).

## `.well-known/` — klikabilni deljeni linkovi

Aplikacija deli linkove oblika `https://mojkomsiluk.com/post/<id>`
(i `/listing/<id>`, `/event/<id>`). Da bi ih telefon otvorio u aplikaciji
umesto u pregledaču, ovde stoje dva fajla:

| fajl | za koga | čemu služi |
|---|---|---|
| `.well-known/assetlinks.json` | Android | App Links verifikacija (`autoVerify`) |
| `.well-known/apple-app-site-association` | iOS | Universal Links |

`.nojekyll` u korenu je **obavezan**: bez njega Jekyll preskače foldere koji
počinju tačkom, pa `.well-known/` nikad ne stigne na sajt.

### Vrednosti su upisane 24.10.2026.

| gde | vrednost | odakle |
|---|---|---|
| `assetlinks.json` | otisak **App signing key**-a (`D6:BE:…`) | Play Console → *App integrity* → *App signing* |
| `assetlinks.json` | otisak **Upload key**-a (`BC:60:…`) | isto mesto |
| `apple-app-site-association` | `N8353YL692.com.mojkomsiluk.app` | Apple Team ID |

Oba otiska su potrebna, ne jedan: Google potpisuje svojim ključem ono što
korisnik preuzme iz Play-a, a EAS `preview` build-ovi nose upload ključ. Bez
drugog otiska App Links **ne radi u internom testu**, pa izgleda kao da je sve
pokvareno.

Menjaju se samo ako se promeni ključ za potpisivanje ili Apple nalog.

### Provera posle objave

```sh
curl -sI https://mojkomsiluk.com/.well-known/assetlinks.json
curl -sI https://mojkomsiluk.com/.well-known/apple-app-site-association
```

Oba moraju da vrate **200**, bez preusmeravanja. Android traži
`Content-Type: application/json`; za fajl bez ekstenzije GitHub Pages to
**ne garantuje**, pa iOS stranu proveriti i Apple-ovim alatom
(Xcode → *Diagnostics*, ili `https://app-site-association.cdn-apple.com/a/v1/mojkomsiluk.com`).

## `404.html`

GitHub Pages je statičan i nema rutu `/post/<id>`, pa svaki deljeni link
pada na ovu stranicu. To je namerno: ona prepozna vrstu i id iz putanje i
ponudi dugme `mojkomsiluk://` — tako linkovi rade i na build-u 11, koji App
Links ne poznaje, i na telefonima bez aplikacije (bedževi prodavnica).
