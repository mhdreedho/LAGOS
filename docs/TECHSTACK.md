# Tech Stack — Lagos Game Center

<!-- #region backend -->
## Backend

| Komponen           | Teknologi  | Versi  |
| ------------------ | ---------- | ------ |
| Framework          | Laravel    | 13     |
| Language           | PHP        | 8.5    |
| Database           | PostgreSQL | latest |
| Realtime/Component | Livewire   | 4.x    |

<!-- #endregion -->

<!-- #region frontend -->
## Frontend

| Komponen      | Teknologi    | Versi  |
| ------------- | ------------ | ------ |
| CSS Framework | Tailwind CSS | 4.x    |
| UI Component  | Flux UI Pro  | 2.13.0 |
| JS Framework  | Alpine.js    | 3.x    |
| Build Tool    | Vite         | latest |

<!-- #endregion -->

<!-- #region iot -->
## IoT

| Komponen           | Teknologi | Keterangan                                                              |
| ------------------ | --------- | ----------------------------------------------------------------------- |
| Runtime            | Python    | 3.x                                                                     |
| Smart Plug Control | TinyTuya  | Library Python untuk kontrol BARDI smart plug 16A-WEM via local network |

<!-- #endregion -->

<!-- #region environment -->
## Environment

| Komponen            | Teknologi  | Versi/Keterangan                     |
| ------------------- | ---------- | ------------------------------------ |
| OS                  | WSL Ubuntu | 24.04                                |
| Runtime JS          | Node.js    | v24.14.0                             |
| Package Manager PHP | Composer   | 2.x                                  |
| Package Manager JS  | NPM        | Bawaan Node.js                       |
| Version Control     | Git        | latest                               |
| Remote Repository   | GitHub     | github.com/mhdreedho/LAGOS (private) |

<!-- #endregion -->

<!-- #region devtools -->
## Development Tools

| Tool                 | Keterangan                                                       |
| -------------------- | ---------------------------------------------------------------- |
| Laravel Boost        | Starter kit & scaffolding awal project                           |
| Laravel Pint         | PHP code formatter via CLI (`./vendor/bin/pint`), standar PSR-12 |
| VS Code + WSL Remote | Editor utama, dijalankan dari WSL via `code .`                   |
| Claude Desktop       | AI assistant untuk perencanaan & dokumentasi project             |
| Claude Code          | AI coding agent untuk generate & modifikasi kode                 |

<!-- #endregion -->

<!-- #region vscode-extensions -->
## VS Code Extensions

| Extension                  | Publisher          | Kegunaan                                    |
| -------------------------- | ------------------ | ------------------------------------------- |
| Alpine.js IntelliSense     | Adrian Wilczyński  | Autocomplete Alpine.js                      |
| DotENV                     | mikestead          | Highlight file `.env`                       |
| Error Lens                 | Alexander          | Tampilkan error inline di kode              |
| GitLens                    | GitKraken          | Git history & blame inline                  |
| Laravel Blade Formatter    | Shuhei Hayashibara | Format otomatis file `.blade.php` saat save |
| Laravel Blade Snippets     | Winnie Lin         | Snippets & syntax highlight Blade           |
| Laravel Extra Intellisense | amir               | Autocomplete route, view, config Laravel    |
| Livewire Language Support  | cierra             | Syntax support Livewire component           |
| Markdown All in One        | Yu Zhang           | Preview & shortcuts file `.md`              |
| Path Intellisense          | Christian Kohler   | Autocomplete path file                      |
| PHP Intelephense           | Intelephense       | Autocomplete & formatter PHP                |
| PHP Namespace Resolver     | Mehedi Hassan      | Auto-import namespace PHP                   |
| Tailwind CSS IntelliSense  | Tailwind Labs      | Autocomplete class Tailwind                 |
| Todo Tree                  | Gruntfuggly        | Tracking `// TODO:` di seluruh project      |

<!-- #endregion -->

<!-- #region vscode-settings -->
## VS Code Remote Settings

```json
{
    "editor.foldingStrategy": "indentation",
    "json.schemaDownload.enable": true,
    "[php]": {
        "editor.defaultFormatter": "bmewburn.vscode-intelephense-client",
        "editor.formatOnSave": true
    },
    "[blade]": {
        "editor.defaultFormatter": "shufo.vscode-blade-formatter",
        "editor.formatOnSave": true
    }
}
```

<!-- #endregion -->