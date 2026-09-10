# Authentik frontend compatibility

Last reviewed: **11 September 2026**

This theme was reviewed against the current stable Authentik **2026.8.2** frontend and the upstream `main` frontend as it existed on 10 September 2026.

> Authentik does not currently publish a 2026.9 release line. As of this review, 2026.8.x is the current stable line; upstream development has already moved beyond it toward the next release line.

## Compatibility findings

### Flow footer branding

Authentik 2026.8 renders the flow footer through `ak-brand-links`, which appends a generated **Powered by authentik** text item after any configured Brand footer links. In content-left/content-right layouts the footer is placed in the opposite content column, which is why this text can appear over the left-hand Cavanagh artwork instead of at the bottom of the page.

`css/cavanagh-authentik.css` now hides only the generated final text item:

```css
ak-brand-links [part="list-item"][data-kind="text"]:last-child {
  display: none !important;
}
```

Configured Brand footer links are not hidden.

### Flow executor parts

The theme's preferred selectors remain valid in Authentik 2026.8.2 and current upstream frontend code. The flow executor continues to expose the structural parts used by this theme, including `branding`, `main`, `footer`, and `locale-select`.

The theme should continue preferring these exposed parts over internal DOM selectors whenever possible.

### Responsive content layout

Authentik 2026.8.2 retains the 70rem desktop/content-layout breakpoint and the `content_left` / `content_right` layout model on which the Cavanagh iPad transition-band correction depends. The existing 70rem–73.5rem compatibility rule remains applicable.

The `--ak-c-login__*` variables used by that correction are component-local implementation details, however, so this section should continue to be rechecked after each major Authentik frontend release.

### Passkey and social-source placement

The passwordless action still renders as a PatternFly button with `data-ouia-component-id="passwordless"`, and the Identification stage still uses the `login-sources` fieldset and `source-button` classes used by the Cavanagh FIDO/passkey positioning rules.

The existing FIDO mark replacement and CSS anchor-positioning enhancement therefore remain compatible with the reviewed frontend.

### PatternFly and Authentik CSS architecture

Authentik 2026.8 contains substantial frontend/CSS organization changes, including cascade-layer work. The **2026.8.2 stable documentation still documents the existing `--ak-global--*`, PatternFly 4, and PatternFly 5 compatibility variables**, so the Cavanagh theme deliberately keeps its current palette and compatibility fallbacks for this release.

Upstream development after 2026.8 is moving toward shorter semantic variables such as `--ak-color-primary`, `--ak-color-accent`, and `--ak-color-link`, with PatternFly variables increasingly treated as compatibility implementation details. The current upstream guidance also recommends exposed `::part()` surfaces for structural customization.

The Cavanagh theme already prefers `::part()` for the flow structure. Its remaining PatternFly selectors and component-local `--ak-c-*` variables are retained only where they are still needed for button/card/input styling and targeted layout fixes. A later release can migrate palette-level overrides to the newer semantic variables once those APIs are part of the stable Authentik release line, while keeping appropriate fallbacks for older installations.

## Upgrade test checklist

After each Authentik major frontend update, verify:

- desktop `content_right` / `content_left` login layout;
- iPad mini landscape and larger iPad landscape layouts;
- compact-height phone flow scrolling;
- light and dark Cavanagh wordmarks/backgrounds;
- primary button, form-control, card, and focus styling;
- FIDO passkey mark placement before the first social source;
- Discord, Google, and Plex source alignment;
- configured Brand footer links, if any; and
- absence of the generated **Powered by authentik** text on the Cavanagh artwork.
