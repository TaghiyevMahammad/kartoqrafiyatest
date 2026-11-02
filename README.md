# Kartoqrafiya Test

Kartoqrafiya və GIS üzrə interaktiv test tətbiqi. Yalnız frontend-dir (HTML, CSS, JS). Server və ya build tələb etmir.

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
- Ayarlar brauzerdə yadda qalır (`localStorage`).

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
├── index.html     # Tətbiqin özü (UI və məntiq)
├── questions.js   # Suallar bazası
└── README.md
```

## İşə salmaq

`index.html` faylını brauzerdə açmaq kifayətdir.

## Deploy

Statik saytdır. Qovluğu olduğu kimi istənilən statik hostinqə yükləmək olar:

- **Netlify:** qovluğu [app.netlify.com/drop](https://app.netlify.com/drop) səhifəsinə sürükləyin.
- **Vercel:** `vercel` əmri ilə və ya GitHub reposunu qoşmaqla.
- **GitHub Pages:** repo → Settings → Pages → Branch: `main` / root.

## Sualların formatı

Suallar `questions.js` faylındadır:

```js
window.QUESTIONS = [
  {
    "t": 1,               // mövzu
    "d": 1,               // çətinlik
    "q": "Sualın mətni",
    "a": ["Düzgün cavab", "Variant 2", "Variant 3", "Variant 4", "Variant 5"]
  }
];
```

> `a` massivində **birinci cavab həmişə düzgün cavabdır**. Ekranda variantlar qarışdırılır.

### Excel faylından yeniləmək

Excel strukturu belədir: `N | Movzu | CETINLIK | Sual | Cavab01 (düzgün) | Cavab02 … Cavab05`.

`questions.js` faylını yenidən yaratmaq üçün (Python və `openpyxl` lazımdır):

```bash
pip install openpyxl
python -c "
import openpyxl, json, re
ws = openpyxl.load_workbook('Kartoqrafiya az test (1).xlsx').active
def c(v):
    if v is None: return ''
    if isinstance(v, float) and v.is_integer(): v = int(v)
    s = re.sub(r'\s+', ' ', str(v)).strip()
    return s[1:-1].strip() if len(s) > 1 and s[0] == s[-1] == '\"' else s
out = []
for r in ws.iter_rows(min_row=2, values_only=True):
    if not c(r[3]): continue
    out.append({'t': int(r[1] or 0), 'd': int(r[2] or 0), 'q': c(r[3]), 'a': [c(x) for x in r[4:9] if c(x)]})
open('questions.js', 'w', encoding='utf-8').write('window.QUESTIONS = ' + json.dumps(out, ensure_ascii=False, indent=0) + ';\n')
print(len(out), 'sual yazıldı')
"
```

