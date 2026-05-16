# Crumb update feed

This repo hosts the [Sparkle](https://sparkle-project.org) auto-update feed
for [Crumb](https://rogerio412.gumroad.com/l/crumb), a floating sticky-notes
app for macOS.

- **Feed URL**: <https://rogerdf30.github.io/crumb-updates/appcast.xml>
- **Landing page**: <https://rogerdf30.github.io/crumb-updates/>
- **DMG releases**: [GitHub Releases](https://github.com/RogerDF30/crumb-updates/releases)

## How a new version ships

From the Crumb project directory, run `./publish-update.sh X.Y` (e.g.
`./publish-update.sh 2.0`). That script:

1. Builds, signs, notarizes, and staples `dist/Crumb.app` + `dist/Crumb.dmg`
2. Uploads the DMG as a GitHub Release on this repo
3. Runs `generate_appcast` to regenerate `appcast.xml` with the new release
4. Commits + pushes both `appcast.xml` and any release-note HTML to `main`
5. GitHub Pages serves the updated feed within ~1 minute

Existing installs check the feed once a day and on launch — the user sees
"Update available" within a day of publish.

## Security model

Every update is signed with an **EdDSA** private key stored in the build
machine's macOS Keychain (item: `https://sparkle-project.org`, account:
`ed25519`). The matching public key is baked into Crumb's `Info.plist`
(`SUPublicEDKey`). Without that private key, no one — including someone
who compromises this repo — can publish an update users will install.

Lose the private key and you lose the ability to ship updates to existing
installs forever. **Back it up** before relying on it for production.
