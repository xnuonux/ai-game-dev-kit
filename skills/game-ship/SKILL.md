---
name: game-ship
description: Use when taking a web game to a phone and into a store. Capacitor, signing, the AAB, attribution, the data-safety form, and the rating mistake that gets games delisted.
---

# Ship

Read this **before P0**, not after. Two decisions in here (package id, age rating) are expensive to
change once you have a listing, and one of them can get you removed.

---

## Web → Android

```bash
npm i @capacitor/core && npm i -D @capacitor/cli
npx cap init "My Game" com.yourstudio.mygame --web-dir=dist
npm i @capacitor/android && npx cap add android
npm run build && npx cap sync && npx cap open android
```

⚠ **The package id (`com.yourstudio.mygame`) is permanent.** It cannot be changed after your first
upload. Not the name, not the icon ... the id. Decide it deliberately.

### Config that matters

```json
{
  "appId": "com.yourstudio.mygame",
  "webDir": "dist",
  "android": { "backgroundColor": "#000000" },
  "plugins": {
    "SplashScreen": { "launchAutoHide": false, "backgroundColor": "#000000" }
  }
}
```

⚠ **`launchAutoHide: false`** then hide it yourself when the game is genuinely ready. Otherwise the
splash vanishes and the player stares at a black screen while assets load, which reads as a crash.

### Signing

```bash
keytool -genkey -v -keystore release.keystore -alias mygame \
        -keyalg RSA -keysize 2048 -validity 10000
```

⚠⚠ **Back this file up somewhere you will still have in five years. Losing the keystore means you can
never update the app again ... not "it's difficult", it is permanent.** Put it in a password manager,
not in the repo, and never in git.

Build the bundle: `./gradlew bundleRelease` → `app/build/outputs/bundle/release/app-release.aab`

---

## Attribution ... generate it, do not write it

```bash
npx license-checker --production --json > licenses.json
node scripts/gen-notices.mjs   # → src/assets/NOTICES.txt, shown in a Credits screen
```

⚠ **Fail the build on any AGPL/GPL/LGPL in the tree.** Both stores want attribution, and discovering
a copyleft dependency during review is expensive. Failing at build time is free.

---

## The Play Console checklist

- **Privacy policy URL** ... required even if you collect nothing. Say that you collect nothing.
- **Data safety form** ... ⚠ answer it honestly. If you use an ad SDK you collect an advertising ID, and
  saying otherwise is a policy violation.
- **Content rating questionnaire** ... see below.
- **Target API level** ... Google raises this annually and delists apps that fall behind.
- **Store listing** ... 2-8 screenshots, a feature graphic, a short and full description.

---

## ⚠ The rating mistake that gets games removed

**Cute art plus blood is not a children's game, and the store decides that by looking at your
listing, not your intent.**

If your art reads as "for kids" ... bright, rounded, cartoon characters ... and your content has violence
or gore, you are in the most dangerous category on the store. Google's Families policy is enforced by
automated review and human appeal is slow.

**Do this, every time:**

- Answer the content rating questionnaire **truthfully**. Violence: yes. Blood: yes. Expect T /
  PEGI 12+ or higher.
- ⚠ **Do not opt into the Families/Designed-for-Families programme.**
- Make store screenshots that show the actual content, including the violent parts. A listing that
  hides them is the thing that triggers removal.
- Set the target age range honestly in the console.

**Getting this wrong is not a moral failure, it is a delisting**, and it usually happens after launch
when you have spent your marketing.

---

## iOS

Same Capacitor path (`npx cap add ios`), plus: an Apple Developer account ($99/yr), Xcode on macOS,
and a review process that is slower and stricter. ⚠ **Ship Android first.** It is faster, cheaper, and
you will find every bug there before paying Apple's review-cycle tax.

---

## Before you upload

- [ ] Package id final and deliberate
- [ ] Keystore generated and backed up **off this machine**
- [ ] Splash hides on actual readiness, not on a timer
- [ ] `NOTICES` generated, no copyleft
- [ ] Privacy policy live at a real URL
- [ ] Data safety form matches what the app actually does
- [ ] Content rating honest; **not** in the Families programme
- [ ] Screenshots show real content
- [ ] Tested on a real mid-range device, not an emulator
- [ ] Verdict gate passed (`skills/game-verdict`)
