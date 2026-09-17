# AniVault API Endpoints Explained

Below is a detailed breakdown of every single API endpoint available in the AniVault application. This guide follows the exact order from the `api-endpoints.txt` file and takes reference from the frontend documentation hub.

---

## 📺 Streaming & Core System

### `GET /api/search`
* **Description:** Searches for an anime by title using the AniList backend.
* **Parameters:** `q` (string, required).
* **Usage:** Returns a structured list of matches including the AniList ID, MAL ID, title, cover image, episode count, and airing status. The AniList ID is vital for routing streaming queries.

### `GET /api/info`
* **Description:** Retrieves basic anime metadata along with site-specific IDs from the scraper sources.
* **Parameters:** Requires either `anilistId` or `malId`.
* **Usage:** Provides a cross-reference mapping of the unique slugs or IDs that different scrapers (like Zoro, Gogoanime, AnimeHeaven, Anikoto) use internally for that particular show.

### `GET /api/episodes`
* **Description:** Fetches the full, comprehensive list of playable episodes for a specific anime from a specific scraper source.
* **Parameters:** Requires either `anilistId` or `malId`. Optionally accepts a `source` override (e.g., `animeheaven` or `anikoto`).

### `GET /api/servers`
* **Description:** Checks a specific episode and returns all available video hosting servers.
* **Parameters:** Requires `anilistId` or `malId`, and `ep` (episode number). Optionally accepts `type` (`sub`, `dub`, `raw`, `all`) and `source`.
* **Usage:** Returns a list of backend servers (e.g., Megacloud, Vidstreaming, StreamRuby) hosting the episode.

### `GET /api/watch/:source/:id/:ep/:type`
* **Description:** The primary streaming endpoint (path-based). Returns the video player embed URL, the direct HLS playlist (`.m3u8`), and subtitle tracks.
* **Parameters:** Path parameters: `source`, `id` (AniList ID or `mal-{id}` format), `ep` (episode number), and `type` (`sub`/`dub`). Accepts an optional `server` query parameter.
* **Usage:** Extracts and decrypts streaming links so a frontend video player can directly play the anime. Includes intro/outro skip timestamps where supported.

### `GET /api/proxy/hls`
* **Description:** A dedicated proxy endpoint for routing `.m3u8` HLS playlist files.
* **Usage:** Circumvents CORS (Cross-Origin Resource Sharing) restrictions that typically block browsers from requesting `.m3u8` playlists directly from external hosting servers.

### `GET /api/proxy/subtitle`
* **Description:** A dedicated proxy endpoint for routing subtitle files (like `.vtt` or `.ass`).
* **Usage:** Ensures subtitle files can be loaded into frontend video players without CORS blockage.

### `GET /api/proxy/video`
* **Description:** A proxy endpoint for routing raw video requests (such as direct `.mp4` chunks).
* **Usage:** Acts as a secure intermediary to fetch raw video data while bypassing client-side origin restrictions.

### `GET /api/watch`
* **Description:** The query-parameter equivalent of the primary watch endpoint.
* **Parameters:** `source`, `anilistId`/`malId`/`heavenId`, `ep`, `type`.
* **Usage:** Retained for backward compatibility. It performs the exact same function as the path-based endpoint but uses URL query arguments.

---

## 📖 MyAnimeList (MAL) Scraper

*These endpoints scrape data directly from MyAnimeList, bypassing the need for third-party wrappers like Jikan.*

### `GET /api/mal/anime/:id`
* **Description:** Fetches full, comprehensive anime details directly from the MAL page.
* **Usage:** Returns data such as synopsis, genres, studios, airing dates, scores, ranks, and English/Japanese titles.

### `GET /api/mal/anime/:id/episodes`
* **Description:** Fetches the paginated episode list directly from MAL's database (returns up to 100 episodes per page).

### `GET /api/mal/anime/:id/episodes/:epNum`
* **Description:** Fetches detailed metadata for one specific episode.
* **Usage:** Retrieves the episode's title, airing date, and flags (such as whether it is a filler or a recap episode).

### `GET /api/mal/search`
* **Description:** Performs a MyAnimeList text search.
* **Usage:** Formats responses in a "Jikan-shaped" structure for compatibility, returning matches based on MAL's search engine.

