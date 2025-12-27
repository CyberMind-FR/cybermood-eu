# SecuBox Multi-Language System

## Overview

The SecuBox website now supports **11 languages** with automatic detection and manual selection capabilities. The internationalization (i18n) system provides seamless translation across all pages.

## Supported Languages

| Code | Language | Flag | Direction |
|------|----------|------|-----------|
| `fr` | Français (French) | 🇫🇷 | LTR |
| `en` | English | 🇬🇧 | LTR |
| `de` | Deutsch (German) | 🇩🇪 | LTR |
| `es` | Español (Spanish) | 🇪🇸 | LTR |
| `it` | Italiano (Italian) | 🇮🇹 | LTR |
| `pt` | Português (Portuguese) | 🇵🇹 | LTR |
| `nl` | Nederlands (Dutch) | 🇳🇱 | LTR |
| `zh` | 中文 (Chinese) | 🇨🇳 | LTR |
| `ja` | 日本語 (Japanese) | 🇯🇵 | LTR |
| `ar` | العربية (Arabic) | 🇸🇦 | RTL |
| `ru` | Русский (Russian) | 🇷🇺 | LTR |

## Features

### Automatic Language Detection
The system automatically detects the user's preferred language using:
1. **localStorage** - Previously selected language (highest priority)
2. **URL Parameter** - `?lang=en` in the URL
3. **Browser Language** - Navigator language settings
4. **Fallback** - Defaults to French if no preference is found

### Manual Language Selection
Users can manually select their preferred language using the language selector in the navigation bar:
- Click the language button (shows current language flag and name)
- Select from the dropdown menu
- The selection is saved to localStorage for future visits

### RTL Support
The system automatically applies right-to-left (RTL) styling for Arabic:
- Sets `dir="rtl"` on the document root
- Adds `.rtl` class to the body
- Adjusts layout accordingly

## File Structure

```
secubox-website/
├── i18n.js                          # Main i18n system script
├── i18n/                            # Translation files directory
│   ├── fr.json                      # French translations
│   ├── en.json                      # English translations
│   ├── de.json                      # German translations
│   ├── es.json                      # Spanish translations
│   ├── it.json                      # Italian translations
│   ├── pt.json                      # Portuguese translations
│   ├── nl.json                      # Dutch translations
│   ├── zh.json                      # Chinese translations
│   ├── ja.json                      # Japanese translations
│   ├── ar.json                      # Arabic translations
│   ├── ru.json                      # Russian translations
│   └── demo-auth.json               # Demo page specific translations
├── index.html                       # Main page with i18n attributes
├── demo-*.html                      # Demo pages with i18n support
└── blog/*.html                      # Blog pages with i18n support
```

## Usage

### Adding i18n to a Page

1. **Include the i18n script** before closing `</body>` tag:
```html
<!-- Multi-language System -->
<script src="/i18n.js"></script>
```

2. **Add `data-i18n` attributes** to translatable elements:
```html
<h1 data-i18n="hero.title">Sécurisez votre réseau avec SecuBox</h1>
<p data-i18n="hero.subtitle">Suite complète de sécurité...</p>
<button data-i18n="common.learnMore">En savoir plus</button>
```

3. **For HTML content**, use `data-i18n-html`:
```html
<div data-i18n-html="features.description">
  <strong>Bold text</strong> and <em>italic text</em>
</div>
```

4. **For input placeholders**, the system auto-detects:
```html
<input type="text" data-i18n="form.email" placeholder="Email">
```

### Translation File Structure

Each language JSON file uses nested keys for organization:

```json
{
  "meta": {
    "title": "SecuBox - Security Suite for OpenWrt",
    "description": "Complete security and network management suite...",
    "keywords": "OpenWrt, security, QoS, VPN, firewall..."
  },
  "nav": {
    "modules": "Modules",
    "features": "Features",
    "demo": "Demo",
    "installation": "Installation",
    "blog": "Blog"
  },
  "hero": {
    "title": "Secure your network with SecuBox",
    "subtitle": "Complete security and network management suite..."
  },
  "modules": {
    "bandwidth": {
      "name": "Bandwidth Manager",
      "description": "Advanced bandwidth management...",
      "features": [
        "8 QoS priority levels",
        "Daily/monthly quotas",
        "Automatic throttling"
      ]
    }
  }
}
```

