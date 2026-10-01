# Trivia Quiz App

An Android trivia quiz app built with **Kotlin + Jetpack Compose**, using the
[Open Trivia Database API](https://opentdb.com/) to fetch multiple-choice questions.

## Features
- Home screen with app intro
- Category & difficulty selection (fetched live from `api_category.php`)
- 10 multiple-choice questions per quiz, options shuffled
- **15-second countdown timer per question** — an unanswered question when
  time runs out is automatically counted as wrong
- Instant green/red answer feedback after selecting
- Progress bar + "Question X of 10" indicator
- Final score screen with a performance message
- "Play Again" (new questions) and "Back to Home" options

## Architecture
- **MVVM**: `QuizViewModel` exposes a single `StateFlow<QuizUiState>` consumed by all screens
- **Repository pattern**: `TriviaRepository` wraps the Retrofit API and decodes URL-encoded text
- **Navigation-Compose**: `TriviaNavGraph` wires Home → Category → Quiz → Result, sharing one ViewModel instance
- **Retrofit + Gson** for networking, **Coroutines** for async calls

```
app/src/main/java/com/example/triviaquiz/
├── MainActivity.kt
├── model/TriviaModels.kt          # API DTOs + domain QuizQuestion
├── network/
│   ├── TriviaApiService.kt        # Retrofit endpoints
│   └── RetrofitInstance.kt
├── repository/TriviaRepository.kt # Fetch + URL-decode + shuffle options
├── viewmodel/QuizViewModel.kt     # State, scoring, timer logic
└── ui/
    ├── navigation/                # Screen routes + NavHost
    ├── screens/                   # Home, Category, Quiz, Result
    └── theme/                     # Material3 theme
```

## API
- Questions: `https://opentdb.com/api.php?amount=10&category={id}&difficulty={easy|medium|hard}&type=multiple&encode=url3986`
- Categories: `https://opentdb.com/api_category.php`
- No API key required. `encode=url3986` is used so text is URL-encoded (decoded
  in `TriviaRepository`) instead of dealing with raw HTML entities.

## Opening the project
1. Open **Android Studio** (Koala/2024.1 or newer recommended).
2. **File → Open** and select the `TriviaQuizApp` folder.
3. Android Studio will detect there's no Gradle wrapper jar bundled and will
   offer to **create the Gradle wrapper automatically** — accept it (or go to
   `File → Sync Project with Gradle Files`). This project only ships the
   text-based Gradle config files; the binary wrapper jar isn't included.
4. Let Gradle sync and download dependencies (needs internet access).
5. Run on an emulator or physical device (minSdk 24 / Android 7.0+).

## Notes
- Requires internet access at runtime (the `INTERNET` permission is already declared).
- Built and tested against Kotlin 1.9.24, AGP 8.5.2, Compose BOM 2024.06.00.
