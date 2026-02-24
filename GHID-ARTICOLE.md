# Ghid publicare articole – FID Blog

## Flux de lucru
1. Scrii textul articolului (sau îl trimiți lui Claude)
2. Claude creează fișierul HTML + adaugă cardul pe blog.html și index.html
3. Claude face un singur push la GitHub (o dată pe zi, batch)
4. Netlify deployează automat în ~1 minut
5. Articolul apare live pe fid.org.ro

---

## Reguli homepage (index.html)
- Afișează **maxim 3 articole** (cele mai recente)
- Cel mai nou este **primul** (stânga)
- Când adaugi al 4-lea articol: cel mai vechi card se **mută DOAR pe blog.html**

## Reguli blog.html
- Conține **TOATE articolele**, cel mai recent primul
- Comentariul `<!-- ARTICOLELE NOI SE ADAUGĂ DUPĂ ACEASTĂ LINIE -->` marchează locul

---

## Articole existente (în ordine, cel mai recent primul)
1. **orase-viitorului.html** — Viitorul este aici: orașe inteligente și inovare digitală (Urban.jpg)
2. **smart-city.html** — Inovarea digitală transformă orașele în comunități inteligente (Orasul inteligent.jpg)
3. **divertisment.html** — Inovația digitală transformă industria divertismentului (Industria divertismentului.png)

---

## Ce să trimiți lui Claude pentru un articol nou
1. Textul articolului (copy-paste din Facebook)
2. Numele fișierului imagine (ex: `Sanatate.jpg`) — imaginea trebuie pusă în `d:\GRAVITY\CLAUDE CODE\FID\`
3. Link-ul postării Facebook originale

Claude va crea automat:
- Fișierul HTML al articolului
- Cardul pe blog.html (adăugat primul)
- Cardul pe index.html (înlocuiește cel mai vechi)

---

## Structura nav (toate paginile)

Toate paginile folosesc nav identic cu index.html:
- `position: fixed`, `padding: 14px 32px`, backdrop blur
- Logo: imagine `FID logo.png` + două linii text ("Fundația pentru / Inovare Digitală")
- Lang-bar RO/EN: `position: fixed; top: 0; right: 0; padding: 4px 32px` — același padding dreapta ca nav-ul → aliniate

```html
<div class="lang-bar">
  <button class="lang-btn active" onclick="setLang('ro')">RO</button>
  <button class="lang-btn" onclick="setLang('en')">EN</button>
</div>

<nav id="mainNav">
  <a class="nav-logo" href="index.html">
    <img src="FID logo.png" alt="FID Logo">
    <div class="nav-logo-text">
      <span data-ro="Fundația pentru" data-en="Foundation for">Fundația pentru</span>
      <span data-ro="Inovare Digitală" data-en="Digital Innovation">Inovare Digitală</span>
    </div>
  </a>
  <ul class="nav-links">
    <li><a href="index.html#about" data-ro="Despre noi" data-en="About">Despre noi</a></li>
    <li><a href="index.html#projects" data-ro="Proiecte" data-en="Projects">Proiecte</a></li>
    <li><a href="index.html#financing" data-ro="Finanțări" data-en="Funding">Finanțări</a></li>
    <li><a href="blog.html" data-ro="Noutăți" data-en="News">Noutăți</a></li>
    <li><a href="index.html#contact" data-ro="Contact" data-en="Contact">Contact</a></li>
  </ul>
  <button class="hamburger" id="hamburgerBtn" aria-label="Meniu" onclick="toggleMobile()">
    <span></span><span></span><span></span>
  </button>
</nav>

<div class="mobile-menu" id="mobileMenu">
  <a href="index.html#about" onclick="closeMobile()">Despre noi</a>
  <a href="index.html#projects" onclick="closeMobile()">Proiecte</a>
  <a href="index.html#financing" onclick="closeMobile()">Finanțări</a>
  <a href="blog.html" onclick="closeMobile()">Noutăți</a>
  <a href="index.html#contact" onclick="closeMobile()">Contact</a>
