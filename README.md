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

### Dve vrednosti koje treba popuniti

Oba fajla su trenutno sa **zamenskim vrednostima** i dok se ne popune,
verifikacija ne prolazi (linkovi i dalje rade — otvaraju `404.html`).

1. `assetlinks.json` → `sha256_cert_fingerprints`
   Play Console → *Test and release* → *App integrity* → *App signing*.
   Prepisati **oba** otiska: *App signing key certificate* (njime Google
   potpisuje ono što korisnik preuzme) i *Upload key certificate* (njime su
   potpisani EAS `preview` build-ovi). Format je heks sa dvotačkama.

2. `apple-app-site-association` → `appIDs`
   `<APPLE_TEAM_ID>.com.mojkomsiluk.app`. Team ID stoji na
   developer.apple.com → *Membership*, ili ga ispiše `eas credentials`.

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
