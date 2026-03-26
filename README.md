# Prepal Dish Catalog

This repository powers the dish recommendations in the **Prepal** app.
The app fetches these JSON files on every launch and caches them locally,
so users always get the latest dishes without an app update.

## File structure

```
chinese.json          ← Chinese cuisine dishes
american.json         ← American cuisine dishes
japanese.json         ← Japanese cuisine dishes
french_italian.json   ← French & Italian cuisine dishes
```

## JSON format

Each file is an array of dish objects:

```json
[
  {
    "name":        "Kung Pao Chicken",
    "emoji":       "🥘",
    "mealType":    "lunch",
    "keywords":    ["chicken", "peanut", "soy sauce", "garlic"],
    "description": "Spicy stir-fried chicken with peanuts and chilli.",
    "calories":    420
  }
]
```

| Field         | Type       | Values                            | Notes                                              |
|---------------|------------|-----------------------------------|----------------------------------------------------|
| `name`        | `String`   | Any                               | Displayed in the app                               |
| `emoji`       | `String`   | Single emoji                      | Shown as the dish icon                             |
| `mealType`    | `String`   | `"breakfast"`, `"lunch"`, `"dinner"` | Controls which tab the dish appears in          |
| `keywords`    | `[String]` | Ingredient words (lowercase)      | Used to match dishes to what's in the user's fridge|
| `description` | `String`   | 1–2 sentences                     | Shown in the suggestion card                       |
| `calories`    | `Int`      | Approximate kcal per serving      | Shown in the suggestion card                       |

## How the app uses this

1. **On launch**, the app immediately shows bundled (offline) dishes.
2. **In the background**, it fetches the latest JSON from this repo.
3. The fresh data is cached on-device so it survives offline use.
4. If the fetch fails (no network), the cached or bundled data is used.

Adding a dish is as simple as adding a new object to the relevant JSON file
and committing. The app picks it up on the user's next launch.

## Connecting the app

In `RegionalCatalogService.swift`, update this one line:

```swift
private let githubBaseURL = "https://raw.githubusercontent.com/YOUR_USERNAME/YOUR_REPO/main"
```

Replace `YOUR_USERNAME` and `YOUR_REPO` with this repository's details.
The app will then fetch:
- `…/main/chinese.json`
- `…/main/american.json`
- `…/main/japanese.json`
- `…/main/french_italian.json`

> **Important:** The repository must be **public** for `raw.githubusercontent.com` links to work without authentication.
