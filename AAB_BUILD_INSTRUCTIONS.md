# Release AAB Build (GitHub Actions + Play Store signing)

## 1) GitHub Secrets add karo

Repo → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**

| Secret name | Value |
|---|---|
| `KEYSTORE_BASE64` | `prt.jks` ka base64 (neeche command) |
| `KEYSTORE_PASSWORD` | keystore password |
| `KEY_ALIAS` | key alias (e.g. `key0`) |
| `KEY_PASSWORD` | key password |

Mac pe keystore base64:

```bash
base64 -i /Users/amityadav/Downloads/prt.jks | pbcopy
```

Clipboard me copy ho jayega — GitHub secret `KEYSTORE_BASE64` me paste karo.

## 2) Workflow push / trigger

1. Is repo me `build-aab.yml` push hona chahiye.
2. GitHub → **Actions** → **Build Release AAB** → **Run workflow**
3. Complete hone ke baad **Artifacts** → `release-aab` → `app-release.aab` download

## Notes

- `.jks` aur passwords kabhi repo me commit mat karo.
- Chat me password share karne ke baad ideally passwords rotate karo.
