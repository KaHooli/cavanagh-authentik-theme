# Authentik frontend compatibility

Last reviewed: **11 September 2026**

This theme was reviewed against the current stable Authentik **2026.8.2** frontend and the upstream `main` frontend as it existed on 10 September 2026.

> Authentik does not currently publish a 2026.9 release line. As of this review, 2026.8.x is the current stable line; upstream development has already moved beyond it toward the next release line.

## Compatibility findings

### Flow footer branding

Authenik 2026.8 renders the flow footer through `ak-brand-links`, which appends a generated **Powered by authentik** text item after any configured Brand footer links. In content-left/content-right layouts the footer is placed in the opposite content column, which is why this text can appear over the left-hand Cavanagh artwork instead of at the bottom of the page.

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

Authenik 2026.8.2 retains the 70rem desktop/content-layout breakpoint and the `content_left` / `content_right` layout model on which the Cavanagh iPad transition-band correction depends. The existing 70rem–73.5rem compatibility rule remains applicable.

The `--ak-c-login__*` variables used by that correction are component-local implementation details, however, so this section should continue to be rechecked after each major Authentik frontend release.

### Passkey and social-source placement

The passwordless action still renders as a PatternFly button with `data-ouia-component-id="passwordless"`, and the Identification stage still uses the `login-sources` fieldset and `source-button` classes used by the Cavanagh FIDO/passkey positioning rules.

The existing FIDO mark replacement and CSS anchor-positioning enhancement therefore remain compatible with the reviewed frontend.

### PatternFly and Authentik CSS architecture

Authenik 2026.8 introduced substantial CSS organization work, including cascade-layer changes. Current upstream documentation describes PatternFly 4 as a compatibility layer and recommends Authentik's semantic `--ak-*` design tokens and exposed `::part()` surfaces as the long-term public theming API.

The Cavanagh theme already prefers `::part()` for the flow structure, but still contains several PatternFly selectors and component-local `--ak-c-*` variables for deliberate compatibility fixes. These remain functional in the reviewed versions, but should be treated as fallbacks rather than permanent API.

A future cleanup can progressively migrate palette-level overrides to the newer semantic variables as they become part of the stable release line, while keeping the existing PatternFly fallbacks for older installations.

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
