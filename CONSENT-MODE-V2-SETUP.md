# Google Consent Mode V2 - Kompletní návod k implementaci

## 📋 Obsah
1. [Přehled](#přehled)
2. [Požadavky](#požadavky)
3. [Krok 1: Inicializace Consent Mode](#krok-1-inicializace-consent-mode)
4. [Krok 2: Google Tag Manager setup](#krok-2-google-tag-manager-setup)
5. [Krok 3: Cookie banner HTML](#krok-3-cookie-banner-html)
6. [Krok 4: CSS styling](#krok-4-css-styling)
7. [Krok 5: JavaScript logika](#krok-5-javascript-logika)
8. [Testování](#testování)
9. [GDPR compliance](#gdpr-compliance)

---

## Přehled

Tato implementace používá **opt-in model** (GDPR compliant):
- Všechny volitelné kategorie jsou defaultně **DENIED**
- Uživatel musí aktivně souhlasit (Accept All nebo Customize)
- Consent se ukládá do **localStorage**
- GTM se načítá **deferred** (po window.load) pro lepší performance

### Consent kategorie:
- ✅ `functionality_storage` - **GRANTED** (nutné pro funkčnost)
- ✅ `security_storage` - **GRANTED** (nutné pro zabezpečení)
- ❌ `analytics_storage` - **DENIED** (Google Analytics)
- ❌ `ad_storage` - **DENIED** (reklamní cookies)
- ❌ `ad_user_data` - **DENIED** (personalizované reklamy)
- ❌ `ad_personalization` - **DENIED** (personalizace reklam)
- ❌ `personalization_storage` - **DENIED** (personalizace obsahu)

---

## Požadavky

- Google Tag Manager account a Container ID (např. `GTM-XXXXXXX`)
- Základní znalost HTML, CSS, JavaScript
- Web server podporující localStorage

---

## Krok 1: Inicializace Consent Mode

**Umístění:** Hned za otevírací `<head>` tag (před vším ostatním!)

```html
<!-- Google Consent Mode V2 - Opt-in Model -->
<script>
window.dataLayer = window.dataLayer || [];
function gtag(){dataLayer.push(arguments);}

// Default consent - all DENIED except necessary (opt-in model)
gtag('consent', 'default', {
  'ad_storage': 'denied',
  'ad_user_data': 'denied',
  'ad_personalization': 'denied',
  'analytics_storage': 'denied',
  'personalization_storage': 'denied',
  'functionality_storage': 'granted',
  'security_storage': 'granted'
});

// If user has saved preferences, update them immediately
const savedConsent = localStorage.getItem('hrafnar_consent_v2');
if (savedConsent) {
  gtag('consent', 'update', JSON.parse(savedConsent));
}
</script>
```

**⚠️ DŮLEŽITÉ:**
- Tento script musí být **první** v `<head>` (před GTM!)
- LocalStorage klíč: změň `'hrafnar_consent_v2'` na vlastní název projektu
- Pattern: `consent('default')` → `consent('update')`

---

## Krok 2: Google Tag Manager setup

**Umístění:** Za Consent Mode script (stále v `<head>`)

```html
<!-- Google Tag Manager - deferred for better performance -->
<script>
window.addEventListener('load', function() {
  (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
  new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
  j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
  'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
  })(window,document,'script','dataLayer','GTM-XXXXXXX');
});
</script>
```

**Změň:**
- `GTM-XXXXXXX` → tvůj GTM Container ID

**Proč deferred loading?**
- Lepší Core Web Vitals (FCP, LCP)
- GTM se načte až po `window.load` eventu
- Consent Mode stále funguje správně

### GTM noscript fallback

**Umístění:** Hned za otevírací `<body>` tag

```html
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-XXXXXXX"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
```

---

## Krok 3: Cookie banner HTML

**Umístění:** Před zavírací `</body>` tag

```html
<!-- Cookie Consent Banner -->
<div class="cookie-banner" id="cookieBanner">
  <div class="cookie-content">
    <div class="cookie-text">
      <h3>🍪 We use cookies</h3>
      <p>We use cookies to improve your experience, analyze traffic, and show personalized ads. You can customize your preferences or accept all.</p>
    </div>
    <div class="cookie-buttons">
      <button class="btn-cookie btn-accept-all" id="acceptAll">Accept All</button>
      <button class="btn-cookie btn-reject" id="rejectOptional">Reject Optional</button>
      <button class="btn-cookie btn-customize" id="customizeBtn">Customize</button>
    </div>
  </div>
</div>

<!-- Cookie Settings Modal -->
<div class="cookie-modal" id="cookieModal">
  <div class="cookie-modal-content">
    <div class="cookie-modal-header">
      <h2>Cookie Preferences</h2>
      <button class="cookie-modal-close" id="closeModal">&times;</button>
    </div>

    <div class="cookie-modal-body">
      <p>Choose which cookies you want to allow. You can change these settings at any time.</p>

      <!-- Essential Cookies (always enabled) -->
      <div class="cookie-category">
        <div class="cookie-category-header">
          <label class="cookie-switch">
            <input type="checkbox" checked disabled>
            <span class="cookie-slider"></span>
          </label>
          <div>
            <h4>Essential Cookies</h4>
            <p>Required for the website to function. Cannot be disabled.</p>
          </div>
        </div>
      </div>

      <!-- Analytics -->
      <div class="cookie-category">
        <div class="cookie-category-header">
          <label class="cookie-switch">
            <input type="checkbox" id="consent_analytics">
            <span class="cookie-slider"></span>
          </label>
          <div>
            <h4>Analytics Cookies</h4>
            <p>Help us understand how visitors interact with our website (Google Analytics).</p>
          </div>
        </div>
      </div>

      <!-- Advertising -->
      <div class="cookie-category">
        <div class="cookie-category-header">
          <label class="cookie-switch">
            <input type="checkbox" id="consent_advertising">
            <span class="cookie-slider"></span>
          </label>
          <div>
            <h4>Advertising Cookies</h4>
            <p>Used to deliver relevant advertisements and track campaign performance.</p>
          </div>
        </div>
      </div>

      <!-- Personalization -->
      <div class="cookie-category">
        <div class="cookie-category-header">
          <label class="cookie-switch">
            <input type="checkbox" id="consent_personalization">
            <span class="cookie-slider"></span>
          </label>
          <div>
            <h4>Personalization Cookies</h4>
            <p>Allow us to remember your preferences and customize content.</p>
          </div>
        </div>
      </div>
    </div>

    <div class="cookie-modal-footer">
      <button class="btn-cookie btn-save" id="savePreferences">Save Preferences</button>
    </div>
  </div>
</div>

<!-- Floating Cookie Icon (shows when banner is hidden) -->
<button class="cookie-float-icon" id="cookieFloatIcon" title="Cookie Settings">
  🍪
</button>
```

---

## Krok 4: CSS styling

```css
/* ---- Cookie Consent Banner ---- */
.cookie-banner {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: var(--surface);
  border-top: 2px solid var(--accent);
  padding: 24px 28px;
  z-index: 100;
  display: none;
  animation: slideUp 0.4s ease-out;
}

@keyframes slideUp {
  from { transform: translateY(100%); }
  to { transform: translateY(0); }
}

.cookie-banner.show {
  display: block;
}

.cookie-content {
  max-width: 1200px;
  margin: 0 auto;
  display: flex;
  gap: 24px;
  align-items: center;
  flex-wrap: wrap;
}

.cookie-text {
  flex: 1;
  min-width: 280px;
}

.cookie-text h3 {
  margin: 0 0 8px 0;
  font-size: 18px;
  color: var(--text);
}

.cookie-text p {
  margin: 0;
  font-size: 14px;
  color: var(--muted);
  line-height: 1.5;
}

.cookie-buttons {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.btn-cookie {
  padding: 10px 20px;
  border-radius: 4px;
  border: none;
  font-weight: 600;
  font-size: 14px;
  cursor: pointer;
  transition: all 0.25s;
  font-family: inherit;
}

.btn-accept-all {
  background: var(--accent);
  color: white;
}

.btn-accept-all:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px -8px rgba(252, 77, 2, 0.5);
}

.btn-reject {
  background: transparent;
  color: var(--text);
  border: 1px solid var(--line);
}

.btn-reject:hover {
  border-color: var(--accent);
  color: var(--accent);
}

.btn-customize {
  background: transparent;
  color: var(--accent);
  border: 1px solid var(--accent);
}

.btn-customize:hover {
  background: var(--accent);
  color: white;
}

/* ---- Cookie Modal ---- */
.cookie-modal {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  z-index: 200;
  display: none;
  align-items: center;
  justify-content: center;
  padding: 20px;
  backdrop-filter: blur(4px);
}

.cookie-modal.show {
  display: flex;
}

.cookie-modal-content {
  background: var(--bg);
  border: 2px solid var(--line);
  border-radius: 12px;
  max-width: 600px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  animation: modalPop 0.3s ease-out;
}

@keyframes modalPop {
  from { transform: scale(0.9); opacity: 0; }
  to { transform: scale(1); opacity: 1; }
}

.cookie-modal-header {
  padding: 24px;
  border-bottom: 1px solid var(--line);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cookie-modal-header h2 {
  margin: 0;
  font-size: 24px;
  color: var(--text);
}

.cookie-modal-close {
  background: transparent;
  border: none;
  font-size: 32px;
  color: var(--muted);
  cursor: pointer;
  line-height: 1;
  padding: 0;
  width: 32px;
  height: 32px;
}

.cookie-modal-close:hover {
  color: var(--accent);
}

.cookie-modal-body {
  padding: 24px;
}

.cookie-modal-body > p {
  margin-top: 0;
  color: var(--muted);
}

.cookie-category {
  margin-bottom: 20px;
  padding: 16px;
  background: var(--surface);
  border-radius: 8px;
  border: 1px solid var(--line);
}

.cookie-category-header {
  display: flex;
  gap: 16px;
  align-items: flex-start;
}

.cookie-category h4 {
  margin: 0 0 6px 0;
  font-size: 16px;
  color: var(--text);
}

.cookie-category p {
  margin: 0;
  font-size: 13px;
  color: var(--muted);
  line-height: 1.4;
}

/* Toggle Switch */
.cookie-switch {
  position: relative;
  display: inline-block;
  width: 48px;
  height: 26px;
  flex-shrink: 0;
}

.cookie-switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.cookie-slider {
  position: absolute;
  cursor: pointer;
  inset: 0;
  background: var(--line);
  transition: 0.3s;
  border-radius: 26px;
}

.cookie-slider:before {
  position: absolute;
  content: "";
  height: 18px;
  width: 18px;
  left: 4px;
  bottom: 4px;
  background: white;
  transition: 0.3s;
  border-radius: 50%;
}

.cookie-switch input:checked + .cookie-slider {
  background: var(--accent);
}

.cookie-switch input:checked + .cookie-slider:before {
  transform: translateX(22px);
}

.cookie-switch input:disabled + .cookie-slider {
  opacity: 0.5;
  cursor: not-allowed;
}

.cookie-modal-footer {
  padding: 24px;
  border-top: 1px solid var(--line);
  display: flex;
  justify-content: flex-end;
}

.btn-save {
  background: var(--accent);
  color: white;
}

.btn-save:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px -8px rgba(252, 77, 2, 0.5);
}

/* ---- Floating Cookie Icon ---- */
.cookie-float-icon {
  position: fixed;
  bottom: 24px;
  right: 24px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background: var(--surface);
  border: 2px solid var(--accent);
  font-size: 24px;
  cursor: pointer;
  z-index: 90;
  display: none;
  align-items: center;
  justify-content: center;
  transition: all 0.3s;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.cookie-float-icon.show {
  display: flex;
}

.cookie-float-icon:hover {
  transform: scale(1.1) rotate(15deg);
  box-shadow: 0 6px 24px rgba(252, 77, 2, 0.3);
}

/* Mobile responsive */
@media (max-width: 768px) {
  .cookie-content {
    flex-direction: column;
    align-items: flex-start;
  }

  .cookie-buttons {
    width: 100%;
    flex-direction: column;
  }

  .btn-cookie {
    width: 100%;
  }

  .cookie-float-icon {
    bottom: 16px;
    right: 16px;
    width: 48px;
    height: 48px;
    font-size: 20px;
  }
}
```

**Poznámky k CSS:**
- Použij vlastní CSS proměnné (`--accent`, `--surface`, atd.)
- Animace slideUp a modalPop pro lepší UX
- Mobile responsive design
- Přístupnost (focus states, disabled states)

---

## Krok 5: JavaScript logika

**Umístění:** Před zavírací `</body>` tag (nebo v samostatném souboru)

```javascript
<script>
// =====================================================
// GOOGLE CONSENT MODE V2 - Cookie Management
// =====================================================

(function() {
  const CONSENT_KEY = 'hrafnar_consent_v2'; // 🔧 Změň na vlastní název projektu!

  // DOM elements
  const banner = document.getElementById('cookieBanner');
  const modal = document.getElementById('cookieModal');
  const floatIcon = document.getElementById('cookieFloatIcon');
  const acceptAllBtn = document.getElementById('acceptAll');
  const rejectOptionalBtn = document.getElementById('rejectOptional');
  const customizeBtn = document.getElementById('customizeBtn');
  const closeModalBtn = document.getElementById('closeModal');
  const savePreferencesBtn = document.getElementById('savePreferences');

  // Checkboxes
  const analyticsCheckbox = document.getElementById('consent_analytics');
  const advertisingCheckbox = document.getElementById('consent_advertising');
  const personalizationCheckbox = document.getElementById('consent_personalization');

  // Footer/header links to reopen settings
  const manageCookiesLinks = document.querySelectorAll('#manageCookiesFooter, #manageCookiesHeader');

  // Check if user has already made a choice
  const savedConsent = localStorage.getItem(CONSENT_KEY);

  if (!savedConsent) {
    // No consent saved → show banner
    banner.classList.add('show');
  } else {
    // Consent saved → show floating icon
    floatIcon.classList.add('show');
  }

  // =====================================================
  // ACCEPT ALL
  // =====================================================
  acceptAllBtn.addEventListener('click', function() {
    const consent = {
      'ad_storage': 'granted',
      'ad_user_data': 'granted',
      'ad_personalization': 'granted',
      'analytics_storage': 'granted',
      'personalization_storage': 'granted'
      // functionality_storage a security_storage jsou vždy granted
    };

    saveConsent(consent);
    hideBanner();
    showFloatIcon();
  });

  // =====================================================
  // REJECT OPTIONAL
  // =====================================================
  rejectOptionalBtn.addEventListener('click', function() {
    const consent = {
      'ad_storage': 'denied',
      'ad_user_data': 'denied',
      'ad_personalization': 'denied',
      'analytics_storage': 'denied',
      'personalization_storage': 'denied'
    };

    saveConsent(consent);
    hideBanner();
    showFloatIcon();
  });

  // =====================================================
  // CUSTOMIZE (open modal)
  // =====================================================
  customizeBtn.addEventListener('click', function() {
    openModal();
    loadSavedPreferences();
  });

  // =====================================================
  // SAVE PREFERENCES (from modal)
  // =====================================================
  savePreferencesBtn.addEventListener('click', function() {
    const consent = {
      'analytics_storage': analyticsCheckbox.checked ? 'granted' : 'denied',
      'ad_storage': advertisingCheckbox.checked ? 'granted' : 'denied',
      'ad_user_data': advertisingCheckbox.checked ? 'granted' : 'denied',
      'ad_personalization': advertisingCheckbox.checked ? 'granted' : 'denied',
      'personalization_storage': personalizationCheckbox.checked ? 'granted' : 'denied'
    };

    saveConsent(consent);
    closeModal();
    hideBanner();
    showFloatIcon();
  });

  // =====================================================
  // MANAGE COOKIES (footer/header links)
  // =====================================================
  manageCookiesLinks.forEach(link => {
    link.addEventListener('click', function(e) {
      e.preventDefault();
      openModal();
      loadSavedPreferences();
    });
  });

  // =====================================================
  // FLOAT ICON (reopen settings)
  // =====================================================
  floatIcon.addEventListener('click', function() {
    openModal();
    loadSavedPreferences();
  });

  // =====================================================
  // CLOSE MODAL
  // =====================================================
  closeModalBtn.addEventListener('click', closeModal);

  // Close modal when clicking outside
  modal.addEventListener('click', function(e) {
    if (e.target === modal) {
      closeModal();
    }
  });

  // =====================================================
  // HELPER FUNCTIONS
  // =====================================================

  function saveConsent(consent) {
    // Save to localStorage
    localStorage.setItem(CONSENT_KEY, JSON.stringify(consent));

    // Update Google Consent Mode
    if (typeof gtag === 'function') {
      gtag('consent', 'update', consent);
    }
  }

  function loadSavedPreferences() {
    const saved = localStorage.getItem(CONSENT_KEY);
    if (saved) {
      const consent = JSON.parse(saved);
      analyticsCheckbox.checked = consent.analytics_storage === 'granted';
      advertisingCheckbox.checked = consent.ad_storage === 'granted';
      personalizationCheckbox.checked = consent.personalization_storage === 'granted';
    }
  }

  function hideBanner() {
    banner.classList.remove('show');
  }

  function showFloatIcon() {
    floatIcon.classList.add('show');
  }

  function openModal() {
    modal.classList.add('show');
    document.body.style.overflow = 'hidden'; // Prevent scrolling
  }

  function closeModal() {
    modal.classList.remove('show');
    document.body.style.overflow = ''; // Restore scrolling
  }
})();
</script>
```

**⚠️ ZMĚŇ:**
- `CONSENT_KEY = 'hrafnar_consent_v2'` → vlastní název projektu (např. `'myproject_consent_v2'`)

---

## Testování

### 1. Testování v prohlížeči

**Otevři DevTools (F12) → Console:**

```javascript
// Zkontroluj saved consent
localStorage.getItem('hrafnar_consent_v2')

// Vymaž consent (resetuje banner)
localStorage.removeItem('hrafnar_consent_v2')

// Zkontroluj dataLayer
window.dataLayer
```

### 2. Google Consent Mode Debug

**Instaluj rozšíření:**
- [Google Tag Assistant](https://tagassistant.google.com/)

**Nebo použij command v Console:**
```javascript
// Zkontroluj consent state
window.dataLayer.filter(item => item[0] === 'consent')
```

### 3. GTM Preview Mode

1. Otevři GTM → Preview
2. Připoj se k webu
3. Zkontroluj "Consent State" v Debug panelu
4. Testuj všechny 3 scénáře:
   - Accept All → všechno granted
   - Reject Optional → všechno denied (kromě essential)
   - Customize → custom kombinace

### 4. Checklist

- [ ] Banner se zobrazí při první návštěvě
- [ ] Accept All → vše granted, banner zmizí, ikona se zobrazí
- [ ] Reject Optional → vše denied, banner zmizí, ikona se zobrazí
- [ ] Customize → modal se otevře
- [ ] Save Preferences → uloží volby, modal se zavře
- [ ] Floating icon → znovu otevře modal
- [ ] Footer link "Cookie Settings" → otevře modal
- [ ] Reload page → consent se zachová (localStorage)
- [ ] GTM taguje správně podle consent stavu
- [ ] Mobile responsive funguje

---

## GDPR Compliance

### Povinné náležitosti pro EU/Island:

1. **Opt-in model** ✅
   - Default = denied (kromě essential)
   - Uživatel musí aktivně souhlasit

2. **Jasné informace** ✅
   - Co jsou cookies
   - K čemu se používají
   - Jaké kategorie existují

3. **Snadné odmítnutí** ✅
   - Tlačítko "Reject Optional" na stejné úrovni jako "Accept All"

4. **Možnost změny** ✅
   - Floating icon + footer link
   - Kdykoliv může změnit preference

5. **Privacy Policy** ✅
   - Link na detailní Privacy Policy
   - Vysvětlení všech cookies
   - Kontakt na DPO (pokud je)

### Privacy Policy - musí obsahovat:

```markdown
## Cookies a sledovací technologie

Používáme následující typy cookies:

### Nezbytné cookies
- Funkce: Základní funkčnost webu
- Doba: Session / 1 rok
- Nelze odmítnout

### Analytické cookies (Google Analytics)
- Funkce: Sledování návštěvnosti
- Doba: 2 roky
- Lze odmítnout

### Reklamní cookies (Google Ads, Meta Pixel)
- Funkce: Personalizované reklamy
- Doba: 90 dní - 2 roky
- Lze odmítnout

Vaše práva podle GDPR:
- Kdykoliv můžete změnit nastavení cookies
- Můžete požádat o výmaz dat
- Kontakt: privacy@yoursite.com
```

---

## Troubleshooting

### Banner se nezobrazí
```javascript
// Console:
localStorage.removeItem('hrafnar_consent_v2');
location.reload();
```

### GTM nefunguje
- Zkontroluj GTM Container ID
- Otevři Network tab → hledej `gtm.js`
- Zkontroluj, jestli se GTM načetl po `window.load`

### Consent se neuloží
```javascript
// Console:
localStorage.setItem('test', 'value'); // Pokud error → localStorage blokován
```

### Extension ukazuje "inactive"
- Zkontroluj, že Consent Mode script je **před GTM**
- Refresh extension
- Hard reload (Ctrl+Shift+R)

---

## Poznámky

- **LocalStorage klíč:** Vždy změň na vlastní název projektu
- **Timezone:** V Google Apps Script email notifikacích změň timezone (např. `"Atlantic/Reykjavik"`, `"Europe/Prague"`)
- **Barvy:** Přizpůsob CSS proměnné (`--accent`, `--surface`, atd.)
- **Texty:** Přelož do vlastního jazyka
- **Legal:** Konzultuj s právníkem pro GDPR compliance

---

## Zdroje

- [Google Consent Mode V2 Docs](https://support.google.com/tagmanager/answer/10718549)
- [GDPR Cookie Consent Guide](https://gdpr.eu/cookies/)
- [Google Tag Manager Best Practices](https://developers.google.com/tag-platform/tag-manager/web)

---

**Vytvořeno:** 2026-06-08
**Projekt:** Hrafnar Digital
**Autor:** Claude Sonnet 4.5
