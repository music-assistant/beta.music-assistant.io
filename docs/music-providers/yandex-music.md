# Yandex Music Provider ![Preview image](../assets/icons/yandex-music-icon.svg){ width=50 align=right }

=== "English"

    Music Assistant has support for [Yandex Music](https://music.yandex.ru). Contributed and maintained by [TrudenBoy](https://github.com/TrudenBoy).

    This provider is built on top of the [yandex-music-api](https://github.com/MarshalX/yandex-music-api) library.

    !!! note
        A Yandex Music Plus subscription is required for lossless (FLAC) quality. Standard accounts can stream up to high quality (320 kbps MP3).

=== "Русский"

    Music Assistant поддерживает [Yandex Music](https://music.yandex.ru). Разработка и поддержка — [TrudenBoy](https://github.com/TrudenBoy).

    Провайдер построен на основе библиотеки [yandex-music-api](https://github.com/MarshalX/yandex-music-api).

    !!! note
        Для воспроизведения в lossless-качестве (FLAC) необходима подписка Yandex Music Plus. Без подписки доступно качество до 320 кбит/с MP3.

## Features

=== "English"

    |           |                     |
    |:-----------------------|:---------------------:|
    | Subscription FREE | Yes (with limitations) |
    | Self-Hosted Local Media | No |
    | Media Types Supported | Artists, Albums, Tracks, Playlists |
    | [Recommendations](../ui.md#view-home) Supported | Yes |
    | Lyrics Supported | Yes |
    | [Radio Mode](../ui.md#track-menu) | Yes |
    | Maximum Stream Quality | Lossless FLAC (with Plus subscription) |
    | Login Method | Token |

    ### Other

    - Searching the Yandex Music catalogue is possible
    - Items in a users Yandex Music library will be synced to Music Assistant
    - Adding/removing items to/from the Music Assistant library will sync back to Yandex Music
    - Browse is available to explore the Yandex Music catalogue
    - **My Wave** — personalized infinite radio powered by Yandex's AI
    - **Picks & Mixes** — curated playlists organized by mood, activity, era, genre, and season
    - **Extended recommendations** — Made for You, Chart, New Releases, New Playlists, and more on the home page
    - **Lyrics** — synced (LRC) and plain text lyrics

=== "Русский"

    |           |                     |
    |:-----------------------|:---------------------:|
    | Бесплатная подписка | Да (с ограничениями) |
    | Локальные медиафайлы | Нет |
    | Поддерживаемые типы | Исполнители, Альбомы, Треки, Плейлисты |
    | [Рекомендации](../ui.md#view-home) | Да |
    | Тексты песен | Да |
    | [Режим радио](../ui.md#track-menu) | Да |
    | Максимальное качество | Lossless FLAC (с подпиской Plus) |
    | Способ входа | Токен |

    ### Дополнительно

    - Поиск по каталогу Yandex Music
    - Библиотека пользователя Yandex Music синхронизируется с Music Assistant
    - Добавление/удаление элементов в библиотеке Music Assistant синхронизируется обратно в Yandex Music
    - Раздел Browse для навигации по каталогу Yandex Music
    - **Моя волна** — персонализированное бесконечное радио на основе ИИ Яндекса
    - **Подборки и Миксы** — плейлисты по настроению, активности, эпохе, жанру и сезону
    - **Расширенные рекомендации** — Собрано для вас, Чарт, Новинки, Новые плейлисты и другое на главной странице
    - **Тексты песен** — синхронизированные (LRC) и обычные тексты

## Audio Quality

=== "English"

    Four quality tiers are available:

    | Quality | Format | Bitrate | Requirements |
    |---------|--------|---------|--------------|
    | Efficient | AAC | ~64 kbps | Free account |
    | Balanced (default) | AAC | ~192 kbps | Free account |
    | High | MP3 | ~320 kbps | Free account |
    | Superb | FLAC | Lossless | Plus subscription |

    You can change the quality at any time in the provider settings. The actual codec and bitrate are selected automatically when playback starts, based on what Yandex Music offers for each track.

    Encrypted FLAC tracks are decrypted on-the-fly as data arrives. Seeking is handled by Music Assistant core via ffmpeg.

=== "Русский"

    Доступны четыре уровня качества:

    | Качество | Формат | Битрейт | Требования |
    |----------|--------|---------|------------|
    | Efficient | AAC | ~64 кбит/с | Бесплатный аккаунт |
    | Balanced (по умолчанию) | AAC | ~192 кбит/с | Бесплатный аккаунт |
    | High | MP3 | ~320 кбит/с | Бесплатный аккаунт |
    | Superb | FLAC | Lossless | Подписка Plus |

    Качество можно изменить в любое время в настройках провайдера. Кодек и битрейт выбираются автоматически при начале воспроизведения в зависимости от того, что Yandex Music предлагает для каждого трека.

    Зашифрованные FLAC-треки дешифруются на лету по мере поступления данных. Перемотка обрабатывается ядром Music Assistant через ffmpeg.

## My Wave

=== "English"

    My Wave is Yandex Music's personalized infinite radio. It uses Yandex's Rotor recommendation engine to generate a continuous stream of tracks tailored to your listening habits.

    ### How it works

    1. When you open My Wave, the provider requests several batches of tracks from Yandex's Rotor API (the same engine behind "My Wave" in the official Yandex Music app).
    2. Each batch contains a few tracks. The provider fetches multiple batches at once to give you a longer playlist to start with.
    3. Duplicate tracks are automatically filtered out — if a track appeared in a previous batch, it won't show up again.
    4. In Browse, a **"Load more"** button appears at the bottom. Tapping it fetches the next batch and adds more tracks.
    5. The maximum number of tracks is configurable (default: 150). Once the limit is reached, no more tracks are loaded.

    ### Improving your recommendations

    The provider sends playback feedback to Yandex, similar to what the official app does:

    - When you **start playing** a track, Yandex is notified.
    - When you **finish** a track (listen to nearly the end), Yandex counts it as "liked."
    - When you **skip** a track (stop before the end), Yandex adjusts future recommendations accordingly.

    This feedback happens automatically — you don't need to do anything. Over time, Yandex learns your preferences and My Wave becomes more personalized.

    ### Where My Wave appears

    - **Browse** — A "My Wave" folder at the root of Yandex Music. You can play it directly or browse individual tracks.
    - **Home page (Discover)** — A "My Wave" recommendation section with a selection of personalized tracks.
    - **Library playlists** — A virtual "My Wave" playlist always appears in your playlist list for quick access.

    ### Similar tracks (Radio mode)

    When you start radio mode from any Yandex Music track, the provider uses Yandex's Rotor engine to find similar tracks. This works for any track, not just My Wave — it creates a station based on that specific track's style and genre.

=== "Русский"

    Моя волна — персонализированное бесконечное радио Yandex Music. Оно использует рекомендательный движок Rotor от Яндекса для генерации непрерывного потока треков, подобранных под ваши музыкальные предпочтения.

    ### Как это работает

    1. При открытии Моей волны провайдер запрашивает несколько пачек треков через Rotor API Яндекса (тот же движок, что стоит за «Моей волной» в официальном приложении Yandex Music).
    2. Каждая пачка содержит несколько треков. Провайдер загружает несколько пачек сразу, чтобы сформировать более длинный плейлист.
    3. Дубликаты автоматически отфильтровываются — если трек уже был в предыдущей пачке, он не появится снова.
    4. В Browse внизу отображается кнопка **«Load more»**. Нажатие загружает следующую пачку и добавляет новые треки.
    5. Максимальное количество треков настраивается (по умолчанию: 150). При достижении лимита загрузка прекращается.

    ### Улучшение рекомендаций

    Провайдер отправляет обратную связь о воспроизведении в Яндекс, аналогично официальному приложению:

    - При **начале воспроизведения** трека Яндекс получает уведомление.
    - При **завершении** трека (прослушивании до конца) Яндекс засчитывает его как «понравившийся».
    - При **пропуске** трека (остановка до окончания) Яндекс корректирует будущие рекомендации.

    Обратная связь отправляется автоматически — никаких действий не требуется. Со временем Яндекс изучает ваши предпочтения, и Моя волна становится более персонализированной.

    ### Где отображается Моя волна

    - **Browse** — папка «My Wave» / «Моя волна» в корне Yandex Music. Можно запустить воспроизведение напрямую или просматривать отдельные треки.
    - **Главная страница (Discover)** — секция рекомендаций «My Wave» с подборкой персонализированных треков.
    - **Плейлисты библиотеки** — виртуальный плейлист «My Wave» / «Моя волна» всегда отображается в списке плейлистов для быстрого доступа.

    ### Похожие треки (режим радио)

    При запуске режима радио с любого трека Yandex Music провайдер использует движок Rotor для поиска похожих треков. Это работает для любого трека, не только для Моей волны — создаётся станция на основе стиля и жанра конкретного трека.

## Liked Tracks

=== "English"

    Your liked (favorited) tracks are available as a virtual playlist:

    - Sorted in **reverse chronological order** — your most recently liked tracks appear first
    - Always visible in your library playlists and Browse section
    - The maximum number of tracks is configurable (default: 500)
    - Full track details (including album art) are fetched automatically

=== "Русский"

    Ваши понравившиеся (избранные) треки доступны в виде виртуального плейлиста:

    - Отсортированы в **обратном хронологическом порядке** — недавно добавленные треки отображаются первыми
    - Всегда видны в плейлистах библиотеки и разделе Browse
    - Максимальное количество треков настраивается (по умолчанию: 500)
    - Полная информация о треках (включая обложки) загружается автоматически

## Picks & Mixes

=== "English"

    Picks and Mixes let you browse curated playlists organized by theme, available in the Browse section.

    ### Browse structure

    ```
    Yandex Music
    ├── Picks
    │   ├── Mood      (e.g. Chill, Sad, Romantic, Party, Relax, ...)
    │   ├── Activity  (e.g. Workout, Focus, Morning, Evening, Driving, ...)
    │   ├── Era       (e.g. 80s, 90s, 2000s, Retro, ...)
    │   └── Genres    (e.g. Rock, Jazz, Classical, Electronic, R&B, Hip-Hop, ...)
    └── Mixes
        └── Seasonal  (e.g. Winter, Summer, Autumn, New Year, ...)
    ```

    ### Dynamic tag discovery

    All tags are **discovered dynamically** from the Yandex Music Landing API — the provider does not rely on a hardcoded list. Only tags that Yandex actually returns are shown, which means:

    - **No empty folders** — if a tag doesn't exist in the current API response, it simply won't appear
    - **New tags appear automatically** — when Yandex adds new curated collections, they show up without a provider update
    - **Categories are hidden when empty** — if no tags were discovered for a category (e.g. Era), the folder is not shown

    Discovered tags are categorized into Mood, Activity, Era, or Genres using an internal mapping. Tags not in the mapping default to the Mood category. Useless tags (like "Albums with video shots") are blacklisted and filtered out.

    Tag discovery results are cached for 1 hour. Tag playlist contents are cached for 10 minutes.

    ### Playback behavior

    Category and tag folders have **Play disabled** (`is_playable=False`). This prevents accidentally queuing thousands of tracks when pressing Play on a folder that contains playlists. To play music, navigate into a specific tag and choose a playlist.

=== "Русский"

    Подборки и Миксы позволяют просматривать тематические плейлисты в разделе Browse.

    ### Структура Browse

    ```
    Yandex Music
    ├── Подборки (Picks)
    │   ├── Настроение  (напр. Chill, Sad, Romantic, Party, Relax, ...)
    │   ├── Активность   (напр. Workout, Focus, Morning, Evening, Driving, ...)
    │   ├── Эпоха        (напр. 80s, 90s, 2000s, Retro, ...)
    │   └── Жанры        (напр. Rock, Jazz, Classical, Electronic, R&B, Hip-Hop, ...)
    └── Миксы (Mixes)
        └── Сезонные     (напр. Winter, Summer, Autumn, New Year, ...)
    ```

    ### Динамическое обнаружение тегов

    Все теги **обнаруживаются динамически** через Landing API Yandex Music — провайдер не использует фиксированный список. Показываются только теги, которые Яндекс реально вернул, что означает:

    - **Нет пустых папок** — если тег не существует в текущем ответе API, он просто не отображается
    - **Новые теги появляются автоматически** — когда Яндекс добавляет новые подборки, они показываются без обновления провайдера
    - **Пустые категории скрываются** — если для категории (например, Эпоха) не нашлось ни одного тега, папка не отображается

    Обнаруженные теги распределяются по категориям Настроение, Активность, Эпоха, Жанры через внутренний маппинг. Теги вне маппинга попадают в категорию Настроение. Бесполезные теги (например, «Альбомы с видеошотами») добавлены в чёрный список и отфильтровываются.

    Результаты обнаружения тегов кэшируются на 1 час. Содержимое плейлистов тегов кэшируется на 10 минут.

    ### Поведение воспроизведения

    У папок категорий и тегов **отключена кнопка Play** (`is_playable=False`). Это предотвращает случайную загрузку тысяч треков при нажатии Play на папке, содержащей плейлисты. Для воспроизведения музыки нужно зайти в конкретный тег и выбрать плейлист.

## Home Page Recommendations

=== "English"

    The home page shows up to 9 recommendation sections. Each section can be shown or hidden, and its position can be changed, via the [Edit Homescreen](../ui.md#view-home) feature (blue icon in the top right of the home page).

    | Section | What it shows | How often it refreshes |
    |---------|--------------|----------------------|
    | **My Wave** | Personalized tracks from Rotor AI | Every 10 minutes |
    | **Made for You** | Playlist of the Day, DejaVu, Premiere, and other auto-generated playlists | Every 30 minutes |
    | **Chart** | Current top tracks | Every hour |
    | **New Releases** | Latest album releases | Every hour |
    | **New Playlists** | Fresh editorial playlists | Every hour |
    | **Top Picks** | Top curated playlists (tag: "top") | Every hour |
    | **Mood Mix** | Playlists for a random mood (rotates between chill, sad, romantic, party, relax) | Every 30 minutes |
    | **Activity Mix** | Playlists for a random activity (rotates between workout, focus, morning, evening, driving) | Every 30 minutes |
    | **Seasonal Mix** | Season-appropriate playlists based on the current month (winter, summer, autumn) | Every 6 hours |

    The Mood Mix and Activity Mix sections show a different mood/activity each time they refresh, so you'll see variety throughout the day. The Seasonal Mix picks the season based on the current month automatically.

=== "Русский"

    На главной странице отображается до 9 секций рекомендаций. Каждую секцию можно показать или скрыть, а также изменить её позицию через функцию [Edit Homescreen](../ui.md#view-home) (синяя иконка в правом верхнем углу главной страницы).

    | Секция | Что показывает | Частота обновления |
    |--------|---------------|-------------------|
    | **My Wave** | Персонализированные треки от Rotor AI | Каждые 10 минут |
    | **Made for You** | Плейлист дня, DejaVu, Премьера и другие автоматически сгенерированные плейлисты | Каждые 30 минут |
    | **Chart** | Текущие популярные треки | Каждый час |
    | **New Releases** | Последние релизы альбомов | Каждый час |
    | **New Playlists** | Свежие редакционные плейлисты | Каждый час |
    | **Top Picks** | Лучшие тематические плейлисты (тег: "top") | Каждый час |
    | **Mood Mix** | Плейлисты по настроению (ротация: chill, sad, romantic, party, relax) | Каждые 30 минут |
    | **Activity Mix** | Плейлисты по активности (ротация: workout, focus, morning, evening, driving) | Каждые 30 минут |
    | **Seasonal Mix** | Сезонные плейлисты на основе текущего месяца (winter, summer, autumn) | Каждые 6 часов |

    Секции Mood Mix и Activity Mix показывают разное настроение/активность при каждом обновлении, обеспечивая разнообразие в течение дня. Seasonal Mix автоматически определяет сезон по текущему месяцу.

## Lyrics

=== "English"

    Lyrics are fetched directly from Yandex Music when viewing track details:

    - **Synced lyrics (LRC)** — With timestamps for karaoke-style synchronized display, when available
    - **Plain text lyrics** — Used as fallback when synced lyrics are not available
    - **Automatic caching** — Lyrics are cached together with track data for 30 days

    !!! note
        Lyrics availability depends on the track and may vary by region due to licensing restrictions. If lyrics are unavailable for your region, the provider handles this silently without errors.

=== "Русский"

    Тексты песен загружаются напрямую из Yandex Music при просмотре информации о треке:

    - **Синхронизированные тексты (LRC)** — С временными метками для караоке-стиля отображения, при наличии
    - **Обычные тексты** — Используются как запасной вариант, когда синхронизированные тексты недоступны
    - **Автоматическое кэширование** — Тексты кэшируются вместе с данными трека на 30 дней

    !!! note
        Доступность текстов зависит от трека и может отличаться по регионам из-за лицензионных ограничений. Если тексты недоступны для вашего региона, провайдер обрабатывает это без ошибок.

## Configuration

=== "English"

    ### Obtaining the Token

    1. Open your browser and navigate to [Yandex OAuth](https://oauth.yandex.ru/authorize?response_type=token&client_id=23cabbbdc6cd418abb4b39c32c41195d)
    2. Log in with your Yandex account if prompted
    3. After authorization, you will be redirected to a URL containing `access_token=YOUR_TOKEN`
    4. Copy the token value (the part after `access_token=` and before `&`)
    5. Paste this token into the Music Assistant Yandex Music provider configuration

    ### Settings

    The provider has 6 settings. The first 3 are shown by default; the rest are under "Show advanced settings."

    | Setting | Default | Description |
    |---------|---------|-------------|
    | **Yandex Music Token** | — | Your OAuth token (see above) |
    | **Reset authentication** | — | Clears the saved token so you can re-authenticate |
    | **Audio quality** | Balanced | Choose between Efficient, Balanced, High, or Superb (FLAC) |

    **Advanced settings** (click "Show advanced settings" to see these):

    | Setting | Default | Description |
    |---------|---------|-------------|
    | **My Wave maximum tracks** | 150 | How many tracks to load for My Wave. Lower = faster loading |
    | **Liked Tracks maximum tracks** | 500 | How many liked tracks to show. Lower = faster loading |
    | **API Base URL** | api.music.yandex.net | Only change if Yandex changes their API endpoint |

=== "Русский"

    ### Получение токена

    1. Откройте браузер и перейдите на страницу [Yandex OAuth](https://oauth.yandex.ru/authorize?response_type=token&client_id=23cabbbdc6cd418abb4b39c32c41195d)
    2. Войдите в свой аккаунт Яндекса, если потребуется
    3. После авторизации вы будете перенаправлены на URL, содержащий `access_token=ВАШ_ТОКЕН`
    4. Скопируйте значение токена (часть после `access_token=` и перед `&`)
    5. Вставьте токен в настройки провайдера Yandex Music в Music Assistant

    ### Настройки

    Провайдер имеет 6 настроек. Первые 3 отображаются по умолчанию; остальные доступны через «Show advanced settings».

    | Настройка | По умолчанию | Описание |
    |-----------|-------------|----------|
    | **Yandex Music Token** | — | Ваш OAuth-токен (см. выше) |
    | **Reset authentication** | — | Сбрасывает сохранённый токен для повторной авторизации |
    | **Audio quality** | Balanced | Выбор между Efficient, Balanced, High или Superb (FLAC) |

    **Расширенные настройки** (нажмите «Show advanced settings»):

    | Настройка | По умолчанию | Описание |
    |-----------|-------------|----------|
    | **My Wave maximum tracks** | 150 | Сколько треков загружать для Моей волны. Меньше = быстрее загрузка |
    | **Liked Tracks maximum tracks** | 500 | Сколько избранных треков показывать. Меньше = быстрее загрузка |
    | **API Base URL** | api.music.yandex.net | Менять только если Яндекс изменит адрес API |

## Localization

=== "English"

    All folder and section names automatically adapt to your Music Assistant language:

    - **Russian** (`ru_*` locales): "Моя волна", "Мне нравится", "Подборки", "Миксы", etc.
    - **Other locales**: "My Wave", "My Favorites", "Picks", "Mixes", etc.

=== "Русский"

    Все названия папок и секций автоматически адаптируются к языку Music Assistant:

    - **Русский** (локали `ru_*`): «Моя волна», «Мне нравится», «Подборки», «Миксы» и т.д.
    - **Другие локали**: «My Wave», «My Favorites», «Picks», «Mixes» и т.д.

## Known Issues / Notes

=== "English"

    - The OAuth token may expire and need to be refreshed periodically
    - During long sessions the API may occasionally disconnect; the provider reconnects automatically
    - Some curated playlists may be unavailable in certain regions due to geo-restrictions
    - Multiple Yandex Music accounts are not yet supported

=== "Русский"

    - OAuth-токен может истечь и потребовать периодического обновления
    - При длительных сессиях API может периодически отключаться; провайдер переподключается автоматически
    - Некоторые тематические плейлисты могут быть недоступны в определённых регионах из-за гео-ограничений
    - Поддержка нескольких аккаунтов Yandex Music пока не реализована