### Accessing Translations in JavaScript

```javascript
// Get translation by key
const title = I18N.t('hero.title');
const fallback = I18N.t('unknown.key', 'Default text');

// Change language programmatically
await I18N.changeLanguage('en');

// Listen for language changes
window.addEventListener('languageChanged', (event) => {
  console.log('Language changed to:', event.detail.lang);
});
```

## Adding a New Language

1. **Create translation file** in `/i18n/` directory:
```bash
cp i18n/en.json i18n/sv.json
```

2. **Translate all values** in the new file (keep keys unchanged)

3. **Add language to i18n.js**:
```javascript
languages: {
  // ... existing languages ...
  'sv': { name: 'Svenska', flag: '🇸🇪', rtl: false }
}
```

4. The language will automatically appear in the language selector

## Best Practices

### Translation Keys
- Use **dot notation** for nested keys: `section.subsection.key`
- Use **descriptive names**: `hero.title` not `h1`
- Group related translations: `modules.bandwidth.*`
- Use **arrays for lists**: `features[0]`, `features[1]`

### Translation Content
- Keep **translations concise** for UI elements
- Maintain **consistent terminology** across pages
- Use **native speakers** for professional translations
- Consider **cultural context** (dates, measurements, idioms)
- Test **text length** - translations may be longer/shorter

### HTML Attributes
- Use `data-i18n` for **text-only** content
- Use `data-i18n-html` for **HTML content** (use sparingly)
- Place attributes on the **element with text**, not parent containers
- Avoid translating **brand names**, **code**, or **URLs**

## SEO Considerations

The i18n system automatically updates:
- `<title>` tag
- `<meta name="description">`
- `<meta name="keywords">`
- `<meta property="og:title">`
- `<meta property="og:description">`
- `lang` attribute on `<html>`

This ensures proper indexing in all supported languages.

## Browser Compatibility

The i18n system works with:
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+
- ✅ Opera 76+

Uses modern JavaScript features:
- `async/await`
- `fetch API`
- `localStorage`
- `CustomEvent`

## Performance

- Translation files are **loaded on demand**
- **15-minute cache** for frequently accessed translations
- **Lightweight** system (~8KB uncompressed)
- **No external dependencies**

## Troubleshooting

### Translations not appearing
1. Check browser console for errors
2. Verify translation file exists: `/i18n/[lang].json`
3. Verify JSON is valid (use JSONLint)
4. Ensure `data-i18n` attributes are correct
5. Clear browser cache and localStorage

### Language not detected
1. Check browser language settings
2. Clear localStorage: `localStorage.removeItem('secubox-lang')`
3. Check URL parameter: `?lang=en`
4. Verify language code in `i18n.js`

### RTL issues with Arabic
1. Verify `dir="rtl"` on `<html>` element
2. Check `.rtl` class on `<body>`
3. Review CSS for RTL-specific rules
4. Test layout with longer Arabic text

## Contributing Translations

We welcome translation contributions! To add or improve translations:

1. **Fork** the repository
2. **Edit** the translation file in `/i18n/[lang].json`
3. **Test** your changes locally
4. **Submit** a pull request with:
   - Language code in PR title: `[FR] Update French translations`
   - Description of changes
   - Any context notes

## License

The i18n system and translations are part of the SecuBox project and follow the same Apache-2.0 license.

## Links

- **Main Website**: [secubox.cybermood.eu](https://secubox.cybermood.eu)
- **GitHub Repository**: [github.com/CyberMind-FR/cybermood](https://github.com/CyberMind-FR/cybermood)
- **CyberMind**: [cybermind.fr](https://cybermind.fr)
- **Documentation**: [docs.cybermind.fr/secubox](https://docs.cybermind.fr/secubox)

---

**Made with ❤️ by CyberMind** | 🇫🇷 France | Open Source Apache-2.0
