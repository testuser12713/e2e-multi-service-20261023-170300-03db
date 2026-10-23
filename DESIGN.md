# Design — Project Identity

> This document is project-long-lived. Tokens are not changed without
> the Architect's approval. Developers MUST use these tokens
> instead of improvising their own colors/spacings.

## Style Direction

Calm, light operations-tool look in the spirit of Linear/Stripe: white cards on a pale grey canvas, one indigo accent, Inter for UI text and JetBrains Mono for ids and results, status communicated through clear semantic badge colours.

## Colors

- `--color-bg`: **#F6F7F9**
- `--color-surface`: **#FFFFFF**
- `--color-fg`: **#111827**
- `--color-muted`: **#5B6472**
- `--color-border`: **#E3E6EB**
- `--color-accent`: **#4F46E5**
- `--color-accent_hover`: **#4338CA**
- `--color-accent_active`: **#3730A3**
- `--color-accent_soft`: **#EEF0FF**
- `--color-focus_ring`: **#818CF8**
- `--color-status_pending_bg`: **#EEF0F4**
- `--color-status_pending_fg`: **#475569**
- `--color-status_running_bg`: **#E0ECFF**
- `--color-status_running_fg`: **#1D4ED8**
- `--color-status_done_bg`: **#DCF5E6**
- `--color-status_done_fg`: **#166534**
- `--color-status_failed_bg`: **#FDE4E4**
- `--color-status_failed_fg`: **#991B1B**
- `--color-code_bg`: **#F3F4F7**
- `--color-error_banner_bg`: **#FFF1F1**
- `--color-error_banner_border`: **#F5B5B5**

## Typography

- `font_family`: "Inter", system-ui, "Segoe UI", Roboto, Arial, sans-serif
- `heading_font`: "Inter", system-ui, "Segoe UI", Roboto, Arial, sans-serif
- `mono_font`: "JetBrains Mono", ui-monospace, "Cascadia Mono", Consolas, "Liberation Mono", monospace
- `heading_weight`: 600
- `body_weight`: 400
- `size_scale`: xs 12px, sm 14px, base 16px, lg 18px, xl 22px, 2xl 28px; line-height 1.5 body, 1.25 headings; fonts loaded from design/fonts/inter/font.css and design/fonts/jetbrains-mono/font.css, self-hosted only
- Inter files: `design/fonts/inter/` — `font.css` holds its @font-face rules, `OFL.txt` its licence
- JetBrains Mono files: `design/fonts/jetbrains-mono/` — `font.css` holds its @font-face rules, `OFL.txt` its licence

## Spacing Scale

- `--space-0`: 4px
- `--space-1`: 8px
- `--space-2`: 12px
- `--space-3`: 16px
- `--space-4`: 24px
- `--space-5`: 32px
- `--space-6`: 48px

## Border-Radii

- `--radius-sm`: 4px
- `--radius-md`: 8px
- `--radius-lg`: 12px
- `--radius-pill`: 999px

## Components

### Button

Primary: bg=accent, fg white, font 14px/600, padding 12px 20px, radius md, min-height 44px. hover=accent_hover, active=accent_active plus translateY(1px), focus-visible 2px focus_ring outline with 2px offset, disabled opacity 0.5, cursor not-allowed, no hover change. Loading state (request running): disabled plus label 'Wird gesendet…'. Label for job form: 'Auftrag erstellen'. Full width on mobile.

### Textarea

Label 'Text' above (14px/500). bg surface, 1px border, radius md, padding 12px, min-height 160px, vertically resizable, font 16px (prevents mobile zoom). Focus: border accent plus 2px focus_ring. Invalid (only after touched or submit attempt): border status_failed_fg plus helper text 'Bitte gib einen Text ein.' in 13px status_failed_fg. Untouched empty form shows no error. Below right: char counter in mono 12px muted '12 / 100.000'; turns status_failed_fg when over limit and then also shows 'Der Text ist zu lang.'. Disabled during submit.

### Select

Label 'Analyse'. Native select styled like Textarea, height 44px, padding 0 12px, custom chevron icon, options 'Wörter zählen', 'Häufigste Wörter', 'Lesezeit schätzen'. Default is the first option. Same focus and disabled states as Textarea.

### StatusBadge

Pill (radius pill), padding 2px 10px, font 12px/600, with a 6px leading dot. Labels: wartend (status_pending colours, grey dot), in Arbeit (status_running colours, dot pulses 1.2s ease-in-out infinite; no animation under prefers-reduced-motion), fertig (status_done colours), fehlgeschlagen (status_failed colours). Meaning is never colour-only: label text is always shown.

### JobCard