</div>
```

---

## Structura header articol (article-hero)

Categoria și butonul "Înapoi la blog" sunt pe același rând:
- Categorie → **stânga** (aliniată cu marginea stângă a imaginii)
- "Înapoi la blog" → **dreapta** (aliniat cu marginea dreaptă a imaginii)
- Săgeata folosită: `<polyline points="15 18 9 12 15 6"/>` (chevron ‹), `width="16" height="16"`, `stroke-width="2"`

```html
<div class="article-hero">
  <div class="article-header-top">
    <div class="article-category" data-ro="CATEGORIE RO" data-en="CATEGORIE EN">CATEGORIE RO</div>
    <a class="back-link" href="blog.html">
      <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"/></svg>
      <span data-ro="Înapoi la blog" data-en="Back to blog">Înapoi la blog</span>
    </a>
  </div>
  <h1 class="article-title" data-ro="TITLU RO" data-en="TITLU EN">TITLU RO</h1>
  <div class="article-meta" data-ro="ZZ luna AAAA · FID – Fundația pentru Inovare Digitală" data-en="Month DD, YYYY · FID – Foundation for Digital Innovation">ZZ luna AAAA · FID – Fundația pentru Inovare Digitală</div>
</div>
```

---

## Butoane CTA în articole

### "Comentează pe Facebook" → stil identic cu "Proiectele noastre" din hero
- Orange, clip-path, hover → teal + translateY(-2px)

```css
.article-cta a {
  display: inline-block;
  background: var(--orange);
  color: white;
  font-family: 'Syne', sans-serif;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 16px 36px;
  text-decoration: none;
  transition: all 0.25s;
  clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
}
.article-cta a:hover { background: var(--teal-light); color: var(--teal-dark); transform: translateY(-2px); }
```

### "← Vezi proiectele FID" → stil identic cu "Contactează-ne" din hero
- Transparent, border teal, clip-path, hover → teal fill

```css
.btn-fid-secondary {
  display: inline-block;
  background: transparent;
  color: var(--teal-dark);
  border: 2px solid var(--teal-dark);
  font-family: 'Syne', sans-serif;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  padding: 15px 36px;
  text-decoration: none;
  transition: all 0.25s;
  clip-path: polygon(0 0, calc(100% - 10px) 0, 100% 10px, 100% 100%, 10px 100%, 0 calc(100% - 10px));
}
.btn-fid-secondary:hover { background: var(--teal-dark); color: white; transform: translateY(-2px); }
```

HTML:
```html
<div class="article-cta">
  <p data-ro="TEXT RO" data-en="TEXT EN">TEXT RO</p>
  <a href="LINK_FACEBOOK" target="_blank" rel="noopener"
     data-ro="Comentează pe Facebook →" data-en="Comment on Facebook →">
    Comentează pe Facebook →
  </a>
</div>
<div style="text-align:center;margin-top:32px;">
  <a href="index.html#projects" class="btn-fid-secondary"
     data-ro="← Vezi proiectele FID" data-en="← See FID projects">
    ← Vezi proiectele FID
  </a>
</div>
```

---

## Structura unui card de blog (referință)
```html
<a class="blog-card" href="FISIER.html">
  <div class="blog-card-img">
    <div class="blog-card-img-inner" style="background:none;">
      <img src="IMAGINE.jpg" alt="ALT TEXT" style="width:100%;height:100%;object-fit:cover;">
    </div>
    <div class="blog-card-category" data-ro="CATEGORIE RO" data-en="CATEGORIE EN">CATEGORIE RO</div>
  </div>
  <div class="blog-card-body">
    <div class="blog-card-date" data-ro="ZZ luna AAAA" data-en="Month DD, YYYY">ZZ luna AAAA</div>
    <div class="blog-card-title" data-ro="TITLU RO" data-en="TITLU EN">TITLU RO</div>
    <div class="blog-card-excerpt" data-ro="REZUMAT RO" data-en="REZUMAT EN">REZUMAT RO</div>
  </div>
</a>
```

---

## Git – comenzi manuale (dacă e nevoie)
```bash
cd "d:/GRAVITY/CLAUDE CODE/FID"
git add .
git commit -m "Adaug articol: [titlu]"
git push
```