### `GET /api/mal/search/debug`
* **Description:** A debugging endpoint for MAL text searches.
* **Usage:** Used internally by developers to inspect raw HTML layouts or scraper responses when MAL changes its DOM structure.

### `GET /api/mal/anime/:id/external`
* **Description:** Fetches official site links and outbound related links.
* **Usage:** Returns official websites, Twitter/X profiles, Wikipedia links, and related database pages.

### `GET /api/mal/anime/:id/characters`
* **Description:** Retrieves the cast and voice actors for a given anime.
* **Usage:** Includes character names, roles (Main/Supporting), and the corresponding voice actors and their languages.

### `GET /api/mal/character/:id`
* **Description:** Retrieves details for a single character in one scrape.
* **Usage:** Returns character bio, animeography (shows they appeared in), and voice actors.

### `GET /api/mal/anime/:id/pictures`
* **Description:** Fetches the anime's picture gallery from MAL.
* **Usage:** Returns high-quality official promotional posters and key visuals.

### `GET /api/mal/character/:id/pictures`
* **Description:** Fetches the picture gallery for a specific character.

### `GET /api/mal/anime/:id/themes`
* **Description:** Scrapes Opening (OP) and Ending (ED) theme song credits.
* **Usage:** Often includes Spotify or Apple Music links when available on MAL.

### `GET /api/mal/anime/:id/videos`
* **Description:** Fetches Trailers (PVs) and official music videos for the anime.
* **Usage:** Returns the associated YouTube video IDs for easy embedding.

### `GET /api/mal/anime/:id/streaming`
* **Description:** Fetches a list of legal streaming platforms (e.g., Crunchyroll, Netflix, Hulu) that officially license the anime.

### `GET /api/mal/anime/:id/recommendations`
* **Description:** Retrieves the top 12 "you might also like" recommendations for an anime, sorted by community votes.

---

## 🎨 Metadata & Art (AniList, TMDB, Kitsu)

### `GET /api/anilist/season`
* **Description:** Fetches the currently airing anime season from AniList.
* **Usage:** Useful for building "Currently Airing" or "Trending this Season" sections on a homepage.

### `GET /api/anilist/top-banners`
* **Description:** Retrieves high-quality banner images (hero backgrounds) for top-rated anime, keyed by their MAL ID.

### `GET /api/anilist/id`
* **Description:** A conversion endpoint to map a MyAnimeList ID (`malId`) to an AniList ID (`anilistId`).

### `GET /api/anilist/episodes`
* **Description:** Retrieves AniList's own `streamingEpisodes` list.
* **Usage:** Provides episode titles and thumbnails. Often used as a last-resort fallback when other scrapers fail.

### `GET /api/anilist/anime`
* **Description:** Retrieves poster and cover art from AniList.
* **Usage:** Formatted to share the same JSON shape across TMDB and Kitsu variants.

### `GET /api/tmdb/episode-thumb`
* **Description:** Retrieves a high-quality still/thumbnail image for a specific episode from The Movie Database (TMDB).

### `GET /api/tmdb/anime`
* **Description:** Retrieves poster, cover art, and transparent logos for an anime from TMDB.

### `GET /api/kitsu/episode-thumb`
* **Description:** Retrieves a specific episode's thumbnail image from Kitsu.

### `GET /api/kitsu/anime`
* **Description:** Retrieves poster and cover art for an anime from Kitsu.

---

## 🧩 Combined Endpoints & System Health

### `GET /api/anime`
* **Description:** A unified, heavy-lifting endpoint.
* **Usage:** Combines full MAL details with high-quality poster/cover/logo art from other sources into a single API call, reducing round trips for the frontend.

### `GET /api/episode`
* **Description:** A unified episode metadata endpoint.
* **Usage:** Combines MAL episode metadata (filler/recap status, titles, air dates) with a high-quality episode thumbnail from TMDB or Kitsu. Can be queried for a single episode or an entire show.

### `GET /api/health`
* **Description:** Server health, uptime, and cache statistics.
* **Usage:** Unified with `anivault.co`'s `/healthz` shape. It outputs the process status, cache hit/miss rates, and active scraper backends (e.g., `animeheaven`, `anikoto`).
