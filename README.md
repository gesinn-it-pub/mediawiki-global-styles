# mediawiki-global-styles

Global styles for MediaWiki: corrections of MediaWiki core, Bootstrap and extension styles that are the same in every
product and every MediaWiki line that uses them, and the theme variables of the semantic::core wikis. The styles are
SCSS for the [Chameleon](https://github.com/ProfessionalWiki/chameleon) skin.

## Use

The repository is added as a git submodule (`context/mediawiki-global-styles`) to the semantic::core image
repositories. The image copies `styles/` to `resources/src/mediawiki.gesinn-it.global.styles/` and registers the files
in `$egChameleonExternalStyleModules` at the position `afterMain`; `_theme-default.scss` is the theme file
(`$egChameleonThemeFile`) that defines the variables.

## Content

- `_theme-default.scss`: the theme variables (colours, grays, state colours). The only place where a colour token is
  defined.
- `extension-<Name>.scss`: corrections for one extension, one extension per file.
- `bootstrap-*.scss`, `mediawiki.scss`, `mediawiki-ooui.scss`: corrections of Bootstrap and MediaWiki core markup.

## Rules

- A style belongs here only if it is the same for every product and every MediaWiki line that uses it. A fix for one
  MediaWiki line only belongs in the image repository of that line, with its reason and a condition for removing it.
- Colour, font, spacing, border and radius values come from the theme variables. A literal is allowed for a deliberate
  pin only, with a comment that says which value is pinned and why.
- No new `!important`; win by specificity and position.
- Use the classes that MediaWiki, Chameleon and the extensions document, not generated ids or `nth-child` chains.

## Development

```shell
npm install
npm run stylelint
```

`npm run stylelint` reports `!important` and hex colours outside the theme files as warnings. The number of warnings
that exist today is the budget in `package.json` (`--max-warnings`); it may only go down. A new violation fails the
check; when old ones are removed, lower the number in the same commit. A deliberate pin is marked with
`// stylelint-disable-next-line color-no-hex -- <reason>`; a disable without a reason is reported. The same
`.stylelintrc.json` is used in every repository that holds styles.

Changed styles take effect after the SCSS is compiled again, for example with `touch LocalSettings.php`.
