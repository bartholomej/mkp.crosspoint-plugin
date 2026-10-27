<div align="center">

# Czech E-books (MKP) for CrossPoint 📚

#### Browse and download the free e-books of the Municipal Library of Prague (Městská knihovna v Praze) straight on a CrossPoint e-reader — Xteink X4, X4 Pro or X3. _(unofficial)_

</div>

---

## ✨ Features

- 📖 **3,200+ free e-books** — Czech classics, poetry, drama, children's books and more from [mlp.cz](https://www.mlp.cz/katalog/davka/NFW-EKNIHY-VOLNE)
- 🗂️ **Lists** — Newly added, Classics, Poetry, Drama, Kids, Youth, In English, Everything by author
- 🔍 **Search on the device** — works without diacritics too (`capek povidky`); open any list and press **Search** on its first row
- ℹ️ **Details before download** — annotation, year and file size
- ⬇️ **Downloads** — EPUBs go into `/MKP` on the SD card, named `Author - Title.epub`
- 🙈 **PDF-only titles are hidden** — the reader can't open them

Works on Xteink X4, X4 Pro and X3 with CrossPoint Reader firmware that supports SD-card plugins.

## 📦 Installation

### Plugin Hub

The easiest way to install and update.

1. Turn on **Settings → Connectivity → Plugins**.
2. Open **Home → Plugins → Plugin Hub**. If it isn't installed yet, **Home → Plugins** offers to install it.
3. Open the **Community Plugins** catalog (refresh it if needed), find **Czech E-books (MKP)** and choose **Install**. New versions show up there as **Update**.
4. Open **Home → Plugins → Czech E-books (MKP)**.

Without Plugin Hub, install it manually.

### Manually

1. Put the reader in **File Transfer** mode and, from a clone of this repo on the same Wi-Fi, run:

   ```bash
   yarn install
   yarn upload   # or: yarn upload 192.168.1.23 — the IP shown on the reader
   ```

2. Turn on **Settings → Connectivity → Plugins**.
3. Open **Home → Plugins → Czech E-books (MKP)**.

<details>
<summary>Without Node.js</summary>

Copy the files from `mkp.crosspoint-plugin/` to `/plugins/mkp/` on the SD card with a card reader, or upload them with curl while the reader is in **File Transfer** mode:

```bash
H=crosspoint.local   # or the IP shown on the reader
curl -X POST -d "name=plugins&path=/" http://$H/mkdir
curl -X POST -d "name=mkp&path=/plugins" http://$H/mkdir
for f in mkp.crosspoint-plugin/*; do
  curl -X POST -d "path=/plugins/mkp/$(basename "$f")" http://$H/delete   # older firmware can't overwrite
  curl -X POST -F "file=@$f" "http://$H/upload?path=/plugins/mkp"
done
```

</details>

## ⚙️ How it works

The plugin is a declarative `device.json` — no code runs on the reader. It reads the library's catalog through a small proxy that prepares it for the reader.

Volumes of multi-part works get their part number and name in the title, and files are named with the author, because the reader silently overwrites a file with the same name.

## 🔒 Privacy

The proxy keeps nothing but a one-hour in-memory cache of catalog pages and doesn't log requests. Search terms are passed on to the library's catalog.

## ⚖️ License

Code: [MIT](LICENSE). The e-books and catalog data belong to the Municipal Library of Prague; this project isn't affiliated with it.