bg surface, 1px border, radius lg, padding 16px, shadow 0 1px 2px rgba(17,24,39,0.04), gap 12px between rows. Header row: mono id '#12' (muted, 13px) + analysis name (14px/600) on the left, StatusBadge on the right; wraps below 360px. Excerpt row: first 140 chars of text, max 2 lines (line-clamp), muted 14px, trailing '…' when truncated; rendered as a plain React text node only. Result row: ResultView. Footer: meta 'Erstellt 05.03.2025, 14:32' in 12px muted. Failed card: 3px left border status_failed_fg. Cards are keyed by id so a refresh does not flicker; no hover effect since the card is not clickable.

### ResultView

Box on code_bg, radius md, padding 12px, 14px. word_count: 'Wörter: 128' with the number in mono 600. top_words: ordered list of up to 5 rows, rank, word (mono) and count with a thin proportional bar (accent_soft with accent fill, 4px high). reading_time: 'ca. 3 Min. Lesezeit' plus '(640 Wörter)' in muted. pending/running: muted placeholder 'Ergebnis folgt…' (running with a small spinner). failed: label 'Fehler' (status_failed_fg 600) plus the error message in mono 13px, wraps with overflow-wrap anywhere. All values as text nodes, never HTML.

### JobForm Card

Surface card, radius lg, padding 24px, 1px border. Title 'Neuer Auftrag' (lg/600), subline 'Text analysieren lassen' muted. Fields stacked with 16px gap: Textarea, Select, Button (primary, submit). Button disabled while text is empty (trimmed) or a request runs. After success the form clears and focus returns to the textarea. A submission error (API unreachable or 4xx/5xx) shows an inline message under the button in status_failed_fg with the typed text preserved. Polling never resets form state.

### ErrorBanner

Full-width bar below the header (role=alert), bg error_banner_bg, 1px error_banner_border, radius md, padding 12px 16px, leading warning icon. Text: 'Die API ist nicht erreichbar. Neuer Versuch läuft automatisch…'. Shown after a failed request; disappears by itself on the next successful poll (brief 'Verbindung wiederhergestellt' in status_done colours for 2s). The previously loaded list stays visible, dimmed to 0.7 opacity.

### EmptyState

Dashed 1px border box, radius lg, padding 48px 24px, centered. Simple inbox line icon 40px in muted, headline 'Noch keine Aufträge' (600), text 'Erstelle links deinen ersten Auftrag, das Ergebnis erscheint hier automatisch.' (muted). On mobile the wording is 'oben'. No button, the form is the action.

### Skeleton

Loading placeholder for JobCard: same dimensions, three bars in border colour with a 1.4s shimmer (static under prefers-reduced-motion). Three skeleton cards on the initial load only; later polls never show skeletons.

### AppHeader

Height 64px, bg surface, bottom border. Left: 24px indigo square logo mark and the product name 'Auftragsverarbeitung' (600, 18px). Right: LiveIndicator, a small dot (done green when connected, failed red when unreachable) with text 'Aktualisiert alle 3 Sek.' in 13px muted, hidden under 480px except the dot.

## Layout Principles

- Container max-width 1120px, centered, horizontal padding 16px (mobile) / 24px (from 640px) / 32px (from 1024px); page bg=bg, cards on surface.
- Breakpoints: 640px, 1024px. Mobile-first; a single column below 1024px.
- Spacing between page sections 24px, between cards in the list 12px, inside cards 16px (form card 24px). Use only the spacing scale.
- Touch targets at least 44px high for all buttons, selects and inputs.
- Format for every date and time: 'TT.MM.JJJJ, HH:mm' in 24h, browser local time, converted from the ISO 8601 UTC value (e.g. '05.03.2025, 14:32'); the seconds are only in the title attribute.
- Format for numbers: German thousands separator with a dot (1.280). Reading time: 'ca. N Min.' (whole minutes, at least 1). Job id: '#N'.
- Text and results are always rendered as plain text nodes; long words wrap (overflow-wrap anywhere).
- Fonts only self-hosted (Inter, JetBrains Mono from design/fonts/), no third-party domains, no CDN icons: icons are inline SVG.
- Visible focus ring on all interactive elements; text contrast at least 4.5:1; respect prefers-reduced-motion.
- All UI texts in German.

## Screens

### Home (/)

Layout: Two columns from 1024px: sticky form card on the left (5/12), job list on the right (7/12); on mobile the form card sits above the list in one column.

- Header: AppHeader with product name and LiveIndicator.
- Error hint: ErrorBanner directly below the header, only while the API is unreachable.
- Job form: JobForm Card with the 'Text' textarea with char counter, the 'Analyse' select and the primary button 'Auftrag erstellen' (the primary action of the page).
- Job list header: heading 'Aufträge' with a count chip (e.g. '7') and a muted subline 'Neueste zuerst'.
- Job list: a stack of JobCards, newest first, each with id, analysis, excerpt, StatusBadge, ResultView or error and creation time; updates in place every 3 seconds, status changes without flicker.
