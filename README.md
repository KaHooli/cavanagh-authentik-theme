# Cavanagh Family Authentik Theme

A light/dark theme package for a self-hosted authentik instance using the Cavanagh Family visual identity.

Package revision: **3.9 — inline FIDO and social-source marks (14 August 2026)**

## Preferred authentik assets

- `branding/cavanagh-logo-light.svg`
- `branding/cavanagh-logo-dark.svg`
- `branding/cavanagh-icon-light.svg`
- `branding/cavanagh-icon-dark.svg`
- `branding/favicon-light.svg`
- `branding/favicon-dark.svg`
- `branding/FIDO_Passkey_mark_A_black.svg`
- `branding/FIDO_Passkey_mark_A_reverse.svg`
- `branding/background-light.webp`
- `branding/background-dark.webp`

The SVG wordmarks use a responsive 7:1 `viewBox`; the icon and favicon SVGs use
a centred 1:1 `viewBox`. They have no fixed `width` or `height` attributes and
retain their proportions with `preserveAspectRatio`. PNG and ICO files remain
in the package as compatibility and operating-system fallbacks:

- `branding/cavanagh-logo-light.png`
- `branding/cavanagh-logo-dark.png`
- `branding/cavanagh-mark-transparent.png`
- `branding/cavanagh-mark-light.png`
- `branding/cavanagh-mark-dark.png`
- `branding/favicon-light.png`
- `branding/favicon-dark.png`
- `branding/favicon.ico`
- `branding/icon-16.png` through `icon-512.png`
- `css/cavanagh-authentik.css`

## Brand palette

- Cavanagh Red: `#8F171B`
- Deep Oxblood: `#4A0C10`
- Antique Gold: `#C69A45`
- Warm Ivory: `#F5EFE3`
- Charcoal: `#151515`
- Near Black: `#0C0C0D`

## Install in authentik

These steps reflect authentik's current 2026 documentation.

1. Sign in as an authentik administrator.
2. Open **Customization > Files**.
3. In **Customization > Files**, create or open the `cavanagh-authentik`
   directory, then upload the package contents so the paths begin with
   `cavanagh-authentik/`. Do not upload only the files inside `branding`, as
   that drops the required package prefix.
4. Open **System > Brands**, edit the Brand used by your family domain, and set:
   - **Title:** `Cavanagh Family`
   - **Logo:** `cavanagh-authentik/branding/cavanagh-logo-%(theme)s.svg`
   - **Favicon:** `cavanagh-authentik/branding/favicon-%(theme)s.svg`
   - **Default flow background:** `cavanagh-authentik/branding/background-%(theme)s.webp`
5. Paste the contents of `css/cavanagh-authentik.css` into **Custom CSS** for the Brand.
6. Save the Brand.
7. Confirm the Brand's **Default authentication flow** is set to the intended
   default flow. Leaving this field empty previously caused the site to enter
   the Braavos Passkey Flow directly.
8. Open **Flows and Stages > Flows**, edit the authentication flow, then choose
   an **Appearance > Layout** that places the form/content on the right so the
   lion artwork remains visible on the left. Clear a per-flow background to
   inherit the Brand default, or set:
   `cavanagh-authentik/branding/background-%(theme)s.webp`.

   If the Braavos Passkey Flow does not resolve theme-aware backgrounds in the
   installed authentik release, retain the known-working explicit value:
   `cavanagh-authentik/branding/background-dark.webp`.
9. Test both light and dark themes in a private/incognito browser window and on mobile.

## Theme-aware paths

authentik supports `%(theme)s` in current shared file-picker fields, including
Brand Logo, Brand Favicon, Brand Default flow background, Flow Background,
Application Icon and Source Icon. It resolves `automatic` to either `light` or
`dark` on the backend before generating the asset URL.

For an Application Icon or Source Icon, use:
`cavanagh-authentik/branding/cavanagh-icon-%(theme)s.svg`.

## Image optimization

- SVG artwork has tightly bounded, responsive `viewBox` values.
- Fixed SVG dimensions are intentionally omitted.
- The wordmark uses the recommended approximate 7:1 ratio.
- Icons and favicons use a centred 1:1 ratio.
- Detailed 2560×1440 backgrounds remain WebP because they are raster artwork.
- The approved wording is **Family Single Sign-On**.
- The gold rule below **CAVANAGH** is half length, with its dot at the midpoint
  and the gold motto centred to the rule.

## Flow wordmark sizing

The supplied Custom CSS targets authentik's exposed `branding` part and retains
PatternFly fallbacks. Authentik 2026.5 applies an internal size limit to the
logo image, so the exposed branding component is scaled as a whole on desktop.
This makes the wordmark fill the available panel width while preserving the
SVG's 7:1 aspect ratio. A smaller scale is used on narrow screens, and the
spacing before the flow heading is reduced to keep the layout balanced.

## Responsive layout corrections

- The 1120–1176 px transition band uses a centred single-panel grid. This
  prevents Authentik 2026.5's wide content layout from wrapping incorrectly on
  an iPad mini in landscape while leaving the 1180 px iPad 11-inch and larger
  split layouts unchanged.
- Phones up to 576 px wide and 704 px high use compact Authentik spacing
  variables so the social sources, recovery link and footer remain reachable
  on devices such as the 375×667 iPhone SE. Form controls and tap targets keep
  their standard size.

## Passkey action

The passwordless action displays the official centred FIDO Alliance passkey
Mark A instead of the visible **Use a security key** label. Light mode uses the
official black SVG and dark mode uses the official reverse gold-and-white SVG.
Both files are included unmodified from the FIDO download package. The original
localized label remains in Authentik's document structure, so assistive
technology still announces the action correctly.

The passkey link is presented as an icon-only authentication option immediately
before the Discord, Google and Plex marks. Its former blue secondary-button box
is removed, while its 44 px clickable area and visible keyboard focus treatment
are retained. CSS anchor positioning associates the passkey link with the first
social-source button; this feature is Baseline 2026 in current browsers. On an
older browser, the safe fallback is a centred, unboxed passkey mark above the
social-source row.

Because Authentik 2026.5 serves uploaded media through temporary signed URLs,
the same unmodified SVG bytes are embedded as base64 data URLs in the Custom
CSS. This keeps the icon reliable with local and S3 storage without creating a
modified or derivative mark. The standalone source SVGs remain in `branding/`
for provenance and verification.

The passkey icon is a trademark of FIDO Alliance, Inc. Its inclusion in this
package does not indicate endorsement, official status or any relationship with
FIDO Alliance, Inc. Use of the icon remains subject to the FIDO Passkey Icon
Usage Agreement and current FIDO Logo Usage Guidelines.

## Upgrade note

Custom CSS is an advanced authentik feature. The theme primarily relies on CSS variables, but a small number of PatternFly selectors are included for buttons/cards/inputs. Review the login flow and user library after major authentik upgrades.

## Suggested flow presentation

For the supplied 2560x1440 backgrounds, a right-aligned login/content layout works best. The right half intentionally has low visual detail for readability while the lion occupies the left side.

## Official documentation consulted

- https://docs.goauthentik.io/brands/
- https://docs.goauthentik.io/brands/custom-css/
- https://docs.goauthentik.io/customize/files/
- https://docs.goauthentik.io/customize/file-picker/
- https://docs.goauthentik.io/customize/interfaces/flow/
