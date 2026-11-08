# Test Platforması

Fənlər üzrə interaktiv test tətbiqi. Yalnız frontend-dir (HTML, CSS, JS). Server və ya build tələb etmir.

## Fənlər

| Fənn | Sual | Status |
|---|---|---|
| AR Konstitusiyası və hüququn əsasları (00149, EN) | 222 | Aktiv |
| Kartoqrafiya və GIS | – | Sessiya bitmişdir |

Girişdə fənn seçilir. Sessiyası bitmiş fənlər siyahıda qalır, amma açılmır.

## Xüsusiyyətlər


- **Sualları qarışdır:** suallar təsadüfi ardıcıllıqla gəlir.
- **Variantları qarışdır:** cavabların yeri hər dəfə dəyişir.
- **Sual nömrələri:** sualın bazadakı nömrəsini göstərmək və ya gizlətmək olur.
- **Sual aralığı və sayı:** məsələn, 20–80 aralığından 30 sual. Hazır seçimlər: 10, 20, 30, 50, hamısı.
- **Nəticə rejimi:**
  - *Anında:* cavab verən kimi düzgün və ya səhv olduğu göstərilir.
  - *Sonda:* nəticələr test bitəndə göstərilir, o vaxta qədər cavabı dəyişmək olar.
- **Səhv suallara qayıt:** səhv və cavabsız suallardan yeni test yaranır.
- **Cavabların təhlili:** hər sual düzgün cavabı ilə göstərilir. Hamısı, səhv, cavabsız və düzgün bölmələrinə görə süzmək olur.
- **Sual xəritəsi:** istənilən suala birbaşa keçmək olur.
- **Açıq və qaranlıq tema.**
- **Mobilə uyğun dizayn.**
- Ayarlar hər fənn üçün ayrıca brauzerdə yadda qalır (`localStorage`).

## Klaviatura qısayolları

| Düymə | Funksiya |
|---|---|
| `Space` / `Enter` | Növbəti sual (sonuncu sualda testi bitirir) |
| `1`–`5` və ya `A`–`E` | Cavab seçmək |
| `←` / `→` | Əvvəlki / növbəti sual |
| `Enter` (ayarlar ekranında) | Testə başlamaq |
| `Esc` | Pəncərəni bağlamaq |

## Fayl strukturu

```
├── index.html        # Tətbiqin özü (UI və məntiq)
├── konstitusiya.js   # Konstitusiya fənninin sualları
└── README.md
```

## İşə salmaq

`index.html` faylını brauzerdə açmaq kifayətdir.

## Deploy

Statik saytdır. Qovluğu olduğu kimi istənilən statik hostinqə yükləmək olar:

- **Netlify:** qovluğu [app.netlify.com/drop](https://app.netlify.com/drop) səhifəsinə sürükləyin.
- **Vercel:** `vercel` əmri ilə və ya GitHub reposunu qoşmaqla.
- **GitHub Pages:** repo → Settings → Pages → Branch: `main` / root.

## Yeni fənn əlavə etmək

1. Yeni fayl yaradın, məsələn `fenn.js`:

```js
window.SUBJECTS = window.SUBJECTS || [];
window.SUBJECTS.push({
  "id": "fenn",                 // unikal açar
  "name": "Fənnin tam adı",
  "short": "Qısa ad",           // naviqasiyada görünür
  "desc": "Qısa təsvir",
  "questions": [
    { "q": "Sualın mətni", "a": ["Düzgün cavab", "Variant 2", "Variant 3", "Variant 4", "Variant 5"] }
  ]
});
```

> `a` massivində **birinci cavab həmişə düzgün cavabdır**. Ekranda variantlar qarışdırılır. İstəyə görə `t` (mövzu) və `d` (çətinlik, 1–3) sahələri də əlavə etmək olar.

2. `index.html`-də `<script src="konstitusiya.js"></script>` sətrinin yanına `<script src="fenn.js"></script>` əlavə edin.

3. Fənnin sessiyası bitəndə onun faylını silin və adını `index.html`-dəki `CLOSED` siyahısına əlavə edin. O zaman fənn "Sessiya bitmişdir" yazısı ilə göstərilir.
